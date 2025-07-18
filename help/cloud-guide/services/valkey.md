---
title: Valkey サービスの設定
description: Cloud Infrastructure 上のAdobe Commerceのバックエンドキャッシュソリューションとして Valkey を設定し、最適化する方法について説明します。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
source-git-commit: cf2e659267445603b3f5eaf877f4eb7ac0c1b54c
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Valkey サービスの設定

[Valkey](https://valkey.io) は、Adobe Commerceがデフォルトで使用する `Zend Framework Zend_Cache_Backend_File` に代わるオプションのバックエンドキャッシュソリューションです。

[ 設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html){target="_blank"} の _Valkey の設定_ を参照してください。

{{service-instruction}}

**Redis を Valkey に置き換えるには、次の 3 つのファイルで設定を更新します**

1. 必要な名前とタイプを `.magento/services.yaml` ファイルに追加します。

   ```yaml
   cache:
       type: valkey:<version>
   ```

   独自の Valkey 設定を指定するには、`core_config` ファイルに `.magento/services.yaml` キーを追加します。

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. `.magento.env.yaml` を次のように設定します。

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## Valkey CLI の使用

Valkey 関係の名前が `valkey` の場合は、`valkey-cli` ツールを使用してアクセスできます。

1. SSH を使用して、Valkey がインストールおよび設定された統合環境に接続します。

1. ホストへの SSH トンネルを開きます。

   ```bash
   valkey-cli -h valkey.internal
   ```

## インストール済みの Valkey バージョンの取得

次のコマンドを使用して、統合環境にインストールされている Valkey のバージョンを取得します。

```bash
valkey-cli -h valkey.internal info | grep version
```

応答：

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### ステージング環境および実稼動環境での Valkey

ステージング環境または実稼動環境にインストールされた Valkey のバージョンを取得するには、`valkey-server` のコマンドを使用します。

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

次のコマンドを使用して、Valkey 設定を Pro ステージング環境または実稼動環境にインストールします。

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

応答：

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
