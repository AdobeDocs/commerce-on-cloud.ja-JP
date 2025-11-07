---
title: RabbitMQ サービスの設定
description: RabbitMQ サービスを有効にして、クラウドインフラストラクチャー上のAdobe Commerceのメッセージキューを管理する方法について説明します。
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# サービス [!DNL RabbitMQ] 設定

[Message Queue Framework （MQF） &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) は、[&#x200B; モジュール &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module) がメッセージをキューに公開できるようにする、Adobe Commerce内のシステムです。 また、メッセージを非同期で受信するコンシューマーも定義します。

MQF では、メッセージの送受信に使用できるスケーラブルなプラットフォームを提供するメッセージングブローカーとして [RabbitMQ](https://www.rabbitmq.com/) を使用します。 また、未配信メッセージを保存するメカニズムも含まれています。 [!DNL RabbitMQ] は、Advanced Message Queuing Protocol （AMQP） 0.9.1 仕様に基づいています。

>[!NOTE]
>
>クラウドインフラストラクチャー上のAdobe Commerceでは、STOMP プロトコルを使用した代替メッセージキューサービスとして [ActiveMQ Artemis](activemq.md) もサポートしています。

>[!IMPORTANT]
>
>クラウドインフラストラクチャ上のAdobe Commerceを使用してサービスを作成するのではなく、[!DNL RabbitMQ] などの既存の AMQP ベースのサービスを使用する場合は、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 環境変数を使用してサービスをサイトに接続します。

{{service-instruction}}

**RabbitMQ を有効にするには**:

1. 必要な名前、タイプ、ディスク値（MB 単位）を、インストールされている RabbitMQ のバージョンと共に `.magento/services.yaml` ファイルに追加します。

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [&#x200B; サービスの関係を確認します &#x200B;](services-yaml.md#service-relationships)。

{{service-change-tip}}

## デバッグ用に RabbitMQ に接続

デバッグの目的では、次のいずれかの方法でサービスインスタンスに直接接続すると便利です。

- ローカル開発環境から接続する
- アプリケーションからの接続
- PHP アプリケーションからの接続

### ローカル開発環境から接続する

1. `magento-cloud` CLI にログインし、次のプロジェクトを実行します。

   ```bash
   magento-cloud login
   ```

1. RabbitMQ がインストールされ設定されている環境を確認します。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH を使用してクラウド環境に接続します。

   ```bash
   magento-cloud ssh
   ```

1. [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) 変数から RabbitMQ 接続の詳細とログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、RabbitMQ 情報を見つけます。例：

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

1. RabbitMQ へのローカルポート転送を有効にします（プロジェクトが US-3、EU-5、AP-3 などの別の地域にある場合は、``us-3`` を ``eu-5``/``ap-3``/``us`` に置き換えます）。

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:15672` の RabbitMQ 管理 Web インターフェイスにアクセスする例を以下に示します。

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. セッションが開いている間は、選択した RabbitMQ クライアントをローカルワークステーションから起動できます。このクライアントは、MAGENTO_CLOUD_RELATIONSHIPS 変数のポート番号、ユーザー名、パスワード情報を使用して `localhost:<portnumber>` に接続するように設定されています。

### アプリケーションからの接続

アプリケーションで動作している RabbitMQ に接続するには、[amqp-utils](https://github.com/dougbarth/amqp-utils) などのクライアントを `.magento.app.yaml` ファイルのプロジェクト依存関係としてインストールします。

以下に例を挙げます。

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

PHP コンテナにログインすると、キューの管理に使用できる `amqp-` コマンドを入力します。

### PHP アプリケーションからの接続

PHP アプリケーションを使用して RabbitMQ に接続するには、ソースツリーに PHP ライブラリを追加します。

## [!DNL RabbitMQ] サービスのトラブルシューティング

[Adobe Commerce Cloud で RabbitMQ に接続できない &#x200B;](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27688) を参照してください。

## [!DNL RabbitMQ] サービスのアップグレード

アップグレード手順については、「[&#x200B; サービスバージョンの変更 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version)」を参照してください。
