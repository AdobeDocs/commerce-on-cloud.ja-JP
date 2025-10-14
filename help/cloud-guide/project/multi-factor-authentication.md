---
title: SSH アクセスの多要素認証を有効にする
description: クラウドインフラストラクチャ環境でAdobe Commerceに SSH アクセスするための認証要件を管理する方法について説明します。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# SSH アクセスの多要素認証を有効にする

セキュリティを強化するために、Adobe Commerce on Cloud Infrastructure では、クラウド環境への SSH アクセスの認証要件を管理するための多要素認証（MFA）の適用を提供しています。

プロジェクトで MFA が有効な場合、SSH アクセスを持つすべてのユーザーアカウントは、環境にアクセスするために、二要素認証（TFA）コードまたは API トークンと SSH 証明書のいずれかを必要とします。

>[!NOTE]
>
>クラウドプロジェクトでは、MFA はデフォルトでは有効になっていません。 Adobe Commerce on cloud infrastructure プロジェクトのアカウント所有者は、有効にするために [Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) する必要があります。 MFA が有効な場合、プロジェクト環境に SSH でアクセスするには、すべてのユーザーがAdobe Commerce on cloud infrastructure アカウントで二要素認証（TFA）を有効にする必要があります。

## SSH アクセス用の証明書

MFA を使用すると、Adobe Cloud Certifier API で生成された短期間有効な SSH 証明書と OAUTH アクセストークンを交換できます。 ユーザーが管理者または投稿者のロール、有効な SSH キー、有効な TFA コードまたは API トークンを持っている場合、クラウドインフラストラクチャー上のAdobe Commerceはこれらの資格情報を使用して一時的な SSH 証明書を生成します。 証明書の有効期限は 1 時間に設定されていますが、現在のセッション中に自動更新されます。

MFA を使用してプロジェクトにログインした後、ユーザーは `magento-cloud` CLI を使用して SSH 証明書を生成する必要があります。

```bash
magento-cloud ssh-cert:load
```

`ssh-cert:load` コマンドは、SSH 証明書を生成して、ローカル ユーザーの SSH エージェントにインストールします。

### ログイン時に証明書を自動生成

`magento-cloud` CLI への認証時に SSH 証明書を自動的に生成するように、ローカル環境を設定することができます。

**SSH 証明書の自動生成を `magento-cloud` CLI 設定に追加するには**:

1. ローカルワークステーションで、`config.yaml` という名前のファイルをホームディレクトリの `.magento-cloud` フォルダー（存在しない場合）に作成します。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 次の設定を `config.yaml` ファイルに追加します。

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. `magento-cloud` CLI を使用して再度認証します。

   >ログアウト：

   ```bash
   magento-cloud logout
   ```

   >ログイン：

   ```bash
   magento-cloud login
   ```

   >次の応答に従います。

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## SSH と TFA を使用した環境への接続

