---
title: スマートウィザード
description: スマートウィザードを使用して、Adobe Commerce on cloud infrastructure プロジェクトがデプロイメントのベストプラクティスに従っているかどうかを評価する方法を説明します。
feature: Cloud, Build, Deploy, SCD
exl-id: a9f042cd-861f-4b1c-b80f-2569f12bcde8
TQID: https://experienceleague.adobe.com/hgBYQsTM3WkX2p9SJ4UeWM1IGRwiPOUTwWBEwSnPq5A
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 326
ht-degree: 0%

---

# スマートウィザード

スマートウィザードは、クラウド設定がベストプラクティスに従っているかどうかを判断するのに役立ちます。 使用可能なウィザードは、次の設定に役立ちます。

- 導入のダウンタイムを最小限に抑える理想的な状態
- データベースとRedisの負荷分散設定
- オンデマンド、ビルドステージ、デプロイメントステージのための静的コンテンツデプロイメント（SCD）

各スマートウィザードコマンドは、確認応答と、適切な設定の推奨事項を提供します。

| コマンド | 説明 |
| ------- | ------------|
| `wizard:ideal-state` | SCDが&#x200B;_ビルド_ ステージにあり、`SKIP_HTML_MINIFICATION`変数が`true`、クラウド環境で設定されたpost_deploy フックであることを確認します。 ローカル開発環境では使用できません。 |
| `wizard:master-slave` | `REDIS_USE_SLAVE_CONNECTION`変数と`MYSQL_USE_SLAVE_CONNECTION`変数が`true`であることを確認してください。 |
| `wizard:scd-on-demand` | `SCD_ON_DEMAND` グローバル環境変数が`true`であることを確認してください。 |
| `wizard:scd-on-build` | _ビルド_ ステージの`SCD_ON_DEMAND` グローバル環境変数が`false`で、`SKIP_SCD`環境変数が`false`であることを確認してください。 `config.php` ファイルにストア、ストアグループ、およびweb サイトの情報が含まれていることを確認します。 |
| `wizard:scd-on-deploy` | _デプロイ_ ステージの`SCD_ON_DEMAND` グローバル環境変数が`false`で、`SKIP_SCD`環境変数が`false`であることを確認してください。 `config.php` ファイルに&#x200B;_NOT_&#x200B;がストア、ストアグループ、および関連情報を含むweb サイトのリストが含まれていることを確認します。 |

例えば、設定でSCD オンデマンド機能が適切に有効になっていることを確認できます。

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

設定が成功すると、次の内容が返されます。

```
SCD on-demand is enabled
```

失敗した設定は次の内容を返します。

```
SCD on-demand is disabled
```

## 理想的な設定の検証

クラウドプロジェクトの&#x200B;_ideal_&#x200B;設定は、キャッシュをウォーミングし、ユーザーから要求されたときに静的コンテンツを生成することで、デプロイメントのダウンタイムを最小限に抑えるのに役立ちます。 このウィザードは、デプロイメントプロセス中に自動的に実行されます。 この&#x200B;_理想的な状態_&#x200B;に対してクラウドが設定されていない場合は、次のようなメッセージが表示されます。

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

出力に基づいて、設定に次の修正を加える必要があります。

1. 「HTMLをスキップ」最小化変数を有効にします。

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. デプロイ後のフックを設定します。

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. コードを変更し、テストを再度実行します。 設定が&#x200B;_ideal_&#x200B;の場合、次のメッセージが表示されます。

   ```
   Ideal state is configured
   ```
