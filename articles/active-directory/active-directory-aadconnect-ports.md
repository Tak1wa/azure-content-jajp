<properties
	pageTitle="Azure AD Connect: ポート | Microsoft Azure"
	description="このページは Azure AD Connect のために開く必要があるポートのテクニカル リファレンス ページです"
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/25/2016"
	ms.author="billmath"/>

# ハイブリッド ID で必要なポートとプロトコル
次のドキュメントは、ハイブリッド ID ソリューションを実装するために必要なポートとプロトコルに関する情報を提供するテクニカルリファレンスです。次の図を使用して、対応する表を参照してください。

![Azure AD Connect とは](./media/active-directory-aadconnect-ports/required1.png)

## 表 1 - Azure AD Connect とオンプレミスの AD
この表は、Azure AD Connect サーバーとオンプレミスの AD 間の通信に必要なポートとプロトコルについて説明しています。

プロトコル | ポート | Description
--------- | --------- |---------
DNS|53 (TCP/UDP)| 送信先フォレストでの DNS 参照です。
Kerberos|88 (TCP/UDP)| AD フォレストに対する Kerberos 認証です。
MS-RPC |135 (TCP/UDP)| Azure AD Connect ウィザードを AD フォレストにバインドするときの初期構成中に使用されます。
LDAP|389 (TCP/UDP)| AD からのデータのインポートに使用されます。データは Kerberos の署名およびシールによって暗号化されます。
LDAP/SSL|636 (TCP/UDP)| AD からのデータのインポートに使用されます。データ転送は署名されて暗号化されます。SSL を使用する場合にのみ使用されます。
RPC |49152- 65535 (ランダム ハイ RPC ポート)(TCP/UDP)| Azure AD Connect を AD フォレストにバインドするときの初期構成中に使用されます。詳細については、[KB929851](https://support.microsoft.com/kb/929851)、[KB832017](https://support.microsoft.com/kb/832017)、[KB224196](https://support.microsoft.com/kb/224196) を参照してください。

## 表 2 - Azure AD Connect と Azure AD
この表は、Azure AD Connect サーバーと Azure AD 間の通信に必要なポートとプロトコルについて説明しています。

プロトコル |ポート |Description
--------- | --------- |---------
HTTP|80 (TCP/UDP)| SSL 証明書を検証するための CRL (証明書失効リスト) をダウンロードするために使用されます。
HTTPS|443 (TCP/UDP)| Azure AD と同期するために使用されます。

ファイアウォールで開く必要のある URL と IP アドレスの一覧については、「[Office 365 URL および IP アドレス範囲](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)」を参照してください。

## 表 3 - Azure AD Connect とフェデレーション サーバー/WAP
この表は、Azure AD Connect サーバーとフェデレーション/WAP サーバー間の通信に必要なポートとプロトコルについて説明しています。

プロトコル |ポート |Description
--------- | --------- |---------
HTTP|80 (TCP/UDP)| SSL 証明書を検証するための CRL (証明書失効リスト) をダウンロードするために使用されます。
HTTPS|443 (TCP/UDP)| Azure AD と同期するために使用されます。
WinRM|5985| WinRM リスナー

## 表 4 - WAP とフェデレーション サーバー
この表は、フェデレーション サーバーと WAP サーバー間の通信に必要なポートとプロトコルについて説明しています。

プロトコル |ポート |Description
--------- | --------- |---------
HTTPS|443 (TCP/UDP)| 認証で使用されます。

## 表 5 - WAP とユーザー
この表は、ユーザーと WAP サーバー間の通信に必要なポートとプロトコルについて説明しています。

プロトコル |ポート |Description
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)| デバイスの認証で使用されます。
TCP|49443 (TCP)| 証明書の認証で使用されます。

## テーブル 6a および 6b - Azure AD Connect Health エージェント (AD FS/Sync) と Azure AD
次の表は、Azure AD Connect Health エージェントと Azure AD 間の通信に必要なエンドポイント、ポート、プロトコルについて説明しています。

### 表 6a - Azure AD Connect Health エージェント (AD FS/Sync) と Azure AD 用のポートとプロトコル
この表は、Azure AD Connect Health エージェントと Azure AD 間の通信に必要な以下の送信ポートとプロトコルについて説明しています。

プロトコル |ポート |Description
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)| 送信
Azure Service Bus|5671 (TCP/UDP)| 送信

### 表 6b - Azure AD Connect Health エージェント (AD FS/Sync) と Azure AD 用のエンドポイント
エンドポイントの一覧については、[Azure AD Connect Health エージェントの「必要条件」セクション](active-directory-aadconnect-health-agent-install.md#requirements)を参照してください。

<!---HONumber=AcomDC_0831_2016-->