プロジェクトで MFA を有効にする場合、SSH を使用してリモート環境に接続するには、アカウントで TFA を有効にしておく必要があります。 [TFA を有効にする &#x200B;](user-access.md#enable-tfa-for-cloud-accounts) を参照してください。

>[!BEGINSHADEBOX]

**前提条件：**

MFA 適用が有効なプロジェクトの場合、SSH アクセスには次の権限とアカウント設定が必要です。

- [環境への管理者または投稿者のアクセス](user-access.md)
- [アカウントに設定されている SSH アクセスキー](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [アカウントで TFA が有効になっています](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**TFA ユーザーアカウント資格情報を使用して SSH を使用して接続するには**:

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインします。

1. ローカル・ワークステーションで、`magento-cloud` CLI を使用して SSH 証明書を生成します。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 応答の例：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. SSH を使用してリモート環境に接続します。

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## SSH と TFA を使用したソースコードの管理

クラウドインフラストラクチャプロジェクト上のAdobe Commerceのソースコードを管理する場合は、SSH を使用してプロジェクトの Git リポジトリに対する認証を行います。 プロジェクトで MFA 適用が有効になっている場合は、Git リポジトリを使用してコマンドライン操作を実行する前に、SSH 証明書を生成する必要があります。

**TFA ユーザーアカウント資格情報を使用して SSH を使用して接続するには**:

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインし、TFA を使用して認証します。

   >[!NOTE]
   >
   >アカウントで TFA が有効になっていない場合は、有効にする必要があります。 [&#x200B; クラウドアカウントで TFA を有効にする &#x200B;](user-access.md#enable-tfa-for-cloud-accounts) を参照してください。

1. ローカル・ワークステーションで、`magento-cloud` CLI を使用して SSH 証明書を生成します。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 応答の例：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. プロジェクト環境の Git リポジトリのクローンを作成します。

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > 応答の例：

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## SSH と API トークンを使用した環境への接続

プロジェクトで MFA が有効な場合、クラウド環境への SSH アクセスを必要とする自動プロセスには API トークンが必要です。 プロジェクトに対する管理者または投稿者のアクセス権を持つ、クラウドインフラストラクチャアカウント上のAdobe Commerceからトークンを生成できます。

API トークンによる認証には、引き続き SSH 証明書の生成が必要です。 自動プロセスでは、SSH 証明書の生成も自動化する必要があります。

>[!BEGINSHADEBOX]

**前提条件：**

- [クラウドインフラストラクチャ環境でのAdobe Commerceへの管理者または投稿者のアクセス](user-access.md)
- [アカウントで有効な API トークンを入手できます](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**SSH を使用して API トークン資格情報で接続するには**:

1. API キー認証を使用して Cloud プロジェクトにログインします。

   ```bash
   magento-cloud auth:api-token
   ```

1. プロンプトで、有効な API トークンの値を入力します。

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### 例：自動 SSH スクリプト

API トークンの保存には 2 つのオプションがあります。

>[!NOTE]
>
>API トークンが格納されている場合は、`magento-cloud` CLI が自動的に認証され、`magento-cloud login` コマンドを実行する必要はありません。

**オプション 1**:API トークンを格納する環境変数を作成する

bash_profile へのトークンの書き込み

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**オプション 2**:`config.yaml` ファイルへのトークンの追加

1. ローカルワークステーションで、`config.yaml` という名前のファイルをホームディレクトリの `.magento-cloud` フォルダー（存在しない場合）に作成します。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 次の設定を `config.yaml` ファイルに追加します。

   ```yaml
   api:
      token: <your api token>
   ```

>bash スクリプトのサンプル

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## トラブルシューティング

次の情報を使用して、`access requires MFA` や `permission denied` などの認証エラーが原因で発生した SSH 接続要求エラーを解決します。

### 要求は有効な証明書を提供していません

リクエストで有効な証明書が指定されない場合は、次のようなメッセージが表示されます。

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

接続の問題を解決するには、次のトラブルシューティング手順を試してください。

- アカウント TFA 設定の検証
- 再度認証し、証明書を再読み込みします

**TFA 設定と認証を確認するには**:

1. [&#x200B; アカウント &#x200B;](https://console.adobecommerce.com) にログインします。

1. 右上のアカウントメニューで、「**[!UICONTROL My Profile]**」をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL Security]**」タブをクリックします。

   TFA が有効になっている場合は、TFA の設定を管理するためのオプションが [ セキュリティ ] セクションに表示されます。

1. TFA が設定されていない場合は、**[!UICONTROL Set up application]** をクリックし、指示に従って有効にします。 [TFA を有効にする &#x200B;](user-access.md#enable-tfa-for-cloud-accounts) を参照してください。

1. TFA が設定されている場合は、認証を再試行します。

**SSH 証明書を認証して再読み込みするには**:

1. `magento-cloud` CLI を使用して再度認証します。

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. SSH 証明書を再読み込みします。

   ```bash
   magento-cloud ssh-cert:load
   ```

### 権限が拒否されました

SSH キーがないか無効な場合、SSH 接続リクエストは `Permission denied (publickey)` エラーを返します。

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

この問題を修正するには、現在のセッションに SSH キーを追加するか、SSH 設定ファイルを更新して SSH キーを自動的に読み込みます。 [SSH 公開鍵の追加 &#x200B;](../development/secure-connections.md#add-an-ssh-public-key-to-your-account) を参照してください。

### MFA のないプロジェクトにアクセスできません

多要素認証（MFA）が有効になっているプロジェクトに対して認証を行う場合、MFA を必要としない他のプロジェクトに接続すると、次のエラーが発生することがあります。

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

応答の例：

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

SSH 証明書の生成中に、`magento-cloud` CLI は追加の SSH キーをローカル環境に追加します。 ローカルの SSH 設定にプロジェクトアクセス用の SSH キーが含まれていない場合、このキーがデフォルトで使用されます。

**SSH キーをローカル設定に追加するには**:

1. `config` ファイルが存在しない場合は作成します。

   ```bash
   touch ~/.ssh/config
   ```

1. `IdentityFile` 設定を追加します。

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>設定に複数の `IdentityFile` エントリを追加することで、複数の SSH キーを指定できます。
