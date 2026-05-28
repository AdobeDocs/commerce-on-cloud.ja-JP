---
title: 安全な接続
description: Adobe Commerce on cloud インフラストラクチャプロジェクトにSSH キーを適用し、リモート環境にログインする方法について説明します。
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: 9c0b4bea11abb2ce5644556ab3dadd361f8ff449
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# リモート環境への安全な接続

Secure Shell （SSH）は、リモートサーバーやシステムに安全にログインするために使用される一般的なプロトコルです。 SSHを使用して、リモート環境にアクセスし、Adobe Commerce アプリケーションを管理したり、リモート環境のログにアクセスしたりできます。 Adobeでは、SSH公開鍵を使用したSecure FTP （sFTP）接続のみがサポートされます。 FTP接続はサポートされていません。

## SSH キーペアの生成

プロジェクトのソースコードと環境へのアクセスを必要とするすべてのマシンとワークスペースにSSH キーペアを作成します。 SSH キーを使用すると、GitHubに接続してソースコードを管理し、ユーザー名とパスワードを頻繁に入力することなくクラウドサーバーに接続できます。 SSH キーペアの作成について詳しくは、[SSHを使用したGitHubへの接続](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)を参照してください。

- _公開鍵_&#x200B;は、サイト、SSH、およびsFTPへのアクセスに安全に提供できます。
- _秘密鍵_&#x200B;はワークステーション上で非公開のままです。

>[!CAUTION]
>
>**秘密鍵を共有しないでください。** チケットに追加したり、チャットにコピーしたり、メールに添付したりしないでください。

## アカウントにSSH公開鍵を追加する

