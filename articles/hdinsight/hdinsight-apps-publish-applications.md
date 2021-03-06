<properties
   	pageTitle="HDInsight アプリケーションの発行 | Microsoft Azure"
   	description="HDInsight アプリケーションを作成して発行する方法について説明します。"
   	services="hdinsight"
   	documentationCenter=""
   	authors="mumian"
   	manager="jhubbard"
   	editor="cgronlun"
	tags="azure-portal"/>

<tags
   	ms.service="hdinsight"
   	ms.devlang="na"
   	ms.topic="hero-article"
   	ms.tgt_pltfrm="na"
   	ms.workload="big-data"
   	ms.date="06/29/2016"
   	ms.author="jgao"/>

# Azure Marketplace への HDInsight アプリケーションの発行

HDInsight アプリケーションは、ユーザーが Linux ベースの HDInsight クラスターにインストールすることのできるアプリケーションです。マイクロソフトや独立系ソフトウェア ベンダー (ISV) によって作成されるほか、ユーザーが独自に作成することもできます。この記事では、HDInsight アプリケーションを Azure Marketplace に発行する方法について説明します。Azure Marketplace への発行全般に関する情報については、「[Azure Marketplace へのプランの発行方法](../marketplace-publishing/marketplace-publishing-getting-started.md)」を参照してください。

HDInsight アプリケーションには*ライセンス持ち込み (BYOL)* モデルが適用され、エンド ユーザーに対するアプリケーションの使用許諾は、そのアプリケーションの提供者が担います。また Azure からエンド ユーザーへの課金は、そのエンド ユーザーによって作成されたリソース (HDInsight クラスターとその VM/ノードなど) についてのみ行われます。このとき、アプリケーションそのものに対する課金は Azure を介さず行われます。

HDInsight アプリケーションに関連したその他の記事もご覧ください。

- [HDInsight アプリケーションをインストールする](hdinsight-apps-install-applications.md): HDInsight アプリケーションをクラスターにインストールする方法について説明します。
- [カスタム HDInsight アプリケーションをインストールする](hdinsight-apps-install-custom-applications.md): カスタム HDInsight アプリケーションをインストールしてテストする方法について説明します。

 
## 前提条件

カスタム アプリケーションを Azure Marketplace に送信するには、あらかじめカスタム アプリケーションの作成とテストを済ませておく必要があります。次の記事を参照してください。

- [カスタム HDInsight アプリケーションをインストールする](hdinsight-apps-install-custom-applications.md): カスタム HDInsight アプリケーションをインストールしてテストする方法について説明します。

さらに、開発者アカウントの登録が必要となります。[Azure Marketplace へのプランの発行](../marketplace-publishing/marketplace-publishing-getting-started.md)に関するページと「[Microsoft 開発者アカウントの作成](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md)」を参照してください。

## アプリケーションの定義

Azure Marketplace へのアプリケーションの発行は 2 つの段階を経て行います。まず、**createUiDef.json** ファイルを定義して、アプリケーションに適合するクラスターを指定し、次に Azure ポータルからテンプレートを発行します。次に示したのは、createUiDef.json ファイルの例です。

	{
		"handler": "Microsoft.HDInsight",
		"version": "0.0.1-preview",
		"clusterFilters": {
			"types": ["Hadoop", "HBase", "Storm", "Spark"],
			"tiers": ["Standard", "Premium"],
			"versions": ["3.4"]
		}
	}


|フィールド | 説明 | 指定できる値|
|-------|---------------|----------------|
|types |アプリケーションと適合するクラスターの種類。 |Hadoop、HBase、Storm、Spark のいずれか (またはその任意の組み合わせ)|
|tiers |アプリケーションと適合するクラスターのプラン。 |Standard または Premium (またはその両方)|
|versions|	アプリケーションと適合する HDInsight クラスターの種類。 |3\.4|

## アプリケーションのパッケージ化

HDInsight アプリケーションのインストールに必要なファイルをすべて含んだ zip ファイルを作成します。この zip ファイルは、「[アプリケーションの発行](#publish-application)」で使用します。

- [createUiDefinition.json](#define-application)。
- mainTemplate.json。「[カスタム HDInsight アプリケーションのインストール](hdinsight-apps-install-custom-applications.md)」でサンプルを参照してください。

	>[AZURE.IMPORTANT] アプリケーションのインストール スクリプトの名前は以下の形式とし、特定のクラスターにおいて一意であることが必要です。さらに、インストールとアンインストールのいずれのスクリプト アクションもべき等である必要があります。つまり、スクリプトは、繰り返し呼び出すことができ、同じ結果を生成します。
	
	>	name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
		
	>スクリプト名は、次の 3 つの部分で構成されていることに注意してください。
		
	>	1. スクリプト名のプレフィックス。アプリケーション名またはアプリケーションに関連する名前のどちらかが含まれている必要があります。
	>	2. "-" (読みやすくするため)。
	>	3. 一意の文字列の関数 (パラメーターとしてアプリケーション名を指定)。

	>	上記の例の場合、保存されたスクリプト アクションの一覧では hue-install-v0-4wkahss55hlas となります。JSON ペイロードのサンプルについては、[https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json) 参照してください。

- 必要なすべてのスクリプト。

> [AZURE.NOTE] アプリケーション ファイル (もしあれば Web アプリケーション ファイルも含む) は、パブリックにアクセスできる任意のエンドポイントに配置できます。

## アプリケーションの発行

次の手順に従って HDInsight アプリケーションを発行します。

1. [Azure 発行ポータル](https://publish.windowsazure.com/)にサインオンします。
2. **[ソリューション テンプレート]** をクリックして新しいソリューション テンプレートを作成します。
3. **[Create Dev Center account and join the Azure program (デベロッパー センター アカウントを作成して Azure プログラムに参加する)]** をクリックして勤務先の会社を登録します (未登録の場合)。「[Microsoft 開発者アカウントの作成](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md)」を参照してください。
4. **[Define some Topologies to get Started (いくつかのトポロジを定義して開始する)]** をクリックします。ソリューション テンプレートは、作成したすべてのトポロジの 「親」 となります。1 つのプランまたはソリューション テンプレートでは、複数のトポロジを定義できます。プランをステージングにプッシュすると、すべてのトポロジも一緒にプッシュされます。
5. 新しいバージョンを追加します。
6. 「[アプリケーションのパッケージ化](#package-application)」で作成した zip ファイルをアップロードします。
7. **[Request Certification (証明書の要求)]** をクリックします。Microsoft の認定チームがファイルを確認し、トポロジを認定します。

## 次のステップ

- [HDInsight アプリケーションをインストールする](hdinsight-apps-install-applications.md): HDInsight アプリケーションをクラスターにインストールする方法について確認します。
- [カスタム HDInsight アプリケーションをインストールする](hdinsight-apps-install-custom-applications.md): 未発行の HDInsight アプリケーションを HDInsight にデプロイする方法について確認します。
- [スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](hdinsight-hadoop-customize-cluster-linux.md): スクリプト アクションを使用してアプリケーションを追加インストールする方法を確認します。
- [Azure Resource Manager テンプレートを使用して HDInsight で Linux ベースの Hadoop クラスターを作成する](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Resource Manager テンプレートを呼び出して HDInsight クラスターを作成する方法を確認します。
- [HDInsight で空のエッジ ノードを使用する](hdinsight-apps-use-edge-node.md): HDInsight クラスター、テスト HDInsight アプリケーション、およびホスティング HDInsight アプリケーションにアクセスするために空のエッジ ノードを使用する方法を確認します。

<!---HONumber=AcomDC_0914_2016-->