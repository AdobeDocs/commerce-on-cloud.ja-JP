---
title: RabbitMQ サービスの設定
description: Adobe Commerce on cloud infrastructureのメッセージキューを管理するためにRabbitMQ サービスを有効にする方法を説明します。
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 3e84f813f8e7e46b6de03e2811b9cf097b6ceb47
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---

# [!DNL RabbitMQ] サービスの設定

[Message Queue Framework （MQF） ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)は、[ モジュール ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)がメッセージをキューに公開できるようにするAdobe Commerce内のシステムです。 また、メッセージを非同期的に受信する消費者も定義します。

MQFは[RabbitMQ](https://www.rabbitmq.com/)をメッセージングブローカーとして使用し、メッセージを送受信するためのスケーラブルなプラットフォームを提供します。 また、未配信のメッセージを保存するメカニズムも含まれています。 [!DNL RabbitMQ]は、Advanced Message Queuing Protocol （AMQP） 0.9.1仕様に基づいています。

>[!NOTE]
>
>Adobe Commerce on cloud infrastructureは、STOMP プロトコルを使用した代替メッセージキューサービスとして[ActiveMQ Artemis](activemq.md)もサポートしています。

>[!IMPORTANT]
>
>[!DNL RabbitMQ]のような既存のAMQP ベースのサービスを使用する場合は、クラウドインフラストラクチャ上のAdobe Commerceに依存して作成するのではなく、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境変数を使用してサイトに接続します。

{{service-instruction}}

**RabbitMQ**&#x200B;を有効にするには：

1. インストールされているRabbitMQ バージョンと共に、必要な名前、タイプ、ディスク値（MB単位）を`.magento/services.yaml` ファイルに追加します。

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. `.magento.app.yaml` ファイルの関係を設定します。

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [ サービス関係を確認します](services-yaml.md#service-relationships)。

{{service-change-tip}}

## デバッグ用にRabbitMQに接続

デバッグの目的では、次のいずれかの方法でサービスインスタンスに直接接続すると便利です。

- ローカル開発製品との連携
- アプリケーションからの接続
- PHP アプリケーションからの接続

### ローカル開発製品との連携

1. `magento-cloud` CLIにログインして、次のプロジェクトを実行します。

   ```bash
   magento-cloud login
   ```

1. RabbitMQがインストールされ、設定された環境を確認します。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSHを使用してクラウド環境に接続します。

   ```bash
   magento-cloud ssh
   ```

1. RabbitMQ接続の詳細とログイン資格情報を[$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)変数から取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、次のようなRabbitMQ情報を見つけます。

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. RabbitMQへのローカルポート転送を有効にします（プロジェクトがUS-3、EU-5、AP-3などの別の地域にある場合は、``us``を``us-3``/``eu-5``/``ap-3``に置き換えます）

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:15672`のRabbitMQ管理web インターフェイスにアクセスする例を次に示します。

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. セッションが開いている間に、MAGENTO_CLOUD_RELATIONSHIPS変数のポート番号、ユーザー名、パスワード情報を使用して`localhost:<portnumber>`に接続するように設定された、ローカルワークステーションから任意のRabbitMQ クライアントを開始できます。

### アプリケーションからの接続

アプリケーションで実行中のRabbitMQに接続するには、[amqp-utils](https://github.com/dougbarth/amqp-utils)などのクライアントを、`.magento.app.yaml` ファイルのプロジェクト依存関係としてインストールします。

以下に例を挙げます。

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

PHP コンテナにログインすると、キューの管理に使用できる`amqp-` コマンドを入力します。

### PHP アプリケーションからの接続

PHP アプリケーションを使用してRabbitMQに接続するには、ソースツリーにPHP ライブラリを追加します。

## [!DNL RabbitMQ] サービスのトラブルシューティング

「[Adobe Commerce CloudでRabbitMQに接続できません](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27688)」を参照してください。

## [!DNL RabbitMQ] サービスをアップグレードしています

>[!IMPORTANT]
>
>統合環境で[!DNL RabbitMQ]をアップグレードする場合は、バージョンをスキップしないでください。 サポートされているのは[ シーケンシャルアップグレード ](https://www.rabbitmq.com/docs/upgrade#rabbitmq-version-upgradability)のみです（例えば、3.8 → 3.9 → 3.10 → 3.11 → 3.12 → 3.13 → 4.0 → 4.1など）。各バージョンバンプは、クラウド環境のデプロイメントを成功させるために実際に対応する必要があります。
>
>一般的なサービスのアップグレード手順については、[ サービスバージョンの変更](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version)を参照してください。