Adobe Commerce on cloud infrastructure アカウントにSSH公開鍵を追加または更新した後、アカウント上のすべてのアクティブな環境](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy)を[再デプロイして鍵をインストールします。

アカウントにSSH キーを追加するには、Cloud CLIまたは[!DNL Cloud Console]のいずれかの方法を使用します。

>[!BEGINTABS]

>[!TAB CLI]

### Cloud CLIを使用してSSH キーを追加する

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. プロジェクトにログインします。

   ```bash
   magento-cloud login
   ```

1. 公開鍵を追加します。

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Cloud CLI コマンド `ssh-key:list`と`ssh-key:delete`を使用して、SSH キーを一覧表示および削除できます。

>[!TAB  コンソール ]

### [!DNL Cloud Console]を使用してSSH キーを追加します

**新しいプロジェクトにSSH キーを追加するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. **[!UICONTROL No SSH key]**&#x200B;をクリックします。 このアイコンはコマンドフィールドの右側にあり、プロジェクトにSSH キーが含まれていない場合に表示されます。

1. 公開SSH キーの内容を&#x200B;**公開鍵** フィールドにコピーして貼り付けます。

1. 残りのプロンプトに従います。

**クラウドプロファイルにSSH キーを追加するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. 右上のアカウントメニューで、**マイプロファイル**&#x200B;をクリックします。

1. _SSH キー_ ビューで、**公開鍵を追加**&#x200B;をクリックします。

1. _SSH キー_ フォームで、キーに&#x200B;**タイトル**&#x200B;を入力し、公開SSH キーを&#x200B;**キー** フィールドに貼り付けます。

1. **保存**&#x200B;をクリックします。

>[!ENDTABS]

## リモート環境への接続

`magento-cloud` CLIまたはSSH コマンドを使用して、リモート環境に接続できます。 `magento-cloud` CLI コマンドは、スターター環境とPro統合環境でのみ使用できます。

### Cloud CLIの使用

**リモート統合環境にログインするには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. プロジェクト内の環境のリストを表示します。

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### SSH コマンドの使用

[!DNL Cloud Console]には、各環境のWeb アクセス コマンドとSSH アクセス コマンドのリストが含まれています。

**SSH コマンドをコピーするには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境を選択します。

1. **[!UICONTROL SSH]**&#x200B;をクリックします。

1. 「_SSH_」タブで、「コピー」ボタンをクリックして、完全なSSH コマンドをクリップボードにコピーします。

1. ターミナルを開き、SSH コマンドを貼り付けて接続を作成します。

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Pro ステージング環境と実稼動環境の場合、SSH コマンドは次のようになります。
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce クラウドインフラストラクチャでは、SSH認証を使用したsFTP （セキュア FTP）を使用した環境へのアクセスがサポートされています。 sFTPのSSH キー認証をサポートするクライアントを使用し、公開SSH キーを使用します。 公開SSH キーをターゲット環境に追加する必要があります。 スターター環境とPro統合環境の場合は、 [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface)経由で[追加できます。

読み取り専用のsFTP接続は&#x200B;_サポートされていません_。sFTP アクセスは、デフォルトで&#x200B;_書き込み_&#x200B;権限で提供されます。

sFTPを設定する場合は、SSH アクセス環境コマンドの情報を使用します：`<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **ユーザー名**: SSH アクセス先の`@`より前のすべてのコンテンツ。
- **パスワード**: sFTPのパスワードは必要ありません。 sFTP アクセスでは、SSH キー認証が使用されます。
- **ホスト**: SSH アクセスの`@`以降のすべてのコンテンツ。
- **ポート**: 22 （デフォルトのSSH ポート）。
- **SSH**&#x200B;秘密鍵：必要に応じて、秘密鍵の場所をsFTP クライアントに指定します。 デフォルトでは、秘密鍵は`~/.ssh` ディレクトリに保存されます。

クライアントによっては、sFTPのSSH認証を完了するために追加のオプションが必要になる場合があります。 選択したクライアントのドキュメントを確認します。

**スターター環境とPro統合環境**&#x200B;の場合は、特定のディレクトリへのアクセス用に[追加`mount`](../application/properties.md#mounts)を検討することもできます。 マウントを`.magento.app.yaml` ファイルに追加します。 書き込み可能なディレクトリの一覧については、[ プロジェクト構造](../project/file-structure.md)を参照してください。 このマウントポイントは、これらの環境でのみ機能します。

**Pro ステージング環境および実稼動環境**&#x200B;の場合、環境へのSSH アクセス権がない場合は、[sFTP アクセスをリクエストするためにAdobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信し、特定のフォルダーへのアクセス用のマウントポイント（例：`pub/media`）を送信する必要があります。

>[!NOTE]
>Pro ステージングおよび実稼動の場合、sFTP接続が&#x200B;**not**&#x200B;を[Cloud プロジェクト ](../project/user-access.md)に追加する必要がある&#x200B;_汎用_ ユーザーの場合、**公開鍵**&#x200B;を添付して[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。 **秘密のSSH キーを入力しないでください。**

## SSH トンネリング

サービスがローカルであるかのように、SSH トンネリングを使用してローカル開発環境からサービスに接続できます。 トンネリングを行う前に、[SSH](#add-an-ssh-public-key-to-your-account)を設定します。

ターミナルアプリケーションを使用してログインし、コマンドを発行します。

```bash
magento-cloud login
```

を使用してトンネルが開いているかどうかを確認します。

```bash
magento-cloud tunnel:list
```

トンネルを構築するには、[ アプリケーション名](../application/properties.md#name)を知っている必要があります。 CLIを使用してアプリケーション名を確認できます。

```bash
magento-cloud apps
```

### SSH トンネルの設定

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

例えば、`mymagento`という名前のアプリを含むプロジェクトの`sprint5` ブランチへのトンネルを開くには、次のように入力します

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

回答サンプル：

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

SSH トンネルを確立した後は、ローカルで実行しているかのようにサービスに接続できます。 例えば、データベースに接続するには、次のコマンドを使用します。

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```

#### MySQL資格情報の取得

環境変数`$MAGENTO_CLOUD_RELATIONSHIPS`の`database` プロパティからMySQL ログイン資格情報を取得します。 ローカル環境またはリモート環境で情報を取得する手順については、[ サービス関係](../services/services-yaml.md#service-relationships)を参照してください。
