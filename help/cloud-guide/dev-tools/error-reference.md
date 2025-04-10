---
title: ECE-Tools パッケージのエラーメッセージ
description: クラウドインフラストラクチャのビルド、デプロイ、デプロイ後のプロセスで、Adobe Commerce中に発生する可能性のあるエラーコードとメッセージのリストをご覧ください。
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---

# ECE ツールのエラーメッセージ

このエラーメッセージリファレンスでは、クラウドインフラストラクチャー上のAdobe Commerceのビルド、デプロイおよびデプロイ後のプロセスで発生する可能性のあるエラーのトラブルシューティングについて説明します。

デプロイ中に発生する重大なエラーメッセージや警告エラーメッセージはすべて、`var/log/cloud.log` ファイルと `/var/log/cloud.error.log` ファイルの両方に書き込まれます。 クラウドエラーログファイルには、最新のデプロイメントのエラーのみが含まれます。 空のファイルは、エラーのないデプロイメントが成功したことを示します。

`cloud.error.log` ファイルでは、解析しやすいように、各エントリが JSON 文字列としてフォーマットされています。

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

エラーメッセージは、デプロイメントステージ（ビルド、デプロイ、デプロイ後）の 1 つで分類されます。 各セクションには、関連するエラーのリストと、各エラーに関する次の情報が表示されます。

- **エラーコード**:Adobe Commerceによって割り当てられたエラーメッセージの識別情報
- **ステージ**：エラーがビルド、デプロイ、またはデプロイ後のステージで発生したかどうかを示します
- **手順**：エラーを返す可能性のあるデプロイメントシナリオの手順を示します。 _ステップ_ 列が空白の場合、エラーは一般的なエラーであり、複数のステップや前処理の操作中に返される可能性があります。 ビルド、デプロイ、デプロイ後の手順について詳しくは、[ シナリオベースのデプロイメント ](../deploy/scenario-based.md) を参照してください。
- **提案**：エラーのトラブルシューティングと解決のガイダンスを提供します。
- **タイトル（エラーの説明）**：エラーの原因を要約した説明
- **種類**：エラーが重大エラーか警告かを示します

{{$include /help/_includes/automated/ece-tools-error-codes.md}}
