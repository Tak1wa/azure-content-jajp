<properties
   pageTitle="Resource Manager テンプレートを使用した VNET ピアリングの作成 | Microsoft Azure"
   description="Resource Manager のテンプレートを使用して仮想ネットワーク ピアリングを作成する方法を説明します。"
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# Resource Manager テンプレートを使用した VNET ピアリングの作成

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Resource Manager テンプレートを使用して VNET ピアリングを作成するには、次の手順に従います。

1. Azure PowerShell を初めて使用する場合は、[Azure PowerShell のインストールおよび構成方法](../powershell-install-configure.md)を参照し、このページにある手順をすべて最後まで実行し、Azure にサインインしてサブスクリプションを選択します。

    > [AZURE.NOTE] VNet ピアリングを管理するための PowerShell コマンドレットは、[Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0) に付属しています。

2. 以下のテキストは、前述のシナリオに基づく VNet1 から VNet2 への VNET ピアリング リンクの定義です。以下の内容をコピーし、VNetPeeringVNet1.json というファイル名で保存してください。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. 以下に示したのは、前述のシナリオに基づく VNet2 から VNet1 への VNET ピアリング リンクの定義です。以下の内容をコピーし、VNetPeeringVNet2.json というファイル名で保存してください。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    前掲のテンプレートを見るとわかるように、VNET ピアリングには、構成可能なプロパティがいくつかあります。

    |オプション|Description|既定値|
    |:-----|:----------|:------|
    |AllowVirtualNetworkAccess|ピア VNET のアドレス空間を virtual\_network タグの一部として含めるかどうかを選択します。|はい|
    |AllowForwardedTraffic|ピアリングされた VNET 以外の送信元のトラフィックを許可するか破棄するかを選択します。|いいえ|
    |AllowGatewayTransit|VNET ゲートウェイの使用をピア VNET に許可するかどうかを選択します。|いいえ|
    |UseRemoteGateways|ピアの VNet ゲートウェイを使用します。ピア VNET でゲートウェイが構成され、かつ AllowGatewayTransit が選択されている必要があります。ゲートウェイをローカルで構成した場合、このオプションは使用できません。|いいえ|

    上記の一連のプロパティは、VNET ピアリングの各リンクに存在します。たとえば、AllowVirtualNetworkAccess は、VNet1 から VNet2 への VNET ピアリング リンクの場合は True に、逆方向の VNET ピアリング リンクの場合は False に設定します。


