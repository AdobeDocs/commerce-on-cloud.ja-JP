---
title: RabbitMQ サービスの設定
description: RabbitMQ サービスを有効にして、クラウドインフラストラクチャー上のAdobe Commerceのメッセージキューを管理する方法について説明します。
feature: Cloud, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# サービス [!DNL RabbitMQ] 設定

[Message Queue Framework （MQF） ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) は、[ モジュール ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module) がメッセージをキューに公開できるようにする、Adobe Commerce内のシステムです。 また、メッセージを非同期で受信するコンシューマーも定義します。

MQF は、メッセージの送受信に使用できるスケーラブルなプラットフォームを提供するメッセージング ブローカーとして [&#128279;](https://www.rabbitmq.com/)0&rbrace;RabbitMQ&rbrace; を使用します。 また、未配信メッセージを保存するメカニズムも含まれています。 [!DNL RabbitMQ] は、Advanced Message Queuing Protocol （AMQP） 0.9.1 仕様に基づいています。

>[!WARNING]
>
>クラウドインフラストラクチャ上のAdobe Commerceを使用してサービスを作成するのではなく、[!DNL RabbitMQ] などの既存の AMQP ベースのサービスを使用する場合は、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 環境変数を使用してサービスをサイトに接続します。

{{service-instruction}}

**RabbitMQを有効にするには**:

1. 必要な名前、タイプ、ディスク値（MB 単位）を、インストールされているRabbitMQのバージョンと共に `.magento/services.yaml` ファイルに追加します。

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

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## デバッグ用にRabbitMQに接続

デバッグの目的では、次のいずれかの方法でサービスインスタンスに直接接続すると便利です。

- ローカル開発環境から接続する
- アプリケーションからの接続
- PHP アプリケーションからの接続

### ローカル開発環境から接続する

1. `magento-cloud` CLI にログインし、次のプロジェクトを実行します。

   ```bash
   magento-cloud login
   ```

1. RabbitMQがインストールおよび設定された環境をチェックアウトします。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH を使用してクラウド環境に接続します。

   ```bash
   magento-cloud ssh
   ```

1. [$connection_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) 変数からRabbitMQMAGENTOの詳細とログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、RabbitMQ情報を確認します。例：

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

1. RabbitMQへのローカルポート転送を有効にする（プロジェクトが US-3、EU-5、AP-3 リージョンなど別のリージョンにある場合は、``us`` の代わりに ``us-3``/``eu-5``/``ap-3`` を使用します）

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:15672` でRabbitMQ管理 web インターフェイスにアクセスする例を次に示します。

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. セッションが開いている間は、選択したRabbitMQ クライアントをローカルワークステーションから起動できます。このクライアントは、ポート番号、ユーザー名およびパスワード情報を使用して `localhost:<portnumber>` にMAGENTOするように設定されています。

### アプリケーションからの接続

アプリケーションで動作しているRabbitMQに接続するには、[amqp-utils](https://github.com/dougbarth/amqp-utils) などのクライアントを `.magento.app.yaml` ファイルのプロジェクト依存関係としてインストールします。

以下に例を挙げます。

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

PHP コンテナにログインすると、キューの管理に使用できる `amqp-` コマンドを入力します。

### PHP アプリケーションからの接続

PHP アプリケーションを使用してRabbitMQに接続するには、ソースツリーに PHP ライブラリを追加します。
