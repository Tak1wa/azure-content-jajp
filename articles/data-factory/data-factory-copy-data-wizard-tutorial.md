<properties 
	pageTitle="チュートリアル: コピー ウィザードを使用してパイプラインを作成する" 
	description="このチュートリアルでは、Data Factory でサポートされているコピー ウィザードを使用して、コピー アクティビティがある Azure Data Factory パイプラインを作成します。" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="09/16/2016" 
	ms.author="spelluru"/>

# チュートリアル: コピー アクティビティがあるパイプラインを Data Factory コピー ウィザードで作成する
> [AZURE.SELECTOR]
- [概要と前提条件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Azure ポータル](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [コピー ウィザード](data-factory-copy-data-wizard-tutorial.md)

このチュートリアルでは、Data Factory コピー ウィザードを使用して、コピー アクティビティがあるパイプラインをデータ ファクトリに作成します。最初に、Azure Portal を使用してデータ ファクトリを作成します。次に、コピー ウィザードを使用して Data Factory のリンクされたサービス、データセット、および Azure BLOB ストレージから Azure SQL データベースにデータをコピーするコピー アクティビティがあるパイプラインを作成します。コピー アクティビティの詳細については、「[データ移動アクティビティ](data-factory-data-movement-activities.md)」をご覧ください。

> [AZURE.IMPORTANT] このチュートリアルを実行する前に、「[チュートリアルの概要](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)」に目を通し、**前提条件**の手順を完了してください。

## データ ファクトリの作成
この手順では、Azure ポータルを使用して、**ADFTutorialDataFactory** という名前の Azure データ ファクトリを作成します。

1.	[Azure ポータル](https://portal.azure.com)にログインしたら、左上隅にある **[新規]** をクリックします。**[作成]** ブレードで **[データ分析]** を選択し、**[データ分析]** ブレードで **[Data Factory]** をクリックします。

	![[新規] -> [DataFactory]](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)

6. **[新しいデータ ファクトリ]** ブレードで以下の手順を実行します。
	1. **[名前]** に「**ADFTutorialDataFactory**」と入力します。
	
  		![[新しいデータ ファクトリ] ブレード](./media/data-factory-copy-data-wizard-tutorial/getstarted-new-data-factory.png)
	2. **[リソース グループ名]** をクリックし、以下の手順を実行します。
		1. **[新しいリソース グループを作成]** をクリックします。
		2. **[リソース グループの作成]** ブレードで、リソース グループの **[名前]** に「**ADFTutorialResourceGroup**」と入力し、**[OK]** をクリックします。

			![リソース グループの作成](./media/data-factory-copy-data-wizard-tutorial/create-new-resource-group.png)

		このチュートリアルの一部の手順は、**ADFTutorialResourceGroup** という名前のリソース グループを使用することを前提としています。リソース グループの詳細については、[リソース グループを使用した Azure のリソースの管理](../resource-group-overview.md)に関するページを参照してください。
7. **[新しいデータ ファクトリ]** ブレードで、**[スタート画面に追加する]** が選択されていることを確認してください。
8. **[新しいデータ ファクトリ]** ブレードで **[作成]** をクリックします。

	Azure Data Factory の名前はグローバルに一意にする必要があります。**""ADFTutorialDataFactory" という名前の Data Factory は使用できません"** というエラーが発生した場合は、データ ファクトリの名前を変更して (yournameADFTutorialDataFactory など) 作成し直してください。Data Factory アーティファクトの名前付け規則については、「[Azure Data Factory - 名前付け規則](data-factory-naming-rules.md)」を参照してください。
	 
	![使用できない Data Factory 名](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
	
	> [AZURE.NOTE] データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。

9. 左側の **[通知]** ハブをクリックし、作成プロセスからの通知を探します。**[通知]** ブレードが開いている場合は、**[X]** をクリックして閉じます。
10. 作成が完了すると、次の図に示すような **[Data Factory]** ブレードが表示されます。

    ![データ ファクトリのホーム ページ](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## パイプラインの作成

1. Data Factory のホーム ページで **[データのコピー]** タイルをクリックして、**コピー ウィザード**を起動します。

	> [AZURE.NOTE] 承認中であることを示すメッセージが表示されたまま Web ブラウザーが停止してしまう場合は、**サード パーティの Cookie とサイト データをブロック**する設定を無効にしてください。または、有効な状態のまま **login.microsoftonline.com** に対する例外を作成し、そのうえで、もう一度ウィザードを起動してください。
2. **[プロパティ]** ページで次の操作を実行します。
	1. **[タスク名]** に「**CopyFromBlobToAzureSql**」と入力します。
	2. **説明**を入力します (省略可能)。
	3. **[開始日時]** と **[終了日時]** の日時をメモします。**[終了日時]** を **[開始日時]** に設定された日の翌日に変更します。
	3. **[次へ]** をクリックします。

	![Copy Tool - Properties page](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png)
3. **[Source data store (ソース データ ストア)]** ページで、**[Azure BLOB ストレージ]** タイルをクリックします。このページを使用して、コピー タスクのソース データ ストアを指定します。既存のデータ ストアのリンクされたサービスを使用するか、新しいデータ ストアを指定できます。既存のリンクされたサービスを使用するには、**[FROM EXISTING LINKED SERVICES (既存のリンクされたサービスから)]** をクリックし、適切なリンクされたサービスを選択します。

	![Copy Tool - Source data store page](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
5. **[Specify the Azure Blob storage account (Azure BLOB ストレージ アカウントの指定)]** ページで次の操作を実行します。
	1. **[リンクされたサービス名]** に「**AzureStorageLinkedService**」と入力します。
	2. **[Account selection method (アカウントの選択方法)]** が **[From Azure subscriptions (Azure サブスクリプションから)]** になっていることを確認します。
	3. 選択したサブスクリプションで利用できる Azure ストレージ アカウントの一覧から、使用する **Azure ストレージ アカウント**を選択します。ストレージ アカウント設定を手動で入力することもできます。その場合は、**[Account selection method (アカウントの選択方法)]** で **[Enter manually (手動で入力)]** オプションを選択し、**[次へ]** をクリックします。

	![Copy Tool - Specify the Azure Blob storage account](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
6. **[Choose the input file or folder (入力ファイルまたはフォルダーの選択)]** ページで次の操作を実行します。
	1. **adftutorial** フォルダーに移動します。
	2. **emp.txt** を選択し、**[選択]** をクリックします。
	3. **[次へ]** をクリックします。

	![Copy Tool - Choose the input file or folder](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
7. **[Choose the input file or folder (入力ファイルまたはフォルダーの選択)]** ページで、**[次へ]** をクリックします。**[Binary copy (バイナリ コピー)]** は選択しないでください。

	![Copy Tool - Choose the input file or folder](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png)
8. **[File format settings (ファイル形式設定)]** ページで、**既定の**値を選択した状態で **[次へ]** をクリックします。

	![Copy Tool - File format settings](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)
8. [Destination data store (ターゲット データ ストア)] ページで、**[Azure SQL Database]** タイル、**[次へ]** の順にクリックします。
9. **[Specify the Azure SQL database (Azure SQL Database の指定)]** ページで次の操作を実行します。
	1. **[リンクされたサービス名]** フィールドに「**AzureSqlLinkedService**」と入力します。
	2. **[Server/database selection method (サーバー/データベースの選択方法)]** が **[From Azure subscriptions (Azure サブスクリプションから)]** に設定されていることを確認します。
	3. **サーバー名**と**データベース**を選択します。
	4. **ユーザー名**と**パスワード**を入力します。
	5. **[次へ]** をクリックします。
9. **[テーブル マッピング]** ページで、**[ターゲット]** フィールドのドロップダウン リストから **[emp]** を選択します。**下向き矢印**をクリックすると (省略可能)、スキーマを表示し、データをプレビューできます。

	![Copy Tool - Table mapping](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png)
10. **[Schema mapping (スキーマ マッピング)]** ページで、**[次へ]** をクリックします。
11. **[Performance settings (パフォーマンス設定)]** ページで、**[次へ]** をクリックします。
11. **[概要]** ページの内容を確認し、**[完了]** をクリックします。これにより、2 つのリンクされたサービス、2 つのデータセット (入力と出力)、1 つのパイプラインが (コピー ウィザードを起動した場所から) データ ファクトリに作成されます。
12. **[デプロイメントに成功しました]** ページで、**[Click here to monitor copy pipeline (コピー パイプラインを監視するには、ここをクリックしてください)]** というリンクをクリックします。

	![Copy Tool - Deployment succeeded](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)
13. [監視アプリを使用したパイプラインの監視と管理](data-factory-monitor-manage-app.md)に関するページの指示に従って、作成したパイプラインを監視する方法を確認します。**[ACTIVITY WINDOWS (アクティビティ ウィンドウ)]** 一覧の **[更新]** アイコンをクリックして、スライスを表示します。

	![Monitoring App](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png)
 

## 関連項目
| トピック | Description |
| :---- | :---- |
| [データ移動アクティビティ](data-factory-data-movement-activities.md) | この記事には、このチュートリアルで使用したコピー アクティビティの詳細な情報が記載されています。 |
| [スケジュールと実行](data-factory-scheduling-and-execution.md) | この記事では、Azure Data Factory アプリケーション モデルのスケジュール設定と実行の側面について説明します。 |
| [パイプライン](data-factory-create-pipelines.md) | この記事では、Azure Data Factory のパイプラインとアクティビティの概要、およびそれらを利用して実際のシナリオやビジネスのためにエンド ツー エンドのデータ主導ワークフローを作成する方法を説明します。 |
| [データセット](data-factory-create-datasets.md) | この記事では、Azure Data Factory のデータセットについて説明します。
| [監視アプリを使用したパイプラインの監視と管理に関する記事](data-factory-monitor-manage-app.md) | この記事では、監視と管理アプリを使用してパイプラインを監視、管理、デバッグする方法について説明します。 

<!---HONumber=AcomDC_0921_2016-->