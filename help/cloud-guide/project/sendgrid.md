---
title: SendGrid メールサービス
description: クラウドインフラストラクチャ上のAdobe CommerceのSendGrid メールサービスと、DNS設定をテストする方法について説明します。
exl-id: 06236068-df32-468f-99ec-c379984be136
TQID: https://experienceleague.adobe.com/I4giHpOngkQ0KZYBXZoGJGBXLWme2fxE39uIOGnON-k
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: beb7a3c1-66ab-4786-b879-7621375b3c40id: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1702
ht-degree: 0%

---

# SendGrid メールサービス

SendGrid Simple Mail Transfer Protocol （SMTP）プロキシサービスは、アウトバウンドメール認証とレピュテーション監視サービスを提供し、次の機能をサポートします。

* すべてのアウトバウンドトランザクションメール
* 専用IP アドレス
* Domain registration, DomainKeys Identified Mail （DKIM）の電子メールドメイン検証用の署名（Pro ステージング環境および実稼動環境のみ）
* カスタムドメイン登録（Proのみ）
* スターター環境とPro統合環境用の自動統合。 プロの実稼動環境とステージング環境では、IaaS （Infrastructure as a Service）ハードウェアプロビジョニングプロセス中に手動によるプロビジョニングと設定が必要です

SendGrid SMTP プロキシは、受信メールを受信するための汎用メールサーバーとして使用したり、メールマーケティングキャンペーンで使用したりすることを目的としていません。 多数の顧客（例えば、10,000人以上）にウェルカムメールをインポートして送信する場合は、インポートとメール送信を複数の日に分割します。 この手法は、日々の送信制限を守り、送信者のレピュテーションを守るのに役立ちます。

>[!TIP]
>
>配信品質とドメイン検証に関する問題を回避するには、ストア/設定/一般に移動して、管理者で適切なストアメールアドレスを設定していることを確認します。 **[!UICONTROL Use Default]**&#x200B;のチェックを外し、デフォルト値を所有するドメインに置き換える必要があります。 gmail.comやoutlook.comなどのパブリック/共有ドメインのメールサービスは、Sendgridを通じてメールを送信する際に、送信者のメールアドレスとして設定しないでください。

## 電子メールを有効または無効にする

Cloud Consoleまたはコマンドラインから、各環境の送信メールを有効または無効にできます。

デフォルトでは、Pro実稼動環境とステージング環境で送信メールが有効になっています。 ただし、[ コマンドライン ](outgoing-emails.md#enable-emails-in-the-cli)または[Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console)を使用して`enable_smtp` プロパティを設定するまで、[!UICONTROL Outgoing emails]は環境設定で無効に表示される場合があります。 統合環境とステージング環境で送信メールを有効にして、Cloud プロジェクトユーザーに2要素認証を送信したり、パスワードリセットのメールを送信したりできます。 [ テスト用メールの設定](outgoing-emails.md)を参照してください。

Pro実稼動環境またはステージング環境で送信メールを無効にするか、再度有効にする必要がある場合は、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)を送信できます。

>[!TIP]
>
>[ コマンドライン ](outgoing-emails.md#enable-emails-in-the-cli)によって[!UICONTROL enable_smtp] プロパティ値を更新すると、[Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console)でこの環境の[!UICONTROL Enable outgoing emails]設定値も変更されます。

## SendGrid ダッシュボード

すべてのクラウドプロジェクトは中央アカウントで管理されるため、SendGrid ダッシュボードにアクセスできるのはサポートのみです。 SendGridにはサブアカウント制限機能がありません。

アクティビティログで配信ステータスまたはバウンス済み、拒否またはブロックされたメールアドレスのリストを確認するには、[Adobe Commerce サポートチケットを送信します](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)。 サポート チーム **は、30日を超えるアクティビティ ログを取得できません。**

可能であれば、次の情報をリクエストに含めます。

* 影響を受けるメールアドレス
* 当該期間（過去30日以内のみ）
* メールの件名

メール配信の設定をより適切に管理するには、独自の[SMTP サーバーまたはメール配信サービスプロバイダー](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications)を使用してください。 また、独自のSendGrid サービスにサインアップして、Cloud サービススタックを詳細に追跡することもできます。

>[!IMPORTANT]
>
>自分のSendGrid アカウントを使用している場合、Adobeを通じてSendGrid サポートを受け取ることができなくなります。
>
>独自のSendGrid サービスを有効にしたり、既存のAPI キーを更新したりするには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)を送信し、SendGrid アカウントのAPI キーを含めてください。

