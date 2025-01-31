---
title: クラウド固有の変数
description: クラウドインフラストラクチャー上のAdobe Commerceに固有の環境変数のリストを参照してください。
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# クラウド固有の変数

クラウドインフラストラクチャー上のAdobe Commerceに固有の環境変数には、`MAGENTO_CLOUD_*` のプレフィックスが使用されます。

| 変数 | 説明 |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | アプリケーションディレクトリの絶対パス。 |
| `MAGENTO_CLOUD_APPLICATION` | アプリケーションを記述する、base64 でエンコードされた JSON オブジェクト。 `.magento.app.yaml` ファイルの内容にマッピングされ、サブキーがあります。 |
| `MAGENTO_CLOUD_APPLICATION_NAME` | `.magento.app.yaml` ファイルで設定されたアプリケーションの名前。 |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Web ドキュメントルートの絶対パス（該当する場合）。 |
| `MAGENTO_CLOUD_ENVIRONMENT` | 環境ブランチの名前。 |
| `MAGENTO_CLOUD_PROJECT` | プロジェクト ID。 |
| `MAGENTO_CLOUD_RELATIONSHIPS` | キー（関係名）と値（関係ペアの配列）のエンドポイント定義を表す、base64 でエンコードされた JSON オブジェクト。 各関係エンドポイント定義は、URL を分解した形式です。 `scheme`、`host`、`port` のほか、_オプションで_`username`、`password`、`path`、`query` のいくつかの追加情報が含まれています。 |
| `MAGENTO_CLOUD_ROUTES` | 環境 `.magento/routes.yaml` ファイルで定義されたルートを記述します。 |
| `MAGENTO_CLOUD_TREE_ID` | アプリケーションのツリー ID （Git のツリーの SHA に対応）。 |
| `MAGENTO_CLOUD_VARIABLES` | `"key":"value"` などのキーと値のペアを持つ base64 でエンコードされた JSON オブジェクト。 |
| `MAGENTO_CLOUD_LOCKS_DIR` | クラウドインフラストラクチャ上のロックプロバイダーのマウントポイントへのパスを提供します。 ロックプロバイダーは、重複した cron ジョブや cron グループの起動を防ぎます。 |

>[!WARNING]
>
>[[!DNL Cloud Console]](../project/overview.md) を使用して環境変数を [ 設定を上書き ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) に追加するには、次の例のように、変数名の前に `env:` を付ける必要があります。
>
>![ 環境変数の例 ](../../assets/set-env-variable-ui.png)

値は時間の経過と共に変化する可能性があるので、実行時に変数を調べて、それを使用してアプリケーションを設定することをお勧めします。 例えば、`MAGENTO_CLOUD_RELATIONSHIPS` 変数を使用して、次のように環境に関連する関係を取得します。

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

[`ece-tools` パッケージ ](../dev-tools/package-overview.md) の `env:config:show` コマンドを使用して、現在の環境の変数のリストを表示できます。

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
