<properties
pageTitle="単一の実験から複数のモデルを作成する | Microsoft Azure"
description="アルゴリズムは同じでトレーニング データセットだけが異なる複数の Machine Learning モデルと複数の Web サービス エンドポイントを PowerShell を使用して作成します。"
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="05/12/2016"
ms.author="garye;haining"/>

# PowerShell を使用して 1 つの実験から複数の Machine Learning モデルと Web サービス エンドポイントを作成する

機械学習について多くの人が考えることは、トレーニング ワークフローと使用アルゴリズムは同じで、入力となるトレーニング データセットだけが異なる複数のモデルを作成できないものだろうか、ということです。この記事では、Azure Machine Learning Studio で 1 つの実験だけを使い、規模の制約なくこの課題に対応する方法を紹介しています。

たとえば皆さんが、自転車レンタルのフランチャイズ事業を世界規模で展開しているとしましょう。過去のデータに基づいてレンタルの需要を予測するために、回帰モデルを構築する必要があります。レンタルの拠点は全世界で 1,000 店舗存在し、各拠点に固有の重要な要素 (日付、時刻、天気、交通状況) を含んだデータセットを拠点ごとに収集済みです。

全拠点のすべてのデータセットをマージして 1 回だけモデルをトレーニングすることは可能です。しかし環境は拠点ごとに異なるため、手法としては、拠点ごとのデータセットを使用して回帰モデルを個別にトレーニングした方が適切と考えられます。そうすれば、トレーニング済みのモデルごとに異なる店舗サイズ、ボリューム、地勢、人口、自転車に配慮した交通環境*など*を反映することができます。

ただ最良の手法であったとしても、それぞれ固有の拠点を表す 1,000 件ものトレーニング実験を Azure Machine Learning で作成するのは非現実的です。個々の実験の構成要素が、トレーニング データセットを除いてすべて同じであることを考えると、膨大な手間のかかる作業であるだけでなく非効率な方法でもあります。

幸いこの処理には、[Azure Machine Learning の再トレーニング API](machine-learning-retrain-models-programmatically.md) を使用できます。[Azure Machine Learning PowerShell](machine-learning-powershell-module.md) でタスクを自動化することが可能です。

> [AZURE.NOTE] ここではサンプルの実行時間を短くするために、拠点数を 1,000 から 10 に減らすことにします。しかし拠点が 1,000 か所あっても原理と手順は同じです。唯一の違いは、1,000 件のデータセットをトレーニングする場合、以下の PowerShell スクリプトを並列実行するかどうかが検討事項になってくるということです。その方法はこの記事で取り上げる範囲を超えていますが、PowerShell のマルチスレッド化の例は、インターネットを検索すれば見つかります。

## トレーニング実験のセットアップ

