<properties
	pageTitle="ロジック アプリに OneDrive コネクタを追加する | Microsoft Azure"
	description="OneDrive コネクタと REST API パラメーターの概要"
	services="logic-apps"    
	documentationCenter=""     
	authors="MandiOhlinger"    
	manager="erikre"    
	editor=""
	tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/26/2016"
   ms.author="mandia"/>

# OneDrive コネクタの使用

OneDrive に接続して、ファイルのアップロード、取得、削除など、ファイルを管理します。

OneDrive では、次の操作を実行できます。

- OneDrive にファイルを保存してワークフローを構築するか、OneDrive 内の既存ファイルを更新できます。
- OneDrive 内でファイルが作成または更新されたときに、トリガーを使用してワークフローを開始できます。
- ファイルの作成、ファイルの削除などのアクションを使用できます。たとえば、添付ファイル付きの新しい Office 365 電子メールを受信したときに (トリガー)、OneDrive に新しいファイルを作成します (アクション)。

このトピックでは、ロジック アプリ内で OneDrive コネクタを使用する方法を説明し、トリガーとアクションの一覧を示します。

>[AZURE.NOTE] 本記事は、一般公開された Logic Apps の一般公開 (GA) を対象としています。

Logic Apps の詳細については、「[Logic Apps とは](../app-service-logic/app-service-logic-what-are-logic-apps.md)」および[ロジック アプリの作成](../app-service-logic/app-service-logic-create-a-logic-app.md)に関するページをご覧ください。

## OneDrive に接続する

ロジック アプリから任意のサービスにアクセスできるようにするには、まず、そのサービスへの "*接続*" を作成します。接続により、ロジック アプリと別のサービスとの接続が実現します。たとえば、OneDrive に接続するには、最初に OneDrive "*接続*" が必要です。接続を作成するには、接続対象のサービスへのアクセスに通常使用する資格情報を入力します。そのため、OneDrive の場合は、OneDrive アカウントの資格情報を入力して接続を作成します。

### 接続の作成

>[AZURE.INCLUDE [OneDrive への接続を作成する手順](../../includes/connectors-create-api-onedrive.md)]

## トリガーを使用する

トリガーとは、ロジック アプリで定義されたワークフローの開始に使用できるイベントです。トリガーは、指定された間隔と頻度でサービスを "ポーリング" します。[トリガーの詳細についてはこちらをご覧ください](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)。

1. ロジック アプリで「onedrive」と入力して、トリガーの一覧を取得します。

	![](./media/connectors-create-api-onedrive/onedrive-1.png)

