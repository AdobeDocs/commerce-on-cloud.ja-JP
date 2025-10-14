---
title: 管理変数
description: クラウドインフラストラクチャー上にAdobe Commerceをインストールする際に使用される環境変数のリストを参照してください。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
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

次の環境変数を使用して、管理 UI へのアクセスを保護します。 指定した場合、インストール時のデフォルト URL はこの値で上書きされます。

`ADMIN_URL` – 管理 UI にアクセスするための相対 URL。 デフォルトの URL は `/admin` です。 セキュリティ上の理由から、Adobeでは、デフォルトを推測しにくい一意のカスタム管理 URL に変更することをお勧めします。

### 管理者 URL の変更

Adobeでは、インストール後に管理者 URL の環境レベル変数を変更することをお勧めします。 クローン `master` 環境から分岐する前に、セキュリティ上の理由から、この設定を設定します。 `master` ブランチから作成されたすべてのブランチは、環境レベルの変数とその値を継承します。

`magento-cloud variable:update` コマンドを使用して、変数値を更新します。 （`variable:set` コマンドは非推奨となり、使用できなくなりました）。 次の例では、ADMIN_URL を `newAdmin_A8v10` に更新します。

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>`ADMIN_URL` の値には、カスタム管理パス用の文字（a ～ z または A ～ Z）、数字（0 ～ 9）、アンダースコア文字（_）を使用できます。 スペースやその他の文字は使用 **きません**。

**[!DNL Cloud Console]** を使用して URL を変更するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. プロジェクトの概要で、環境を選択し、設定アイコンをクリックします。

   ![&#x200B; プロジェクト設定 &#x200B;](../../assets/icon-configure.png){width="36"}

1. 「**変数**」タブを選択します。

1. **変数を作成** をクリックします。

1. 以下を入力します。

   - **変数名** = `ADMIN_URL`
   - **value** =新しい URL。 例えば、管理者 URL を `magento_A8v10` に設定します。

   デフォルトでは、`Available during runtime` と `Make inheritable` が選択されています。

1. **変数を作成** をクリックして、デプロイメントが完了するまで待ちます。 このボタンは、必須フィールドに値が含まれている場合にのみ表示されます。
