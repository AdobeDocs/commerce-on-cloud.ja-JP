---
title: パフォーマンス監視
description: Adobe Commerce on cloud infrastructureのパフォーマンスモニタリングについて説明します。
feature: Cloud, Observability
topic: Performance
exl-id: 51e371cf-8374-4dae-8e5a-f31477dc59c1
TQID: 'https://experienceleague.adobe.com/m90Dn1QULOga6Q-Rlq6bvfYkN2FXd3k1GRkpAvayuig'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 4239b8a6-e74f-567d-a7a5-b98b9ead0ea4
    internal-label: Observability
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%
---
# パフォーマンス監視

次のサービスを使用して、クラウド環境のパフォーマンスを監視できます。

- [&#x200B; ヘルス通知](../integrations/health-notifications.md) - StarterおよびPro統合環境でディスク容量アラートを提供する統合。
- [Adobe Commerce向け観察ガイド &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/observation-for-adobe-commerce/intro)：複数のソースからのログデータを組み合わせたすべてのCommerce サイトを包括的にモニタリングして、Adobe Commerce サイトのパフォーマンスをより適切に管理し、問題を診断します。
- [運用テレメトリ サービス &#x200B;](operational-telemetry.md) - ストアフロントのクライアント側トラフィックのパフォーマンス監視。

## New Relic サービス

[Adobe Commerce サービスを使用して、ステージング環境と実稼動環境でNew Relicのパフォーマンスを監視してパフォーマンスを監視](new-relic-service.md)。

- [Adobe Managed Alerts ポリシー](investigate-performance.md#monitor-performance-with-managed-alerts) – 様々なパフォーマンス指標を追跡し、サイト インフラストラクチャまたはアプリケーションの問題がパフォーマンスに影響を与える場合に、トリガーの警告とクリティカル通知のしきい値を設定するアラートのコレクションを含むポリシー。
