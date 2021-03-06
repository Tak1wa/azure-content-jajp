<properties
	pageTitle="Azure 診断ログの概要 | Microsoft Azure"
	description="Azure 診断ログの概要と、診断ログを使用して Azure リソース内で発生するイベントを把握する方法について説明します。"
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="johnkem"/>

# Azure 診断ログの概要
**Azure 診断ログ**は、リソースによって出力されるログであり、そのリソースの操作に関する豊富なデータを提供します。これらのログの内容は、リソースの種類によって異なります (たとえば、Windows イベント システム ログは、VM の診断ログのカテゴリの 1 つであり、BLOB ログ、テーブル ログ、キュー ログは、ストレージ アカウントの診断ログのカテゴリです)。また、診断ログは、サブスクリプションのリソースに対して実行された操作に関する洞察が得られる[アクティビティ ログ (以前の監査ログまたは操作ログ)](monitoring-overview-activity-logs.md) とは異なります。ここで説明する新しい種類の診断ログは、すべてのリソースでサポートされているわけではありません。後述のサポートされているサービスの一覧に、新しい診断ログをサポートするリソースの種類が示されています。

![診断ログの論理的配置](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## 診断ログで実行できること
診断ログでは次のことを実行できます。

- 監査や手動での検査に使用するために診断ログを**ストレージ アカウント**に保存する。**診断設定**を使用して、リテンション期間 (日数) を指定できます。
- サード パーティのサービスや PowerBI などのカスタム分析ソリューションで取り込むために、[診断ログを **Event Hubs** にストリーミング](monitoring-stream-diagnostic-logs-to-event-hubs.md)する。
- 診断ログを [OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md) で分析する。

## 診断設定
非コンピューティング リソースの診断ログは、診断設定を使用して構成します。リソースの**診断設定**では、以下を制御します。

- 診断ログの送信先 (ストレージ アカウント、Event Hubs、OMS Log Analytics)。
- 送信するログ カテゴリ。
- 各ログ カテゴリをストレージ アカウントに保持する期間。リテンション期間 0 日の場合、ログは永続的に保持されます。それ以外の場合、この値は 1 ～ 2,147,483,647 の範囲の数にすることができます。保持ポリシーが設定されていても、ストレージ アカウントへのログの保存が無効になっている場合 (Event Hubs または OMS オプションだけが選択されている場合)、保持ポリシーは無効になります。

これらの設定は、Azure ポータルのリソースの [診断] ブレード、Azure PowerShell および CLI のコマンド、または [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx) を使用して簡単に構成できます。

> [AZURE.WARNING] コンピューティング リソース (VM や Service Fabric など) の診断ログとメトリックでは、[出力の構成と選択に別のメカニズム](../azure-diagnostics.md)を使用します。

## 診断ログの収集を有効にする方法
診断ログの収集は、リソースの作成中に有効にすることも、リソースの作成後にポータルのリソースのブレードで有効にすることもできます。また、診断ログは、Azure PowerShell または CLI のコマンドを使用するか、Insights REST API を使用していつでも有効にすることができます。

> [AZURE.TIP] これらの手順は、すべてのリソースに直接適用できるわけではありません。特定のリソースの種類に適用できる具体的な手順については、このページの最後にあるスキーマのリンクを参照してください。

[リソース テンプレートを使用して、リソースの作成時に診断設定を有効にする方法については、こちらの記事をご覧ください。](./monitoring-enable-diagnostic-logs-using-template.md)

### ポータルでの診断ログの有効化
リソースの作成時に Azure ポータルで診断ログを有効にするには、次の手順に従います。

1.	**[新規]** に移動し、興味のあるリソースを選択します。
2.	基本設定を構成し、サイズを選択したら、**[設定]** ブレードの **[監視]** で **[有効]** を選択し、診断ログの保存先となるストレージ アカウントを選択します。診断データをストレージ アカウントに送信する際には、ストレージとトランザクションの通常のデータ料金が発生します。

    ![リソースの作成時に診断ログを有効にする](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.	**[OK]** をクリックしてリソースを作成します。

リソースの作成後に Azure ポータルで診断ログを有効にするには、次の手順を実行します。

1.	リソースのブレードに移動し、**[診断]** ブレードを開きます。
2.	**[オン]** をクリックし、ストレージ アカウントおよび Event Hubs を選択します。

    ![リソースの作成後に診断ログを有効にする](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.	**[ログ]** で、収集またはストリーミングする**ログ カテゴリ**を選択します。
4.	[**Save**] をクリックします。

### プログラムによる診断ログの有効化
Azure PowerShell コマンドレットを使用して診断ログを有効にするには、次のコマンドを使用します。

ストレージ アカウントへの診断ログの保存を有効にするには、次のコマンドを使用します。

    Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -StorageAccountId [your storage account id] -Enabled $true

ストレージ アカウント ID は、ログの送信先となるストレージ アカウントのリソース ID です。

Event Hubs への診断ログのストリーミングを有効にするには、次のコマンドを使用します。

    Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Service Bus 規則 ID は、`{service bus resource ID}/authorizationrules/{key name}` の形式の文字列です。

Azure CLI を使用して診断ログを有効にするには、次のコマンドを使用します。

ストレージ アカウントへの診断ログの保存を有効にするには、次のコマンドを使用します。

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

ストレージ アカウント ID は、ログの送信先となるストレージ アカウントのリソース ID です。

Event Hubs への診断ログのストリーミングを有効にするには、次のコマンドを使用します。

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Service Bus 規則 ID は、`{service bus resource ID}/authorizationrules/{key name}` の形式の文字列です。

Insights REST API を使用して診断設定を変更する場合は、[こちらのドキュメント](https://msdn.microsoft.com/library/azure/dn931931.aspx)をご覧ください。

## 診断ログでサポートされているサービスとスキーマ
診断ログのスキーマは、リソースとログ カテゴリによって異なります。サポートされているサービスとそのスキーマを次に示します。

| サービス | スキーマとドキュメント |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
| ソフトウェア ロード バランサー | [Azure Load Balancer のログ分析 (プレビュー)](../load-balancer/load-balancer-monitor-log.md) |
| ネットワーク セキュリティ グループ | [ネットワーク セキュリティ グループ (NSG) のためのログ分析](../virtual-network/virtual-network-nsg-manage-log.md) |
| Application Gateway | [Application Gateway の診断ログ](../application-gateway/application-gateway-diagnostics.md) |
| Key Vault | [Azure Key Vault のログ記録](../key-vault/key-vault-logging.md) |
| Azure Search | [検索トラフィックの分析の有効化と使用](../search/search-traffic-analytics.md) |
| Data Lake Store | [Azure Data Lake Store の診断ログへのアクセス](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Data Lake Analytics | [Azure Data Lake Analytics の診断ログへのアクセス](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Logic Apps | 使用可能なスキーマはありません。 |
| Azure Batch | 使用可能なスキーマはありません。 |
| Azure Automation | 使用可能なスキーマはありません。 |

## リソースの種類ごとのサポートされているログ カテゴリ

|リソースの種類|カテゴリ|カテゴリの表示名|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|ジョブ ログ|
|Microsoft.Automation/automationAccounts|JobStreams|ジョブ ストリーム|
|Microsoft.Batch/batchAccounts|ServiceLog|サービス ログ|
|Microsoft.DataLakeAnalytics/accounts|Audit|Audit Logs|
|Microsoft.DataLakeAnalytics/accounts|要求数|要求ログ|
|Microsoft.DataLakeStore/accounts|Audit|Audit Logs|
|Microsoft.DataLakeStore/accounts|要求数|要求ログ|
|Microsoft.KeyVault/vaults|AuditEvent|Audit Logs|
|Microsoft.Logic/workflows|WorkflowRuntime|ワークフロー ランタイムの診断イベント|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|ネットワーク セキュリティ グループ イベント|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|ネットワーク セキュリティ グループの規則数|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|ネットワーク セキュリティ グループの規則フロー イベント|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|ロード バランサーのアラート イベント|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|ロード バランサーのプローブ正常性状態|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|アプリケーション ゲートウェイのアクセス ログ|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|アプリケーション ゲートウェイのパフォーマンス ログ|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|アプリケーション ゲートウェイのファイアウォール ログ|
|Microsoft.Search/searchServices|OperationLogs|操作ログ|

## 次のステップ
- [診断ログを **Event Hubs** にストリーミングする](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Insights REST API を使用して診断設定を変更する](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [ログを OMS Log Analytics で分析する](../log-analytics/log-analytics-azure-storage-json.md)

<!---HONumber=AcomDC_0907_2016-->