ここでは、[Cortana インテリジェンス ギャラリー](http://gallery.cortanaintelligence.com)で作成した[トレーニング実験](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1)を例として使用します。この実験を [Azure Machine Learning Studio](https://studio.azureml.net) ワークスペースで開いてください。

>[AZURE.NOTE] この例に沿って理解するためには、無料ワークスペースではなく標準のワークスペースを使用する必要があります。エンドポイントは顧客ごとに 1 つ作成します (合計 10 エンドポイント)。無料のワークスペースはエンドポイント数が 3 個に限定されているため、標準のワークスペースが必要となります。無料のワークスペースしかない場合は、拠点数が 3 つのみとなるように以下のスクリプトを変更してください。

この実験では**データのインポート** モジュールを使用して、Azure ストレージ アカウントからトレーニング データセット *customer001.csv* をインポートします。トレーニング データセットを自転車レンタルの全拠点から収集し、*rentalloc001.csv* ～ *rentalloc10.csv* のファイル名で同じ Blob Storage の場所に保存したとします。

![image](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

**Train Model** モジュールに **Web Service Output** モジュールが追加されていることに注目してください。この実験を Web サービスとしてデプロイすると、その出力に関連付けられているエンドポイントから、トレーニング済みのモデルが .ilearner ファイル形式で返されます。

また、**データのインポート** モジュールで使用する URL の Web サービス パラメーターを設定しています。このパラメーターを使用することで、拠点ごとのモデルをトレーニングするためのトレーニング データセットを個別に指定することができます。同じことは他の方法で行うこともできます。たとえば、Web サービス パラメーターを持った SQL クエリを使用して SQL Azure データベースからデータを取り込んだり、単に **Web Service Input** モジュールを使用してデータセットを Web サービスに渡したりすることができます。

![image](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

それではトレーニング データセットとして既定値の *rental001.csv* を使用し、このトレーニング実験を実行してみましょう。**Evaluate** モジュールの出力を表示 (出力をクリックして **[Visualize]** (視覚化) を選択) すると、*AUC* = 0.91 という良好なパフォーマンスが得られていることを確認できます。これで、このトレーニング実験から Web サービスをデプロイする準備ができました。

## トレーニング Web サービスとスコア付け Web サービスのデプロイ

トレーニング Web サービスをデプロイするには、実験キャンバスの下にある **[Set Up Web Service]** (Web サービスのセットアップ) ボタンをクリックし、**[Deploy Web Service]** (Web サービスのデプロイ) を選択します。この Web サービスを "Bike Rental Training" と呼ぶことにします。

次にスコア付け Web サービスをデプロイする必要があります。そのためには、キャンバスの下にある **[Set Up Web Service]** (Web サービスのセットアップ) をクリックし、**[Predictive Web Service]** (予測 Web サービス) を選択します。これでスコア付け実験が作成されます。これを Web サービスとして利用するためには、若干の調整を加える必要があります。たとえば入力データからラベル列 "cnt" を削除すると共に、出力内容はインスタンス ID および対応する予測値に限定します。

この作業を省略する場合は、既に作成済みの[予測実験](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1)をギャラリーで開いてもかまいません。

Web サービスをデプロイするには、予測実験を実行し、キャンバスの下にある **[Deploy Web Service]** (Web サービスのデプロイ) ボタンをクリックします。スコア付け Web サービスには "Bike Rental Scoring" という名前を付けます。

## まったく同じ 10 個の Web サービス エンドポイントを PowerShell で作成する

この Web サービスには、既定のエンドポイントが付属しています。しかし既定のエンドポイントは更新できないため、ここでは使用しません。必要なことは、エンドポイントを拠点ごとに 1 つ、合計 10 個作成することです。これを PowerShell で行います。

まず、PowerShell 環境をセットアップします。

	Import-Module .\AzureMLPS.dll
	# Assume the default configuration file exists and is properly set to point to the valid Workspace.
	$scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
	$trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

そのうえで次の PowerShell コマンドを実行します。

	# Create 10 endpoints on the scoring web service.
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    Write-Host ('adding endpoint ' + $endpointName + '...')
	    Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
	}

これで 10 個のエンドポイントが作成されました。いずれのエンドポイントにも、*customer001.csv* でトレーニングされた同じトレーニング済みモデルが含まれています。これらは Azure 管理ポータルで確認できます。

![イメージ](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## 個別のトレーニング データセットを使用するように PowerShell を使ってエンドポイントを更新する

次に、各顧客の個別のデータで独自にトレーニングされたモデルでエンドポイントを更新します。ただし最初に、これらのモデルを **Bike Rental Training** Web サービスから生成する必要があります。**Bike Rental Training** Web サービスに戻りましょう。10 個の異なるモデルを作成するためには、対応する BES エンドポイントを 10 回、10 個の異なるトレーニング データセットで呼び出す必要があります。ここでは、PowerShell コマンドレット **InovkeAmlWebServiceBESEndpoint** を使用してこの処理を実行します。

また、Blob Storage アカウントの資格情報を `$configContent` (つまり、`AccountName`、`AccountKey`、`RelativeLocation` の各フィールド) に与える必要があります。`AccountName` には、自分が所有するいずれかのアカウント名を指定できます。アカウント名は、**従来の Azure 管理ポータル** (*[ストレージ]* タブ) に表示されます。ストレージ アカウントをクリックし、一番下にある **[アクセス キーの管理]** ボタンを押して*プライマリ アクセス キー*をコピーすることによって、対応する `AccountKey` を確認できます。`RelativeLocation` には、新しいモデルの保存先を、ストレージを起点とする相対パスで指定します。たとえば、以下のスクリプトでパス `hai/retrain/bike_rental/` が指し示しているのは、`hai` という名前のコンテナーであり、`/retrain/bike_rental/` はサブフォルダーです。現在サブフォルダーをポータルの UI で作成することはできませんが、[いくつかの Azure ストレージ エクスプローラー](../storage/storage-explorers.md)で作成することはできます。トレーニング済みの新しいモデル (.ilearner ファイル) は、ストレージに新しいコンテナーを作成して保存することをお勧めします。コンテナーを作成するには、ストレージ ページの一番下にある **[追加]** ボタンをクリックし、`retrain` という名前を付けます。まとめると、以下のスクリプトでは、`AccountName`、`AccountKey`、`RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`) に関して変更が必要となります。

	# Invoke the retraining API 10 times
	# This is the default (and the only) endpoint on the training web service
	$trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
	$submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
	$apiKey = $trainingSvcEp.PrimaryKey;
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
	    $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
	    Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
	    Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
	}

>[AZURE.NOTE] この操作でサポートされているモードは、BES エンドポイントのみです。RRS は、トレーニング済みモデルの生成には使用できません。

ご覧のように、この例では、BES ジョブ構成 json ファイルを 10 個構築する代わりに、構成文字列を動的に作成し、それを **InvokeAmlWebServceBESEndpoint** コマンドレットの *jobConfigString* パラメーターに渡しています。実際、ディスク上にコピーを保持する必要はありません。

問題がなければ、しばらくすると Azure ストレージ アカウントに 10 個の .ilearner ファイルが生成されます (*model001.ilearner* ～ *model010.ilearner*)。後は、PowerShell コマンドレット **Patch-AmlWebServiceEndpoint** を使用し、スコア付け Web サービスの 10 個のエンドポイントをこれらのモデルで更新することになります。既に述べたように更新できるのは、先ほどプログラムから作成した既定以外のエンドポイントだけであることに注意してください。

	# Patch the 10 endpoints with respective .ilearner models
	$baseLoc = 'http://bostonmtc.blob.core.windows.net/'
	$sasToken = '<my_blob_sas_token>'
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
	    Write-Host ('Patching endpoint ' + $endpointName + '...');
	    Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
	}

