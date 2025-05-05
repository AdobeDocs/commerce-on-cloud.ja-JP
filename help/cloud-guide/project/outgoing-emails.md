---
title: 送信メールの設定
description: クラウドインフラストラクチャでAdobe Commerce用の送信メールを有効にする方法を説明します。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# 送信メールの設定

[!DNL Cloud Console] またはコマンドラインから、統合（スターターのみの場合はステージング）環境用の送信メールを有効または無効にできます。 送信メールを有効にして、クラウドプロジェクトユーザーに二要素認証またはパスワードリセットメールを送信できるようにします。

デフォルトでは、送信メールは実稼動環境とステージング環境（Pro のみ）で有効になります。 ただし、[ コマンドライン ](#enable-emails-in-the-cli) または [Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console) から `enable_smtp` プロパティを設定するまで、ステータスに関係なく、環境設定で **[!UICONTROL Enable outgoing emails]** 設定が無効に表示される場合があります。

[ コマンドライン ](#enable-emails-in-the-cli) で `enable_smtp` プロパティ値を更新すると、Cloud Console でこの環境の [!UICONTROL Enable outgoing emails] 設定値も変更されます。

>[!NOTE]
>
>**[!UICONTROL Enable outgoing emails]** 設定を有効/無効にしても、ステージング環境または実稼動環境でメールを有効/無効にすることはできません。

{{redeploy-warning}}

## Cloud Console でメールを有効にする

_環境を設定_ ビューの **[!UICONTROL Outgoing emails]** 切り替えスイッチを使用して、メールのサポートを有効または無効にします。

実稼動環境またはステージング環境で送信メールを無効にするか再度有効にする必要がある場合は、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide) を送信できます。

>[!TIP]
>
>送信メールのステータスは、Cloud Console のステージング環境または実稼動環境では反映されない場合があります。

**[!DNL Cloud Console]** からメールのサポートを管理するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。
1. _すべてのプロジェクト_ リストからプロジェクトを選択します。
1. プロジェクトダッシュボードで、右上の設定アイコンをクリックします。
1. 「**[!UICONTROL Environments]**」をクリックし、リストから特定の環境を選択します（「ステージング」と「Production for Pro」を除く）。
1. 送信メールを有効または無効にするには、_送信メールを有効にする_&#x200B;**オン** または **オフ** を切り替えます。

   ![ 送信メール設定を有効にする ](../../assets/outgoing-emails.png)

設定を変更すると、環境は新しい設定でビルドおよびデプロイされます。

## CLI でのメールの有効化

アクティブな環境のメール設定を変更するには、`magento-cloud` CLI `environment:info` コマンドを使用して `enable_smtp` プロパティを設定します。 SMTP を有効にすると、メール送信用の SMTP ホストの IP アドレスで `MAGENTO_CLOUD_SMTP_HOST` 環境変数が更新されます。

**コマンドラインからメールサポートを管理するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. 環境の送信メールの設定を確認します。

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. `enable_smtp` 環境変数を `true` または `false` に設定して、メールサポートの設定を変更します。

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   環境がビルドおよびデプロイされるのを待ちます。

1. SSH を使用してリモート環境にログインします。

1. メールが機能することを確認します。確認可能なアドレスにテストメールを送信します。

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. メールが SendGrid で受信されることを確認します。

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