4. テンプレート ファイルをデプロイするには、New-AzureRmResourceGroupDeployment コマンドレットを実行してデプロイを作成または更新します。Resource Manager テンプレートの使用方法の詳細については、こちらの[記事](../resource-group-template-deploy.md)を参照してください。

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] リソース グループ名とテンプレート ファイルは適宜置き換えてください。

    前述のシナリオに基づく例を次に示します。

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    このコマンドの出力結果を次に示します。

        DeploymentName		: VNetPeeringVNet1
        ResourceGroupName	: VNet101
        ProvisioningState		: Succeeded
        Timestamp			: 7/26/2016 9:05:03 AM
        Mode			: Incremental
        TemplateLink		:
        Parameters			:
        Outputs			:
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    このコマンドの出力結果を次に示します。

        DeploymentName		: VNetPeeringVNet2
        ResourceGroupName	: VNet101
        ProvisioningState		: Succeeded
        Timestamp			: 7/26/2016 9:07:22 AM
        Mode			: Incremental
        TemplateLink		:
        Parameters			:
        Outputs			:
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. デプロイが完了したら、以下のコマンドレットを実行してピアリング状態を確認できます。

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    このコマンドの出力結果を次に示します。

        Name			: LinkToVNet2
        Id				: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag			: W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName	: VNet101
        VirtualNetworkName	: VNet1
        ProvisioningState		: Succeeded
        RemoteVirtualNetwork	: {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess	: True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

	このシナリオでピアリングが確立された後は、双方の VNET の任意の仮想マシンから任意の仮想マシンへの接続を開始することができます。既定では AllowVirtualNetworkAccess が True に設定されているため、VNET 間の通信を許可する適切な ACL が VNET ピアリングによってプロビジョニングされます。ただし、ネットワーク セキュリティ グループ (NSG) ルールを適用して接続をブロックすることはできます。特定のサブネット間や仮想マシン間の接続をブロックすることで、2 つの仮想ネットワーク間のアクセスを細かく制御することができます。NSG ルールの作成の詳細については、こちらの[記事](virtual-networks-create-nsg-arm-ps.md)を参照してください。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

サブスクリプション間の VNET ピアリングを作成するには、次の手順に従います。

1. サブスクリプション A の特権 User-A アカウントで Azure にサインインし、次のコマンドレットを実行します。

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

	これは必須ではありません。ユーザーが個々の VNET に対して個別にピアリング要求を行った場合でも、双方の要求が合致すればピアリングは確立されます。相手側 VNET の特権ユーザーをローカル VNET のユーザーとして追加すると、セットアップしやすくなります。

2. サブスクリプション B の特権 User-B アカウントで Azure にサインインし、次のコマンドレットを実行します。

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. 次に、User-A のログイン セッションで次のコマンドレットを実行します。

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    この JSON ファイルの定義は以下のとおりです。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. 次に、User-B のログイン セッションで次のコマンドレットを実行します。

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

	この JSON ファイルの定義は以下のとおりです。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

 	このシナリオでピアリングが確立された後は、双方の VNet の任意の仮想マシンから、サブスクリプションの境界を越えて任意の仮想マシンへの接続を開始することができます。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. このシナリオでは、以下のサンプル テンプレートをデプロイすることによって VNET ピアリングを確立できます。AllowForwardedTraffic プロパティは True に設定する必要があります。こうすることで、ピアリングされた VNET のネットワーク仮想アプライアンスがトラフィックを送受信することができます。

	以下に示したのは、HubVNet から VNet1 への VNET ピアリングを作成するためのテンプレートです。AllowForwardedTraffic が false に設定されていることに注目してください。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. 以下に示したのは、VNet1 から HubVnet への VNET ピアリングを作成するためのテンプレートです。AllowForwardedTraffic が true に設定されていることに注目してください。

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. ピアリングが確立されたら、こちらの[記事](virtual-network-create-udr-arm-ps.md)を参照してください。VNet1 トラフィックを仮想アプライアンス経由でリダイレクトするようにユーザー定義ルート (UDR) を設定することで、その機能を利用することができます。ルートの次ホップ アドレスを指定するときは、ピア VNET (HubVNet) に存在する仮想アプライアンスの IP アドレスを設定します。

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

異なるデプロイメント モデルの仮想ネットワーク間にピアリングを作成するには、次の手順に従います。
1. 以下のテキストは、このシナリオにおける VNET1 から VNET2 への VNet ピアリング リンクの定義です。クラシック仮想ネットワークから Azure Resource Manager 仮想ネットワークへのピアリングに必要なリンクは 1 つだけです。

    クラシック仮想ネットワーク (VNET2) が属しているサブスクリプションのサブスクリプション ID を入力し、MyResouceGroup を適切なリソース グループ名に変更してください。

    { "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parameters": { }, "variables": { }, "resources": [ { "apiVersion": "2016-06-01", "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "name": "VNET1/LinkToVNET2", "location": "[resourceGroup().location]", "properties": { "allowVirtualNetworkAccess": true, "allowForwardedTraffic": false, "allowGatewayTransit": false, "useRemoteGateways": false, "remoteVirtualNetwork": { "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]" } } } ] }

2. テンプレート ファイルをデプロイするには、次のコマンドレットを実行して、デプロイを作成または更新します。

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. デプロイが正常に完了したら、次のコマンドレットを実行してピアリング状態を確認できます。

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

クラシック VNet と Resource Manager VNet の間にピアリングが確立されたら、VNET1 の任意の仮想マシンから VNET2 の任意の仮想マシンへの接続とその逆方向の接続を開始できます。

<!---HONumber=AcomDC_0921_2016-->