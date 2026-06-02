---
title: ActiveMQ サービスの設定
description: ActiveMQ Artemis サービスを有効にして、Adobe Commerce on cloud infrastructureのメッセージキューを管理する方法を説明します。
feature: Cloud, Services
exl-id: 39eb03a7-3345-4db9-88fa-dd7c422228f9
TQID: https://experienceleague.adobe.com/YYGonI3614QouFjVftfShC1Mq7IJB7YcrxynBt6AnuY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 0%

---

# [!DNL ActiveMQ] サービスの設定

[Message Queue Framework （MQF） &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=ja)は、[&#x200B; モジュール &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/glossary#module)がメッセージをキューに公開できるようにするAdobe Commerce内のシステムです。 また、メッセージを非同期的に受信する消費者も定義します。

MQFは[ActiveMQ Artemis](https://activemq.apache.org/components/artemis/)をメッセージングブローカーとして使用でき、メッセージを送受信するためのスケーラブルなプラットフォームを提供します。 また、未配信のメッセージを保存するメカニズムも含まれています。 [!DNL ActiveMQ Artemis]は、メッセージ用のSTOMP （Streaming Text Oriented Messaging Protocol） プロトコルをサポートしています。

[!DNL ActiveMQ Artemis]は、メッセージキューを管理するためのRabbitMQの代替として利用できます。 これは、ActiveMQに固有の機能が必要な場合や、STOMP プロトコルを使用したい場合に特に便利です。

>[!IMPORTANT]
>
>[!DNL ActiveMQ]のような既存のメッセージブローカーサービスを使用する場合は、クラウドインフラストラクチャ上のAdobe Commerceに依存して作成するのではなく、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境変数を使用してサイトに接続します。

{{service-instruction}}

**ActiveMQ Artemis**&#x200B;を有効にするには：

1. インストールされているActiveMQ バージョンと共に、必要な名前、タイプ、およびディスク値（MB単位）を`.magento/services.yaml` ファイルに追加します。

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. `.magento.app.yaml` ファイルの関係を設定します。

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [&#x200B; サービス関係を確認します](services-yaml.md#service-relationships)。

{{service-change-tip}}

## デバッグ用にActiveMQに接続

デバッグの目的では、次のいずれかの方法でサービスインスタンスに直接接続できます。

- ローカル開発製品との連携
- アプリケーションからの接続
- PHP アプリケーションからの接続

### ローカル開発製品との連携

1. `magento-cloud` CLIにログインして、次のプロジェクトを実行します。

   ```bash
   magento-cloud login
   ```

1. ActiveMQ Artemisがインストールされ、設定されている環境をチェックアウトします。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSHを使用してクラウド環境に接続します。

   ```bash
   magento-cloud ssh
   ```

1. [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)変数からActiveMQ接続の詳細とログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、ActiveMQ情報を見つけます。 関係の名前は、`.magento.app.yaml` ファイルで設定した方法によって異なります。 例えば、`activemq-artemis`をリレーションシップ名として使用した場合は、次のようになります。

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

1. ActiveMQへのローカルポート転送を有効にします（プロジェクトがUS-3、EU-5、AP-3などの別の地域にある場合は、``us``を``us-3``/``eu-5``/``ap-3``に置き換えます）

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:8161`のActiveMQ Artemis Web コンソールにアクセスする例は次のとおりです。

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemisは、STOMP メッセージにはポート 61616を、web コンソールにはポート 8161を使用します。

1. セッションが開いている間は、MAGENTO_CLOUD_RELATIONSHIPS変数のユーザー名とパスワードを使用して、`http://localhost:8161`のActiveMQ Artemis web コンソールにアクセスできます。

### アプリケーションからの接続

アプリケーションで実行中のActiveMQに接続するには、PHP アプリケーションにSTOMP クライアントライブラリをインストールします。

### PHP アプリケーションからの接続

PHP アプリケーションを使用してActiveMQに接続するには、ソースツリーにPHP STOMP ライブラリを追加します。 Adobe CommerceはActiveMQ通信にSTOMP プロトコルを使用し、ActiveMQ Artemisが設定済みサービスとして検出されると、デプロイメント中に設定が自動的に設定されます。

## プロトコルのサポート

Adobe Commerce クラウドインフラストラクチャのActiveMQ Artemisでは、次のSTOMP （Streaming Text Oriented Messaging Protocol）プロトコルを使用しています。

- **STOMP**: キュー操作に使用されるメッセージングプロトコル （ポート 61616）
- **Web コンソール**: HTTP経由でアクセス可能な管理インターフェイス （ポート 8161）

## RabbitMQとの違い

[!DNL ActiveMQ Artemis]と[!DNL RabbitMQ]の両方がAdobe Commerceのメッセージブローカーとして機能しますが、いくつかの違いがあります。

- **プロトコル**: ActiveMQ ArtemisはSTOMP プロトコルを使用し、RabbitMQはAMQPを使用します
- **Configuration**: ActiveMQ Artemisが設定されると、Adobe Commerceは自動的にSTOMP プロトコルを使用します
- **優先度**: ActiveMQとRabbitMQの両方が設定されている場合、STOMP ベースの操作ではActiveMQが優先され、AMQPの操作ではRabbitMQが使用されます
- **Web コンソール**: ActiveMQは、キューとメッセージを監視するためのweb ベースの管理コンソールを提供します

## キュー設定

ActiveMQ Artemisがサービスとして設定されている場合、Adobe CommerceはSTOMP プロトコルを使用するようにキューシステムを自動的に設定します。 設定は、デプロイメント中に`app/etc/env.php` ファイルに書き込まれます。

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

必要に応じて、[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境変数を使用してこの設定を上書きできます。
