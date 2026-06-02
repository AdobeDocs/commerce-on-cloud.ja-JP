---
title: Valkey サービスの設定
description: Adobe Commerce on Cloud InfrastructureのバックエンドキャッシュソリューションとしてValkeyを設定および最適化する方法について説明します。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 212
ht-degree: 0%

---

# Valkey サービスの設定

[Valkey](https://valkey.io)は、Adobe Commerceがデフォルトで使用する`Zend Framework Zend_Cache_Backend_File`に代わるオプションのバックエンドキャッシュソリューションです。

_設定ガイド_&#x200B;の「[Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html?lang=ja){target="_blank"}の設定」を参照してください。

{{service-instruction}}

**RedisをValkeyに置き換えるには、次の3つのファイル**&#x200B;で設定を更新します。

1. 必要な名前と種類を`.magento/services.yaml` ファイルに追加します。

   ```yaml
   cache:
       type: valkey:<version>
   ```

   独自のValkey設定を提供するには、`.magento/services.yaml` ファイルに`core_config` キーを追加します。

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. `.magento.app.yaml` ファイルの関係を設定します。

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. `.magento.env.yaml`を次のように設定します。

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [&#x200B; サービス関係を確認します](services-yaml.md#service-relationships)。

{{service-change-tip}}

## Valkey CLIの使用

Valkey関係の名前が`valkey`であると仮定すると、`valkey-cli` ツールを使用してアクセスできます。

1. Valkeyをインストールして設定した統合環境に接続するには、SSHを使用します。

1. ホストへのSSH トンネルを開きます。

   ```bash
   valkey-cli -h valkey.internal
   ```

## インストール済みValkey バージョンを取得

統合環境にインストールされているValkey バージョンを取得するには、次のコマンドを使用します。

```bash
valkey-cli -h valkey.internal info | grep version
```

応答：

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey on Proのステージングと制作

ステージング環境または実稼動環境にインストールされたValkey バージョンを取得するには、`valkey-server` コマンドを使用します。

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

次のコマンドを使用して、Pro ステージング環境または実稼動環境にインストールされたValkey設定を取得します。

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
