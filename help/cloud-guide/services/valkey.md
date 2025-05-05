---
title: Valkey サービスの設定
description: Cloud Infrastructure 上のAdobe Commerceのバックエンドキャッシュソリューションとして Valkey を設定し、最適化する方法について説明します。
feature: Cloud, Cache, Services
source-git-commit: f73c742cbdbf56ac073802074d5a9cd921591f0f
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Valkey サービスの設定

[Valkey](https://valkey.io) は、Adobe Commerceがデフォルトで使用する `Zend Framework Zend_Cache_Backend_File` に代わるオプションのバックエンドキャッシュソリューションです。

_設定ガイド_ の [Valkey の設定 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html?lang=ja){target="_blank"} を参照してください。

{{service-instruction}}

**Valkey を有効にするには**:

1. 必要な名前とタイプを `.magento/services.yaml` ファイルに追加します。

   ```yaml
   cache:
       type: valkey:<version>
   ```

   独自の Valkey 設定を指定するには、`.magento/services.yaml` ファイルに `core_config` キーを追加します。

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## Valkey CLI の使用

Valkey 関係の名前が `valkey` の場合は、`valkey-cli` ツールを使用してアクセスできます。

1. SSH を使用して、Valkey がインストールおよび設定された統合環境に接続します。

1. ホストへの SSH トンネルを開きます。

   ```bash
   valkey-cli -h valkeycache.internal
   ```

## インストール済みの Valkey バージョンの取得

次のコマンドを使用して、統合環境にインストールされている Valkey のバージョンを取得します。

```bash
valkey-cli -h valkeycache.internal info | grep version
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
