---
title: ユーザーアクセスの管理
description: クラウドインフラストラクチャプロジェクトおよび環境でAdobe Commerceへのユーザーアクセスを管理する方法について説明します。
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1459'
ht-degree: 0%

---

# ユーザーアクセスの管理

クラウドインフラストラクチャ上のAdobe Commerce プロジェクトでは、役割ベースのアクセスを使用します。 プロジェクトレベルでは、次の 2 つの役割を使用できます。

- **プロジェクト管理者** – すべてのプロジェクト環境への書き込みアクセス権。ユーザーの管理、コードのプッシュ、プロジェクト設定の更新を行うことができます。 （旧称 **スーパー管理者**）
- **プロジェクトビューア** – すべてのプロジェクト環境への表示のみのアクセス。

プロジェクトビューアはどの環境でもタスクを実行できませんが、プロジェクトビューアに特定の環境タイプへの書き込みアクセス権を付与できます。

環境レベルのアクセスは、実稼働、ステージング、開発の環境タイプに基づいています。 ユーザー _ビューア_ に _開発_ 環境に対する権限を付与すると、そのユーザーはプロジェクト内の **すべて** の開発環境を表示できます。 次の表に、各権限レベルに付与される機能を示します。

| 権限レベル | アクセス | SSH アクセス |
| ------------------ | ----------- | :----------: |
| **管理者** | 設定の変更、プッシュコード、タスクの実行、親環境とのマージを含むブランチ管理などの管理タスクを実行します。 | はい |
| **投稿者** | 環境のプッシュコードおよびブランチ（設定の変更やアクションの実行ができない） | はい |
| **ビューア** | 環境タイプへの表示専用アクセス | 不可 |
| **アクセスなし** | 環境タイプへのアクセス権がありません | 不可 |

{style="table-layout:auto"}

`magento-cloud` CLI または [!DNL Cloud Console] を使用して、ユーザーを追加したり、役割を割り当てたりできます。

>[!BEGINSHADEBOX]

**前提条件：**

