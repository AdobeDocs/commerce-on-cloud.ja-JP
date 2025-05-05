---
title: Redis サービスの設定
description: クラウドインフラストラクチャー上のAdobe Commerceのバックエンドキャッシュソリューションとして Redis を設定し最適化する方法について説明します。
feature: Cloud, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Redis サービスの設定

[Redis](https://redis.io) は、Adobe Commerceがデフォルトで使用する Zend フレームワークの Zend_Cache_Backend_File に代わる、オプションのバックエンドキャッシュソリューションです。

[ 設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html?lang=ja) の _Redis の設定_ を参照してください。

{{service-instruction}}

**Redis を有効にするには**:

1. 必要な名前とタイプを `.magento/services.yaml` ファイルに追加します。

   ```yaml
   myredis:
       type: redis:<version>
   ```

   独自の Redis 設定を指定するには、`.magento/services.yaml` ファイルに `core_config` キーを追加します。

   ```yaml
   cache:
       type: redis:<version>
   ```

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## Redis CLI の使用

Redis 関係の名前が `redis` の場合は、`redis-cli` ツールを使用してアクセスできます。

1. SSH を使用して、Redis がインストールおよび設定された統合環境に接続します。

1. ホストへの SSH トンネルを開きます。

   ```bash
   redis-cli -h redis.internal
   ```

## インストールされた Redis バージョンを取得します。

次のコマンドを使用して、統合環境にインストールされている Redis のバージョンを取得します。

```bash
redis-cli -h redis.internal info | grep version
```

応答の例：

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Pro ステージング環境と実稼動環境

ステージング環境または実稼動環境に Redis バージョンをインストールするには、`redis-server` のコマンドを使用します。

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

次のコマンドを使用して、Redis 設定を Pro ステージング環境または実稼動環境にインストールします。

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

応答の例：

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

## Redis のトラブルシューティング

Redis の問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [Redis 問題により、管理者ログインまたはチェックアウトが遅れる ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html?lang=ja)
- [ 拡張 Redis キャッシュ実装Adobe Commerce 2.3.5 以降 ](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html?lang=ja)
- [Managed alerts on Adobe Commerce:Redis memory warning アラート ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html?lang=ja)
- [Adobe Commerceの管理アラート：Redis メモリクリティカルアラート ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html?lang=ja)
