---
title: Redis サービスの設定
description: Adobe Commerce on cloud infrastructure用のバックエンドキャッシュソリューションとしてRedisを設定および最適化する方法について説明します。
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# Redis サービスの設定

[Redis](https://redis.io)は、Adobe Commerceがデフォルトで使用する`Zend Framework Zend_Cache_Backend_File`に代わるオプションのバックエンドキャッシュソリューションです。

>[!IMPORTANT]
>
>Redis キャッシュは、2.4.5-p16、2.4.6-p14、2.4.7-p9、および2.4.8-p4以降のAdobe Commerce 2.4.9またはパッチリリースではサポートされていません。 Redisがサポートされていないキャッシュ設定には[Valkey](valkey.md)を使用してください。 リリース別のサポートされているキャッシュサービスについては、[必要システム構成](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)を参照してください。

{{service-instruction}}

## Redisを有効にする

Redisを有効にするには、次のファイルを更新します。

- `.magento/services.yaml`
- `.magento.app.yaml`

### サービスの設定

`.magento/services.yaml`で、Redis サービス定義を追加します。 `<version>`を、お使いのAdobe Commerceのバージョンと現在のCloud テンプレートでサポートされているRedisのバージョンに置き換えます。

```yaml
cache:
  type: redis:<version>
```

例えば、Redis 7.2をサポートするCommerce リリースとCloud テンプレートの場合は、次のようになります。

```yaml
cache:
  type: redis:7.2
```

例のバージョンは普遍的ではありません。 実際のデフォルトバージョンとサポートされているサービスバージョンは、Adobe Commerceのバージョン、パッチレベル、現在のCloud テンプレートによって異なります。 [&#x200B; システム要件](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)と現在のプロジェクトテンプレートでサポートされている組み合わせを確認します。

### サービス関係の設定

`.magento.app.yaml`で、アプリケーションとRedis サービスの関係を設定します。

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

関係キー`redis`は、アプリケーションがサービスにアクセスするために使用する名前です。 値`cache:redis`は、`.magento/services.yaml`で定義されたサービス ID （`cache`）とサービスの種類（`redis`）で構成されています。

### 変更をコミットしてデプロイする

設定の変更を追加、コミット、プッシュします。

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

デプロイメントが完了したら、Redis サービス関係が使用可能であることを確認します。

{{service-change-tip}}

## サービス関係の確認

設定をデプロイしたら、アプリケーションコンテナから次のコマンドを実行して、デコードされた`MAGENTO_CLOUD_RELATIONSHIPS` オブジェクトを表示します。

SSHを使用してリモートクラウド環境に接続し、次を実行します。

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

このコマンドは、設定されたすべてのサービス関係を表示します。 Redis接続の詳細を識別するには、`redis`関係を探します。

次の省略形の例は、`redis`関係を示しています。 ユニバーサルスキーマではありません。

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

出力は、環境とサービス設定によって異なります。 この例では、ホスト名、ポート、IP アドレス、クラスター名、サービスバージョン、ユーザー名、パスワードをハードコーディングしないでください。 ターゲット環境で`MAGENTO_CLOUD_RELATIONSHIPS`によって返される値を使用します。

`jq`が使用可能な場合は、次のコマンドを使用して、Redis関係のみを表示します。

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

サービス関係について詳しくは、[&#x200B; サービスの設定](services-yaml.md)を参照してください。

## Redis設定のカスタマイズ

キャッシュ、セッション、L2、およびレプリカ接続に関する推奨事項については、_実装プレイブックのベストプラクティスガイド_&#x200B;の「[ValkeyとRedis サービス設定のベストプラクティス &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)」を参照してください。

## Redis CLIの使用

Redis関係が`redis`であると仮定すると、`MAGENTO_CLOUD_RELATIONSHIPS`から返されたホストとポートを使用してRedisに接続します。

Redisがインストールおよび設定された環境に接続し、次のコマンドを実行します。

```terminal
redis-cli -h <host> -p <port>
```

**例**

```terminal
redis-cli -h redis.internal -p 6379
```

## インストール済みのRedis バージョンを取得

>[!BEGINTABS]

>[!TAB 統合環境]

統合環境で、`redis`関係によって返されるホストとポートを使用して、次を実行します。

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**応答の例**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

バージョンとビルドの詳細は環境によって異なります。 表示されたサンプルバージョンを必須またはユニバーサルサービスバージョンとして扱わないでください。

>[!TAB Pro ステージングおよび実稼動]

Pro ステージング環境と実稼動環境では、次を実行します。

```terminal
redis-server -v
```

**応答の例**

```text
Redis server v=<installed-version> ...
```

バージョンとビルドの詳細は環境によって異なります。 表示されたサンプルバージョンを必須またはユニバーサルサービスバージョンとして扱わないでください。

>[!ENDTABS]

## Redisのトラブルシューティング

Redisの問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [Adobe Commerceの管理されたアラート：Redis メモリ警告アラート](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Adobe Commerceのマネージドアラート：Redis メモリクリティカルアラート](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### キャッシュクリーンエラーは、Valkey設定のキャッシュでRedisを参照します

`cache` サービスがValkeyとして設定されている場合でも、デプロイ前のキャッシュクリーンのエラーで、エラーコード `[107]` （`clean-redis-cache`）と`Connection to Redis` メッセージが表示される可能性があります。 `ece-tools`は、どのサービスが`cache`関係をサポートするかにかかわらず、キャッシュクリーン手順に対してこの従来のRedis指向エラーコードとメッセージを使用します。そのため、Redisがインストールされていることを示す文言はありません。

関係ホストの`Name or service not known`などのDNS エラーが原因で発生した場合、サービス関係が利用可能になる前にデプロイ手順が実行されるか、`.magento.app.yaml`の関係名が`.magento/services.yaml`のサービス IDと一致しません。 [&#x200B; サービス関係の確認](#verify-the-service-relationship)を参照してください。
