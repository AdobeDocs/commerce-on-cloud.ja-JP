---
title: 自動スケーリング
description: Adobe Commerce on cloud infrastructureが、どのようにリソースの需要に応じて拡張できるのかをご確認ください。
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 605
ht-degree: 0%

---

# 自動スケーリング

自動スケーリングは、最適なパフォーマンスと合理的なコストを維持するために、クラウドインフラストラクチャにリソースを自動的に追加または削除します。 現在、この機能は、[拡張アーキテクチャ &#x200B;](scaled-architecture.md)で構成されたプロジェクトでのみ使用できます。

## Web サーバーノード

[Web層](scaled-architecture.md#web-tier)は、プロセス要求の増加と高いトラフィック要件に対応するように拡張します。 現在、自動スケーリング機能は、web サーバーノードの追加または削除によって水平方向にのみスケーリングします。

自動スケーリングイベントは、CPUの使用状況とトラフィックがあらかじめ定義されたしきい値に達したときに発生します。

- **ノードが追加されました**：すべてのアクティブなweb ノードのCPU/コアは、1分間は75%の容量で、5分間はトラフィックが20%増加しています。
- **ノードが削除されました**：すべてのアクティブなweb ノードのCPU/コアが60%で20分間ロードされます。 ノードは、追加された順序で削除されます。

最小値と最大値のしきい値は、各加盟店が契約しているリソース制限にもとづいて決定および設定されるので、無限に拡張できるリスクが軽減されます。

## New Relicのしきい値の監視

[New Relic サービス &#x200B;](../monitor/new-relic-service.md)を使用して、ホスト数やCPUの使用状況など、特定のしきい値を監視できます。 次のNew Relic クエリでは、例えば`cluster-id`に対してのみ変数表記を使用しています。

>[!TIP]
>
>クエリの作成に関する詳細は、_New Relic_ ドキュメントの[NRQL構文、句、関数](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/)を参照してください。
> クエリを使用して、[New Relic ダッシュボード &#x200B;](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/)を構築します。

### ホスト数

次のNew Relic クエリの例は、環境内のホスト数を示しています。

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

次のスクリーンショットでは、**APM hosts seen**&#x200B;は、選択した期間中にログに記録されたトランザクションを持つホストの数を指します。

![New Relic ホスト数](../../assets/new-relic/host-count.png)

### CPUの利用状況

次のNew Relic クエリの例は、web ノードに対するCPUの使用状況を示しています。

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic web nodes CPUの使用状況](../../assets/new-relic/web-node-cpu-usage.png)

## 自動スケーリングを有効にする

Adobe Commerce オンクラウド インフラストラクチャ プロジェクトの自動スケーリングを有効または無効にするには、[Adobe Commerce サポート チケットを送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)してください。 チケットで次の理由を選択します。

- **連絡先の理由**: インフラストラクチャの変更リクエスト
- **Adobe Commerce インフラストラクチャの連絡先の理由**：その他のインフラストラクチャの変更リクエスト

>[!IMPORTANT]
>
>自動スケーリング機能は、予期せぬイベントをキャプチャします。 自動スケーリングが有効になっている場合でも、今後のイベントが予想される場合は、[Adobe Commerce サポートチケットを送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)に進むことをお勧めします。

### 負荷テスト

Adobeでは、最初にCloud プロジェクト _ステージング_ クラスターで自動スケーリングが有効になります。 環境で負荷テストを実行して完了すると、Adobeは実稼動クラスターで自動スケーリングを有効にします。 負荷テストに関するガイダンスについては、[&#x200B; パフォーマンステスト &#x200B;](../launch/checklist.md#performance-testing)を参照してください。

### IP許可リストに加える

自動スケーリングを有効にすると、アウトバウンド web ノードトラフィックはサービスノードのIP アドレスから送信されます。 Adobe Commerce on cloud infrastructure プロジェクトにバンドルされていないサードパーティのサービスで許可リストに加える許可リストを使用する場合は、サードパーティのサービスのIP アドレスを確認します。

例：

- 許可リストにサービスノード（1、2、3）のIP アドレスが含まれている場合、操作は必要ありません。
- 許可リストにサービスノード（1、2、3）とweb ノード（4、5、6）のIP アドレスが含まれている場合（この場合は6つのノードすべて）、アクションは必要ありません。
- Web ノード（4、5、6）のIP アドレス _only_&#x200B;が許可リストに含まれている場合は、サービスノードのIP アドレスを含めるように許可リストを更新する必要があります。

