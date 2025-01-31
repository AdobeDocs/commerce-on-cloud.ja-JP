---
title: スマート・ウィザード
description: スマートウィザードを使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceがデプロイメントのベストプラクティスに従っているかどうかを評価する方法を説明します。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# スマート・ウィザード

スマートウィザードを使用すると、クラウド設定がベストプラクティスに従っているかどうかを判断できます。 使用可能なウィザードは、次の構成に役立ちます。

- 導入のダウンタイムを最小限に抑える理想的な状態
- データベースと Redis のロードバランシング設定
- オンデマンド、ビルドステージ、デプロイメントステージの静的コンテンツデプロイメント（SCD）

各スマート・ウィザード・コマンドは、検証応答を提供し、該当する場合は、適切な構成を推奨します。

| コマンド | 説明 |
| ------- | ------------|
| `wizard:ideal-state` | SCD が _ビルド_ ステージにあり、`SKIP_HTML_MINIFICATION` 変数が `true` であり、クラウド環境に post_deploy フックが設定されていることを確認します。 ローカル開発環境では使用されません。 |
| `wizard:master-slave` | `REDIS_USE_SLAVE_CONNECTION` 変数と `MYSQL_USE_SLAVE_CONNECTION` 変数が `true` しいことを確認します。 |
| `wizard:scd-on-demand` | `SCD_ON_DEMAND` グローバル環境変数が `true` になっていることを確認します。 |
| `wizard:scd-on-build` | `SCD_ON_DEMAND` グローバル環境変数が `false` であり、`SKIP_SCD` 環境変数が _ビルド_ ステージ用に `false` されていることを確認します。 `config.php` ファイルにストア、ストア グループ、および Web サイトの情報が含まれていることを確認します。 |
| `wizard:scd-on-deploy` | `SCD_ON_DEMAND` グローバル環境変数が `false` であり、`SKIP_SCD` 環境変数が _deploy_ ステージ用に `false` されていることを確認します。 `config.php` ファイルに、ストア、ストア グループ、および Web サイトの一覧と関連情報が含まれて _ない_ ことを確認します。 |

例えば、お使いの設定で SCD オンデマンド機能が適切に有効になっていることを確認できます。

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

設定が成功すると、次の値が返されます。

```
SCD on-demand is enabled
```

失敗した設定は次を返します。

```
SCD on-demand is disabled
```

## 理想的な設定の検証

クラウドプロジェクトの _理想的_ な設定は、ユーザーからリクエストがあった場合にキャッシュをウォームアップし、静的コンテンツを生成することで、デプロイメントのダウンタイムを最小限に抑えるのに役立ちます。 このウィザードは、配置プロセス中に自動的に実行されます。 クラウドがこの _理想的な状態_ に設定されていない場合は、次のようなメッセージが表示されます。

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

出力に基づいて、設定に次の修正を加える必要があります。

1. 「HTMLをスキップ」縮小変数を有効にします。

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. デプロイ後フックを設定します。

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. コードの変更をプッシュし、テストを再度実行します。 設定が _理想的_ な場合は、次のメッセージが表示されます。

   ```
   Ideal state is configured
   ```