2. **[When a file is modified (ファイルの変更時)]** を選択します。接続が既に存在する場合は、[ピッカーの表示] ボタンを選択してフォルダーを選択します。

	![](./media/connectors-create-api-onedrive/sample-folder.png)

	サインインを求められたら、サインインの詳細を入力して接続を作成します。この手順については、このトピックの「[接続の作成](connectors-create-api-onedrive.md#create-the-connection)」をご覧ください。

	> [AZURE.NOTE] この例では、選択したフォルダー内のファイルが更新されたときに、ロジック アプリが実行されます。このトリガーの結果を確認するには、自身に電子メールを送信する別のアクションを追加してください。たとえば、Office 365 Outlook の "*電子メールを送信する*" アクションを追加します。これにより、ファイルが更新されると電子メールが送信されます。

3. **[編集]** を選択し、**[頻度]** と **[間隔]** の値を設定します。たとえば、トリガーを使用して 15 分ごとにポーリングを実行するには、**[頻度]** を **[分]** に設定し、**[間隔]** を **15** に設定します。

	![](./media/connectors-create-api-onedrive/trigger-properties.png)

4. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。ロジック アプリが保存され、場合によっては、自動的に有効になります。


## アクションを使用する

アクションとは、ロジック アプリで定義されたワークフローによって実行される操作です。[アクションの詳細についてはこちらをご覧ください](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts)。

1. プラス記号を選択します。**[アクションの追加]**、**[条件の追加]**、**[More (その他)]** のいずれかのオプションという複数の選択肢があります。

	![](./media/connectors-create-api-onedrive/add-action.png)

2. **[アクションの追加]** を選択します。

3. テキスト ボックスに「onedrive」と入力して、使用可能なすべてのアクションの一覧を取得します。

	![](./media/connectors-create-api-onedrive/onedrive-actions.png)

4. この例では、**[OneDrive - Create file (OneDrive - ファイルを作成する)]** を選択します。接続が既に存在する場合は、**[フォルダー パス]** で、ファイルを配置する場所を選択し、**[ファイル名]** に入力して、目的の**ファイルのコンテンツ**を選択します。

	![](./media/connectors-create-api-onedrive/sample-action.png)

	接続情報の入力を求められたら、詳細を入力して接続を作成します。これらのプロパティについては、このトピックの「[接続の作成](connectors-create-api-onedrive.md#create-the-connection)」をご覧ください。

	> [AZURE.NOTE] この例では、OneDrive フォルダーに新しいファイルを作成します。別のトリガーからの出力を使用して、OneDrive ファイルを作成できます。たとえば、Office 365 Outlook の "*When a new email arrives (新しい電子メールが届いたとき)*" トリガーを追加します。次に、OneDrive の "*Create file (ファイルを作成する)*" アクションを追加します。このアクションは、ForEach 内の Attachments フィールドと Content-Type フィールドを使用して OneDrive に新しいファイルを作成します。
	> 
	> ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。ロジック アプリが保存され、場合によっては、自動的に有効になります。


## 技術的な詳細

## トリガー

|トリガー | 説明|
|--- | ---|
|[ファイルの作成時](connectors-create-api-onedrive.md#when-a-file-is-created)|この操作では、フォルダーに新しいファイルが作成されたときにフローをトリガーします。|
|[ファイルの変更時](connectors-create-api-onedrive.md#when-a-file-is-modified)|この操作では、フォルダー内のファイルが変更されたときにフローをトリガーします。|


## アクション

|アクション|説明|
|--- | ---|
|[ファイルのメタデータを取得する](connectors-create-api-onedrive.md#get-file-metadata)|この操作では、ファイルのメタデータを取得します。|
|[ファイルを更新する](connectors-create-api-onedrive.md#update-file)|この操作では、ファイルを更新します。|
|[ファイルを削除する](connectors-create-api-onedrive.md#delete-file)|この操作では、ファイルを削除します。|
|[パスを使用してファイルのメタデータを取得する](connectors-create-api-onedrive.md#get-file-metadata-using-path)|この操作では、パスを使用してファイルのメタデータを取得します。|
|[パスを使用してファイルの内容を取得する](connectors-create-api-onedrive.md#get-file-content-using-path)|この操作では、パスを使用してファイルの内容を取得します。|
|[ファイルの内容を取得する](connectors-create-api-onedrive.md#get-file-content)|この操作では、ファイルの内容を取得します。|
|[ファイルを作成する](connectors-create-api-onedrive.md#create-file)|この操作では、ファイルを作成します。|
|[ファイルをコピーする](connectors-create-api-onedrive.md#copy-file)|この操作では、ファイルを OneDrive にコピーします。|
|[フォルダー内のファイルを一覧表示する](connectors-create-api-onedrive.md#list-files-in-folder)|この操作では、フォルダー内のファイルとサブフォルダーの一覧を取得します。|
|[ルート フォルダー内のファイルを一覧表示する](connectors-create-api-onedrive.md#list-files-in-root-folder)|この操作では、ルート フォルダー内のファイルとサブフォルダーの一覧を取得します。|
|[アーカイブをフォルダーに抽出する](connectors-create-api-onedrive.md#extract-archive-to-folder)|この操作では、フォルダーにアーカイブ ファイル (例: .zip) を抽出します。|

### アクションの詳細

このセクションでは、必須または任意の入力プロパティ、コネクタに関連付けられた対応する出力など、各アクションに関する具体的な詳細について説明します。


#### ファイルのメタデータを取得する
この操作では、ファイルのメタデータを取得します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|id*|ファイル|ファイルを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|string|


#### ファイルを更新する
この操作では、ファイルを更新します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|id*|ファイル|ファイルを選択する|
|body*|ファイルのコンテンツ|ファイルの内容|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|string|


#### ファイルを削除する
この操作では、ファイルを削除します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|id*|ファイル|ファイルを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
ありません。

#### パスを使用してファイルのメタデータを取得する
この操作では、パスを使用してファイルのメタデータを取得します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|path*|ファイル パス|ファイルを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|文字列|


#### パスを使用してファイルの内容を取得する
この操作では、パスを使用してファイルの内容を取得します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|path*|ファイル パス|ファイルを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
ありません。


#### ファイルの内容を取得する
この操作では、ファイルの内容を取得します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|id*|ファイル|ファイルを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
ありません。

#### ファイルを作成する
この操作では、ファイルを作成します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|folderPath*|フォルダー パス|フォルダーを選択する|
|name*|ファイル名|ファイルの名前|
|body*|ファイルのコンテンツ|ファイルの内容|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|string|


#### ファイルをコピーする
この操作では、ファイルを OneDrive にコピーします。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|source*|Source url (コピー元 URL)|ソース ファイルの URL|
|destination*|Destination file path (コピー先ファイル パス)|対象ファイル名を含む、コピー先ファイル パス|
|overwrite|Overwrite? (上書きを許可)|’true’ に設定すると、宛先ファイルが上書きされます|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|string|


#### ファイルの作成時
この操作では、フォルダーに新しいファイルが作成されたときにフローをトリガーします。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|folderId*|フォルダー|フォルダーを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
ありません。

#### ファイルの変更時
この操作では、フォルダー内のファイルが変更されたときにフローをトリガーします。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|folderId*|フォルダー|フォルダーを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
ありません。

#### フォルダー内のファイルを一覧表示する
この操作では、フォルダー内のファイルとサブフォルダーの一覧を取得します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|id*|フォルダー|フォルダーを選択する|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string||
|IsFolder|ブール値|
|ETag|string|
|FileLocator|文字列|


#### ルート フォルダー内のファイルを一覧表示する
この操作では、ルート フォルダー内のファイルとサブフォルダーの一覧を取得します。

この呼び出しには、パラメーターはありません。


##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|string|

#### アーカイブをフォルダーに抽出する
この操作では、フォルダーにアーカイブ ファイル (例: .zip) を抽出します。

|プロパティ名| Displayname Settings|説明|
| ---|---|---|
|source*|Source archive file path (ソース アーカイブ ファイルのパス)|アーカイブ ファイルのパス|
|destination*|Destination folder path (抽出先フォルダー パス)|アーカイブの内容を抽出するパス|
|overwrite|Overwrite? (上書きを許可)|’true’ に設定すると、宛先ファイルが上書きされます|

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### 出力の詳細
BlobMetadata

| プロパティ名 | データ型 |
|---|---|
|ID|string|
|名前|string|
|DisplayName|string|
|パス|string|
|LastModified|string|
|サイズ|integer|
|MediaType|string|
|IsFolder|ブール値|
|ETag|string|
|FileLocator|文字列|


## HTTP 応答

次の表に、アクションやトリガーへの応答とその応答の説明を示します。

|名前|説明|
|---|---|
|200|OK|
|202|承認済み|
|400|正しくない要求|
|401|権限がありません|
|403|許可されていません|
|404|見つかりません|
|500|内部サーバー エラー。不明なエラーが発生しました|
|default|操作に失敗しました。|


## 次のステップ

[ロジック アプリを作成します](../app-service-logic/app-service-logic-create-a-logic-app.md)。[API の一覧](apis-list.md)で、Logic Apps で使用できる他のコネクタを確認してください。

<!---HONumber=AcomDC_0803_2016-->