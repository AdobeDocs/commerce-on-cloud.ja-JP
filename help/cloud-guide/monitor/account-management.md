---
title: New Relic アカウント管理
description: New Relic アカウントにアクセスし、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのアクセス、統合、ツール使用を管理する方法について説明します。
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 558c645e353e38ce8455ef17e1d0e9fa99b22c6e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# New Relic アカウント管理

Adobeがクラウドインフラストラクチャプロジェクトをプロビジョニングすると、ライセンスオーナーは、New Relic アカウントにアクセスするための資格情報と手順が記載されたメールをNew Relicから受け取ります。 メールを受け取っていない場合は、ライセンス所有者のメールアドレスを使用して、New Relicのパスワードをリセットします。

ライセンス所有者が変更され、新しいライセンス所有者が現在New Relicにアクセスできない場合は、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) してください。

## ユーザーアクセスの管理（管理者の役割）

>[!NOTE]
>
>完全な機能セットへのアクセスを厳密に必要とするユーザーにのみ、フルアクセス権を付与します。

**New Relicで User Management にアクセスするには**:

1. [New Relic アカウント &#x200B;](https://login.newrelic.com/login) にログインします。

1. 左下のナビゲーションからユーザー名を選択します。

1. 「**[!UICONTROL Administration]**」をクリックし、リストから次のいずれかを選択します。

   - ユーザーを追加し、アクティブなユーザーと保留中の招待を管理で **[!UICONTROL User management]** ます。

   - ユーザーグループ、役割、アカウントを管理で **[!UICONTROL Access management]** ます。

[2&rbrace;New Relic](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) ドキュメントの &lbrace;User Management _を参照してください。_

## スターター環境用のNew Relicの設定

>[!NOTE]
>
>**Pro 環境** は、New Relic サービスを使用するように事前設定されており、有効化と接続の手順をスキップできます。 New Relic APM がステージング環境と実稼動環境にインストールされていない場合、またはNew Relic インフラストラクチャが実稼動環境で使用できない場合は、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) してインストールをリクエストします。

スターター環境の場合、`.magento.app.yaml` ファイルを確認して、`runtime` セクションにNew Relic拡張機能が含まれていることを確認する必要があります。 拡張機能が設定されていない場合は、次を追加します。

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### ライセンスキーを適用

クラウド環境をNew Relicに接続するには、New Relic ライセンスキーを環境に追加します。

- **Pro プロジェクト** の場合、Adobeは、プロビジョニングプロセスの間に、実稼動環境とステージング環境にライセンスキーを追加します。 [New Relic アカウント &#x200B;](https://login.newrelic.com/login) にログインすると、クラウドインフラストラクチャサイト上のAdobe CommerceとNew Relic間の接続を確認できます。

- **スタータープロジェクト** の場合は、最大 _3_ 環境をサポートするNew Relic ライセンスキーがあります。 キーは手動で環境設定に追加する必要があります。 スターター環境は、New Relic サービスを使用するように事前にプロビジョニングされていません。

スターター環境の場合は、環境設定にNew Relic ライセンスキーを追加して、New Relic統合を有効にします。 キーをステージング環境、実稼動環境、および任意のもう 1 つの環境に追加します。 設定に必要なのはNew Relic ライセンスキーのみです。 その他の設定オプションについて詳しくは、[New Relic ユーザーガイドの &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html)Adobe Commerce レポート _に関するトピックを参照してくだ_ い。

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce アカウントページまたはプロジェクトに関連付けられたNew Relic ライセンスのログイン資格情報
>- 設定するスターター環境への [&#x200B; 管理者レベルのアクセス &#x200B;](../project/user-access.md)
>- 環境の [&#x200B; 管理者 &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) にアクセスするための資格情報

**スターター環境用のNew Relicを設定するには**:

1. [!DNL Cloud Console] または Cloud CLI でNew Relic ライセンスキーを見つけます。

   **[!DNL Cloud Console]メソッド**:

   - クラウドプロジェクト [&#x200B; アカウントページ &#x200B;](https://accounts.magento.cloud/user) を開きます。

   - 「_プロジェクト_」タブで、プロジェクトを検索します。

   - プロジェクトインフラストラクチャの情報を表示するには、「**詳細を表示**」をクリックします。

   - 「**New Relic サービス**」セクションを展開すると、ライセンスキーが表示されます。

   - ライセンスキーをコピーします。

   **Cloud CLI メソッド**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. `magento-cloud` CLI を使用して、New Relic ライセンスキーを環境に追加します。

   - ライセンスキーを必要とする環境に変更します。
   - 次の `magento-cloud` CLI コマンドを使用して、変数値を更新します。

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   オプションで、[Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store) から追加できます。

1. Adobe Commerceからデータを表示できることを確認するには [&#128279;](https://login.newrelic.com/login)New Relic アカウントにログインします。 [&#x200B; パフォーマンスの調査 &#x200B;](investigate-performance.md) を参照してください。

### ライセンスキーを削除

New Relic ライセンスキーは、3 つのアクティブな環境でのみ使用できます。 キーが 3 つの環境で使用されている場合、別の環境に追加する前に、いずれかの環境からキーを削除する必要があります。

**環境からライセンスキーを削除するには**:

1. 環境変数のリスト。

   ```bash
   magento-cloud variable:list
   ```

   応答の例：

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >ライセンス キーを _プロジェクト_ 変数として追加した場合は、そのプロジェクト レベル変数を削除する必要があります。 プロジェクト変数は、作成された _すべて_ の環境ブランチにライセンスを追加します。ライセンスは、ライセンスを消費するか、ライセンス制限を超える可能性があります。 プロジェクト変数を一覧表示するには、次の手順に従います。`magento-cloud variable:list --level project`

1. ライセンス変数を削除します。

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## Cloud 上のNew Relicのアカウントオーナーの変更

Adobe Commerce on cloud infrastructure プロジェクトのNew Relic アカウント所有者を変更するには：

1. New Relic UI の **所有者を変更** します。 New Relic ドキュメントの [&#x200B; アカウント所有者の変更 &#x200B;](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/) を参照してください。

2. **アカウントにまだ登録されていない場合は** 最初にユーザーを追加します。 New Relic ドキュメントの [&#x200B; ユーザーの追加と更新 &#x200B;](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users) を参照してください。

3. **サポートが必要な場合** 既存のオーナーまたは管理者が手助けできない場合、[Adobe Commerce パートナーシップオーナーアカウントにアクセスできるAdobe Commerce ユーザーであれば &#x200B;](https://account.newrelic.com/accounts/1311131/users) 誰でも代理でユーザーを追加できます。

詳しくは、[New Relic サービスの概要 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service) を参照してください。
