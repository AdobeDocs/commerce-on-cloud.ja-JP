---
title: ユーザーアクセスの管理
description: magento-cloud CLIまたはCloud Consoleを使用して、Adobe Commerceのクラウドインフラストラクチャプロジェクトおよび環境上でユーザーを追加し、役割を割り当てる方法を説明します。
role: Admin
feature: Cloud, Roles/Permissions
level: Beginner
short-description: ユーザーを追加し、Cloud ConsoleまたはCLIでプロジェクトと環境の役割を割り当てます。
last-substantial-update: 2026-06-11T00:00:00Z
topic: Security
exl-id: 953593de-f675-49fd-988f-f11306f67fbd
source-git-commit: de324897e87232393f20d95b2867d8a95605fa23
workflow-type: tm+mt
source-wordcount: '1690'
ht-degree: 0%

---

# ユーザーアクセスの管理

クラウドインフラストラクチャ上のAdobe Commerce プロジェクトでは、ロールベースのアクセスが使用されます。 プロジェクトレベルで利用できる役割は2つあります。

- **プロジェクト管理者** – すべてのプロジェクト環境への書き込みアクセス権を持ち、ユーザーの管理、コードのプッシュ、プロジェクト設定の更新を行うことができます。 （以前は&#x200B;**スーパー管理者**&#x200B;として知られていました）
- **プロジェクト ビューア** – すべてのプロジェクト環境への表示専用アクセス。

プロジェクトビューアは、どの環境でもタスクを実行することはできませんが、特定の環境タイプへの書き込みアクセス権をプロジェクトビューアに付与することはできます。

環境レベルのアクセスは、環境タイプ（実稼動、ステージング、開発）に基づきます。 ユーザー&#x200B;_viewer_&#x200B;に&#x200B;_development_&#x200B;環境への権限を付与すると、ユーザーはプロジェクト内の&#x200B;**all**&#x200B;開発環境を表示できます。 次の表に、各権限レベルに付与される機能を示します。

| 権限レベル | アクセス | SSH アクセス |
| ---------------- | ------ | :--------: |
| **管理者** | 設定の変更、プッシュコード、タスクの実行、親環境とのマージなどのブランチ管理などの管理者タスクを実行します | はい |
| **コントリビューター** | コードをプッシュして環境をブランチします。設定を変更したり、アクションを実行したりすることはできません | はい |
| **ビューアー** | 環境タイプへの表示専用アクセス | いいえ |
| **アクセスなし** | 環境タイプへのアクセス権がありません | いいえ |

{style="table-layout:auto"}

ユーザーを追加し、`magento-cloud` CLIまたは[!DNL Cloud Console]を使用して役割を割り当てることができます。

