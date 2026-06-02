---
title: ECE-Tools パッケージのエラーメッセージ
description: Adobe Commerce on cloud infrastructureのビルド、デプロイ、デプロイ後のプロセス中に発生する可能性のあるエラーコードとメッセージの一覧を参照してください。
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
TQID: https://experienceleague.adobe.com/FpWmyt0POi3RgKQOf6ihQQVV6GazxRByu-4HdPIhTPY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 253
ht-degree: 0%

---

# ECE-Toolsのエラーメッセージ

このエラーメッセージのリファレンスでは、Adobe Commerce on cloud infrastructureのビルド、デプロイ、デプロイ後のデプロイプロセス中に発生する可能性のあるエラーのトラブルシューティングに関する情報を提供します。

デプロイメント中に発生するすべての重大なエラーメッセージと警告エラーメッセージは、`var/log/cloud.log`と`/var/log/cloud.error.log` ファイルの両方に書き込まれます。 クラウドエラーログファイルには、最新のデプロイメントのエラーのみが含まれます。 空のファイルは、エラーのないデプロイメントが成功したことを示します。

`cloud.error.log` ファイルでは、各エントリは解析しやすいようにJSON文字列としてフォーマットされます。

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

エラーメッセージは、デプロイメントの段階（ビルド、デプロイ、デプロイ後）によって分類されます。 各セクションには、関連するエラーのリストと、各エラーに関する次の情報が表示されます。

- **エラーコード**：エラーメッセージのAdobe Commerce割り当てID
- **ステージ**：ビルド、デプロイ、またはデプロイ後のステージでエラーが発生したかどうかを示します
- **手順**: エラーを返すデプロイメントシナリオの手順を示します。 _手順_&#x200B;列が空白の場合、エラーは一般的なエラーであり、複数の手順または前処理処理中に返すことができます。 ビルド、デプロイ、デプロイ後の手順について詳しくは、[&#x200B; シナリオベースのデプロイメント &#x200B;](../deploy/scenario-based.md)を参照してください。
- **提案**: エラーのトラブルシューティングと解決に関するガイダンスを提供します
- **タイトル（エラーの説明）**：エラーの原因を要約する説明
- **Type**: エラーが重大なエラーであるか、警告であるかを示します

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
