---
title: ActiveMQ サービスの設定
description: ActiveMQ Artemis サービスを有効にして、クラウドインフラストラクチャー上のAdobe Commerceのメッセージキューを管理する方法について説明します。
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# サービス [!DNL ActiveMQ] 設定

[Message Queue Framework （MQF） ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) は、[ モジュール ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module) がメッセージをキューに公開できるようにする、Adobe Commerce内のシステムです。 また、メッセージを非同期で受信するコンシューマーも定義します。

MQF は、メッセージを送受信するためのスケーラブルなプラットフォームを提供するメッセージングブローカーとして [ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) を使用できます。 また、未配信メッセージを保存するメカニズムも含まれています。 [!DNL ActiveMQ Artemis] は、メッセージング用の STOMP （Streaming Text Oriented Messaging Protocol）プロトコルをサポートしています。

[!DNL ActiveMQ Artemis] は、メッセージキューを管理するための RabbitMQ の代わりに使用できます。 これは、ActiveMQ 固有の機能が必要な場合や、STOMP プロトコルを使用する場合に特に便利です。

>[!IMPORTANT]
>
>クラウドインフラストラクチャ上のAdobe Commerceを使用してサービスを作成する代わりに、[!DNL ActiveMQ] などの既存の Message Broker サービスを使用する場合は、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 環境変数を使用してサービスをサイトに接続します。

{{service-instruction}}

**ActiveMQ Artemis を有効にするには**:

1. 必要な名前、タイプ、ディスク値（MB 単位）を、インストールされている ActiveMQ のバージョンとともに `.magento/services.yaml` ファイルに追加します。

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## デバッグ用に ActiveMQ に接続

デバッグの目的で、次のいずれかの方法でサービスインスタンスに直接接続できます。

- ローカル開発環境から接続する
- アプリケーションからの接続
- PHP アプリケーションからの接続

### ローカル開発環境から接続する

1. `magento-cloud` CLI にログインし、次のプロジェクトを実行します。

   ```bash
   magento-cloud login
   ```

1. ActiveMQ Artemis がインストールおよび設定されている環境をチェックアウトします。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH を使用してクラウド環境に接続します。

   ```bash
   magento-cloud ssh
   ```

1. [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) 変数から ActiveMQ 接続の詳細とログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、ActiveMQ 情報を見つけます。 関係名は、`.magento.app.yaml` ファイルでの設定方法によって異なります。 例えば、関係名として `activemq-artemis` を使用した場合は、次のようになります。

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. ActiveMQ へのローカルポート転送を有効にする（プロジェクトが US-3、EU-5、AP-3 などの別の地域にある場合は、``us-3`` を ``eu-5``/``ap-3``/``us`` に置き換えてください）

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:8161` で ActiveMQ Artemis web コンソールにアクセスする例を次に示します。

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis は、STOMP メッセージングにポート 61616 を使用し、Web コンソールにポート 8161 を使用します。

1. セッションが開いている間は、MAGENTO_CLOUD_RELATIONSHIPS 変数のユーザー名とパスワードを使用して、`http://localhost:8161` の ActiveMQ Artemis web コンソールにアクセスできます。

### アプリケーションからの接続

アプリケーションで動作している ActiveMQ に接続するには、PHP アプリケーションに STOMP クライアントライブラリをインストールします。

### PHP アプリケーションからの接続

PHP アプリケーションを使用して ActiveMQ に接続するには、ソースツリーに PHP STOMP ライブラリを追加します。 Adobe Commerceは ActiveMQ 通信に STOMP プロトコルを使用し、ActiveMQ Artemis が設定済みのサービスとして検出されると、デプロイメント時に設定が自動的に設定されます。

## プロトコルのサポート

クラウドインフラストラクチャー上のAdobe Commerceの ActiveMQ Artemis は、STOMP （Streaming Text Oriented Messaging Protocol）プロトコルを使用します。

- **STOMP**：キュー操作に使用されるメッセージングプロトコル（ポート 61616）
- **Web コンソール**:HTTP 経由でアクセス可能な管理インターフェイス（ポート 8161）

## RabbitMQ との違い

[!DNL ActiveMQ Artemis] と [!DNL RabbitMQ] は両方ともAdobe Commerceのメッセージブローカーとして機能しますが、いくつかの違いがあります。

- **プロトコル**:ActiveMQ Artemis は STOMP プロトコルを使用するのに対して、RabbitMQ は AMQP を使用します
- **設定**:ActiveMQ Artemis が設定されると、Adobe Commerceは自動的に STOMP プロトコルを使用します
- **優先度**:ActiveMQ と RabbitMQ の両方が設定されている場合、STOMP ベースの操作は ActiveMQ が優先され、AMQP 操作では RabbitMQ が使用されます
- **Web コンソール**: ActiveMQ は、キューとメッセージを監視するための web ベースの管理コンソールを提供します

## キューの設定

ActiveMQ Artemis をサービスとして設定すると、Adobe Commerceはキューシステムを STOMP プロトコルを使用するように自動的に設定します。 設定は、デプロイメント時に `app/etc/env.php` ファイルに書き込まれます。

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

必要に応じて、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 環境変数を使用してこの設定を上書きできます。