>[!PREREQUISITES]
>
>- Adobe IDの登録済みユーザー。 ユーザーは[Adobe アカウント &#x200B;](https://account.adobe.com)に登録し、Cloud プロジェクトに追加する前に[Cloud アカウント &#x200B;](https://console.adobecommerce.com)を初期化する必要があります。
>- **管理者**&#x200B;の役割を割り当てられたユーザーは、`magento-cloud` CLIでユーザーを管理できません。 **アカウント所有者**&#x200B;の役割を付与されたユーザーのみがユーザーを管理できます。

## CLIによるユーザーの管理

`magento-cloud` CLIを使用してユーザーを管理し、自動システムと統合します。

- `magento-cloud user:add` - ユーザーをプロジェクトに追加
- `magento-cloud user:delete` – ユーザーを削除
- `magento-cloud user:list [users]` リストのプロジェクト ユーザー
- `magento-cloud user:role` – ユーザーの役割を表示または変更します
- プロジェクトの`magento-cloud user:update` – 更新ユーザーロール

次の例では、`magento-cloud` CLIを使用して、ユーザーの追加、役割の設定、プロジェクトの割り当ての変更、ユーザーの役割の割り当てを行います。

**ユーザーを追加して役割を割り当てるには**:

1. `magento-cloud` CLIを使用してユーザーを追加します。

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >ユーザーはAdobe IDを持っている必要があります。 前提条件を確認します。

1. プロンプトに従います。ユーザーのメールアドレスを指定し、プロジェクトと環境タイプの役割を設定し、ユーザーを追加します。

   > サンプルプロンプト

   ```text
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

   ユーザーを追加すると、Adobeは指定したアドレスに電子メールを送信し、クラウドインフラストラクチャプロジェクトでAdobe Commerceにアクセスする手順を示します。

### ユーザーのプロジェクトの役割の表示

```bash
magento-cloud user:get alice@example.com
```

>回答サンプル：

```text
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 複数の環境へのユーザーの追加

ユーザーを`Production`環境の`viewer`として、および`Integration`環境の`contributor`として追加するには：

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### ユーザー環境の権限の更新

ユーザー環境権限を`Production`環境の`admin`に更新するには：

```bash
magento-cloud user:update alice@example.com -r production:a
```

## [!DNL Cloud Console]からのユーザーの管理

[[!DNL Cloud Console]](../../get-started/cloud-console.md)を使用して権限を追加し、_編集_&#x200B;機能を使用して既存のユーザーの権限を変更できます。

>[!IMPORTANT]
>
>ユーザーはAdobe IDを持っている必要があります。 前提条件を確認します。

### プロジェクトへのユーザーの追加

1. [[!DNL Cloud Console]](https://console.adobecommerce.com/)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. プロジェクトダッシュボードで、右上の設定アイコンをクリックします。

1. _プロジェクト設定_&#x200B;で、**[!UICONTROL Access]**&#x200B;をクリックします。

1. _アクセス_ ビューで、**[!UICONTROL Add]**&#x200B;をクリックします。

1. _[!UICONTROL Add User]_&#x200B;フォームに入力します。

   - ユーザーのメールアドレスを入力します。

   - **[!UICONTROL Project admin]** – すべての設定と環境タイプに管理者権限を付与します。

   - **[!UICONTROL Environment types and permissions]** – 特定の環境タイプに対するアクセス権限レベルと特定の権限レベルを付与します。 _アクセスなし_、_管理者_ （設定の変更、アクションの実行、結合コード）、_コントリビューター_ （プッシュコード）、または&#x200B;_ビューアー_ （表示のみ）。

   >[!TIP]
   >
   >任意の環境でユーザーを管理できるのは、**プロジェクト管理者**&#x200B;のみです。 ユーザーに&#x200B;**アクセス** タブへのアクセス権を付与するには、別の&#x200B;**プロジェクト管理者**&#x200B;または&#x200B;**アカウント所有者**&#x200B;がそのユーザーに&#x200B;**プロジェクト管理者**&#x200B;の役割を割り当てる必要があります。

1. **[!UICONTROL Add User]**&#x200B;をクリックします。

   >[!IMPORTANT]
   >
   >ユーザーを追加しても、デプロイメントは自動的にトリガーされません。

1. ユーザーを追加したら、すべての環境を再デプロイして変更を適用します。

   再デプロイメントにより、ユーザーがSSHを使用して環境にアクセスしたり、管理者タスクを実行したりできるようになります。

ユーザーを追加すると、Adobeは指定したアドレスに電子メールを送信し、クラウドインフラストラクチャプロジェクトでAdobe Commerceにアクセスする手順を示します。

### 招待状の状態

[!DNL Cloud Console]では、アカウントの初期化が完了する前に管理者が招待を送信できます。 その場合、アクセスリストには[!UICONTROL Invite pending]などのステータスを持つユーザーが表示されます。 オンボーディングが完了するまで、アクセスは完全にアクティブではありません。

コンソールとユーザーのアカウントの状態に応じて、ユーザーは次のいずれかの状態で表示されます。

- **[!UICONTROL Not invited]** — プロジェクト アクセス レコードが存在しません。
- **[!UICONTROL Invite pending]** – 招待が送信されましたが、アカウントの初期化または承認が不完全です。
- **[!UICONTROL Active]** — ユーザーはオンボーディングを完了し、アクティブなプロジェクトへのアクセス権を持っています。

>[!NOTE]
>
>[!DNL Cloud Console]には、[!DNL Legacy Cloud Console] （`https://<region-id>.magento.cloud/projects/<project_id>`）よりも招待状の状態が明示的に表示されます。 表示されるユーザーまたは招待エントリは、ユーザーがすべての環境にすぐにアクセスできるとは限りません。 SSH キーの設定やその他の伝搬手順が必要な場合があります。 [&#x200B; ユーザー認証要件](#user-authentication-requirements)を参照してください。

## ユーザー認証の要件

セキュリティを強化するために、Adobeでは、クラウドインフラストラクチャプロジェクトのソースコードと環境でAdobe CommerceにSSHでアクセスする際に2要素認証（TFA）を必要とする、プロジェクトレベルの多要素認証（MFA）の実施を提供しています。 「[SSHのMFAを有効にする](multi-factor-authentication.md)」を参照してください。

Adobe Commerce on cloud infrastructure プロジェクトでMFAの適用が有効になっている場合、そのプロジェクト内の環境へのSSH アクセス権を持つすべてのユーザーは、Adobe Commerce on cloud infrastructure アカウントでTFAを有効にする必要があります。 自動プロセスの場合は、コマンドラインから認証するマシンユーザーとAPI トークンを作成できます。

クラウドプロジェクトにユーザーを追加したら、ユーザーにアカウントセキュリティ設定を確認してもらい、必要に応じて次のセキュリティ設定を追加します。

- **TFA**&#x200B;を有効にする – 2要素認証を設定することで、セキュリティとコンプライアンスの基準を満たします。 [MFAの適用](multi-factor-authentication.md)で構成されたプロジェクトでは、SSHを使用してプロジェクトにアクセスするアカウントにTFAが必要です。

- **SSH キーを有効にする** – クラウドインフラストラクチャのソースコード リポジトリでAdobe Commerceへのアクセスを必要とするユーザーは、アカウントでSSH キーを有効にする必要があります。 [&#x200B; セキュア接続](../development/secure-connections.md)を参照してください。

- **API トークンを作成**：ユーザーは、環境へのSSH アクセスに使用されるAPI トークンを生成する必要があります。 自動化されたプロセスの認証ワークフローを有効にするには、トークンが必要です。

  MFAの適用が有効になっているプロジェクトでは、API トークンを使用して、自動化されたアカウントからのSSH アクセス要求を認証する必要があります。 トークンを使用すると、TFAを必要とする認証ワークフローを自動プロセスがバイパスできます。

### クラウドアカウントのTFAを有効にする

Adobe Commerce on cloud infrastructureは、次のいずれかのアプリケーションを使用してTFAをサポートしています。

- [Google Authenticator （Android/iPhone）](https://support.google.com/accounts/answer/1066447?hl=en)
- [Authy （Android/iPhone）](https://authy.com/features/)
- [FreeOTP （Android）](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth認証ツール （Firefox OS、デスクトップ、その他）](https://github.com/gbraad-apps/gauth)

認証アプリケーションのインストールとTFAの有効化に関する手順は、[!DNL Cloud Console]の&#x200B;_アカウント設定_ ページで確認できます。

**ユーザーアカウントでTFAを有効にするには**:

1. アカウント [&#128279;](https://console.adobecommerce.com)のにログインします。

1. 右上のアカウントメニューで、**[!UICONTROL My Profile]**&#x200B;をクリックします。

1. 「_セキュリティ_」タブで、「**[!UICONTROL Set up application]**」をクリックします。

1. モバイルデバイスに承認済みの認証アプリケーションがない場合は、リンクされた手順を使用してインストールします。

1. Adobe Commerce on cloud infrastructure アカウントを認証アプリケーションに追加します。

   - モバイルデバイスで、認証アプリケーションを開きます。 次に、アプリケーションにセットアップコードを追加します。

   - **[!UICONTROL TFA set up - Application]** ページで、**[!UICONTROL Application verification code]** フィールドにモバイルデバイスのTFA コードを入力します。

   - **[!UICONTROL Verify and save]**&#x200B;をクリックします。

     コードが有効な場合、Adobeは、アカウントにTFAが付与されたことを確認する通知をアカウントのメールアドレスに送信します。

1. オプション。 _信頼できるブラウザー_&#x200B;の設定を有効にして、認証コードをブラウザーに30日間キャッシュします。

   この設定により、プロジェクトのログイン中に発生する認証の課題を減らすことができます。

1. **保存**&#x200B;または&#x200B;**スキップ**&#x200B;をクリックします。

1. 回復用コードを保存します。

   - _TFA セットアップ - Recovery_ コード ページで、回復コードをコピーして保存し、モバイル デバイスまたは認証アプリケーションにアクセスできない場合にAdobe Commerce on cloud infrastructure プロジェクトにログインできるようにします。

   - デバイスや認証アプリケーションにアクセスできなくなった場合に備えて、回復用コードを別の場所にコピーするか、書き留めます。

   - 「**保存**」をクリックしてコードをアカウントに保存し、アカウントのセキュリティ設定からコードを表示および管理できるようにします。

     >[!WARNING]
     >
     >TFA アカウントへのアクセス権を失い、回復用コード リストを持っていない場合は、プロジェクト管理者に連絡するか、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信してTFA アプリケーションをリセットする必要があります。

1. TFA設定が完了したら、**保存**&#x200B;をクリックしてアカウントを更新します。

1. TFAで現在のセッションを認証します。

   - アカウントからログアウトします。
   - ユーザー名とパスワードでログインします。
   - プロンプトが表示されたら、モバイルデバイスの認証アプリケーションから`accounts.magento.cloud` エントリのTFA コードを入力します。

### TFA設定と回復用コードの管理

Adobe Commerce on cloud infrastructure アカウントのTFA設定は、_マイプロファイル_ ページの&#x200B;_セキュリティ_ セクションから管理できます。

1. アカウント [&#128279;](https://console.adobecommerce.com)のにログインします。

1. 右上のアカウントメニューで、**[!UICONTROL My Profile]**&#x200B;をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL Security]**」タブをクリックします。

1. 使用可能なリンクを使用して、Adobe Commerce on cloud infrastructure アカウントのTFA設定を更新します。

   - TFAを無効にする
   - 認証アプリケーションをリセット
   - 信頼できるブラウザーを追加または削除
   - アカウントのTFA回復用コードの表示または更新

### API トークンの作成

API トークンは、OAuth 2 アクセストークンと交換でき、その後、リクエストの認証に使用できます。

MFAの適用が有効になっているプロジェクトでは、マシンユーザーと自動プロセスのSSH アクセスを有効にするためのAPI トークンが必要です。

>[!IMPORTANT]
>
>アカウントのAPI トークン値を保護します。 コードサンプル、スクリーンキャプチャ、または安全でないクライアントサーバー間の通信では、値を公開しないでください。 また、パブリックリポジトリに格納されているソースコード内の値を公開しないでください。

**API トークンを作成するには**:

1. アカウント [&#128279;](https://console.adobecommerce.com)のにログインします。

1. 右上のアカウントメニューで、**[!UICONTROL My Profile]**&#x200B;をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL API tokens]**」タブをクリックします。

1. **[!UICONTROL Create API token]**&#x200B;をクリックして、名前を入力します。例えば、API トークンを使用するマシンユーザーまたは自動プロセスと一致する名前を指定します。

   「![Cloud Console API トークン」タブと「API トークン名を作成」フィールド &#x200B;](../../assets/api-token-name.png)

1. **[!UICONTROL Create API token]**&#x200B;をクリックします。

## このトピックの詳細ヘルプ

- [Adobe Commerce クラウドプロジェクトにユーザーを追加できません](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/unable-add-user-adobe-commerce-cloud-project) — ユーザーの追加時に発生するトラブルシューティングが失敗しました。
