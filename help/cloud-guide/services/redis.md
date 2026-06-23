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
source-git-commit: bec442a5b442adafbd23c7c8eac1adbdb7b93b65
workflow-type: tm+mt
source-wordcount: 328
ht-degree: 0%

---

# Redis サービスの設定

[Redis](https://redis.io)は、Adobe Commerceがデフォルトで使用するZend Framework Zend_Cache_Backend_Fileに代わるオプションのバックエンドキャッシュソリューションです。

_実装プレイブックのベストプラクティスガイド_&#x200B;の「[Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)の設定」を参照してください。

{{service-instruction}}

**Redis**&#x200B;を有効にするには：

1. 必要な名前と種類を`.magento/services.yaml` ファイルに追加します。

   ```yaml
   myredis:
       type: redis:<version>
   ```

   独自のRedis設定を提供するには、`.magento/services.yaml` ファイルに`core_config` キーを追加します。

   ```yaml
   cache:
       type: redis:<version>
   ```

1. `.magento.app.yaml` ファイルの関係を設定します。

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [&#x200B; サービス関係を確認します](services-yaml.md#service-relationships)。

{{service-change-tip}}

## Redis CLIの使用

Redis関係が`redis`であると仮定すると、`redis-cli` ツールを使用してアクセスできます。

1. Redisをインストールして設定した統合環境に接続するには、SSHを使用します。

1. ホストへのSSH トンネルを開きます。

   ```bash
   redis-cli -h redis.internal
   ```

## インストールされたRedis バージョンを取得

統合環境にインストールされているRedis バージョンを取得するには、次のコマンドを使用します。

```bash
redis-cli -h redis.internal info | grep version
```

回答サンプル：

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Proのステージングと実稼動

ステージング環境または実稼動環境にインストールされたRedis バージョンを取得するには、`redis-server` コマンドを使用します。

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

次のコマンドを使用して、Pro ステージング環境または実稼動環境にインストールされたRedis設定を取得します。

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

回答サンプル：

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Redisのトラブルシューティング

Redisの問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [Redis問題の遅延管理者のログインまたはチェックアウト](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [拡張Redis キャッシュ実装Adobe Commerce 2.3.5以降](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [Adobe Commerceの管理されたアラート：Redis メモリ警告アラート](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Adobe Commerceのマネージドアラート：Redis メモリクリティカルアラート](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
