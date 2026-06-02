---
title: Commerceのクラウドアーキテクチャ
description: スターターアーキテクチャとPro プロジェクトアーキテクチャをCommerce on Cloud インフラストラクチャと比較する方法について説明します。
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 7c1e895d-0f88-4f11-919a-b3b5748ca5f0
TQID: https://experienceleague.adobe.com/01S8Fhs8J-qy3nc0lXGg3u17h66rF2Qgs2bRG135tVE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75id: f42e0a1a-0d79-488d-a83f-f2c30672b137
subfeature_v2: id: df5e974b-6742-4873-a687-a6bedaafdaa2id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 807
ht-degree: 0%

---

# Commerceのクラウドアーキテクチャ

Adobe Commerce オンクラウドインフラストラクチャには、スタータープランとPro プランがあります。 各プランには、Adobe Commerceの開発およびデプロイメントプロセスを推進するための独自のアーキテクチャが用意されています。 スタータープランとプロプランアーキテクチャの両方で、データベース、web サーバー、キャッシングサーバーを複数の環境に展開し、エンドツーエンドのテストを実施しながら、継続的な統合をサポートします。

比較のために、各プランには以下のインフラストラクチャ機能とサポート対象の製品が含まれています。

|          | スターター | Pro |
| -------- | --------------------| ------------------ |
| 主な機能 | <ul><li>[すべてのAdobe Commerce機能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal オンボーディングツール</li><li>[Commerce レポート ](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[すべてのAdobe Commerce機能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal オンボーディングツール</li><li>[Commerce レポート ](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B モジュール ](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| インフラストラクチャとデプロイメント | <ul><li>無制限のユーザーを持つ継続的なクラウド統合ツール</li><li>Fastlyのコンテンツ配信ネットワーク（CDN）、画像最適化（IO）を活用し、余裕のある帯域幅でセキュリティを強化。 Web Application Firewall （WAF）サービスは、本番環境でのみ利用できます。</li><li>[New Relic](../monitor/new-relic-service.md) APM （Performance Monitoring）、3つのブランチ：`master`および任意の2つ<br>Adobe Commerce用に最適化されたPlatform as a Service （PaaS）の本番環境、ステージング環境、開発環境（合計4つのアクティブ環境）</li><li>出力フィルタリング（アウトバウンドファイアウォール）</li></ul> | <ul><li>無制限のユーザーを持つ継続的なクラウド統合ツール</li><li>Fastlyのコンテンツ配信ネットワーク（CDN）、画像最適化（IO）を活用し、余裕のある帯域幅でセキュリティを強化。 Web Application Firewall （WAF）サービスは、本番環境でのみ利用できます。</li><li>ステージングと実稼動環境の[New Relic](../monitor/new-relic-service.md) Infrastructure on Production + APM （Performance Monitoring）。 Adobe Commerceの[管理済みアラートポリシー](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)は、サイトパフォーマンスに影響を与えるアプリケーションとインフラストラクチャの問題をプロアクティブに通知するためのモニタリングのベストプラクティスを実装します。</li><li>Adobe Commerce用に最適化された[Integration development](pro-architecture.md#integration-environment)環境（合計アクティブ環境2つ）に基づくPaaS （Platform as a Service） ベース</li><li>IaaS （Infrastructure as a Service）：ステージング環境と本番環境用の専用仮想インフラストラクチャ</li></ul> |
| 可用性の高いインフラストラクチャ | | エンタープライズグレードの信頼性と可用性を提供するために、基盤となるInfrastructure as a Service （IaaS）で3台のサーバーをセットアップした[高可用性アーキテクチャ ](pro-architecture.md#redundant-hardware) |
| 専用ハードウェア | | 基盤となるIaaS （Infrastructure as a Service）内の分離された専用ハードウェアにより、より高レベルの信頼性と可用性を提供します |
| 年中無休のメールサポート | コアアプリケーションとクラウドインフラストラクチャの監視と電子メールのサポートを24時間年中無休で提供 | コアアプリケーションとクラウドインフラストラクチャの監視と電子メールのサポートを24時間年中無休で提供 |
| 専任のカスタマーテクニカルアドバイザー（CTA） | | サブスクリプションから最初のサイト立ち上げまでの最初の立ち上げ期間における、専用のテクニカルアカウント管理 |
| アドオン\* | <ul><li>[B2B モジュール ](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> | |

\* _追加料金で利用可能_

## スタータープロジェクト

[ スタータープランのアーキテクチャ ](starter-architecture.md)には、次の4つの環境があります。

- **統合** – 統合環境には2つのテスト可能な環境が用意されています。 各環境には、アクティブなGit ブランチ、データベース、web サーバー、キャッシュ、一部のサービス、環境変数、設定が含まれます。

- **ステージング** - コードと拡張機能がテストに合格すると、`integration` ブランチをステージング環境にマージして、実稼働前のテスト環境にすることができます。 これには、`staging` アクティブなブランチ、データベース、web サーバー、キャッシュ、サードパーティサービス、環境変数、設定、およびFastlyやNew Relicなどのサービスが含まれます。

- **実稼動** - コードの準備とテストが完了すると、すべてのコードが`master`にマージされ、実稼動ライブサイトにデプロイされます。 この環境には、アクティブな`master` ブランチ、データベース、web サーバー、キャッシュ、サードパーティ サービス、環境変数、設定が含まれます。

- **非アクティブ** – 非アクティブな分岐の数は無制限です。

## プロプロジェクト

[Pro プランアーキテクチャ ](pro-architecture.md)には、次の3つの環境を含むグローバル `master`があります。

- **統合** – 統合環境は、データベース、web サーバー、キャッシュ、一部のサービス、環境変数、設定を含むテスト可能な環境を提供します。 ステージング環境にマージする前に、コードを開発、デプロイ、テストできます。

   - _非アクティブ_ - `integration`環境に基づいて非アクティブな分岐を無制限に設定できますが、アクティブな分岐は1つだけです（`integration`を含まない）。

- **ステージング** - ステージング環境はプリプロダクションテスト用で、データベース、web サーバー、キャッシュ、サードパーティサービス、環境変数、設定、およびFastlyなどのサービスが含まれます。

- **実稼動環境** – 実稼動環境には、データ、サービス、キャッシュ、およびストア用の3 ノードの高可用性アーキテクチャが含まれます。 本番環境は、環境変数、設定、サードパーティサービスを備えたライブパブリックストア環境です。

## サポートされているソフトウェアとサービス

Adobe Commerce on cloud infrastructureでは、次の機能を使用します。

- オペレーティングシステム：Debian GNU/Linux
- ウェブサーバー：Nginx
- データベース：MySQL （MariaDB）
- コンテンツ配信ネットワーク（CDN）:Fastly CDN

次のサービスを設定できます。

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [ActiveMQ](../services/activemq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>推奨バージョンについては、_インストールガイド_&#x200B;の[必要システム構成](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)を参照してください。

Fastly CDN モジュールは、ステージング環境と本番環境のCDN サービスとキャッシュサービスに使用されます。 [Fastly サービスの設定](../cdn/fastly.md)を参照してください。

実装で使用するソフトウェアバージョンの設定について詳しくは、次のAdobe Commerce on cloud infrastructure設定ファイルを参照してください。

- [アプリケーション設定（.magento.app.yaml）](../application/configure-app-yaml.md)
- [環境設定（.magento.env.yaml）](../environment/configure-env-yaml.md)
- [ルート設定（routes.yaml）](../routes/routes-yaml.md)
- [サービス設定（services.yaml）](../services/services-yaml.md)