このコードはすぐに実行が完了すると思われます。実行が完了すると、予測 Web サービスの 10 個のエンドポイントが作成されます。それぞれのエンドポイントは、レンタル拠点に固有のデータセットで独自にトレーニングされたトレーニング済みのモデルを含んでいますが、そのすべては単一のトレーニング実験から得られたものです。これを検証するには、**InvokeAmlWebServiceRRSEndpoint** コマンドレットに同じ入力データを渡してこれらのエンドポイントを呼び出します。モデルはそれぞれ異なるトレーニング セットでトレーニングされているため、エンドポイントが正しく機能していれば、異なる予測結果が返されます。

## PowerShell スクリプト全体

以下に、すべてのソース コードを掲載します。

	Import-Module .\AzureMLPS.dll
	# Assume the default configuration file exists and properly set to point to the valid workspace.
	$scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
	$trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

	# Create 10 endpoints on the scoring web service
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    Write-Host ('adding endpoint ' + $endpontName + '...')
	    Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
	}

	# Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
	$trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
	$submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
	$apiKey = $trainingSvcEp.PrimaryKey;
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
	    $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
	    Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
	    Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
	}

	# Patch the 10 endpoints with respective .ilearner models
	$baseLoc = 'http://bostonmtc.blob.core.windows.net/'
	$sasToken = '?test'
	For ($i = 1; $i -le 10; $i++){
	    $seq = $i.ToString().PadLeft(3, '0');
	    $endpointName = 'rentalloc' + $seq;
	    $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
	    Write-Host ('Patching endpoint ' + $endpointName + '...');
	    Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
	}

<!---HONumber=AcomDC_0914_2016-->