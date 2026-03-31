---
title: 管理者変数
description: Adobe Commerceをクラウドインフラストラクチャにインストールする際に使用される環境変数の一覧を参照してください。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: ac1b2001294ba72304fc7ad3c760872dbd73e44f
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 管理者変数

クラウドインフラストラクチャプロジェクト上のAdobe Commerceに対する管理アクセス権を持つユーザーは、次のプロジェクト環境変数を使用して、管理ユーザーアカウントの設定を上書きして管理UIにアクセスできます。

## 管理者の資格情報

次の表のADMIN変数を使用して、Commerceのインストール中に管理者ユーザーの資格情報を上書きできます。

インストール後に値を変更する場合は、SSHを使用して環境に接続し、Adobe Commerce CLI [`admin:user` コマンド &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html)を使用してAdmin ユーザー資格情報を作成または編集します。

| 変数 | Default | 説明 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | ライセンス所有者のユーザー名 | 管理ユーザーを含む他のユーザーを作成できる管理ユーザーのユーザー名。 |
| `ADMIN_FIRSTNAME` | ライセンス所有者の名 | 管理ユーザーの名。 |
| `ADMIN_LASTNAME` | ライセンス所有者の姓 | 管理ユーザーの姓。 |
| `ADMIN_EMAIL` | ライセンス所有者のメール | 管理ユーザーのメールアドレス。 このアドレスは、パスワードリセット通知の送信に使用されます。 |
| `ADMIN_PASSWORD` | ライセンス所有者のパスワード | 管理ユーザーのパスワード。 プロジェクトが作成されると、ランダムなパスワードが生成され、ライセンス所有者に電子メールが送信されます。 プロジェクトの作成時に、ライセンス所有者は既にパスワードを変更している必要があります。 更新されたパスワードについては、ライセンス所有者にお問い合わせください。 |
| `ADMIN_LOCALE` | `en_US` | 管理者が使用するデフォルトのロケール。 |

## 管理者URL

次の環境変数を使用して、管理者UIへのアクセスを保護します。 指定した場合、この値はインストール時にデフォルト URLを上書きします。 クラウドインフラストラクチャの[!DNL Adobe Commerce]では、（[!DNL Cloud Console]または[!DNL Cloud CLI]）の`ADMIN_URL`変数を使用して管理者URLを設定または変更する必要があります。 [!DNL Admin]からの設定の変更は、オンプレミスのインストールにのみ適用されます。

`ADMIN_URL` – 管理者UIにアクセスするための相対URL。 既定のURLは`/admin`です。

### 管理者URLの変更

デフォルトでは、[Commerce管理者](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html) URLは&#x200B;*&lt;domain_name>/admin*&#x200B;に設定されています。 セキュリティ上の理由から、Adobeでは、推測しにくい一意のカスタム管理者URLに変更することをお勧めします。

**クラウドインフラストラクチャ**&#x200B;の[!DNL Adobe Commerce]では、（[!DNL Cloud Console]または[!DNL Cloud CLI]）の`ADMIN_URL`環境変数を使用して管理者URLを変更する必要があります。 [!DNL Admin]からの設定の変更は、オンプレミスのインストールにのみ適用されます。 オンプレミスのインストールの場合は、[&#x200B; カスタム管理者URLを使用](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html#use-a-custom-admin-url)します。

Adobeでは、インストール後に管理者URLの環境レベル変数を変更することをお勧めします。 クローンされた`master`環境から分岐する前に、セキュリティ上の理由から、この設定を構成します。 `master` ブランチから作成されたすべてのブランチは、継承をfalseに設定しない限り、環境レベルの変数とその値を継承します。

[!DNL Cloud Console]または[!DNL Cloud CLI]のいずれかを使用して、`ADMIN_URL`を設定または更新します。

#### オプション A: [!DNL Cloud Console]を使用して管理者URLを変更する

##### 統合環境

[Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html)から、次の新しい変数を追加します。

- **名前：** `ADMIN_URL`
- **値：**&#x200B;新しい管理者URL （例：`magento_A8v10`）

- 詳細な手順については、開発者向けドキュメントの「[環境変数を追加](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment)」または「[環境変数](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html)」を参照してください。

##### [!DNL Cloud Console]で管理者URLを設定

1. [Cloud Console](https://console.adobecommerce.com/)にログインします。
2. **[!UICONTROL All projects]** リストからプロジェクトを選択します。
3. プロジェクトの概要で、環境を選択し、設定アイコンをクリックします。
4. 「**[!UICONTROL Variables]**」タブを選択します。
5. **[!UICONTROL Create Variable]**&#x200B;をクリックします（既存の`ADMIN_URL`変数がある場合は編集します）。
6. 以下を入力します。
   - **変数名：** `ADMIN_URL`
   - **値：**&#x200B;新しい管理者パス（例：`magento_A8v10`）。

   デフォルトでは、**[!UICONTROL Available during runtime]**&#x200B;と&#x200B;**[!UICONTROL Make inheritable]**&#x200B;が選択されています。 子環境がこの値を継承しないようにするには、この変数の&#x200B;**[!UICONTROL Make inheritable]**&#x200B;をクリアします。
7. **[!UICONTROL Create variable]** （または&#x200B;**[!UICONTROL Save]**）をクリックし、デプロイメントが完了するまで待ちます。 ボタンは、必須フィールドに値が含まれている場合にのみ表示されます。

##### ステージングと実稼動が[!DNL Cloud Console]で利用できない場合

ステージング環境または実稼動環境の`ADMIN_URL`変数の追加を要求する[&#x200B; サポートチケット &#x200B;](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)を送信します。 ステージングと実稼動が[!DNL Cloud Console]からアクセスできる場合は、[統合環境](#integration-environment)で説明されているように、変数を追加します。

#### オプション B: [!DNL Cloud CLI]を使用して管理者URLを変更する

変数を更新するには、`magento-cloud variable:update` コマンドを使用します。 （`variable:set` コマンドは非推奨となったため、使用できません）。

次の例では、`master`環境`ADMIN_URL`を`newAdmin_A8v10`に更新し、子環境が値を継承しないようにします。

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **再デプロイメント：** [!DNL Cloud CLI]トリガーの`ADMIN_URL`変数を、環境の再デプロイメントに変更します。
- **継承：**&#x200B;変数はデフォルトで継承されます。 値が子環境に継承されないようにするには、図に示すように`--inheritable false` オプションを使用します。 詳しくは、[変数レベルの表示](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html#visibility)を参照してください。

>[!NOTE]
>
>`ADMIN_URL`の値には、文字（a ～ z、A ～ Z）、数字（0 ～ 9）、およびアンダースコア文字（_）を使用できます。 スペースやその他の文字は使用できません。