## DomainKeys Identified Mail （DKIM）

DKIMは、インターネットサービスプロバイダー（ISP）が、フィッシング詐欺やメール詐欺で一般的に使用される手法である、正規の送信者アドレスと偽の送信者アドレスの両方を識別できるようにするメール認証テクノロジーです。 DKIMは、DNS レコードを管理するドメイン所有者に依存しています。 DKIMを使用する場合、送信者サーバーは秘密鍵を使用してメッセージに署名します。 また、ドメイン所有者は、変更された`TXT` レコードであるDKIM レコードを送信者ドメインのDNS レコードに追加します。 この`TXT` レコードには、受信者メール サーバーがメッセージの署名を検証するために使用する公開鍵が含まれています。 DKIM公開鍵暗号化手順により、受信者は送信者の真正性を検証できます。 詳しくは、[DKIM レコードの説明](https://docs.sendgrid.com/ui/account-and-settings/dkim-records)を参照してください。

>[!WARNING]
>
>SendGrid DKIMの署名とドメイン認証のサポートは、Pro プロジェクトの実稼動環境とステージング環境でのみ利用できます。 スターター環境ではサポートされていません。
>
>このため、スターター環境から送信されたトランザクションメールは、完全に認証できないため、スパムとしてマークされる可能性が高くなります。  Pro環境では、DKIMを有効にすると送信ドメインが認証されるため、メールの配信品質が大幅に向上し、メッセージがスパムとしてフィルタリングされる可能性が低くなります。
>
>メッセージ配信率を向上させるには、StarterからProにアップグレードするか、独自のSMTP サーバーまたはメール配信サービスプロバイダーを使用します。 _管理者システムガイド_&#x200B;の「[ メール接続の設定](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications)」を参照してください。

### 送信者とドメインの認証

SendGridがPro実稼動環境またはステージング環境からトランザクションメールを送信するには、3つのSendGrid サブドメイン DNS エントリを含めるようにDNS設定を構成する必要があります。 各SendGrid アカウントには、送信メールの認証に使用される一意の`TXT` レコードが割り当てられます。

>[!TIP]
>
>**[!UICONTROLSストア電子メールアドレス]**&#x200B;を&#x200B;**[!UICONTROL Stores > Configuration > General > Store Email Addresses]**&#x200B;の適切なドメインで設定していることを確認してください。 ドメイン認証は、送信者の電子メールアドレスに対して実行されます。 デフォルト設定（`example.com`）が設定されている場合、`example.com`からのメールはSendgridによってブロックされます。

**ドメイン認証を有効にするには**:

1. [ サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)を送信して、特定のドメイン（**Pro ステージングおよび実稼動環境のみ**）に対するDKIMの有効化をリクエストします。
1. サポートチケットに記載されている`TXT`および`CNAME`件のレコードを使用して、DNS設定を更新します。

**アカウント ID**&#x200B;の`TXT` レコードの例：

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**例`CNAME` レコード**:

| ドメイン | へのポイント | レコードタイプ |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIMの署名と自動セキュリティ

認証済みドメインを設定する際に、自動セキュリティと手動セキュリティのどちらかを選択できます。 自動セキュリティを選択した場合、SendGridはDKIMとSPF レコードを自動的に管理します。 新しい専用の送信用IP アドレスをアカウントに追加すると、SendGridはDNS設定とDKIM署名を即座に更新します。 自動セキュリティをオフにすると、送信ドメインを変更するたびにDKIM署名を更新する責任があります。

**自動セキュリティが有効になっている例**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**自動セキュリティの例が無効になりました**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

ドメイン認証が設定されると、SendGridはセキュリティポリシーフレームワーク（SPF）とDKIM レコードを自動的に処理します。 SendGridが`CNAME` レコードを提供してDNS レコードに追加すると、SPF レコードを手動で管理することなく、専用IP アドレスを追加したり、その他のアカウントを更新したりできます。 [自動セキュリティとDKIM署名](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature)を参照してください。

DNS設定をテストするには：

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## トランザクションメールのしきい値

トランザクションメールのしきい値とは、特定の期間内にPro環境から送信できるトランザクションメールメッセージの数を指します。例えば、実稼動以外の環境では、1か月あたり12,000通のメールが送信されます。 しきい値は、迷惑メールの送信や、電子メールのレピュテーションが損なわれる可能性から保護するように設計されています。

送信者のレピュテーションスコアが95%を超えている限り、本番環境で送信できるメールの数に制限はありません。 レピュテーションは、バウンスされた電子メールの数または拒否された電子メールの数、およびDNS ベースのスパムレジストリがドメインに潜在的なスパムソースとしてフラグを立てたかどうかに影響されます。 _Commerce サポート ナレッジベース_&#x200B;の「](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) Adobe CommerceでSendGrid クレジットを超えた場合に送信されない電子メール」を参照してください。[

**最大クレジット数が**&#x200B;を超えているかどうかを確認するには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. `/var/log/mail.log`で`authentication failed : Maxium credits exceeded`個のエントリを確認してください。

   `authentication failed`件のログエントリが表示され、**電子メール送信レピュテーション**&#x200B;が95以上の場合、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)を送信してクレジット割り当ての引き上げをリクエストできます。

>[!NOTE]
>
>`var/log/mail.log` ファイルは&#x200B;*実行ログ*&#x200B;です。 新しいエントリが追加されると、古いエントリは時間の経過とともにファイルから削除されます。 最新のログアクティビティのみがログに記録されます。 古いログエントリは、mail.logから削除するとアーカイブまたは保持されません。

### メール送信のレピュテーション

メール送信のレピュテーションとは、メールメッセージを送信する企業にインターネットサービスプロバイダー（ISP）によって割り当てられたスコアのことです。 スコアが高いほど、ISPが受信者の受信トレイにメッセージを配信する可能性が高くなります。 スコアが一定レベルを下回った場合、ISPはメッセージを受信者のスパムフォルダーに振り分けたり、メッセージを完全に拒否したりします。 レピュテーションスコアは、IP アドレスの30日間平均が他のIP アドレスに対してランク付けされること、迷惑メール苦情率など、いくつかの要因によって決まります。 [電子メールのレピュテーションを確認する8つの方法](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation)を参照してください。

### メール抑制リスト

メール抑制リストとは、送信したメールのレピュテーションと配信率が低下する場合に、そのメールを送信しない受信者のリストのことです。 CAN-SPAM法では、メール送信者が、メールを登録解除または迷惑メールとしてマークした受信者をオプトアウトする方法を有していることを確認するために必要です。 抑制リストには、バウンス、ブロック、無効なメールも収集されます。

メールが迷惑メールフォルダーに送信されないようにするには、Sendgridのベストプラクティスの記事「[電子メールが迷惑メールに振り分けられる理由は？](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder)」に従ってください。

一部の受信者がメールを受信していない場合は、[Adobe Commerce サポートチケットを送信して、抑制リストのレビューをリクエストし、必要に応じて受信者を削除できます](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)。

詳細については、[抑制リストとは何ですか？](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)を参照してください
