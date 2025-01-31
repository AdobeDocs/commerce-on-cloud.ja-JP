---
title: New Relic アカウント管理
description: New Relic アカウントにアクセスし、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのアクセス、統合、ツール使用を管理する方法について説明します。
feature: Cloud, Observability
role: Admin
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 0%

---

# New Relic アカウント管理

Adobeがクラウドインフラストラクチャプロジェクトをプロビジョニングすると、ライセンスオーナーは、New Relic アカウントにアクセスするための資格情報と手順が記載されたメールをNew Relicから受け取ります。 メールを受け取っていない場合は、ライセンス所有者のメールアドレスを使用して、New Relicのパスワードをリセットします。

## ユーザーアクセスの管理

>[!NOTE]
>
>完全な機能セットへのアクセスを厳密に必要とするユーザーにのみ、フルアクセス権を付与します。

**New Relicで User Management にアクセスするには**:

1. [New Relic アカウント ](https://login.newrelic.com/login) にログインします。

1. 左下のナビゲーションからユーザー名を選択します。

1. 「**[!UICONTROL Administration]**」をクリックし、リストから次のいずれかを選択します。

   - ユーザーを追加し、アクティブなユーザーと保留中の招待を管理で **[!UICONTROL User management]** ます。

   - ユーザーグループ、役割、アカウントを管理で **[!UICONTROL Access management]** ます。

[2}New Relic](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) ドキュメントの {User Management _を参照してください。_

## スターター環境用のNew Relicの設定

>[!NOTE]
>
>**Pro 環境** は、New Relic サービスを使用するように事前設定されており、有効化と接続の手順をスキップできます。 New Relic APM がステージング環境と実稼動環境にインストールされていない場合、またはNew Relic インフラストラクチャが実稼動環境で使用できない場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) してインストールをリクエストします。

スターター環境の場合、`.magento.app.yaml` ファイルを確認して、`runtime` セクションにNew Relic拡張機能が含まれていることを確認する必要があります。 拡張機能が設定されていない場合は、次を追加します。

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### ライセンスキーを適用

クラウド環境をNew Relicに接続するには、New Relic ライセンスキーを環境に追加します。

- **Pro プロジェクト** の場合、Adobeは、プロビジョニングプロセスの間に、実稼動環境とステージング環境にライセンスキーを追加します。 [New Relic アカウント ](https://login.newrelic.com/login) にログインすると、クラウドインフラストラクチャサイト上のAdobe CommerceとNew Relic間の接続を確認できます。

- **スタータープロジェクト** の場合は、最大 _3_ 環境をサポートするNew Relic ライセンスキーがあります。 キーは手動で環境設定に追加する必要があります。 スターター環境は、New Relic サービスを使用するように事前にプロビジョニングされていません。

スターター環境の場合は、環境設定にNew Relic ライセンスキーを追加して、New Relic統合を有効にします。 キーをステージング環境、実稼動環境、および任意のもう 1 つの環境に追加します。 設定に必要なのはNew Relic ライセンスキーのみです。 その他の設定オプションについて詳しくは、_New Relic ユーザーガイドの [Adobe Commerce レポート ](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) に関するトピックを参照してくだ_ い。

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce アカウントページまたはプロジェクトに関連付けられたNew Relic ライセンスのログイン資格情報
>- 設定するスターター環境への [ 管理者レベルのアクセス ](../project/user-access.md)
>- 環境の [ 管理者 ](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) にアクセスするための資格情報

**スターター環境用のNew Relicを設定するには**:

1. [!DNL Cloud Console] または Cloud CLI でNew Relic ライセンスキーを見つけます。

   **[!DNL Cloud Console]メソッド**:

   - クラウドプロジェクト [ アカウントページ ](https://accounts.magento.cloud/user) を開きます。

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

1. Adobe Commerceからデータを表示できることを確認するには ](https://login.newrelic.com/login)[New Relic アカウントにログインします。 [ パフォーマンスの調査 ](investigate-performance.md) を参照してください。

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
