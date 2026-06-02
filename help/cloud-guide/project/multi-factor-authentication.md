---
title: SSH アクセスの多要素認証を有効にする
description: クラウドインフラストラクチャ環境でAdobe CommerceにSSHでアクセスするための認証要件を管理する方法について説明します。
feature: Cloud, Security
topic: Security
exl-id: 90458fa8-42b0-4825-948e-56ef7884eb82
TQID: https://experienceleague.adobe.com/KWGl-ZyF5aKZ-XxOOmL85ip8arBeH-G1pN5ckUdAqNw
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: bd989d82-1e15-4534-88db-f1f51dd77ffaid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1080
ht-degree: 0%

---

# SSH アクセスの多要素認証を有効にする

セキュリティを強化するために、Adobe Commerce オンクラウドインフラストラクチャでは、クラウド環境へのSSH アクセスに対する認証要件を管理するための多要素認証（MFA）の適用が提供されます。

プロジェクトでMFAが有効になっている場合、SSH アクセスを持つすべてのユーザーアカウントには、環境にアクセスするために2要素認証（TFA） コードまたはAPI トークンとSSH証明書のいずれかが必要です。

>[!NOTE]
>
>クラウドプロジェクトでは、デフォルトでMFAは有効になっていません。 Adobe Commerce オンクラウド インフラストラクチャ プロジェクトのアカウントオーナーは、これを有効にするには、[Adobe Commerce サポートチケットを送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)する必要があります。 MFAが有効になっている場合、プロジェクト環境へのSSH アクセスのために、すべてのユーザーがAdobe Commerce on cloud infrastructure アカウントで2要素認証（TFA）を有効にする必要があります。

## SSH アクセス用の証明書

MFAでは、Adobe Cloud Certifier APIによって生成された短期間有効なSSH証明書を使用して、OAUTH アクセストークンを交換できます。 ユーザーが管理者またはコントリビューターの役割、有効なSSH キー、有効なTFA コードまたはAPI トークンを持っている場合、Adobe Commerce オンクラウドインフラストラクチャは、これらの資格情報を使用して一時的なSSH証明書を生成します。 証明書の有効期限は1時間に設定されますが、現在のセッション中に自動的に更新されます。

MFAでプロジェクトにログインした後、ユーザーは`magento-cloud` CLIを使用してSSH証明書を生成する必要があります。

```bash
magento-cloud ssh-cert:load
```

`ssh-cert:load` コマンドはSSH証明書を生成し、ローカル ユーザーのSSH エージェントにインストールします。

### ログイン時に証明書を自動的に生成

`magento-cloud` CLIに対する認証時に、SSH証明書を自動的に生成するようにローカル環境を設定できます。

**SSH証明書の自動生成を`magento-cloud` CLI設定**&#x200B;に追加するには：

1. ローカル ワークステーションで、`config.yaml`という名前のファイルが存在しない場合は、ホーム ディレクトリの`.magento-cloud` フォルダーに作成します。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 次の設定を`config.yaml` ファイルに追加します。

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. `magento-cloud` CLIを使用して、再認証を行います。

   >ログアウト：

   ```bash
   magento-cloud logout
   ```

   >ログイン：

   ```bash
   magento-cloud login
   ```

   >応答に従います。

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

## TFAでSSHを使用して環境に接続する

