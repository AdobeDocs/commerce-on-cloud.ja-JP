---
title: 自動スケーリング
description: クラウドインフラストラクチャー上のAdobe Commerceをリソースのニーズに合わせて拡張する方法について説明します。
feature: Cloud, Auto Scaling
topic: Architecture
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# 自動スケーリング

自動スケーリングでは、最適なパフォーマンスと妥当なコストを維持するために、クラウドインフラストラクチャにリソースを自動的に追加または削除します。 現在、この機能は、[ 拡張アーキテクチャ ](scaled-architecture.md) で設定されたプロジェクトでのみ使用できます。

## Web サーバーノード

[Web 層 ](scaled-architecture.md#web-tier) は、プロセスリクエストの増加とトラフィック要件の増加に対応するように拡張できます。 現在、自動スケーリング機能は、web サーバーノードを追加または削除することで水平方向にのみスケーリングできます。

CPUの使用状況とトラフィックが事前定義されたしきい値に達すると、自動スケールイベントが発生します。

- **ノードの追加** – すべてのアクティブな web ノードの CPU/コアは、1 分間は 75% の処理能力で、5 分連続でトラフィックが 20% 増加します。
- **ノードが削除されました** – すべてのアクティブな web ノードの CPU/コアが 60% で 20 分間読み込まれます。 ノードは、追加された順序で削除されます。

最小および最大しきい値は、各マーチャントの契約済みリソース制限に基づいて決定および設定されます。これにより、無限スケーリングのリスクが軽減されます。

## New Relicによるしきい値の監視

[New Relic サービス ](../monitor/new-relic-service.md) を使用して、ホスト数やCPUの使用状況など、特定のしきい値を監視できます。 次のNew Relic クエリでは、`cluster-id` 用の変数表記を例としてのみ使用しています。

>[!TIP]
>
>クエリの作成について詳しくは、_New Relic[ ドキュメントの ](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/)NRQL 構文、句、関数_ を参照してください。
>クエリを使用して、[New Relic ダッシュボード ](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/) を作成します。

### ホスト数

次のNew Relic クエリの例では、環境内のホスト数を示しています。

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

次のスクリーンショットでは **APM ホストが表示されています** は、選択した期間中にトランザクションがログに記録されたホストの数を示しています。

![New Relic ホスト数 ](../../assets/new-relic/host-count.png)

### CPUの使用

次のNew Relic クエリの例は、web ノードに対するCPUの使用状況を示しています。

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic web ノードのCPUの使用状況 ](../../assets/new-relic/web-node-cpu-usage.png)

## 自動スケーリングを有効にする

クラウドインフラストラクチャプロジェクトでAdobe Commerceの自動スケーリングを有効または無効にするには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) します。 チケットで次の理由を選択します。

- **問い合わせ理由**：インフラストラクチャ変更リクエスト
- **Adobe Commerce インフラストラクチャへの問い合わせ理由**：その他のインフラストラクチャの変更リクエスト

>[!IMPORTANT]
>
>自動スケーリング機能は、予期しないイベントをキャプチャします。 Adobe自動スケーリングが有効になっている場合でも、今後のイベントを想定している場合は、引き続き [Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) することをお勧めします。

### 負荷テスト

Adobeでは、最初にクラウドプロジェクト _ステージング_ クラスターで自動スケーリングを有効にします。 環境で負荷テストを実行して完了すると、Adobeは実稼動クラスターで自動スケーリングを有効にします。 負荷テストのガイダンスについては、[ パフォーマンステスト ](../launch/checklist.md#performance-testing) を参照してください。

### IP許可リスト

自動スケーリングを有効にした後、送信 web ノードトラフィックは、サービスノードの IP アドレスから発信されます。 クラウドインフラストラクチャプロジェクトのAdobe Commerce許可リストに加えるにバンドルされていないサードパーティサービスと許可リストを使用する場合は、サードパーティサービスの IP アドレスを確認します。

例：

- 許可リストにサービスノードの IP アドレス（1、2、3）が含まれている場合は、対処は必要ありません。
- 許可リストにサービスノード（1、2、3）と web ノード（4、5、6）の IP アドレスが含まれている場合（この場合、6 つのノードすべて）、アクションは必要ありません。
- 許可リストに Web ノード（4、5、6 _の IP アドレス_ のみ）が含まれている場合は、サービスノードの IP アドレスを含めるように許可リストを更新する必要があります。
