---
title: 管理変数
description: クラウドインフラストラクチャー上にAdobe Commerceをインストールする際に使用される環境変数のリストを参照してください。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: 4e751f02b92f954cd41d5523237da295a068661a
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# 管理変数

クラウドインフラストラクチャプロジェクト上のAdobe Commerceに対する管理アクセス権を持つユーザーは、次のプロジェクト環境変数を使用して、管理 UI にアクセスするための管理ユーザーアカウントの設定を上書きできます。

## 管理者の資格情報

次の表の管理変数を使用して、Commerceのインストール中に管理者ユーザーの資格情報を上書きできます。

インストール後に値を変更する場合は、SSH を使用して環境に接続し、Adobe Commerce CLI [`admin:user` コマンドを使用して &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=ja) 管理者ユーザーの資格情報を作成または編集します。

| 変数 | デフォルト | 説明 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | ライセンス所有者の E メールアドレス | 管理者ユーザーのユーザー名。管理者ユーザーを含む他のユーザーを作成できます。 |
| `ADMIN_EMAIL` |                             | 管理者ユーザーのメールアドレス。 このアドレスは、パスワードリセット通知の送信に使用されます。 |
| `ADMIN_PASSWORD` |                             | 管理者ユーザーのパスワード。 プロジェクトを作成すると、ランダムなパスワードが生成され、ライセンス所有者にメールが送信されます。 プロジェクトの作成時に、ライセンス所有者が既にパスワードを変更している必要があります。 更新されたパスワードについては、ライセンス所有者にお問い合わせください。 |
| `ADMIN_LOCALE` | `en_US` | 管理者が使用するデフォルトのロケールです。 |

## 管理者 URL

次の環境変数を使用して、管理 UI へのアクセスを保護します。 指定した場合、インストール時のデフォルト URL はこの値で上書きされます。 クラウドインフラストラクチャー上の [!DNL Adobe Commerce] では、（[!DNL Cloud Console] または [!DNL Cloud CLI]）の `ADMIN_URL` 変数を使用して管理者 URL を設定または変更する必要があります。 [!DNL Admin] からの設定の変更は、オンプレミスのインストールにのみ適用されます。

`ADMIN_URL` – 管理 UI にアクセスするための相対 URL。 デフォルトの URL は `/admin` です。

### 管理者 URL の変更

デフォルトでは、[Commerce管理者 &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html?lang=ja) の URL は *&lt;domain_name>/admin* に設定されています。 セキュリティ上の理由から、Adobeでは、推測しにくい一意のカスタム管理 URL に変更することをお勧めします。

**クラウドインフラストラクチャー上の [!DNL Adobe Commerce] では**、（[!DNL Cloud Console] または [!DNL Cloud CLI]）の `ADMIN_URL` 環境変数を使用して管理者 URL を変更する必要があります。 [!DNL Admin] からの設定の変更は、オンプレミスのインストールにのみ適用されます。 オンプレミスのインストールの場合は、[&#x200B; カスタム管理 URL を使用 &#x200B;](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=ja#use-a-custom-admin-url) に従います。

Adobeでは、インストール後に管理者 URL の環境レベル変数を変更することをお勧めします。 クローン `master` 環境から分岐する前に、セキュリティ上の理由から、この設定を設定します。 `master` ブランチから作成されたすべてのブランチは、継承を false に設定しない限り、環境レベルの変数とその値を継承します。

[!DNL Cloud Console] または [!DNL Cloud CLI] のいずれかを使用して、`ADMIN_URL` を設定または更新します。

#### オプション A:[!DNL Cloud Console] を使用して管理者 URL を変更

##### 統合環境

[Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=ja) から、次の設定で新しい変数を追加します。

- **名前：** `ADMIN_URL`
- **値：** 新しい管理者 URL （例：`magento_A8v10`）

- 詳しい手順については、開発者向けドキュメントの [&#x200B; 環境変数の追加 &#x200B;](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=ja#configure-environment) または [&#x200B; 環境変数 &#x200B;](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html?lang=ja) を参照してください。

##### [!DNL Cloud Console] に管理者 URL を設定

1. [Cloud Console](https://console.adobecommerce.com/) にログインします。
2. **[!UICONTROL All projects]** リストからプロジェクトを選択します。
3. プロジェクトの概要で、環境を選択し、設定アイコンをクリックします。
4. 「**[!UICONTROL Variables]**」タブを選択します。
5. 「**[!UICONTROL Create Variable]**」をクリックします（または、既存の `ADMIN_URL` 変数が存在する場合はそれを編集します）。
6. 以下を入力します。
   - **変数名：** `ADMIN_URL`
   - **値：** 新しい管理者パス（`magento_A8v10` など）。

   デフォルトでは、**[!UICONTROL Available during runtime]** と **[!UICONTROL Make inheritable]** が選択されています。 子環境がこの値を継承しないようにするには、この変数の **[!UICONTROL Make inheritable]** をクリアします。
7. **[!UICONTROL Create variable]** （または **[!UICONTROL Save]**）をクリックして、デプロイメントが完了するまで待ちます。 このボタンは、必須フィールドに値が含まれている場合にのみ表示されます。

##### [!DNL Cloud Console] でステージングと実稼動が使用できない場合

[&#x200B; サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/ja/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) ステージング環境または実稼動環境用の `ADMIN_URL` 変数の追加をリクエストします。 [!DNL Cloud Console] からステージングおよび実稼動環境にアクセスできる場合は、[&#x200B; 統合環境 &#x200B;](#integration-environment) の説明に従って変数を追加します。

#### オプション B:[!DNL Cloud CLI] を使用して管理者 URL を変更

`magento-cloud variable:update` コマンドを使用して、変数を更新します。 （`variable:set` コマンドは非推奨となり、使用できなくなりました）。

次の例では、`master` 環境の `ADMIN_URL` を `newAdmin_A8v10` に更新し、子環境が値を継承しないようにします。

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **再配置：** [!DNL Cloud CLI] の `ADMIN_URL` 変数を変更すると、環境の再配置がトリガーされます。
- **継承：** 変数は、デフォルトで継承できます。 子環境に値が継承されないようにするには、次に示すように、`--inheritable false` オプションを使用します。 詳細は、「変数レベルの表示 [&#x200B; を参照してください &#x200B;](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html?lang=ja#visibility)。

>[!NOTE]
>
>`ADMIN_URL` の値には、文字（a ～ z、A ～ Z）、数字（0 ～ 9）、アンダースコア文字（_）を使用できます。 スペースやその他の文字は使用できません。