プロジェクトでMFAが有効になっている場合、SSHを使用してリモート環境に接続する前に、アカウントでTFAを有効にする必要があります。 「[TFAを有効にする](user-access.md#enable-tfa-for-cloud-accounts)」を参照してください。

>[!BEGINSHADEBOX]

**前提条件：**

MFAの適用が有効になっているプロジェクトの場合、SSH アクセスには次の権限とアカウント設定が必要です。

- [環境への管理者またはコントリビューターのアクセス](user-access.md)
- [アカウントで設定されたSSH アクセスキー](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [アカウントでTFAを有効にする](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**TFA ユーザーアカウント資格情報を使用してSSHを使用して接続するには**:

1. アカウント ](https://console.adobecommerce.com)の[にログインします。

1. ローカル ワークステーションで、`magento-cloud` CLIを使用してSSH証明書を生成します。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 回答サンプル：

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

1. SSHを使用してリモート環境に接続します。

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

## TFAでSSHを使用してソースコードを管理する

クラウドインフラストラクチャプロジェクトでAdobe Commerceのソースコードを管理する場合、SSHを使用してプロジェクトのGit リポジトリに対する認証を行います。 プロジェクトでMFAの適用が有効になっている場合は、Git リポジトリを使用してコマンドライン操作を実行する前に、SSH証明書を生成する必要があります。

**TFA ユーザーアカウント資格情報を使用してSSHを使用して接続するには**:

1. [ アカウント ](https://console.adobecommerce.com)にログインし、TFAを使用して認証します。

   >[!NOTE]
   >
   >アカウントでTFAを有効にしていない場合は、有効にする必要があります。 「[ クラウドアカウントでTFAを有効にする](user-access.md#enable-tfa-for-cloud-accounts)」を参照してください。

1. ローカル ワークステーションで、`magento-cloud` CLIを使用してSSH証明書を生成します。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 回答サンプル：

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

1. プロジェクト環境のGit リポジトリを複製します。

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > 回答サンプル：

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## API トークンを使用したSSHを使用した環境への接続

プロジェクトでMFAが有効になっている場合、クラウド環境へのSSH アクセスを必要とする自動プロセスには、API トークンが必要です。 プロジェクトの管理者またはコントリビューターのアクセス権を持つAdobe Commerce on cloud インフラストラクチャアカウントからトークンを生成できます。

API トークンを使用した認証を行うには、引き続きSSH証明書を生成する必要があります。 自動化されたプロセスでは、SSH証明書の生成も自動化する必要があります。

>[!BEGINSHADEBOX]

**前提条件：**

- [Adobe Commerce on cloud infrastructure環境への管理者またはコントリビューターのアクセス](user-access.md)
- [有効なAPI トークンがアカウントで利用可能](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**API トークン資格情報を使用してSSHを使用して接続するには**:

1. API キー認証を使用してCloud プロジェクトにログインします。

   ```bash
   magento-cloud auth:api-token
   ```

1. プロンプトで、有効なAPI トークンの値を入力します。

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### 例：自動SSH スクリプト

API トークンを保存するには、2つのオプションがあります。

>[!NOTE]
>
>API トークンが保存されている場合、`magento-cloud` CLIは自動的に認証され、`magento-cloud login` コマンドを実行する必要はありません。

**オプション 1**: API トークンを保存する環境変数を作成します

bash_profileへのトークンの書き込み

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**オプション 2**: トークンを`config.yaml` ファイルに追加します

1. ローカル ワークステーションで、`config.yaml`という名前のファイルが存在しない場合は、ホーム ディレクトリの`.magento-cloud` フォルダーに作成します。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 次の設定を`config.yaml` ファイルに追加します。

   ```yaml
   api:
      token: <your api token>
   ```

>Bash スクリプトのサンプル

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## トラブルシューティング

次の情報を使用して、`access requires MFA`や`permission denied`などの認証エラーによるSSH接続要求の失敗を解決します。

### リクエストは有効な証明書を提供していません

リクエストで有効な証明書が提供されない場合は、次のようなメッセージが表示されます。

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

接続の問題を解決するには、次のトラブルシューティング手順を試してください。

- アカウント TFA設定の確認
- もう一度認証し、証明書をリロードします

**TFAの設定と認証を確認するには**:

1. アカウント ](https://console.adobecommerce.com)の[にログインします。

1. 右上のアカウントメニューで、**[!UICONTROL My Profile]**&#x200B;をクリックします。

1. _マイプロファイル_ ページで、「**[!UICONTROL Security]**」タブをクリックします。

   TFAが有効になっている場合、「セキュリティ」セクションには、TFA設定を管理するためのオプションが用意されています。

1. TFAが設定されていない場合は、**[!UICONTROL Set up application]**&#x200B;をクリックし、指示に従って有効にします。 「[TFAを有効にする](user-access.md#enable-tfa-for-cloud-accounts)」を参照してください。

1. TFAが設定されている場合は、認証を再試行してください。

**SSH証明書を認証して再読み込みするには**:

1. `magento-cloud` CLIを使用して、再認証を行います。

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. SSH証明書をリロードします。

   ```bash
   magento-cloud ssh-cert:load
   ```

### 権限がありません

SSH キーが見つからないか無効な場合、SSH接続要求は`Permission denied (publickey)` エラーを返します。

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

この問題を解決するには、現在のセッションにSSH キーを追加するか、SSH設定ファイルを更新してSSH キーを自動的に読み込みます。 「[公開SSH キーを追加](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)」を参照してください。

### MFAなしでプロジェクトにアクセスできない

多要素認証（MFA）が有効になっているプロジェクトに対して認証を行うと、MFAを必要としない他のプロジェクトに接続する際に次のエラーが表示される場合があります。

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

回答サンプル：

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

SSH証明書の生成中、`magento-cloud` CLIはローカル環境に追加のSSH キーを追加します。 ローカルのSSH設定にプロジェクトアクセス用のSSH キーが含まれていない場合、このキーはデフォルトで使用されます。

**SSH キーをローカル設定に追加するには**:

1. `config` ファイルが存在しない場合は作成します。

   ```bash
   touch ~/.ssh/config
   ```

1. `IdentityFile`設定を追加します。

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>複数の`IdentityFile` エントリを設定に追加することで、複数のSSH キーを指定できます。
