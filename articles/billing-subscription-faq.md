<properties
	pageTitle="Azure の課金とサブスクリプションに関する FAQ | Microsoft Azure"
	description="Azure の課金とサブスクリプションに関する一般的な質問に回答しています。"
	services=""
	documentationCenter=""
	authors="genlin"
	manager="msmbaldwin"
	editor=""
	tags="billing"/>

<tags
	ms.service="billing"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/07/2016"
	ms.author="genli"/>

#  Azure の課金とサブスクリプションに関する FAQ

この記事では、Azure の課金とサブスクリプションについて特によく寄せられる質問に回答しています。

> [AZURE.NOTE] この記事についてさらにヘルプが必要な場合は、問題を迅速に解決できるよう[サポートにお問い合わせ](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)ください。

## アカウント管理

- [新しい Azure ポータルではどのようにして管理者アカウントを管理するのですか。](#how-do-i-manage-administrator-accounts-in-the-new-azure-portal)
- [通知は、マイ アカウントに関連付けられたアカウント所有者のメール アドレス以外のメール アドレスに送信できますか。](#can-notifications-be-sent-to-a-different-email-address-other-than-the-account-owner-email-address-associated-with-my-account)
- [サブスクリプションの編集や詳細の追加ができないのはなぜですか。](#why-cant-i-edit-or-add-details-to-my-subscription)
- [Azure ポータルにログインできません](#i-am-unable-to-log-in-to-the-azure-portal)

## 課金

- [請求書のコピーを取得するにはどうすればよいですか。](#how-do-i-get-a-copy-of-my-invoice)
- [前回の請求期間の請求書が表示されません。なぜですか?](#i-do-not-see-an-invoice-for-the-last-billing-period-why)
- [支払い方法を教えてください。](#how-do-i-make-a-payment)

## 無料試用版

- [Azure の試用は無料ですか。課金される可能性はありませんか。](#can-i-try-azure-for-free-without-any-risk-of-being-charged)
- [無料試用版サブスクリプションとは何ですか。また、有効期間はどのくらいですか。](#what-is-a-free-trial-subscription-and-how-long-does-it-last)
- [無料試用版で利用できるリソースを教えてください。](#what-resources-are-available-whja-JPing-a-free-trial)
- [無料試用版の使用制限を削除するとどうなりますか。](#what-happens-if-i-remove-the-spending-limit-on-my-free-trial)
- [無料試用版サブスクリプションでクォータの引き上げは可能ですか。](#can-i-get-a-quota-increase-on-a-free-trial-subscription)
- [無料試用版についての詳しい情報はどこで確認できますか。](#where-can-i-get-more-details-on-free-trial)

## Azure の購入方法

- [Microsoft Azure が販売されている国と地域はどこですか。Azure の購入に使用できる通貨はどれですか。](#in-which-countries-and-regions-is-microsoft-azure-commercially-available-and-what-currencies-can-be-used-to-purchase-azure)
- [禁輸措置を受けている国に対する Azure ベースのサービスの再販は制限を受けますか。](#do-we-restrict-resale-of-azure-based-service-into-countries-under-embargo)
- [Azure と SQL Database は Microsoft Services Provider License Agreement で利用できますか。](#are-azure-and-sql-database-available-through-microsoft-services-provider-license-agreement)
- [Azure 以外で購入した既存の Windows ライセンスは使用できますか。](#can-i-use-my-existing-windows-licenses-that-i-bought-outside-of-azure)
- [Azure サービスはだれが購入できますか。](#who-can-purchase-azure-services)

## 支払いについて

- [Azure の購入には、どのような支払い方法がありますか。](#what-payment-options-do-i-have-in-purchasing-azure)
- [請求書による支払いをリクエストするにはどうすればよいですか。](#how-can-i-request-the-invoice-method-of-payment)
- [クレジット カードによる支払い状況を確認するにはどうすればよいですか。](#how-do-i-check-the-status-of-a-payment-made-by-credit-card)
- [Azure の支払い方法として使用しなくなったクレジット カードを削除するにはどうすればよいですか。](#how-do-i-remove-a-credit-card-that-i-no-longer-use-as-an-azure-payment-method)
- [クレジット カードの情報を更新または変更するにはどうすればよいですか。](#how-can-i-update-or-change-my-credit-card-information)
- [Azure サブスクリプションの支払い情報を編集するにはどうすればよいですか。](#how-can-i-edit-my-payment-information-for-my-azure-subscriptions)
- [国際クレジット カードを使用する必要があるというメッセージが表示されるのはなぜですか。](#why-am-i-seeing-a-message-that-i-need-to-use-an-international-credit-card)

## Azure の価格

- [Azure サービスの利点と価格に関する情報はどこで確認できますか。](#where-can-i-find-the-benefits-and-pricing-information-for-azure-services)
- [価格プランを変更するにはどうすればよいですか。](#how-do-i-change-my-pricing-plan)

## サービス レベル アグリーメント (SLA)

- [予定されている保守作業のためのサービス ダウンタイムについて事前に知るにはどうすればよいですか。](#how-do-we-know-in-advance-about-service-downtime-for-planned-maintenance)
- [稼働時間と接続に関する Azure SLA 契約はどのような内容ですか。](#what-is-the-azure-sla-agreement-for-uptime-and-connectivity)
- [Azure SLA クレジットとは何ですか。](#what-are-the-azure-sla-credits)
- [Azure サービス レベル アグリーメントは、現在のオンプレミスのマイクロソフト ライセンス契約にどのように関係しますか。](#how-will-azure-service-level-agreements-work-with-current-on-premises-microsoft-licensing-agreements)

## 使用制限

- [使用制限を無効にした場合、再度有効にすることはできますか。](#can-i-turn-the-spending-limit-back-on-if-i-turn-off-it)
- [使用制限の量は調整できますか。](#can-i-adjust-the-amount-of-the-spending-limit)

## サポート プラン

- [Azure サポートの購入方法は?](#how-do-i-purchase-azure-support)
- [月額料金は 1 つの Azure プランが対象ですか? それともアカウント全体が対象になりますか?](#does-the-monthly-rate-cover-a-single-azure-plan-or-the-entire-account)
- [Azure サポート プランを上位プランにアップグレードするには?](#how-do-i-upgrade-to-a-higher-tier-azure-support-plan)
- [Azure サポートはいつ課金されますか?](#when-will-i-be-billed-for-azure-support)
- [期間が終わるとどうなりますか?](#what-happens-at-the-end-of-the-term)
- [Azure サポート プランを解約するにはどうすればよいですか?](#how-do-i-cancel-my-azure-support-plan)

## サブスクリプション、サービス、データの移行

- [Azure サブスクリプションのデータとサービスを新しいサブスクリプションに移行するにはどうすればよいですか。](#how-do-i-migrate-data-and-services-for-my-azure-subscription-to-a-new-subscription)
- [自分のサブスクリプションの所有権を譲渡するにはどうすればよいですか。](#how-do-i-transfer-ownership-of-my-subscriptions)

## サブスクリプション サービスの制限 (クォータ)

- [無料試用版サブスクリプションでクォータの引き上げは可能ですか。](#can-i-get-a-quota-increase-on-a-free-trial-subscription)

### Azure の購入には、どのような支払い方法がありますか。

Azure の購入には、クレジット カードかデビット カード、または請求書払いをご利用いただけます。

> [AZURE.NOTE]

> - いったん請求書払いのオプションを選択すると、クレジット カード払いには変更できなくなります。請求書払いを申し込む方法については、「[Azure の請求処理](https://azure.microsoft.com/pricing/invoicing/)」を参照してください。
> - プリペイド カードおよびバーチャル クレジット カードでのお支払いは受け付けておりません。
> - お客様は、クレジット カードの利息および発生する可能性のある他のクレジット カードの請求について全責任を負います。

### 請求書による支払いをリクエストするにはどうすればよいですか。

「[Azure の請求処理](https://azure.microsoft.com//pricing/invoicing/)」に記載されている手順に従って、請求書による支払いのリクエストを送信してください。リクエストが承認されると、請求書による支払いで、サブスクリプションを設定するための手続きの方法が通知されます。

### クレジット カードによる支払い状況を確認するにはどうすればよいですか。

サポート チケットを送信してサポートを要求する必要があります。クレジット カードによる支払い状況を確認するためのサポート チケットを作成する方法については、「[Azure の請求とサブスクリプションの問題についてサポート チケットを作成する方法](billing-how-to-create-billing-support-ticket.md)」を参照してください。

### 請求書のコピーを取得するにはどうすればよいですか。

Azure アカウント管理者の場合、Azure アカウント センターで現在の料金を確認し、過去 6 か月間の料金の明細書をダウンロードすることができます。詳細については、「[Azure の請求書と毎日の使用状況データをダウンロードする方法](billing-download-azure-invoice-daily-usage-date.md)」を参照してください。

### Azure の支払い方法として使用しなくなったクレジット カードを削除するにはどうすればよいですか。

サポート チケットを送信してサポートを要求する必要があります。クレジット カードを削除するためのサポート チケットを作成する方法については、「[Azure の請求とサブスクリプションの問題についてサポート チケットを作成する方法](billing-how-to-create-billing-support-ticket.md)」を参照してください。

### クレジット カードの情報を更新または変更するにはどうすればよいですか。

クレジット カード番号が変更されない更新の場合は、有効期限など、既存のクレジット カードの詳細情報を更新するだけです。カード番号が変更された場合、または別のカードを使用する場合は、新しいカードを追加する必要があります。クレジット カードを更新または追加する手順については、「[Azure サブスクリプションの支払いに使用するクレジット カードを変更する方法](billing-how-to-change-credit-card.md)」を参照してください。

### 予定されている保守作業のためのサービス ダウンタイムについて事前に知るにはどうすればよいですか。

可用性に関するマイクロソフトのサービス レベル アグリーメントは、2 つ以上のロール インスタンスを使用しているアプリケーションに適用されます。単一のインスタンスに対する SLA はありません。詳細については、お客様の [SLA のページ](https://azure.microsoft.com/support/legal/sla/)を参照してください。

#### 計画的なメンテナンス (クラシック ポータル)

単一インスタンス構成とマルチインスタンス構成のどちらの場合でも、Azure では計画的なメンテナンスが近づいていることを注意喚起するために電子メールを事前に送信します (単一インスタンスについては 1 週間前、マルチインスタンスについては 48 時間前に)。この電子メールは、サブスクリプションに設定されているアカウント管理者、サービス管理者、共同管理者の電子メール アカウントに送信されます。

>[AZURE.NOTE] Virtual Machines 固有の詳細については、[Azure 仮想マシンの計画的なメンテナンス](virtual-machines/virtual-machines-linux-planned-maintenance.md)に関する記事を参照してください。

#### 計画外のメンテナンス

Azure プラットフォームにおいて予定外のサービス中断が発生した場合は、[サービス ダッシュボード](http://status.azure.com/)の状態が更新され、[Azure クラシック ポータル](https://manage.windowsazure.com/)にお客様への通知が届きます。

### 稼働時間と接続に関する Azure SLA 契約はどのような内容ですか。

プレビュー版ではなく一般提供されているすべての Azure サービスに対して、個別に SLA が用意されています。サービス レベル アグリーメント (SLA) では、稼働時間と接続に関するマイクロソフトの確約内容について説明しています。最新の詳細情報については、お客様の [SLA のページ](https://azure.microsoft.com/support/legal/sla/)を参照してください。

### Azure SLA クレジットとは何ですか。

Azure SLA クレジットは、サブスクリプションごとに、SLA が履行されなかった請求月の該当サービスの請求に対する比率として計算されます。サービス クレジットは、翌月の請求に適用されます。一般に、最初のしきい値 (サービスによって 99.95% または 99.9%) を下回ると 10% のクレジット、次のしきい値 (99%) を下回ると 25% のクレジットが提供されます。詳細については、お客様の [SLA のページ](https://azure.microsoft.com/support/legal/sla/)を参照してください。

### Azure サービス レベル アグリーメントは、現在のオンプレミスのマイクロソフト ライセンス契約にどのように関係しますか。

Azure サービス レベル アグリーメントは、オンプレミスのマイクロソフト ライセンス契約から独立したものです。Azure の SLA では、クラウドでご利用いただくサービスの月間アップタイム保証と、保証を履行できなかった場合に請求に対して適用される SLA クレジットを提供しています。

### 前回の請求期間の請求書が表示されません。なぜですか?

これにはいくつかの理由が考えられます。

- Azure にサブスクライブした日から 30 日以上経過していない。

- サブスクリプションの種類が無料試用版であるか、メンバー プラン (MSDN/Open/BizSpark/DreamSpark) のいずれかである (クレジット カードまたはクレジット カードに関連付けられた他の支払い方法が使用されていない)。

- まだ請求書の生成日になっていない。

### Azure ポータルにログインできません

「[Azure サブスクリプションにログインして管理することができない](billing-cannot-login-subscription.md)」を参照してください。

>[AZURE.NOTE] 適切な管理者としてログインしていることを確認してください。アカウント管理者は[アカウント センター](https://account.windowsazure.com/)のみにアクセスできるのに対し、サービス管理者 (SA) と共同管理者 (CA) がアクセスできるのは [Azure ポータル](https://portal.azure.com/)または [Azure クラシック ポータル](https://manage.windowsazure.com/)のみです。

### Azure サポートの購入方法は?

Azure サポート プランは、オンラインで、またはエンタープライズ契約を介してご購入いただけます。オンラインでは、[Azure サポート プラン](https://azure.microsoft.com/support/plans/)のページから Developer、Standard、Professional Direct の各サポート プランを購入できます。サポート プランを購入するには、Microsoft Azure アカウントの所有者である必要があります。

エンタープライズ契約 (EA) を通じて Microsoft Azure を購入した場合は、ラージ アカウント リセラー (LAR) に連絡すれば、エンタープライズ契約に Standard サポート プランまたは Professional Direct サポート プランを追加できます。Developer サポートは、エンタープライズ契約を通じてはご利用いただけません。

### 月額料金は 1 つの Azure プランが対象ですか? それともアカウント全体が対象になりますか?

Azure サポートの月額固定料金は、アカウントに含まれるサブスクリプション数やユーザー数に関係なく、1 つのアカウントが対象になります。

1 つのアカウントの下にあるすべてのサブスクリプションには、同一のサポート プランが適用されます。サポート プランの付いたアカウントの下にあるいずれかのサブスクリプションに対して管理者または所有者アクセス権限を持つすべてのユーザーも、自分がアクセス権限を持つサブスクリプションを所有する特定のアカウントに対するサポートを受ける資格があります。

Microsoft Azure のアカウントには、[Azure アカウント センター](https://account.windowsazure.com/Home/Index)からアクセスできます。アカウントに含まれており、サポートの対象になるサブスクリプションの一覧を、このポータルから確認できます。

### Azure サポート プランを上位プランにアップグレードするには?

上位プランにアップグレードするには、[サポート要求を送信し](billing-how-to-create-billing-support-ticket.md)、**[サブスクリプション管理]** を選択してください。元の Azure サポート プランに対する残りの料金は請求されず、初期コミットメントで定められた期間の残り月数には、上位プランの料金が課金されます。

### Azure サポートはいつ課金されますか?

Azure サポート プランをオンラインで購入すると、直ちに、最初の月に対する料金が課金されます。その後、各請求サイクルの最初の日に、月額が課金されます。エンタープライズ契約 (EA) でのご購入の場合は、契約で規定された請求サイクルが適用されます。

### 期間が終わるとどうなりますか?

6 か月の期間が終了すると、プランは自動的に更新され、次の 6 か月間について同じ Azure サポート プランが同じ支払い方法を使用して開始されます。アカウントの所有者は、[Azure アカウント センター](https://account.windowsazure.com/Home/Index)から、自動更新機能をオフにすることを選択できます。

自動更新をご希望でない場合は、[Azure アカウント センター](https://account.windowsazure.com/Home/Index)にログインし、対象のサポート プランを選択してから、[更新の詳細の変更] をクリックして自動更新の既定のオプションをオフにします。

### Azure サポート プランを解約するにはどうすればよいですか?

アカウントの所有者は、[Azure アカウント センター](https://account.windowsazure.com/Home/Index)にログインし、**サブスクリプション**を選択することにより、Azure サポート プランを解約できます。サブスクリプションを所有している場合、サブスクリプションの一覧が表示されます。解約を希望するサブスクリプションを選択します。画面の右側にサブスクリプションの詳細がオプションの一覧と一緒に表示されます。**[キャンセル]** を選択して次に進みます。サブスクリプションを所有していない場合は、"使用できるサブスクリプションがありません" というメッセージが表示されます。

>[AZURE.NOTE] Azure サポート プランの期間は 6 か月単位であり、Microsoft では、その期間全体について継続的な料金のコミットメントを必須としております。期間の満了前に解約した場合でも、残りの期間についてのコミットメントが免除されることはなく、既にお支払いいただいた料金の払い戻しもありません。詳細については、「[Azure サブスクリプションをキャンセルする方法](billing-how-to-cancel-azure-subscription.md)」を参照してください。

### Azure サブスクリプションのデータとサービスを新しいサブスクリプションに移行するにはどうすればよいですか。

[Azure サポート](https://azure.microsoft.com/support/options/)に詳細をお問い合わせください。Azure サブスクリプションのデータとサービスを新しいサブスクリプションに移行するためのサポート チケットを作成する方法については、「[Azure の請求とサブスクリプションの問題についてサポート チケットを作成する方法](billing-how-to-create-billing-support-ticket.md)」を参照してください。

### 新しい Azure ポータルではどのようにして管理者アカウントを管理するのですか。

Azure のロールベースのアクセス制御 (RBAC) では、Azure のアクセス権の詳細な管理を実現します。RBAC を使用して、開発チーム内で職務を分離し、職務に必要なアクセス権のみをユーザーに付与します。詳細については、「[Azure のロールベースのアクセス制御](.\\active-directory\\Azure Role-Based Access Control.md)」を参照してください。

### 自分のサブスクリプションの所有権を譲渡するにはどうすればよいですか。

従量課金、MSDN、Action Pack、または BizSpark のサブスクリプションについては、Microsoft Azure アカウント センターで今すぐ簡単に実行できます。サブスクリプションを別のユーザーに譲渡する機能が追加されました。つまり、所有する任意の従量課金、MSDN、Action Pack、または BizSpark のサブスクリプションでアカウント管理者を変更できるようになりました。ただし、譲渡先のユーザー アカウントが同じ国内にある必要があります。また、Marketplace での購入物に関連付けられているサブスクリプションを譲渡することはできません。詳細については、[Azure サブスクリプションを譲渡する方法](billing-subscription-transfer.md)に関する記事を参照してください。

### Azure 以外で購入した既存の Windows ライセンスは使用できますか。

はい。ライセンス モビリティを利用することで可能です。SA をお持ちの場合は、Virtual Machines をサポートしているすべてのサーバー製品 (Windows Server 以外) に "ライセンスを持ち込む" ことができます。詳細については、「[Azure でのソフトウェア アシュアランスによるライセンス モビリティ](https://azure.microsoft.com/pricing/license-mobility/)」を参照してください。

### Azure サービスの利点と価格に関する情報はどこで確認できますか。

Azure サービスの利点については、「[Microsoft Azure プランの詳細](https://azure.microsoft.com/support/legal/offer-details/)」を参照してください。料金情報については、「[Azure の価格](https://azure.microsoft.com/pricing/)」ページを参照してください。

### 価格プランを変更するにはどうすればよいですか。

従量課金制サブスクリプションを [12 か月の前払いプラン](https://azure.microsoft.com/offers/ms-azr-0026p/)に切り替えることができます。このプランでは、12 か月間の Azure サービスについて前払いをご利用いただくと、Azure サービスに 5% の割引が適用されます。プランを切り替えるには、[Azure アカウント センター](https://account.windowsazure.com/Subscriptions)で対象のサブスクリプションにアクセスし、[プランの切り替え] をクリックします。現時点では、12 か月の前払いプランが、アカウント センターで切り替えることができる唯一のプランです。別のプランに切り替える場合は、[Azure サポート](https://azure.microsoft.com/support/options/)までお問い合わせください。

### 通知は、マイ アカウントに関連付けられたアカウント所有者のメール アドレス以外のメール アドレスに送信できますか。

はい。別のメール アドレスで通知を受け取る場合は、次の手順に従って指定してください。

1.	Azure アカウント ポータルで [[プロファイル] タブ](https://account.windowsazure.com/Profile)に移動します。
2.	**[詳細の編集]** をクリックして、通知を受け取るメール アドレスを更新します。

### Azure サブスクリプションの支払い情報を編集するにはどうすればよいですか。

Azure アカウント情報を表示して編集するには、Azure アカウント センターにアカウント管理者としてサインインする必要があります。Azure サブスクリプションの支払い方法を管理する手順は次のとおりです。**

1.	[Azure アカウント センター](https://account.windowsazure.com/Subscriptions)にアクセスします。
2.	[サブスクリプション] ページで、支払い方法を更新するサブスクリプションをクリックします。
3.	**サブスクリプション**の概要ページで、[支払い方法の変更] をクリックします。**[支払い方法の変更]** ツールが別のウィンドウに表示されます。

	**注:** Microsoft Azure クラシック ポータルからアカウント センターにアクセスすることもできます。その場合は、自分のアカウント名をクリックし、[明細の表示] をクリックします。
4.	**[支払方法の選択]** ページで、ドロップダウン リストをクリックし、更新する支払い方法を選択して、**[編集]** をクリックします。
5.	[詳細] ページで、正しいクレジット カードの種類と番号が選択されていることを確認します。
6.	修正が必要な場合はカードの詳細に変更を加え、**[次へ]** をクリックします。

詳細については、「[Azure サブスクリプションの支払いに使用するクレジット カードを変更する方法](billing-how-to-change-credit-card.md)」を参照してください。

>[AZURE.NOTE] 現時点では、[支払い方法の変更] ツールでは支払い方法として設定されている既存のクレジット カードを削除できません。クレジット カードの削除方法の詳細については、「[Azure の支払い方法として使用しなくなったクレジット カードを削除するにはどうすればよいですか。](#how-do-i-remove-a-credit-card-that-i-no-longer-use-as-an-azure-payment-method)」を参照してください。

### 国際クレジット カードを使用する必要があるというメッセージが表示されるのはなぜですか。

Azure サービスの支払いを現地/優先通貨で行っているが、現地以外の Microsoft エンティティを通じてサービスを購入したことで、クレジット カードが地元以外の銀行で処理される場合があります。この場合、地元の銀行によって発行されたカードは承認されず、これによって Azure サービスに影響が出る可能性があります。ご使用のクレジット カードが国際取引に対応しているか不明な場合は、ご利用の銀行またはカード発行会社にお問い合わせください。[サポートされる国および通貨](billing-countries-and-currencies.md)に関するページを参照してください。

### サブスクリプションの編集や詳細の追加ができないのはなぜですか。

Azure アカウント情報を表示して編集するには、Azure アカウント センターにアカウント管理者としてサインインする必要があります。アカウント管理者としてサインインしたのにサブスクリプションを編集できない場合は、サポート チケットを送信してサポートを要求してください。サポート チケットを作成する方法については、「[Azure の請求とサブスクリプションの問題についてサポート チケットを作成する方法](billing-how-to-create-billing-support-ticket.md)」を参照してください。

### Azure サービスはだれが購入できますか。

Azure は、クラウド規模のアプリケーションとサービスを構築する事業者による使用を想定していますが、Azure サービスは、どなたでも購入していただけます。

### Azure の試用は無料ですか。課金される可能性はありませんか。

はい。[使用制限機能](https://azure.microsoft.com/pricing/spending-limits/)を利用すると、その機能を有効にしている限り、無料試用版、MSDN、MPN、または BizSpark プランにご契約いただいたお客様は、課金される心配なしに Azure をご利用いただけます。これらのプランにサインアップした新しいお客様はすべて、価格が $0 として使用制限が有効になります。

Azure にサインアップする方法の詳細については、[Azure サブスクリプションのサインアップ、購入、アップグレード、または有効化の方法](billing-buy-sign-up-azure-subscription.md)に関する記事を参照してください。

### 使用制限を無効にした場合、再度有効にすることはできますか。

メンバー特典プラン (MSDN など) のお客様は、次の請求サイクルの始めに使用制限機能を再度有効にすることができます。使用制限は、一度無効にすると、現在の請求期間に対して再度有効にすることはできません。詳細については、「[Azure 使用制限の変更](https://msdn.microsoft.com/library/azure/dn465781.aspx)」を参照してください。

### 使用制限の量は調整できますか。

この機能の初回リリースでは、使用制限は $0 に設定されており、調整はできません。この機能は、無料試用版、MSDN、MPN、または BizSpark プランのお客様が、課金されないという完全な保証付きで Azure を利用できるように設計されています。詳細については、「[Azure 使用制限](https://azure.microsoft.com/pricing/spending-limits/)」を参照してください。

### Microsoft Azure が販売されている国と地域および Azure の購入に使用できる通貨

Azure は、世界の 140 を超える国で購入でき、課金は多数の通貨でサポートされています。国と通貨の一覧は[こちら](billing-countries-and-currencies.md)で確認できます。

### 禁輸措置を受けている国に対する Azure ベースのサービスの再販は制限を受けますか。

はい。

### Azure と SQL Database は Microsoft Services Provider License Agreement で利用できますか。

現在、Azure または SQL Database を SPLA で提供する計画はありません。

### 無料試用版サブスクリプションとは何ですか。また、有効期間はどのくらいですか。

Azure 無料試用版サブスクリプションは、新規ユーザーを対象とした 1 か月間無料の試用版で、$200 (米ドル) の Azure クレジットを無料で提供しています。無料試用版クレジットの対象となるサービスと対象外のサービスについては、[無料試用版のページ](https://azure.microsoft.com/offers/ms-azr-0044p/)を参照してください。

試用版サブスクリプションには、[こちら](http://azure.microsoft.com/pricing/free-trial/)からサインアップできます。詳細な手順については、[こちらのガイド](billing-buy-sign-up-azure-subscription.md#sign-up-for-an-azure-free-trial-subscription)を参照してください。

### 無料試用版で利用できるリソースを教えてください。

無料試用版では、30 日間使用できる $200 (米ドル) の Azure クレジットを受け取れます。Azure クレジットをどう使うかは、お客様しだいです。詳細については、[無料試用版のページ](https://azure.microsoft.com/offers/ms-azr-0044p/)を参照してください。

試用版サブスクリプションには、[こちら](http://azure.microsoft.com/pricing/free-trial/)からサインアップできます。

[無料試用版の FAQ](https://azure.microsoft.com/pricing/free-trial-faq/) も参照してください。

### 無料試用版の使用制限を削除するとどうなりますか。

使用制限を削除すると、無料試用版のクレジットを超えた部分について、リソースの使用に応じた課金が開始されます。いったん削除した使用制限をリセットすることはできません。

### 無料試用版サブスクリプションでクォータの引き上げは可能ですか。

無料試用版サブスクリプションは、クォータ引き上げの適用対象外です。無料試用版をお持ちの場合、[従量課金](https://azure.microsoft.com/offers/ms-azr-0003p/)のサブスクリプションにアップグレードしてください。アップグレードの詳しい方法については、「[Azure 無料試用版を従量課金制にアップグレード](billing-buy-sign-up-azure-subscription.md#UpgradeFreeToPYG)」をご覧ください。

>[AZURE.NOTE] DreamSpark サブスクリプションも、クォータ引き上げの適用対象外となります。

### 無料試用版についての詳しい情報はどこで確認できますか。

[無料試用版のページ](https://azure.microsoft.com/offers/ms-azr-0044p/)と[無料試用版に関する公式 FAQ](https://azure.microsoft.com/pricing/free-trial-faq/) を参照してください。

> [AZURE.NOTE] まだ他に質問がある場合は、問題を迅速に解決できるよう[サポートにお問い合わせ](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)ください。

### 支払い方法を教えてください。

支払い方法としてクレジット カードやデビット カードを設定している場合、支払いは自動的に行われます。

[支払い方法として請求書](https://azure.microsoft.com/pricing/invoicing/)を利用するように設定している場合、請求書の下部に記載されている場所に支払いを送付します。詳しくは、[サポートにお問い合わせください](billing-how-to-create-billing-support-ticket.md)。

<!---HONumber=AcomDC_0914_2016-->