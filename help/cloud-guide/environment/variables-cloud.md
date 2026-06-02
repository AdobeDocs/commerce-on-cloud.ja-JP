---
title: クラウド固有の変数
description: Adobe Commerce on cloud infrastructureに固有の環境変数の一覧を参照してください。
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
exl-id: 82923b6f-221d-4902-a1b8-5ba6c7b3339a
TQID: https://experienceleague.adobe.com/Zk52OMqjrB74v9djO1PVOYd3wOS8EbdfL1rnqIdA8B4
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 289
ht-degree: 0%

---

# クラウド固有の変数

クラウドインフラストラクチャ上のAdobe Commerceに固有の環境変数では、`MAGENTO_CLOUD_*`接頭辞が使用されます。

| 変数 | 説明 |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | アプリケーションディレクトリへの絶対パス。 |
| `MAGENTO_CLOUD_APPLICATION` | アプリケーションを記述するbase64 エンコード JSON オブジェクト。 これは`.magento.app.yaml` ファイルの内容にマッピングされ、サブキーを持ちます。 |
| `MAGENTO_CLOUD_APPLICATION_NAME` | `.magento.app.yaml` ファイルで設定されたアプリケーションの名前。 |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Web ドキュメントのルートへの絶対パス（該当する場合）。 |
| `MAGENTO_CLOUD_ENVIRONMENT` | 環境ブランチの名前。 |
| `MAGENTO_CLOUD_PROJECT` | プロジェクト ID。 |
| `MAGENTO_CLOUD_RELATIONSHIPS` | キー（関係名）と値（関係ペアの配列）のエンドポイント定義を表す、base64 エンコードされたJSON オブジェクト。 各関係エンドポイント定義は、URLの分解形式です。 `scheme`、a `host`、a `port`、および&#x200B;_オプションで_、a `username`、`password`、`path`、および一部の追加情報が`query`にあります。 |
| `MAGENTO_CLOUD_ROUTES` | 環境`.magento/routes.yaml` ファイルで定義されたルートを記述します。 |
| `MAGENTO_CLOUD_TREE_ID` | アプリケーションのツリーID。GitのツリーのSHAに対応します。 |
| `MAGENTO_CLOUD_VARIABLES` | キーと値のペア （`"key":"value"`など）を持つbase64 エンコードされたJSON オブジェクト。 |
| `MAGENTO_CLOUD_LOCKS_DIR` | クラウドインフラストラクチャ上のロックプロバイダーのマウントポイントへのパスを提供します。 ロックプロバイダーは、重複したcron ジョブとcron グループの起動を防ぎます。 |

>[!WARNING]
>
>[[!DNL Cloud Console]](../project/overview.md)を使用して環境変数を[&#x200B; オーバーライド構成設定](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html)に追加するには、次の例のように、変数名の前に`env:`を付ける必要があります。
>
>![環境変数の例](../../assets/set-env-variable-ui.png)

値は時間の経過とともに変化する可能性があるため、実行時に変数を検査し、それを使用してアプリケーションを設定するのが最善です。 例えば、`MAGENTO_CLOUD_RELATIONSHIPS`変数を使用して、環境関連の関係を次のように取得します。

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## 環境変数の表示

`ece-tools` パッケージ [&#128279;](../dev-tools/package-overview.md)のから`env:config:show` コマンドを使用して、現在の環境の変数のリストを表示できます。

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

`variables` オプションの出力例：

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
