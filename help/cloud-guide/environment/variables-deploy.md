---
title: 変数のデプロイ
description: クラウドインフラストラクチャー上でのAdobe Commerceのデプロイフェーズでアクションを制御する環境変数のリストを参照してください。
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 275a2a5c58b7c5db12f8f31bffed85004e77172f
workflow-type: tm+mt
source-wordcount: '2483'
ht-degree: 0%

---

# 変数のデプロイ

次の _デプロイ_ 変数は、デプロイフェーズでのアクションを制御し、[ グローバル変数 ](variables-global.md) の値を継承および上書きできます。 `.magento.env.yaml` ファイルの `deploy` のステージに、次の変数を挿入します。

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズに関する詳細情報：

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

Redis ページとデフォルトのキャッシュを設定します。 `cm_cache_backend_redis` パラメーターを設定する場合、`server`、`port`、`database` の各オプションを指定する必要があります。

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

次の例では、新しい値を既存の設定に結合します。

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

次の例では、_設定ガイド_ で定義されている [Redis プリロード機能 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature) を使用しています。

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

カスタムの [REDIS_BACKEND](#redis_backend) モデルを使用するには（許可リストからだけでなく）、`_custom_redis_backend` のオプションを `true` に設定し、次の例のように正しい検証を有効にします。

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

- **デフォルト**—`true`
- **バージョン** - Adobe Commerce 2.1.4 以降

ビルドまたはデプロイ フェーズで生成された [ 静的コンテンツ ファイル ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) のクリーニングを有効または無効にします。 開発では、デフォルト値 _true_ をベストプラクティスとして使用します。

- **`true`** – 更新された静的コンテンツをデプロイする前に、既存の静的コンテンツをすべて削除します。
- **`false`** – 生成されたコンテンツに新しいバージョンが含まれている場合にのみ、既存の静的コンテンツ・ファイルが配置によって上書きされます。

静的コンテンツを別のプロセスで変更する場合は、値を _false_ に設定します。

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

デプロイ前に静的ビューファイルをクリーンアップしないと、以前のバージョンを削除せずに既存のファイルに更新をデプロイすると、問題が発生する可能性があります。 [ 静的ファイルのフォールバック ](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache) ルールが原因で、ディレクトリに同じファイルの複数のバージョンが含まれている場合、フォールバック操作で誤ったファイルが表示される可能性があります。

## `CRON_CONSUMERS_RUNNER`

- **Default**—`cron_run = false`、`max_messages = 1000`
- **バージョン** - Adobe Commerce 2.2.0 以降

この環境変数を使用して、メッセージキューがデプロイメント後に実行されていることを確認します。

- `cron_run` - `consumers_runner` cron ジョブを有効または無効にするブール値（デフォルト=`false`）。
- `max_messages` – 各消費者が終了するまでに処理する必要があるメッセージの最大数を指定する数値（デフォルトは `1000`）。 値を `0` に設定して、コンシューマーが終了しないようにすることができます。
- `consumers` – 実行するコンシューマーを指定する文字列の配列。 空の配列が _all_ コンシューマーを実行します。

- `multiple_processes` – 各消費者に対して生成するプロセスの数を指定する数値。 Commerce **2.4.4** 以降でサポートされます。

>[!NOTE]
>
>メッセージキュー `consumers` のリストを返すには、リモート環境で `./bin/magento queue:consumers:list` コマンドを実行します。

特定の `consumers` を実行する配列の例と、各消費者に対して生成する `multiple_processes` を次に示します。

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

すべての `consumers` を実行する空の配列の例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

デフォルトでは、デプロイメントプロセスによって `env.php` ファイル内のすべての設定が上書きされます。 オンプレミスのAdobe Commerceの場合は、_Commerce設定ガイド_ の [ メッセージキューの管理 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html) を参照してください。

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.2.0 以降

次のいず `consumers` かのオプションを選択して、メッセージキューからのメッセージの処理方法を設定します。

- `false` - キュー内 `Consumers` 使用可能なメッセージを処理し、TCP 接続を閉じて終了します。 処理され `Consumers` メッセージの数が `CRON_CONSUMERS_RUNNER` のデプロイ変数で指定された `max_messages` 値より少ない場合でも、追加のメッセージがキューに入るのを待たないでください。

- `true` - TCP 接続 `Consumers` 閉じてコンシューマ・プロセスを終了する前に、`CRON_CONSUMERS_RUNNER` デプロイ変数で指定された最大メッセージ数（`max_messages`）に達するまで、メッセージ・キューからのメッセージの処理を続行します。 キューが `max_messages` に到達する前に空になった場合、コンシューマーはさらに多くのメッセージが到着するのを待ちます。

>[!WARNING]
>
>cron ジョブを使用する代わりにワーカーを使用して `consumers` を実行する場合は、この変数を true に設定します。

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

>[!WARNING]
>
>環境のソースコードリポジトリで鍵が公開されないようにするには、`.magento.env.yaml` ファイルではなく [!DNL Cloud Console] を使用して `CRYPT_KEY` 値を設定します。 [ 環境およびプロジェクト変数の設定 ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html#configure-environment) を参照してください。

インストール処理を行わずに、ある環境から別の環境にデータベースを移動する場合は、対応する暗号化情報が必要です。 Adobe Commerceは、[!DNL Cloud Console] で設定された暗号化キーの値を `env.php` ファイルの `crypt/key` 値として使用します。

## `DATABASE_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

`.magento.app.yaml` ファイルの [relationships プロパティ ](../application/properties.md#relationships) でデータベースを定義した場合は、データベース接続を配置用にカスタマイズできます。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

次の例では、新しい値を既存の設定に結合します。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

また、テーブルプレフィックスを設定することもできます。

>[!WARNING]
>
>テーブル接頭辞で結合オプションを使用しない場合は、デフォルトの接続設定を指定する必要があります。指定しないと、配備の検証が失敗します。

次の例では、`_merge` オプションを使用する代わりに、デフォルトの接続設定で `ece_` テーブルのプレフィックスを使用しています。

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

サンプル出力：

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

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0 以降

デプロイメント間でカスタマイズされた [!DNL Elastic Suite] サービス設定を保持し、メイン [!DNL Elastic Suite] 設定の「system/default/smile_elasticsuite_core_base_settings」セクションで使用します。 [!DNL Elastic Suite] composer パッケージがインストールされている場合は、自動的に設定されます。

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
>3 つのノード（または 3 つのサービスノードを [ スケールアーキテクチャ ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier) 上に持つ Pro ステージング/実稼動クラスターでは、`indices_settings` を次のように設定する必要があります。
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

次の例では、新しい値を既存の設定に結合します。

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

- 検索エンジンを `elasticsuite` 以外のタイプに変更すると、デプロイエラーが発生し、適切な検証エラーが表示されます
- Elasticsearch サービスを削除すると、デプロイが失敗し、適切な検証エラーが表示されます

>[!NOTE]
>
>Adobe Commerceでの [!DNL Elastic Suite] プラグインの使用またはトラブルシューティングについて詳しくは、[[!DNL Elastic Suite]  ドキュメント ](https://github.com/Smile-SA/elasticsuite) を参照してください。

## `ENABLE_GOOGLE_ANALYTICS`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.1.4 以降

ステージング環境および統合環境にデプロイする場合に、Google Analyticsを有効または無効にします。 デフォルトでは、Google Analyticsは実稼動環境でのみ true になります。 この値を `true` に設定して、ステージング環境と統合環境でGoogle Analyticsを有効にします。

- **`true`** - ステージング環境および統合環境でGoogle Analyticsを有効にします。
- **`false`** - ステージング環境および統合環境でGoogle Analyticsを無効にします。

`ENABLE_GOOGLE_ANALYTICS` 環境変数を `.magento.env.yaml` ファイルの `deploy` ステージに追加します。

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>デプロイプロセスにより、実稼動環境でGoogle Analyticsが常に有効になります。

## `FORCE_UPDATE_URLS`

- **デフォルト**—`true`
- **バージョン** - Adobe Commerce 2.1.4 以降

Pro または Starter のステージング環境および実稼動環境にデプロイメントする場合、この変数は、データベース内のAdobe Commerceのベース URL を [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) 変数で指定されたプロジェクト URL に置き換えます。 この設定を使用して、[UPDATE_URLS](#update_urls) デプロイ変数のデフォルトの動作を上書きします。ステージング環境または実稼動環境にデプロイする場合は、この変数は無視されます。

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **デフォルト**—`file`
- **バージョン** - Adobe Commerce 2.2.5 以降

ロックプロバイダーは、重複した cron ジョブや cron グループの起動を防ぎます。 実稼動環境で `file` lock プロバイダーを使用します。 スターター環境と Pro 統合環境では、[MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md) 変数が使用されないので、`db` ロックプロバイダー `ece-tools` 自動的に適用されます。

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

[ インストールガイド ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html) ロックの設定 _を参照してください_。

## `MYSQL_USE_SLAVE_CONNECTION`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.1.4 以降

>[!TIP]
>
>`MYSQL_USE_SLAVE_CONNECTION` 変数は、クラウドインフラストラクチャー上のAdobe Commerce ステージング環境および実稼動 Pro クラスター環境でのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数のデータベースを非同期で読み取ることができます。 データベースへの _読み取り専用_ 接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true` に設定します。 この接続では、読み取り/書き込みトラフィックを処理するノードが 1 つだけなので、ロード・バランシングによってパフォーマンスが向上します。 既存の読み取り専用接続配列を `env.php` ファイルから削除するには、`false` に設定します。

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

`MYSQL_USE_SLAVE_CONNECTION` 変数が `true` に設定されている場合、Pro ステージング環境と実稼動環境の `env.php` ファイルでは、`synchronous_replication` パラメーターがデフォルトで `true` に設定されます。 `MYSQL_USE_SLAVE_CONNECTION` が `false` に設定されている場合、`synchronous_replication` パラメーターは設定されません。

## `QUEUE_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

この環境変数を使用して、カスタマイズされた AMQP サービス設定をデプロイメント間で保持します。 例えば、クラウドインフラストラクチャを利用して作成するのではなく、既存のメッセージキューサービスを使用する場合は、`QUEUE_CONFIGURATION` の環境変数を使用してサービスをサイトに接続します。

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

{{merge-options}}

次の例では、新しい値を既存の設定に結合します。

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

- **デフォルト**—`Cm_Cache_Backend_Redis`
- **バージョン** - Adobe Commerce 2.3.0 以降

Redis キャッシュのバックエンド モデル構成を指定します。

Adobe Commerce バージョン 2.3.0 以降には、次のバックエンドモデルが含まれています。

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

`REDIS_BACKEND` の設定方法の例

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` を Redis バックエンドモデルとして指定して [L2 キャッシュ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html) を有効にすると、`ece-tools` はキャッシュ設定を自動的に生成します。 [2}Adobe Commerce設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) の {configuration ファイル _の例を参照してください。_&#x200B;生成されたキャッシュ設定を上書きするには、[CACHE_CONFIGURATION](#cache_configuration) デプロイ変数を使用します。

## `REDIS_USE_SLAVE_CONNECTION`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.1.16 以降

>[!WARNING]
>
>[ スケール _アーキテクチャ_ プロジェクトでは、この変数を有効にしないで ](../architecture/scaled-architecture.md) ださい。 Redis 接続エラーが発生します。 Redis スレーブはアクティブですが、Redis 読み取りには使用されません。 別の方法として、Adobeでは、Adobe Commerce 2.3.5 以降を使用し、新しい Redis バックエンド設定を実装し、Redis 用の L2 キャッシングを実装することをお勧めします。

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION` 変数は、クラウドインフラストラクチャー上のAdobe Commerce ステージング環境および実稼動 Pro クラスター環境でのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数の Redis インスタンスを非同期で読み取ることができます。 Redis インスタンスへの _読み取り専用_ 接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true` に設定します。 この接続では、読み取り/書き込みトラフィックを処理するノードが 1 つだけなので、ロード・バランシングによってパフォーマンスが向上します。 既存の読み取り専用接続配列を `env.php` ファイルから削除するには、`false` に設定します。

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

`.magento.app.yaml` ファイルと `services.yaml` ファイルに Redis サービスが設定されている必要があります。

[ECE-Tools バージョン 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) 以降では、よりフォールトトレラントな設定を使用します。 Adobe Commerceが Redis _slave_ インスタンスからデータを読み取れない場合は、Redis _master_ インスタンスからデータを読み取ります。

読み取り専用接続は、統合環境では使用できません。また、[`CACHE_CONFIGURATION` 変数 ](#cache_configuration) を使用しても使用できません。

## `VALKEY_BACKEND`

- **デフォルト**—`Cm_Cache_Backend_Redis`
- **バージョン** - Adobe Commerce 2.8.0 以降

`VALKEY_BACKEND` は、Valkey キャッシュのバックエンドモデル設定を指定します。

Adobe Commerce バージョン 2.8.0 以降には、次のバックエンドモデルが含まれています。

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

次の例は、`VALKEY_BACKEND` を設定する方法を示しています。

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` を Valkey バックエンドモデルとして指定して [L2 キャッシュ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html) を有効にすると、`ece-tools` によってキャッシュ設定が自動的に生成されます。 [2}Adobe Commerce設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) の {configuration ファイル _の例を参照してください。_&#x200B;生成されたキャッシュ設定を上書きするには、[CACHE_CONFIGURATION](#cache_configuration) デプロイ変数を使用します。

## `VALKEY_USE_SLAVE_CONNECTION`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.4.8 以降

>[!WARNING]
>
>[ スケール _アーキテクチャ_ プロジェクトでは、この変数を有効にしないで ](../architecture/scaled-architecture.md) ださい。 Valkey 接続エラーが発生します。 Redis スレーブはアクティブですが、Redis 読み取りには使用されません。 または、Adobeでは、Adobe Commerce 2.4.8 以降を使用し、新しい Valkey バックエンド設定を実装し、Valkey の L2 キャッシングを実装することをお勧めします。

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION` 変数は、クラウドインフラストラクチャー上のAdobe Commerce ステージング環境および実稼動 Pro クラスター環境でのみサポートされており、スタータープロジェクトではサポートされていません。

Adobe Commerceは、複数の Redis インスタンスを非同期で読み取ることができます。 `VALKEY_USE_SLAVE_CONNECTION` Redis インスタンスへの _読み取り専用_ 接続を自動的に使用して、非マスターノードで読み取り専用トラフィックを受信するには、`true` に設定します。 この接続では、読み取り/書き込みトラフィックを処理するノードが 1 つだけなので、ロード・バランシングによってパフォーマンスが向上します。 既存の読み取り専用接続配列を `env.php` ファイルから削除するには、`VALKEY_USE_SLAVE_CONNECTION` を `false` に設定します。

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

`.magento.app.yaml` ファイルと `services.yaml` ファイルに Redis サービスが設定されている必要があります。

[ECE-Tools バージョン 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) 以降では、よりフォールトトレラントな設定を使用します。 Adobe Commerceが Valkey _slave_ インスタンスからデータを読み取れない場合は、Redis _master_ インスタンスからデータを読み取ります。

読み取り専用接続は、統合環境では使用できません。また、[`CACHE_CONFIGURATION` 変数 ](#cache_configuration) を使用しても使用できません。

## `RESOURCE_CONFIGURATION`

- **Default** – 設定しない
- **バージョン** - Adobe Commerce 2.1.4 以降

リソース名をデータベース接続にマップします。 この設定は、`env.php` ファイルの `resource` セクションに対応します。

{{merge-options}}

次の例では、新しい値を既存の設定に結合します。

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **デフォルト**—`4`
- **バージョン** - Adobe Commerce 2.1.4 以降

静的コンテンツを圧縮するときに使用する [gzip](https://www.gnu.org/software/gzip) 圧縮レベル （`0` ～ `9`）を指定します。`0` では圧縮を無効にします。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **デフォルト**—`600`
- **バージョン** - Adobe Commerce 2.1.4 以降

静的アセットの圧縮に要する時間が圧縮タイムアウトの制限を超えると、デプロイメントプロセスが中断されます。 静的コンテンツ圧縮コマンドの最大実行時間を秒単位で設定します。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

テーマごとに複数のロケールを設定できます。 このカスタマイズにより、不要なテーマファイルの数が減るので、デプロイメントプロセスが迅速化されます。 例えば、_magento/backend_ テーマを英語で、カスタムテーマを他の言語でデプロイできます。

次の例では、3 つのロケールで `Magento/backend` テーマをデプロイします。

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

また、テーマをデプロイ _ない_ ように選択することもできます。

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0 以降

静的コンテンツのデプロイメントの予想最大実行時間を増やすことができます。

デフォルトでは、Adobe Commerceは想定される最大実行時間を 900 秒に設定しますが、場合によっては、Cloud プロジェクトの静的コンテンツのデプロイメントを完了するためにより多くの時間が必要になることがあります。

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.4.2 以降

デプロイフェーズでは、親テーマの静的コンテンツの生成がデプロイフェーズ中に発生しないように `SCD_NO_PARENT: true` を設定します。 この設定により、デプロイメント時間が最小限に抑えられ、デプロイメント中に静的コンテンツのビルドが失敗した場合に発生する可能性のあるサイトのダウンタイムが回避されます。 [ 静的コンテンツのデプロイメント ](../deploy/static-content.md) を参照してください。

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **デフォルト**—`quick`
- **バージョン** - Adobe Commerce 2.2.0 以降

静的コンテンツの [ デプロイメント戦略 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) をカスタマイズできます。 [ 静的表示ファイルのデプロイ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) を参照してください。

複数のロケールがある場合は、次のオプション _のみ_ を使用します。

- `standard` – すべてのパッケージのすべての静的ビューファイルをデプロイします。
- `quick` - （_デフォルト_）展開時間を最小限に抑えます。
- `compact` - サーバー上のディスク領域を節約します。 Adobe Commerce バージョン 2.2.4 以前では、この設定によって `scd_threads` の値が `1` で上書きされます。

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default** – 自動
- **バージョン** - Adobe Commerce 2.1.4 以降

静的コンテンツのデプロイメントのスレッド数を設定します。 デフォルト値は、検出されたCPU スレッドの数に基づいて設定され、値 4 を超えることはありません。 スレッド数を増やすと、静的コンテンツのデプロイメントが高速化されます。スレッド数を減らすと、速度が低下します。 スレッドの値は、次のように設定できます。

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

デプロイメント時間をさらに短縮するには、`scd-dump` コマンドで [ 設定管理 ](../store/store-settings.md) を使用して、静的デプロイメントをビルドフェーズに移行します。

## `SEARCH_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

この環境変数を使用して、カスタマイズされた検索サービス設定をデプロイメント間で保持します。 例：

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

OpenSearch 設定（Commerce 2.4.6 以降）:

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

次の例では、新しい値を既存の設定に結合します。

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

Redis セッションストレージの設定 セッションストレージ変数の `save`、`redis`、`host`、`port`、`database` の各オプションが必要です。 例：

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

次の例では、新しい値を既存の設定に結合します。

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **デフォルト**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

デプロイフェーズで静的コンテンツのデプロイメントをスキップする場合は、`true` に設定します。

デプロイフェーズでは、静的コンテンツのビルドがデプロイフェーズ中に発生しないように `SKIP_SCD: true` を設定します。 この設定により、デプロイメント時間が最小限に抑えられ、デプロイメント中に静的コンテンツのビルドが失敗した場合に発生する可能性のあるサイトのダウンタイムが回避されます。 [ 静的コンテンツのデプロイメント ](../deploy/static-content.md) を参照してください。

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **デフォルト**—`true`
- **バージョン** - Adobe Commerce 2.1.4 以降

デプロイメント時に、データベース内のAdobe Commerceのベース URL を [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) 変数で指定されたプロジェクト URL に置き換えます。 この設定はローカル開発で役に立ちます。ローカル環境用にベース URL が設定されている場合です。 クラウド環境にデプロイすると、URL が更新され、プロジェクトの URL を使用してストアフロントと管理者にアクセスできるようになります。

Pro または Starter のステージング環境および実稼動環境にデプロイするときに URL を更新する必要がある場合は、[`FORCE_UPDATE_URLS`](#force_update_urls) 変数を使用します。

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

デプロイメントフェーズで実行される CLI コマンドの [Symfony](https://symfony.com/doc/current/console/verbosity.html) debug 冗長レベル `bin/magento` 有効または無効にします。

>[!NOTE]
>
>VERBOSE_COMMANDS 設定を使用して、CLI コマンドの成功と失敗の両方に対するコマンド出力の詳細を制御す `bin/magento` には、[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug` を設定する必要があります。

ログに表示される詳細レベルを選択します。

- `-v`=通常出力
- `-vv`=より詳細な出力
- `-vvv` =デバッグに最適な詳細出力

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
