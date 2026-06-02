---
title: データ収集
description: New RelicでCommerce データ取り込みを表示および管理する方法について説明します。
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 209
ht-degree: 0%

---

# データ収集

New Relicでは、効果的なモニタリングと分析を提供するために豊富なデータを利用します。大規模なデータセットは、タイムリーな結果、パフォーマンス、コンプライアンスに影響を与える可能性があります。 このトピックでは、データの取り込みの管理に関するガイダンスと、データが最も効果的になるようにデータを絞り込む戦略について説明します。

New Relicでは、データソース別にプランの使用状況を要約した&#x200B;_データ管理_ ビューを提供しています。

**取り込んだデータとソースを表示するには**:

1. New Relic ユーザーメニューから、**[!UICONTROL Manage your data]**&#x200B;をクリックします。
1. _管理_ リストの&#x200B;**[!UICONTROL Data management]**&#x200B;をクリックします。

   ![&#x200B; データ管理](../../assets/new-relic/data-ingestion.png)

   「**[!UICONTROL Data ingestion]**」タブには、その日に取り込まれたデータとデータのソースが表示されます。
「データ保持」タブは、データの保存期間を表示および制御します。

1. 「**[!UICONTROL Limits]**」タブを選択し、アカウントの制限を確認します。

Adobe Adobe Commerceのデータソースは、次のとおりです。

- **APM イベント** - チャートとダッシュボードで使用されるイベントデータ
- **インフラストラクチャ** - CPU、ストレージ、ネットワークなどのプロセスおよびホスト指標
- **Logging** - CDN、APM、およびアプリケーションサーバーのログ

ログデータは、取り込みの大部分に影響します。 ログ データを[表示および分析し、Adobe担当者と協力してデータの取り込みと保持ニーズの戦略を策定する方法について説明します。 &#x200B;](log-management.md#view-and-analyze-log-data) [&#x200B; データ収集の管理](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/)について詳しくは、_New Relic ドキュメント_&#x200B;を参照してください。
