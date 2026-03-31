---
title: 変数のデプロイ
description: Adobe Commerce on cloud infrastructureのデプロイフェーズでアクションを制御する環境変数の一覧を参照してください。
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: fbbe98573e3e7bf60139d404ca3653f76abf0d8c
workflow-type: tm+mt
source-wordcount: '2551'
ht-degree: 0%

---

# 変数のデプロイ

次の&#x200B;_デプロイ_&#x200B;変数は、デプロイフェーズのアクションを制御し、[&#x200B; グローバル変数](variables-global.md)から値を継承して上書きできます。 これらの変数を`.magento.env.yaml` ファイルの`deploy` ステージに挿入します。

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズについて詳しくは、次を参照してください。

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

Redis ページとデフォルトのキャッシュを設定します。 `cm_cache_backend_redis` パラメーターを設定する場合は、`server`、`port`および`database`のオプションを指定する必要があります。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

次の例では、_設定ガイド_&#x200B;で定義されている[Redis プリロード機能](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html?lang=ja#redis-preload-feature)を使用しています。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

カスタム [REDIS_BACKEND](#redis_backend) モデルを（許可リストからだけでなく）使用するには、`_custom_redis_backend` オプションを`true`に設定して、次の例のように正しい検証を有効にします。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Default**—`true`
- **バージョン** - Adobe Commerce 2.1.4以降

ビルドまたはデプロイのフェーズで生成された[静的コンテンツファイル &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=ja)のクリーニングを有効または無効にします。 開発時のデフォルト値&#x200B;_true_&#x200B;をベストプラクティスとして使用します。

- **`true`** – 更新された静的コンテンツをデプロイする前に、既存のすべての静的コンテンツを削除します。
- **`false`** – 生成されたコンテンツに新しいバージョンが含まれている場合にのみ、デプロイメントは既存の静的コンテンツファイルを上書きします。

別のプロセスで静的コンテンツを変更する場合は、値を&#x200B;_false_&#x200B;に設定します。

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

デプロイ前に静的ビューファイルをクリーンアップしないと、以前のバージョンを削除せずに既存のファイルにアップデートをデプロイすると問題が発生する可能性があります。 [静的ファイルのフォールバック &#x200B;](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache) ルールにより、ディレクトリに同じファイルの複数のバージョンが含まれている場合、フォールバック操作で間違ったファイルが表示される可能性があります。

## `CRON_CONSUMERS_RUNNER`

- **Default**—`cron_run = false`, `max_messages = 1000`
- **バージョン** - Adobe Commerce 2.2.0以降

この環境変数を使用して、デプロイメント後にメッセージキューが実行されていることを確認します。

- `cron_run` - `consumers_runner` cron ジョブを有効または無効にするブール値（デフォルト = `false`）。
- `max_messages` – 各コンシューマーが終了する前に処理する必要があるメッセージの最大数を指定する数値（デフォルト = `1000`）。 値を`0`に設定すると、コンシューマーが終了しないようにできます。
- `consumers` – 実行するコンシューマーを指定する文字列の配列。 空の配列は&#x200B;_all_&#x200B;個の消費者を実行します。

- `multiple_processes` – 各コンシューマーに対して生成するプロセスの数を指定する数値。 Commerce **2.4.4**&#x200B;以降でサポートされています。

>[!NOTE]
>
>メッセージキュー`consumers`のリストを返すには、リモート環境で`./bin/magento queue:consumers:list` コマンドを実行します。

各コンシューマーに対して生成する特定の`consumers`と`multiple_processes`を実行する配列の例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

すべての`consumers`を実行する空の配列の例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

デフォルトでは、デプロイメントプロセスは`env.php` ファイルのすべての設定を上書きします。 オンプレミス Adobe Commerceについては、_Commerce設定ガイド_&#x200B;の「[&#x200B; メッセージキューの管理](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html?lang=ja)」を参照してください。

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.2.0以降

次のいずれかのオプションを選択して、`consumers`がメッセージキューからのメッセージを処理する方法を設定します。

- `false`—`Consumers`は、キュー内の使用可能なメッセージを処理し、TCP接続を閉じて終了します。`Consumers` 処理済みメッセージの数が`CRON_CONSUMERS_RUNNER` デプロイ変数で指定された`max_messages`値よりも少ない場合でも、追加のメッセージがキューに入るのを待つことはありません。

- `true`—`Consumers`は、TCP接続を閉じてコンシューマープロセスを終了する前に、`CRON_CONSUMERS_RUNNER` デプロイ変数で指定されたメッセージの最大数（`max_messages`）に達するまで、メッセージキューからのメッセージを処理し続けます。 キューが`max_messages`に到達する前に空になった場合、消費者はより多くのメッセージが到着するのを待ちます。

>[!WARNING]
>
>cron ジョブを使用する代わりに`consumers`を実行するためにワーカーを使用する場合は、この変数をtrueに設定します。

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

>[!WARNING]
>
>`.magento.env.yaml` ファイルではなく[!DNL Cloud Console]を通じて`CRYPT_KEY`値を設定し、お使いの環境のソースコードリポジトリでキーを公開しないようにします。 [環境とプロジェクト変数の設定](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html?lang=ja#configure-environment)を参照してください。

インストールプロセスなしでデータベースを環境から別の環境に移動する場合は、対応する暗号化情報が必要です。 Adobe Commerceは、[!DNL Cloud Console]で設定された暗号化キーの値を`env.php` ファイルの`crypt/key`値として使用します。

## `DATABASE_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

`.magento.app.yaml` ファイルの[関係プロパティ &#x200B;](../application/properties.md#relationships)でデータベースを定義した場合、デプロイメント用にデータベース接続をカスタマイズできます。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

また、テーブルの接頭辞を設定することもできます。

>[!WARNING]
>
>テーブルのプレフィックスで結合オプションを使用しない場合は、デフォルトの接続設定を指定する必要があります。そうしないと、デプロイで検証が失敗します。

次の例では、`_merge` オプションではなく、デフォルトの接続設定で`ece_` テーブルのプレフィックスを使用しています。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

出力サンプル：

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0以降

デプロイメント間でカスタマイズされた[!DNL Elastic Suite] サービス設定を保持し、メインの[!DNL Elastic Suite]設定の「system/default/smile_elasticsuite_core_base_settings」セクションで使用します。 [!DNL Elastic Suite] コンポーザーパッケージがインストールされている場合は、自動的に設定されます。

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>3つのノード（[Scaled Architecture](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)上の3つのサービスノード）を持つPro ステージング/実稼動クラスターでは、`indices_settings`を次のように設定する必要があります。
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**既知の制限事項**:

- 検索エンジンを`elasticsuite`以外の種類に変更すると、デプロイ失敗が発生し、適切な検証エラーが発生します
- Elasticsearch サービスを削除すると、デプロイに失敗し、適切な検証エラーが発生します

>[!NOTE]
>
>Adobe Commerceでの[!DNL Elastic Suite] プラグインの使用またはトラブルシューティングについて詳しくは、[[!DNL Elastic Suite]  ドキュメント &#x200B;](https://github.com/Smile-SA/elasticsuite)を参照してください。

## `ENABLE_GOOGLE_ANALYTICS`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.1.4以降

ステージング環境と統合環境にデプロイする際に、Google Analyticsを有効または無効にします。 デフォルトでは、Google Analyticsは実稼動環境に対してのみtrueです。 ステージング環境と統合環境でGoogle Analyticsを有効にするには、この値を`true`に設定します。

- **`true`** - ステージング環境と統合環境でGoogle Analyticsを有効にします。
- **`false`** - ステージング環境と統合環境でGoogle Analyticsを無効にします。

`ENABLE_GOOGLE_ANALYTICS`環境変数を`.magento.env.yaml` ファイルの`deploy` ステージに追加します。

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>デプロイプロセスは、実稼動環境でGoogle Analyticsを常に有効にします。

## `FORCE_UPDATE_URLS`

- **Default**—`true`
- **バージョン** - Adobe Commerce 2.1.4以降

Proまたはスターターステージングおよび実稼動環境へのデプロイメント時に、この変数は、データベース内のAdobe Commerce ベース URLを、[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)変数で指定されたプロジェクト URLに置き換えます。 この設定を使用して、[UPDATE_URLS](#update_urls) デプロイ変数のデフォルトの動作を上書きします。この変数は、ステージング環境または実稼動環境にデプロイする際に無視されます。

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Default**—`file`
- **バージョン** - Adobe Commerce 2.2.5以降

ロックプロバイダーは、重複したcron ジョブとcron グループの起動を防ぎます。 実稼動環境で`file` ロックプロバイダーを使用します。 スターター環境とPro統合環境では[MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md)変数が使用されないため、`ece-tools`は`db` ロックプロバイダーを自動的に適用します。

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

_インストールガイド_&#x200B;の「[&#x200B; ロックの設定](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html?lang=ja)」を参照してください。

## `MYSQL_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.1.4以降

>[!TIP]
>
>`MYSQL_USE_SLAVE_CONNECTION`変数は、クラウドインフラストラクチャのステージング環境およびProduction Pro クラスター環境のAdobe Commerceでのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数のデータベースを非同期で読み取ることができます。 データベースへの&#x200B;_読み取り専用_&#x200B;接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true`に設定します。 1つのノードのみが読み取りと書き込みのトラフィックを処理するため、負荷分散によってパフォーマンスが向上します。 `env.php` ファイルから既存の読み取り専用の接続配列を削除するには、`false`に設定します。

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

`MYSQL_USE_SLAVE_CONNECTION`変数が`true`に設定されている場合、Pro ステージング環境および実稼動環境の`env.php` ファイルでは、`synchronous_replication` パラメーターがデフォルトで`true`に設定されます。 `MYSQL_USE_SLAVE_CONNECTION`が`false`に設定されている場合、`synchronous_replication` パラメーターは設定されていません。

## `QUEUE_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

この環境変数を使用して、デプロイメント間でカスタマイズされたキューサービス設定を保持します。 この変数は、AMQP （RabbitMQの場合）とSTOMP （ActiveMQ Artemisの場合）の両方のプロトコルをサポートします。 例えば、クラウドインフラストラクチャに依存せずに既存のメッセージキューサービスを使用して作成する場合は、`QUEUE_CONFIGURATION`環境変数を使用してサイトに接続します。

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

STOMP プロトコルを使用するActiveMQ Artemisの場合：

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      stomp:
        host: activemq.host
        port: 61616
        user: username
        password: password
```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Default**—`Cm_Cache_Backend_Redis`
- **バージョン** - Adobe Commerce 2.3.0以降

Redis キャッシュのバックエンドモデル設定を指定します。

Adobe Commerce バージョン 2.3.0以降には、次のバックエンドモデルが含まれています。

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

`REDIS_BACKEND`の設定方法の例

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`をRedis バックエンドモデルとして指定して[L2 キャッシュ &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=ja)を有効にすると、`ece-tools`はキャッシュ設定を自動的に生成します。 _Adobe Commerce設定ガイド_&#x200B;の[設定ファイル &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=ja#configuration-example)の例を参照してください。 生成されたキャッシュ設定を上書きするには、[CACHE_CONFIGURATION](#cache_configuration) デプロイ変数を使用します。

## `REDIS_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.1.16以降

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION`変数は、クラウドインフラストラクチャのステージング環境およびProduction Pro クラスター環境のAdobe Commerceでのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数のRedis インスタンスを非同期で読み取ることができます。 Redis インスタンスへの&#x200B;_読み取り専用_&#x200B;接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true`に設定します。 1つのノードのみが読み取りと書き込みのトラフィックを処理するため、負荷分散によってパフォーマンスが向上します。 `env.php` ファイルから既存の読み取り専用の接続配列を削除するには、`false`に設定します。

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

`.magento.app.yaml` ファイルと`services.yaml` ファイルでRedis サービスを設定する必要があります。

[ECE-Tools バージョン 2002.0.18](../release-notes/cloud-release-archive.md#v2002018)以降では、より多くのフォールトトレラント設定が使用されます。 Adobe CommerceがRedis _スレーブ_ インスタンスからデータを読み取れない場合は、Redis _マスター_ インスタンスからデータを読み取ります。

読み取り専用の接続は、統合環境で使用できないか、[`CACHE_CONFIGURATION`変数](#cache_configuration)を使用している場合に使用できます。

## `VALKEY_BACKEND`

- **Default**—`Cm_Cache_Backend_Redis`
- **バージョン** - Adobe Commerce 2.8.0以降

`VALKEY_BACKEND`は、Valkey キャッシュのバックエンド モデル設定を指定します。

Adobe Commerce バージョン 2.8.0以降には、次のバックエンドモデルが含まれています。

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

次の例は、`VALKEY_BACKEND`を設定する方法を示しています。

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Valkey バックエンドモデルとして`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`を指定して[L2 キャッシュ &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=ja)を有効にすると、`ece-tools`はキャッシュ設定を自動的に生成します。 _Adobe Commerce設定ガイド_&#x200B;の[設定ファイル &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=ja#configuration-example)の例を参照してください。 生成されたキャッシュ設定を上書きするには、[CACHE_CONFIGURATION](#cache_configuration) デプロイ変数を使用します。

## `VALKEY_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.4.8以降

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION`変数は、クラウドインフラストラクチャのステージング環境およびProduction Pro クラスター環境のAdobe Commerceでのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数のRedis インスタンスを非同期で読み取ることができます。`VALKEY_USE_SLAVE_CONNECTION` Redis インスタンスへの&#x200B;_読み取り専用_&#x200B;接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true`に設定します。 1つのノードのみが読み取りと書き込みのトラフィックを処理するため、負荷分散によってパフォーマンスが向上します。 `VALKEY_USE_SLAVE_CONNECTION`を`false`に設定して、既存の読み取り専用の接続配列を`env.php` ファイルから削除します。

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

`.magento.app.yaml` ファイルと`services.yaml` ファイルでRedis サービスを設定する必要があります。

[ECE-Tools バージョン 2002.0.18](../release-notes/cloud-release-archive.md#v2002018)以降では、より多くのフォールトトレラント設定が使用されます。 Adobe CommerceがValkey _slave_ インスタンスからデータを読み取れない場合、Redis _master_ インスタンスからデータを読み取ります。

読み取り専用の接続は、統合環境で使用できないか、[`CACHE_CONFIGURATION`変数](#cache_configuration)を使用している場合に使用できます。

## `RESOURCE_CONFIGURATION`

- **Default** – 未設定
- **バージョン** - Adobe Commerce 2.1.4以降

リソース名をデータベース接続にマッピングします。 この設定は、`env.php` ファイルの`resource` セクションに対応しています。

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Default**—`4`
- **バージョン** - Adobe Commerce 2.1.4以降

静的コンテンツを圧縮する際に使用する[gzip](https://www.gnu.org/software/gzip)圧縮レベル （`0` ～ `9`）を指定します。`0`は圧縮を無効にします。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Default**—`600`
- **バージョン** - Adobe Commerce 2.1.4以降

静的アセットの圧縮にかかる時間が圧縮タイムアウトの制限を超えると、デプロイメントプロセスが中断されます。 静的コンテンツ圧縮コマンドの最大実行時間を秒単位で設定します。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

テーマごとに複数のロケールを設定できます。 このカスタマイズにより、不要なテーマファイルの数を減らすことで、デプロイメントプロセスが高速化されます。 例えば、_magento/backend_ テーマを英語でデプロイし、カスタムテーマを他の言語でデプロイできます。

次の例では、3つのロケールを持つ`Magento/backend` テーマをデプロイします。

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

また、テーマを&#x200B;_not_ デプロイするように選択できます。

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0以降

静的コンテンツのデプロイメントで想定される最大実行時間を増やすことができます。

デフォルトでは、Adobe Commerceは想定される最大実行時間を900秒に設定しますが、一部のシナリオでは、Cloud プロジェクトの静的コンテンツのデプロイメントを完了するのに多くの時間が必要になる場合があります。

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.4.2以降

展開フェーズで、親テーマの静的コンテンツの生成が展開フェーズ中に発生しないように`SCD_NO_PARENT: true`を設定します。 この設定により、デプロイメント時間が最小限に抑えられ、デプロイメント中に静的コンテンツのビルドに失敗した場合に発生する可能性のあるサイトのダウンタイムが回避されます。 [静的コンテンツ展開](../deploy/static-content.md)を参照してください。

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Default**—`quick`
- **バージョン** - Adobe Commerce 2.2.0以降

静的コンテンツの[&#x200B; デプロイメント戦略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=ja)をカスタマイズできます。 [静的ビューファイルのデプロイ &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=ja)を参照してください。

複数のロケールがある場合は、次のオプション _のみ_&#x200B;を使用します。

- `standard` – すべてのパッケージのすべての静的ビューファイルをデプロイします。
- `quick` – （_default_）は、デプロイメント時間を最小限に抑えます。
- `compact` - サーバー上のディスク領域を節約します。 Adobe Commerce バージョン 2.2.4以前では、この設定は`scd_threads`の値を`1`の値で上書きします。

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default** – 自動
- **バージョン** - Adobe Commerce 2.1.4以降

静的コンテンツのデプロイメント用のスレッド数を設定します。 デフォルト値は、検出されたCPU スレッド数に基づいて設定され、値4を超えることはありません。 スレッド数を増やすと、静的コンテンツのデプロイメントが高速化されます。スレッド数を減らすと、速度が低下します。 スレッドの値を設定できます。例：

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

デプロイメントの時間をさらに短縮するには、`scd-dump` コマンドで[構成管理](../store/store-settings.md)を使用して、静的デプロイメントをビルド フェーズに移動します。

## `SEARCH_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

この環境変数を使用して、デプロイメント間でカスタマイズされた検索サービス設定を保持します。 例：

Elasticsearch設定：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

OpenSearch設定（Commerce 2.4.6以降）:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

Redis セッションストレージを設定します。 セッションストレージ変数には、`save`、`redis`、`host`、`port`および`database`のオプションが必要です。 例：

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

次の例では、新しい値を既存の設定にマージします。

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Default**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

デプロイメントフェーズ中に静的コンテンツのデプロイメントをスキップするには、`true`に設定します。

デプロイフェーズで、デプロイフェーズ中に静的コンテンツのビルドが発生しないように`SKIP_SCD: true`を設定します。 この設定により、デプロイメント時間が最小限に抑えられ、デプロイメント中に静的コンテンツのビルドに失敗した場合に発生する可能性のあるサイトのダウンタイムが回避されます。 [静的コンテンツ展開](../deploy/static-content.md)を参照してください。

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Default**—`true`
- **バージョン** - Adobe Commerce 2.1.4以降

デプロイメント時に、データベース内のAdobe Commerce ベース URLを、[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)変数で指定されたプロジェクト URLに置き換えます。 この設定は、ベース URLがローカル環境に設定されているローカル開発に役立ちます。 クラウド環境にデプロイすると、URLが更新され、プロジェクト URLを使用してストアフロントと管理者にアクセスできるようになります。

Proまたはスターターステージングおよび実稼動環境にデプロイする際にURLを更新する必要がある場合は、[`FORCE_UPDATE_URLS`](#force_update_urls)変数を使用します。

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

デプロイメントフェーズ中に実行される`bin/magento` CLI コマンドの[Symfony](https://symfony.com/doc/current/console/verbosity.html) デバッグの冗長性レベルを有効または無効にします。

>[!NOTE]
>
>VERBOSE_COMMANDS設定を使用して、成功したCLI コマンドと失敗した`bin/magento` CLI コマンドの両方のコマンド出力の詳細を制御するには、[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`を設定する必要があります。

ログに記載されている詳細レベルを選択します。

- `-v`=通常の出力
- `-vv`=より詳細な出力
- `-vvv` = デバッグに最適な詳細な出力

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
