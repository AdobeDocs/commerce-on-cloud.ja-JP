---
title: Commerceのクラウドアーキテクチャ
description: スタータープロジェクトアーキテクチャと Pro プロジェクトアーキテクチャがクラウドインフラストラクチャー上のCommerceとどのように違うかを説明します。
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Commerceのクラウドアーキテクチャ

クラウドインフラストラクチャー上のAdobe Commerceには、スターターと Pro プランがあります。 各計画には、Adobe Commerceの開発とデプロイメントプロセスを推進する独自のアーキテクチャがあります。 スタータープランと Pro プラン アーキテクチャの両方で、継続的な統合をサポートしながら、複数の環境にわたってデータベース、Web サーバー、およびキャッシュ サーバーを展開し、エンドツーエンドのテストを行います。

比較のために、各プランには次のインフラストラクチャ機能とサポート対象製品が含まれています。

|          | スターター | プロ |
| -------- | --------------------| ------------------ |
| 主な機能 | <ul><li>[ すべてのAdobe Commerce機能 ](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal オンボーディングツール</li><li>[Commerce レポート ](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[ すべてのAdobe Commerce機能 ](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal オンボーディングツール</li><li>[Commerce レポート ](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B モジュール ](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| インフラストラクチャとデプロイメント | <ul><li>ユーザー数に制限のない継続的なクラウド統合ツール</li><li>Fastly コンテンツ配信ネットワーク（CDN）、画像の最適化（IO）および寛大な帯域幅許可によるセキュリティの追加。 Web Application Firewall （WAF）サービスは、実稼動環境でのみ使用できます。</li><li>[New Relic](../monitor/new-relic-service.md)3 つのブランチでの APM （パフォーマンスモニタリング）:`master` および 2 つの任意の環境 <br>Platform as a service （PaaS）の実稼働、ステージングおよび開発環境（合計 4 つのアクティブな環境）がAdobe Commerce用に最適化されています</li><li>エグレスフィルタリング（アウトバウンドファイアウォール）</li></ul> | <ul><li>ユーザー数に制限のない継続的なクラウド統合ツール</li><li>Fastly コンテンツ配信ネットワーク（CDN）、画像の最適化（IO）および寛大な帯域幅許可によるセキュリティの追加。 Web Application Firewall （WAF）サービスは、実稼動環境でのみ使用できます。</li><li>[New Relic](../monitor/new-relic-service.md) 実稼動環境のインフラストラクチャ + ステージング環境と実稼動環境の APM （パフォーマンスモニタリング）。 Adobe Commerce ポリシーの [Managed Alerts policy](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) は、サイトのパフォーマンスに影響を与えるアプリケーションとインフラストラクチャの問題をプロアクティブに通知する監視のベストプラクティスを実装します。</li><li>Adobe Commerce向けに最適化された Platform as a service [PaaS）ベースの統合開発 ](pro-architecture.md#integration-environment) 環境（合計 2 つのアクティブな環境）</li><li>サービスとしてのインフラストラクチャ（IaaS）：ステージング環境および実稼動環境用の専用の仮想インフラストラクチャ</li></ul> |
| 高可用性インフラストラクチャ | | [ 高可用性アーキテクチャ ](pro-architecture.md#redundant-hardware) 基盤となるサービスとしてのインフラストラクチャ（IaaS）に 3 サーバをセットアップし、エンタープライズクラスの信頼性と可用性を提供 |
| 専用ハードウェア | | 基盤となるサービスとしてのインフラストラクチャ（IaaS）に分離された専用ハードウェアにより、より高いレベルの信頼性と可用性を提供 |
| 24 時間年中無休のメールサポート | コアアプリケーションとクラウドインフラストラクチャの 24 時間年中無休の監視およびメールのサポート | コアアプリケーションとクラウドインフラストラクチャの 24 時間年中無休の監視およびメールのサポート |
| 専任のカスタマーテクニカルアドバイザー（CTA） | | 最初のサイトのローンチまで、サブスクリプションから始まる最初のローンチ期間に対する専用のテクニカルアカウント管理 |
| アドオン\* | <ul><li>[B2B モジュール ](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _追加料金がかかります_

## スタータープロジェクト

[ スタータープランアーキテクチャ ](starter-architecture.md) には、次の 4 つの環境があります。

- **統合**：統合環境は、2 つのテスト可能な環境を提供します。 各環境には、アクティブな Git ブランチ、データベース、web サーバー、キャッシュ、一部のサービス、環境変数および設定が含まれます。

- **ステージング** - コードと拡張機能がテストに合格すると、実 `integration` 動前のブランチをステージング環境に結合して、実稼動前のテスト環境にすることができます。 これには、`staging` のアクティブなブランチ、データベース、web サーバー、キャッシング、サードパーティのサービス、環境変数、設定およびサービス（Fastly やNew Relicなど）が含まれます。

- **実稼動** - コードの準備とテストが完了すると、すべてのコードが `master` に結合されて、実稼動ライブサイトにデプロイされます。 この環境には、アクティブな `master` ブランチ、データベース、web サーバー、キャッシュ、サードパーティのサービス、環境変数、設定が含まれます。

- **非アクティブ** – 非アクティブなブランチの数に制限はありません。

## Pro プロジェクト

[Pro プランアーキテクチャ ](pro-architecture.md) には、次の 3 つの環境を持つグローバルな `master` があります。

- **統合**：統合環境は、データベース、Web サーバ、キャッシュ、一部のサービス、環境変数、構成を含む、テスト可能な環境を提供します。 ステージング環境に結合する前に、コードを開発、デプロイ、テストできます。

   - _非アクティブ_ - `integration` 環境に基づいて、非アクティブなブランチの数に制限はありませんが、アクティブなブランチは 1 つだけです（`integration` を含まない）。

- **ステージング** - ステージング環境は実稼動前にテストするためのもので、データベース、web サーバー、キャッシュ、サードパーティのサービス、環境変数、設定、Fastly などのサービスが含まれています。

- **本番環境**：本番環境には、データ、サービス、キャッシュ、ストアに対応する 3 ノードの高可用性アーキテクチャが含まれています。 実稼動環境は、環境変数、設定、サードパーティのサービスを備えたライブのパブリックストア環境です。

## サポート対象のソフトウェアとサービス

クラウドインフラストラクチャー上のAdobe Commerceでは次を使用します。

- オペレーティングシステム：Debian GNU/Linux
- Web サーバー：Nginx
- データベース：MySQL （MariaDB）
- コンテンツ配信ネットワーク（CDN）: Fastly CDN

次のサービスを設定できます。

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>推奨バージョンについては、『 _インストレーション ガイド_ の [ システム要件 ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) を参照してください。

Fastly CDN モジュールは、ステージング環境と実稼動環境で CDN およびキャッシュサービスに使用されます。 詳しくは、[Fastly サービスの設定 ](../cdn/fastly.md) を参照してください。

実装で使用するソフトウェアバージョンの設定について詳しくは、次のクラウドインフラストラクチャー上のAdobe Commerce設定ファイルを参照してください。

- [アプリケーション設定（.magento.app.yaml）](../application/configure-app-yaml.md)
- [環境設定（.magento.env.yaml）](../environment/configure-env-yaml.md)
- [ルートの設定（routes.yaml）](../routes/routes-yaml.md)
- [サービス設定（services.yaml）](../services/services-yaml.md)