- Adobe IDに登録されているユーザー。 ユーザーをクラウドプロジェクトに追加するには、[Adobeアカウントに登録 &#x200B;](https://account.adobe.com) してから [&#x200B; クラウドアカウントを初期化 &#x200B;](https://console.adobecommerce.com) する必要があります。
- **管理者** の役割を割り当てられたユーザーは、`magento-cloud` CLI を使用してユーザーを管理できません。 **アカウント所有者** の役割を付与されたユーザーのみが、ユーザーを管理できます。

>[!ENDSHADEBOX]

## CLI を使用したユーザー管理

`magento-cloud` CLI を使用してユーザーを管理し、自動システムと統合します。

- ユーザーをプロジェクトに `magento-cloud user:add` 追加
- ユーザーの `magento-cloud user:delete` 削除
- プロジェクトユーザーの `magento-cloud user:list [users]` リスト
- ユーザーロールの `magento-cloud user:role` 表示または変更
- プロジェクトのユーザーの役割の `magento-cloud user:update` 更新

次の例では、`magento-cloud` CLI を使用して、ユーザーの追加、役割の設定、プロジェクトの割り当ての変更、ユーザーの役割の割り当てを行います。

**ユーザーを追加して役割を割り当てるには**:

1. `magento-cloud` CLI を使用してユーザーを追加します。

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >ユーザーにはAdobe IDが必要です。[&#x200B; 前提条件 &#x200B;](#add-users-and-manage-access) を参照してください。

1. プロンプトに従って、ユーザーのメールアドレスを指定し、プロジェクトおよび環境タイプの役割を設定し、ユーザーを追加します。

   > サンプルプロンプト

   ```
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   ユーザーを追加すると、Adobeから、クラウドインフラストラクチャプロジェクト上のAdobe Commerceにアクセスするための手順が記載されたメールが、指定されたアドレスに送信されます。

### ユーザーのプロジェクトの役割を表示する

```bash
magento-cloud user:get alice@example.com
```

>応答の例：

```
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 複数の環境へのユーザーの追加

ユーザーを `Production` 環境では `viewer` として、`Integration` 環境では `contributor` として追加するには：

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### ユーザー環境権限の更新

ユーザー環境の権限を更新して `Production` 環境で `admin` 用するには：

```bash
magento-cloud user:update alice@example.com -r production:a
```

## [!DNL Cloud Console] からのユーザーの管理

[[!DNL Cloud Console]](../../get-started/cloud-console.md) を使用して権限を追加し、_編集_ 機能を使用して既存のユーザーの権限を変更できます。

>[!IMPORTANT]
>
>ユーザーにはAdobe IDが必要です。[&#x200B; 前提条件 &#x200B;](#add-users-and-manage-access) を参照してください。

### プロジェクトにユーザーを追加する

1. [[!DNL Cloud Console]](https://console.adobecommerce.com/) にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. プロジェクトダッシュボードで、右上の設定アイコンをクリックします。

1. _プロジェクト設定_ の下で、「**[!UICONTROL Access]**」をクリックします。

1. _アクセス_ ビューで、「**[!UICONTROL Add]**」をクリックします。

1. _[!UICONTROL Add User]_&#x200B;フォームに入力します。

   - ユーザーのメールアドレスを入力します。

   - **[!UICONTROL Project admin]** – すべての設定および環境タイプに管理者権限を付与します。

   - **[!UICONTROL Environment types and permissions]** – 特定の環境タイプに対してアクセス・レベルと特定の権限レベルを付与します。 _アクセスなし_、_管理者_ （設定の変更、アクションの実行、結合コード）、_投稿者_ （プッシュコード）、_ビューア_ （表示のみ）。

   >[!TIP]
   >
   >どの環境でも、ユーザーを管理できるのは **プロジェクト管理者** のみです。 「**アクセス**」タブへのアクセス権をユーザーに付与するには、別の **プロジェクト管理者** または **アカウント所有者** がそのユーザーに **プロジェクト管理者** の役割を割り当てる必要があります。

1. 「**[!UICONTROL Add User]**」をクリックします。

   >[!IMPORTANT]
   >
   >ユーザーを追加しても、デプロイメントは自動的にはトリガーされません。

1. ユーザーを追加したら、すべての環境を再デプロイして変更を適用します。 ユーザーを追加しても、デプロイメントは自動的にはトリガーされません。 再デプロイメントは、ユーザーが SSH を使用して環境にアクセスしたり、管理者タスクを実行したりできるようにするための重要な手順です。

ユーザーを追加すると、Adobeから、クラウドインフラストラクチャプロジェクト上のAdobe Commerceにアクセスするための手順が記載されたメールが、指定されたアドレスに送信されます。

## ユーザー認証の要件

セキュリティを強化するために、Adobeは、クラウドインフラストラクチャプロジェクトのソースコードおよび環境のAdobe Commerceに SSH アクセスするために、2 要素認証（TFA）を必要とする、プロジェクトレベルの多要素認証（MFA）の適用を提供します。 [SSH 用 MFA の有効化 &#x200B;](multi-factor-authentication.md) を参照してください。

クラウドインフラストラクチャプロジェクト上のAdobe Commerceで MFA 適用が有効になっている場合、そのプロジェクト内の環境に SSH アクセス権を持つすべてのユーザーは、クラウドインフラストラクチャアカウント上のAdobe Commerceで TFA を有効にする必要があります。 自動プロセスの場合、コマンドラインから認証するマシンユーザーと API トークンを作成できます。

クラウドプロジェクトにユーザーを追加した後、そのユーザーにアカウントセキュリティ設定を確認するように依頼し、必要に応じて次のセキュリティ設定を追加します。

- **TFA の有効化**：二要素認証を構成することにより、セキュリティとコンプライアンスの標準を満たします。 [MFA 強制 &#x200B;](multi-factor-authentication.md) が設定されたプロジェクトにアクセスするには、SSH を使用するアカウントで TFA が必要です。

- **SSH キーを有効にする** - クラウドインフラストラクチャー上のAdobe Commerceのソースコードリポジトリーにアクセスする必要があるユーザーは、自分のアカウントで SSH キーを有効にする必要があります。 [&#x200B; 安全な接続 &#x200B;](../development/secure-connections.md) を参照してください。

- **API トークンの作成** - ユーザーは、環境への SSH アクセスに使用される API トークンを生成する必要があります。 自動プロセスの認証ワークフローを有効にするには、トークンが必要です。

  MFA 適用が有効なプロジェクトでは、API トークンを使用して、自動アカウントからの SSH アクセスリクエストを認証する必要があります。 トークンを使用すると、TFA が必要な認証ワークフローを自動プロセスでバイパスできます。

### クラウドアカウントの TFA の有効化

クラウドインフラストラクチャー上のAdobe Commerceは、次のいずれかのアプリケーションを使用して TFA をサポートします。

- [Google認証（Android/iPhone） &#x200B;](https://support.google.com/accounts/answer/1066447?hl=en)
- [&#x200B; 作成者（Android/iPhone） &#x200B;](https://authy.com/features/)
- [FreeOTP （Android） &#x200B;](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth 認証（Firefox OS、デスクトップなど） &#x200B;](https://github.com/gbraad-apps/gauth)

認証アプリケーションをインストールして TFA を有効にする手順については、[!DNL Cloud Console] の _アカウント設定_ ページを参照してください。

**ユーザーアカウントで TFA を有効にするには**:

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインします。

1. 右上のアカウントメニューで、「**[!UICONTROL My Profile]**」をクリックします。

1. 「_セキュリティ_」タブで、「**[!UICONTROL Set up application]**」をクリックします。

1. モバイルデバイスに承認済みの認証アプリケーションがない場合は、リンクされている手順に従ってインストールします。

1. クラウドインフラストラクチャアカウント上のAdobe Commerceを認証アプリケーションに追加します。

   - モバイルデバイスで、認証アプリケーションを開きます。 次に、設定コードをアプリケーションに追加します。

   - [!UICONTROL **[!UICONTROL TFA set up - Application]**] ページの **[!UICONTROL Application verification code]** フィールドに、モバイルデバイスの TFA コードを入力します。

   - 「**[!UICONTROL Verify and save]**」をクリックします。

     コードが有効な場合、Adobeはアカウントのメールアドレスに、アカウントに TFA が設定されたことを確認する通知を送信します。

1. オプション。 _信頼済みブラウザー_ 設定を有効にして、認証コードをブラウザーに 30 日間キャッシュします。

   この設定により、プロジェクトへのログイン時に発生する認証の問題を軽減できます。

1. **保存** または **スキップ** をクリックします。

1. リカバリ・コードを保存します。

   - _TFA 設定 – 回復_ コードページで、回復コードをコピーして保存します。これにより、モバイルデバイスや認証アプリケーションにアクセスできない場合に、Cloud Infrastructure プロジェクトのAdobe Commerceにログインできます。

   - デバイスまたは認証アプリケーションへのアクセスが失われた場合に備えて、回復コードを別の場所にコピーするか、書き留めます。

   - 「**保存**」をクリックしてコードをアカウントに保存し、アカウントのセキュリティ設定からコードを表示および管理できるようにします。

     >[!WARNING]
     >
     >TFA のアカウントへのアクセス権を失い、復旧コード リストを持っていない場合は、プロジェクト管理者に連絡するか、[Adobe Commerce サポート チケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) して TFA アプリケーションをリセットする必要があります。

1. TFA 設定が完了したら、「**保存**」をクリックしてアカウントを更新します。

1. 現在のセッションを TFA で認証します。

   - アカウントからログアウトします。
   - ユーザー名とパスワードを使用してログインします。
   - プロンプトが表示されたら、お使いのモバイルデバイスの authenticator アプリケーションから `accounts.magento.cloud` エントリの TFA コードを入力します。

### TFA 構成および回復コードの管理

_マイプロファイル_ ページの _セキュリティ_ セクションで、クラウドインフラストラクチャアカウント上のAdobe Commerceの TFA 設定を管理できます。

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインします。

1. 右上のアカウントメニューで、「**[!UICONTROL My Profile]**」をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL Security]**」タブをクリックします。

1. 使用可能なリンクを使用して、クラウドインフラストラクチャアカウントのAdobe Commerceの TFA 設定を更新します。

   - TFA を無効にする
   - 認証アプリケーションのリセット
   - 信頼済みブラウザーの追加または削除
   - アカウントの TFA 回復コードを表示または更新します

### API トークンの作成

API トークンを OAuth 2 アクセストークンと交換し、リクエストの認証に使用できます。

MFA 適用が有効になっているプロジェクトでは、マシンユーザーと自動プロセス用の SSH アクセスを有効にするには、API トークンが必要です。

>[!IMPORTANT]
>
>アカウントのProtect API トークン値。 コードサンプル、画面のキャプチャ、安全でないクライアントサーバー通信で値を公開しないでください。 また、公開リポジトリに格納されているソースコードで値を公開しないでください。

**API トークンを作成するには**:

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインします。

1. 右上のアカウントメニューで、「**[!UICONTROL My Profile]**」をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL API tokens]**」タブをクリックします。

1. 「**[!UICONTROL Create API token]**」をクリックし、名前を入力します。例えば、マシンユーザーに一致する名前や、API トークンを使用する自動プロセスを指定します。

   ![API トークン &#x200B;](../../assets/api-token-name.png)

1. 「**[!UICONTROL Create API token]**」をクリックします。
