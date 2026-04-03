---
title: New Relic Account Management
description: New Relic アカウントにアクセスし、Adobe Commerce on cloud インフラストラクチャプロジェクトのアクセス、統合、ツールの使用状況を管理する方法について説明します。
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 83fde1f9771c0b3c7ad557233d76dfff91fa7a6c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---

# New Relic account management

Adobeがクラウドインフラストラクチャプロジェクトをプロビジョニングすると、ライセンス所有者は、New Relicから、New Relic アカウントにアクセスするための資格情報と手順が記載されたメールを受け取ります。 電子メールが届かない場合は、ライセンス所有者の電子メールアドレスを使用してNew Relicのパスワードをリセットします。

ライセンス所有者が変更され、新しいライセンス所有者が現在New Relicにアクセスできない場合は、[Adobe Commerce サポートチケットを送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

## ユーザーアクセスの管理（管理者の役割）

>[!NOTE]
>
>完全な機能セットへのアクセスを厳密に必要とするユーザーに対してのみ、完全なアクセス権を付与します。

**New Relicのユーザー管理にアクセスするには**:

1. [New Relic アカウント &#x200B;](https://login.newrelic.com/login)にログインします。

1. 左下のナビゲーションからユーザー名を選択します。

1. **[!UICONTROL Administration]**&#x200B;をクリックし、リストから次のいずれかを選択します。

   - **[!UICONTROL User management]**&#x200B;を使用すると、ユーザーの追加、アクティブ ユーザーと保留中の招待の管理、アクセスが不要になったユーザー（会社を離脱したユーザーなど）の削除または非アクティブ化を行うことができます。

   - **[!UICONTROL Access management]**&#x200B;を使用して、ユーザーグループ、役割、アカウントを管理します。

_New Relic_ ドキュメントの[User management](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/)を参照してください。

## New Relic for Starter環境の設定

>[!NOTE]
>
>**Pro環境**&#x200B;は、New Relic サービスを使用するように事前設定されており、有効にする手順と接続手順をスキップできます。 New Relic APMがステージング環境と実稼働環境にインストールされていない場合、またはNew Relic インフラストラクチャが実稼働環境で利用できない場合は、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信してインストールをリクエストしてください。

Starter環境の場合、`.magento.app.yaml` ファイルを確認して、`runtime` セクションにNew Relic拡張機能が含まれていることを確認する必要があります。 拡張機能が設定されていない場合は、以下を追加します。

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### ライセンスキーの適用

Cloud環境をNew Relicに接続するには、New Relic ライセンスキーを環境に追加します。

- **Pro プロジェクト**&#x200B;の場合、Adobeはプロビジョニングプロセス中に実稼動環境とステージング環境にライセンスキーを追加します。 [New Relic アカウント &#x200B;](https://login.newrelic.com/login)にログインして、Adobe Commerce on cloud infrastructure サイトとNew Relic間の接続性を確認できます。

- **スタータープロジェクト**&#x200B;の場合、最大&#x200B;_3_&#x200B;環境をサポートするNew Relic ライセンスキーがあります。 環境設定にキーを手動で追加する必要があります。 スターター環境は、New Relic サービスを使用するように事前にプロビジョニングされていません。

Starter環境の場合は、New Relic ライセンスキーを環境設定に追加して、New Relic統合を有効にします。 ステージング環境および実稼動環境と、選択した1つの環境にキーを追加します。 設定にはNew Relic ライセンスキーのみが必要です。 その他の設定オプションに関する情報については、_New Relic ユーザーガイド_&#x200B;の[Adobe Commerce レポート &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html)のトピックを参照してください。

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce アカウントページ、またはプロジェクトに関連付けられているNew Relic ライセンスのログイン資格情報
>- 構成するスターター環境への[管理者レベルのアクセス &#x200B;](../project/user-access.md)
>- 環境の[管理者](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html)にアクセスするための資格情報

**Starter環境用にNew Relicを設定するには**:

1. [!DNL Cloud Console]またはCloud CLIからNew Relicのライセンスキーを見つけます。

   **[!DNL Cloud Console]メソッド**:

   - クラウドプロジェクト [&#x200B; アカウントページ &#x200B;](https://accounts.magento.cloud/user)を開きます。

   - 「_プロジェクト_」タブで、自分のプロジェクトを見つけます。

   - プロジェクト インフラストラクチャ情報については、**詳細を表示**&#x200B;をクリックしてください。

   - **New Relic サービス** セクションを展開して、ライセンスキーを表示します。

   - ライセンスキーをコピーします。

   **Cloud CLI メソッド**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. `magento-cloud` CLIを使用して、New Relicのライセンスキーを環境に追加します。

   - ライセンスキーを必要とする環境に変更します。
   - 次の`magento-cloud` CLI コマンドを使用して変数値を更新します。

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   オプションで、[Commerce管理者](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store)から追加できます。

1. [New Relic アカウント &#x200B;](https://login.newrelic.com/login)にログインして、Adobe Commerce環境からデータを表示できることを確認します。 [&#x200B; パフォーマンスの調査](investigate-performance.md)を参照してください。

### ライセンスキーの削除

New Relicのライセンスキーは、3つのアクティブな環境でのみ使用できます。 キーが3つの環境で使用されている場合、別の環境に追加する前に、いずれかの環境からキーを削除する必要があります。

**環境からライセンスキーを削除するには**:

1. 環境変数のリスト。

   ```bash
   magento-cloud variable:list
   ```

   回答サンプル：

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >ライセンスキーを&#x200B;_プロジェクト_&#x200B;変数として追加した場合は、そのプロジェクトレベル変数を削除する必要があります。 プロジェクト変数は、作成された&#x200B;_環境ブランチごとに_&#x200B;にライセンスを追加します。このブランチは、ライセンス制限を消費または超過する可能性があります。 プロジェクト変数を一覧表示するには：`magento-cloud variable:list --level project`

1. ライセンス変数を削除します。

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## New Relic on Cloudのアカウントオーナーを変更する

Adobe Commerce on cloud infrastructure プロジェクトのNew Relic アカウントオーナーを変更するには、次の手順を実行します。

1. **New Relic UIでオーナー**&#x200B;を変更します。 New Relic ドキュメントの「[&#x200B; アカウント所有者を変更](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/)」を参照してください。

2. **ユーザーがアカウントにまだ登録していない場合は、最初に** ユーザーを追加します。 New Relic ドキュメントの[&#x200B; ユーザーの追加と更新](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users)を参照してください。

3. **サポートが必要ですか？** 既存のオーナーまたは管理者が支援できない場合は、[Adobe Commerce パートナーシップ オーナーのアカウント &#x200B;](https://account.newrelic.com/accounts/1311131/users)にアクセスできるAdobe Commerce ユーザーは、自分に代わってユーザーを追加できます。

詳しくは、[New Relic サービスの概要](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service)を参照してください。
