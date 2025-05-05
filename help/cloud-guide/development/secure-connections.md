---
title: 安全な接続
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceに SSH キーを適用し、リモート環境にログインする方法を説明します。
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: c25e5b74ae8105995107860246ecb9ba45910bb1
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---

# リモート環境への安全な接続

Secure Shell （SSH）は、リモートサーバーおよびシステムに安全にログインするために使用される共通プロトコルです。 SSH を使用してリモート環境にアクセスし、Adobe Commerce アプリケーションを管理してリモート環境ログにアクセスできます。 Adobeは、SSH 公開鍵を使用したセキュア FTP （sFTP）接続のみをサポートしています。 FTP 接続はサポートされていません。

## SSH キーペアの生成

プロジェクトのソースコードと環境にアクセスする必要があるすべてのマシンとワークスペースに、SSH キーペアを作成します。 SSH キーを使用すると、GitHub に接続してソースコードを管理したり、クラウドサーバーに接続したりできます。ユーザー名やパスワードを常に指定する必要はありません。 SSH キーペアの作成について詳しくは、[SSH を使用した GitHub への接続 ](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) を参照してください。

- _公開鍵_ は、サイト、SSH および sFTP へのアクセスに安全に使用できます。
- _秘密鍵_ は、ワークステーションでは非公開のままです。

>[!CAUTION]
>
>**秘密鍵は共有しないでください。** チケットに追加したり、チャットにコピーしたり、メールに添付したりしないでください。

## SSH 公開鍵をアカウントに追加

クラウドインフラストラクチャアカウント上でAdobe Commerceに SSH 公開鍵を追加または更新したら、アカウントに [ すべてのアクティブな環境を再デプロイ ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy) して鍵をインストールします。

Cloud CLI または [!DNL Cloud Console] のいずれかの方法を使用して、アカウントに SSH キーを追加できます。

>[!BEGINTABS]

>[!TAB CLI]

### Cloud CLI を使用して SSH キーを追加する

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. プロジェクトへのログイン

   ```bash
   magento-cloud login
   ```

1. 公開鍵を追加します。

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Cloud CLI コマンド `ssh-key:list` および `ssh-key:delete` を使用して、SSH キーの一覧表示および削除を行うことができます。

>[!TAB  コンソール ]

### [!DNL Cloud Console] を使用して SSH キーを追加する

**新規プロジェクトに SSH キーを追加するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。

1. 「**[!UICONTROL No SSH key]**」をクリックします。 このアイコンはコマンドフィールドの右側にあり、プロジェクトに SSH キーが含まれていない場合に表示されます。

1. SSH 公開鍵の内容をコピーして「**公開鍵**」フィールドに貼り付けます。

1. 残りのプロンプトに従います。

**クラウドプロファイルに SSH キーを追加するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。

1. 右上のアカウントメニューで、「**マイプロファイル**」をクリックします。

1. _SSH キー_ ビューで、「**公開鍵を追加**」をクリックします。

1. _SSH キーを追加_ フォームで、キーに **タイトル** を入力し、「**キー**」フィールドに SSH 公開鍵を貼り付けます。

1. **保存** をクリックします。

>[!ENDTABS]

## リモート環境への接続

`magento-cloud` CLI または SSH コマンドを使用して、リモート環境に接続できます。 `magento-cloud` の CLI コマンドは、Starter および Pro 統合環境でのみ使用できます。

### Cloud CLI の使用

**リモート統合環境にログインするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. そのプロジェクト内の環境をリストします。

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### SSH コマンドの使用

この [!DNL Cloud Console] には、各環境の Web および SSH アクセス・コマンドのリストが含まれています。

**SSH コマンドをコピーするには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境を選択します。

1. 「**[!UICONTROL SSH]**」をクリックします。

1. 「_SSH_」タブで、「コピー」ボタンをクリックして、SSH コマンド全体をクリップボードにコピーします。

1. ターミナルを開き、SSH コマンドを貼り付けて接続を作成します。

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>ステージング環境および実稼動環境では、SSH コマンドは次のようになります。
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

クラウドインフラストラクチャー上のAdobe Commerceでは、SSH 認証を使用した sFTP （セキュア FTP）を使用した環境へのアクセスをサポートしています。 sFTP 用の SSH キー認証をサポートするクライアントを使用し、SSH 公開キーを使用します。 SSH 公開鍵をターゲット環境に追加する必要があります。 スターター環境および Pro 統合環境の場合は、[ を使用して追加  [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface) できます。

読み取り専用の sFTP 接続はサポートされていま _ん_。sFTP アクセスは、デフォルトで _書き込み_ 権限で提供されます。

sFTP の設定時には、SSH アクセス環境のコマンドから次の情報を使用します：`<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **ユーザー名**:SSH アクセス先にある `@` より前のすべてのコンテンツ。
- **パスワード**:sFTP のパスワードは必要ありません。 sFTP アクセスでは、SSH キー認証を使用します。
- **ホスト**:SSH アクセス権での `@` 以降のすべてのコンテンツ。
- **ポート**:22 （デフォルトの SSH ポート）。
- **SSH** 秘密鍵：必要に応じて、秘密鍵の場所を sFTP クライアントに指定します。 デフォルトでは、秘密鍵は `~/.ssh` ディレクトリに保存されます。

クライアントによっては、sFTP の SSH 認証を完了するために、追加のオプションが必要になる場合があります。 選択したクライアントのドキュメントを確認します。

**スターター環境と Pro 統合環境** の場合は、[ 特定のディレクトリにアクセスするための `mount`](../application/properties.md#mounts) ールを追加する」ことも検討してください。 `.magento.app.yaml` ファイルにマウントを追加します。 書き込み可能なディレクトリのリストについては、[ プロジェクト構造 ](../project/file-structure.md) を参照してください。 このマウントポイントは、これらの環境でのみ機能します。

**Pro ステージング環境および実稼動環境** の場合、環境に SSH アクセスできない場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、sFTP アクセスおよび特定のフォルダー（`pub/media` など）にアクセスするためのマウントポイントをリクエストする必要があります。

>[!NOTE]
>ステージング環境および実稼動環境で、sFTP 接続が **クラウドプロジェクトに追加**&#x200B;[ する必要がない _汎用_ ユーザー用の場合は、](../project/user-access.md) 公開 **キーを添付した [Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)** する必要があります。 **秘密の SSH キーを入力しないでください。**

## SSH トンネリング

SSH トンネリングを使用すると、ローカル開発環境からサービスに対して、サービスがローカルであるかのように接続できます。 トンネリングを行う前に、[SSH](#add-an-ssh-public-key-to-your-account) を設定します。

ターミナルアプリケーションを使用してログインし、コマンドを発行します。

```bash
magento-cloud login
```

を使用してトンネルが開いているかどうかを確認します。

```bash
magento-cloud tunnel:list
```

トンネルを構築するには、[ アプリケーション名 ](../application/properties.md#name) を知っている必要があります。 アプリケーション名は、CLI を使用して確認できます。

```bash
magento-cloud apps
```

### SSH トンネルのセットアップ

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

たとえば、`mymagento` という名前のアプリケーションを使用して、プロジェクトの `sprint5` ブランチへのトンネルを開くには、次のように入力します

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

応答の例：

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**トンネルに関する情報を表示するには**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### サービスへの接続

SSH トンネルを確立すると、ローカルで実行されているかのようにサービスに接続できます。 例えば、データベースに接続するには、次のコマンドを使用します。

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
