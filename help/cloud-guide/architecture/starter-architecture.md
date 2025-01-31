---
title: スターターアーキテクチャ
description: スターターアーキテクチャがサポートする環境について説明します。
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# スターターアーキテクチャ

Adobe Commerce on cloud infrastructure スターターアーキテクチャは、初期プロジェクトコードを含む `master` 環境、ステージング環境、および最大 2 つの統合環境を含む、最大 **4** の環境をサポートします。

すべての環境は、PaaS （Platform as a service）コンテナ内にあります。 これらのコンテナは、サーバーのグリッド上にある、制約の厳しいコンテナ内にデプロイされます。 これらの環境は読み取り専用で、ローカルワークスペースからプッシュされたブランチからデプロイされたコードの変更を受け入れます。 各環境は、データベースと Web サーバーを提供します。

任意の開発手法やブランチ手法を使用できます。 プロジェクトへの初期アクセス権を取得したら、`master` 環境から `staging` 環境を作成します。 次に、`staging` から分岐して `integration` 環境を作成します。

## スターター環境のアーキテクチャ

次の図は、スターター環境の階層関係を示しています。

![ スタータープロジェクトの概要 ](../../assets/starter/architecture.png)

## 実稼動環境

実稼動環境は、Adobe Commerceをクラウドインフラストラクチャにデプロイするためのソースコードを提供します。このインフラストラクチャは、一般向けの単一およびマルチサイトのストアフロントを実行します。 実稼動環境では、`master` ブランチのコードを使用して、web サーバー、データベース、設定済みサービスおよびアプリケーションコードを設定して有効にします。

`production` 環境は読み取り専用なので、`integration` 環境を使用してコードを変更し、アーキテクチャ全体を `integration` から `staging`、最後に `production` 環境にデプロイします。 詳しくは [ ストアのデプロイ ](../deploy/staging-production.md) および [ サイトのローンチ ](../launch/overview.md) を参照してください。

Adobeでは、`production` 環境にデプロイする `master` ブランチにプッシュする前に、`staging` ブランチで完全にテストすることをお勧めします。

## ステージング環境

Adobeでは、`master` から `staging` というブランチを作成することをお勧めします。 `staging` ブランチは、コード、モジュールと拡張機能、支払いゲートウェイ、配送、製品データなどをテストするための実稼動前環境を提供するために、コードをステージング環境にデプロイします。 この環境は、Fastly、New Relic APM、検索を含む、実稼動環境に一致するすべてのサービスの設定を提供します。

このガイドの追加セクションでは、安全なステージング環境での最終コードデプロイメントと実稼動レベルのインタラクションのテストに関する手順を説明します。 最高のパフォーマンスと機能のテストを実現するには、データベースをステージング環境にレプリケートします。

>[!WARNING]
>
>Adobeでは、実稼動環境にデプロイする前に、ステージング環境でマーチャントと顧客のインタラクションをすべてテストすることをお勧めします。 詳しくは [ ストアのデプロイ ](../deploy/staging-production.md) および [ デプロイメントのテスト ](../test/staging-and-production.md) を参照してください。

## 統合環境

開発者は、`integration` の環境を使用して、開発、デプロイ、テストを行います。

- Adobe Commerce アプリケーションコード

- カスタムコード

- 拡張機能

- サービス

**推奨されるユースケース：**

統合環境は、限定的なテストと開発のために設計されています。 例えば、統合環境を使用して、次のタスクを実行できます。

- 継続的統合（CI）プロセスへの変更がクラウドと互換性があることを確認します

- ホーム、カテゴリ、製品詳細ページ（PDP）、チェックアウト、管理者などの主要なページでの重要なワークフローのテスト

統合環境で最高のパフォーマンスを得るには、次のベストプラクティスに従います。

- カタログサイズの制限 – 参考までに、サンプルデータには約 2,048 個の製品が含まれています。 カタログサイズを約 4,000～5,000 の製品に減らしてみてください。
カタログ内の製品数を確認するには、次の MySQL クエリを実行します。

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- 顧客グループの数を減らす – 顧客グループが多すぎると、インデックス作成のパフォーマンスや全体的なパフォーマンスに影響を与える可能性があります。

- 使用を 1 人または 2 人の同時ユーザーに制限する

- cron ジョブを無効にし、必要に応じて手動で実行する

最大 **2 つのアクティブな統合環境を持つこ** ができます。 統合環境を作成するには、`staging` ブランチからブランチを作成します。 統合環境を作成する場合、環境名はブランチ名と一致します。 統合環境は、ウェブサーバ及びデータベースを含む。 すべてのサービスが含まれるわけではありません（例：Fastly CDN とNew Relicは利用できません）。

コードストレージ用の非アクティブなブランチの数に制限はありません。 非アクティブなブランチにアクセス、表示、テストするには、ブランチをアクティベートする必要があります

{{enhanced-integration-envs}}

## 実稼動とステージングのテクノロジースタック

実稼動環境とステージング環境には、次のテクノロジーが含まれます。 これらのテクノロジーは、[`.magento.app.yaml`](../application/configure-app-yaml.md) ファイルを通じて変更および設定できます。

- Fastly （HTTP キャッシュおよび CDN の場合）
- 複数のワーカーを持つ 1 つのインスタンスである PHP-FPM と通信する Nginx web サーバー
- Redis サーバー
- Adobe Commerce 2.2 から 2.4.3-p2 へのカタログ検索のElasticsearch
- OpenSearch でAdobe Commerce 2.3.7-p3、2.4.3-p2、2.4.4 以降のカタログ検索
- エグレスフィルタリング（アウトバウンドファイアウォール）

### サービス

現在、クラウドインフラストラクチャー上のAdobe Commerceは、PHP、MySQL （MariaDB）、Elasticsearch（Adobe Commerce 2.2 から 2.4.3-p2）、OpenSearch （2.3.7-p3、2.4.3-p2、2.4.4 以降）、Redis および [!DNL RabbitMQ] のサービスをサポートしています。

各サービスは、個別の安全なコンテナで実行されます。 コンテナは、プロジェクトで一緒に管理されます。 次のような、一部のサービスは標準です。

- HTTP ルーター（受信リクエストの処理だけでなく、キャッシュとリダイレクトも処理）

- PHP アプリケーションサーバー

- Git

- セキュアシェル（SSH）

### ソフトウェアバージョン

クラウドインフラストラクチャー上のAdobe Commerceは、Debian GNU/Linux オペレーティングシステムと NGINX web サーバーを使用します。 このソフトウェアはアップグレードできませんが、次のバージョンを構成できます。

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

ステージング環境と実稼動環境では、CDN とキャッシュに Fastly を使用します。 最新バージョンの Fastly CDN 拡張機能は、プロジェクトの初期プロビジョニング時にインストールされます。 拡張機能をアップグレードすると、最新のバグ修正と改善を取得できます。 [Magento 2 用 Fastly CDN モジュール ](https://github.com/fastly/fastly-magento2) を参照してください。 また、パフォーマンス監視のために ](../monitor/account-management.md)0}New Relic} にアクセスできます。[

次のファイルを使用して、実装環境で使用するソフトウェアバージョンを設定します。

- [&#39;.magento.app.yaml&#39;](../application/configure-app-yaml.md)

- [&#39;routes.yaml&#39;](../routes/routes-yaml.md)

- [&#39;services.yaml&#39;](../services/services-yaml.md)

### バックアップと障害回復

[!DNL Cloud Console] または CLI を使用して、データベースとファイル・システムのバックアップを作成できます。 [ バックアップ管理 ](../storage/snapshots.md) を参照してください。

## 開発の準備

次のワークフローは、コードを分岐し、ストアを開発およびデプロイするプロセスの概要を示しています。

1. ローカル環境の設定

1. `master` ブランチをローカル環境にクローンします。

1. `master` からの `staging` ブランチの作成

1. `staging` からの開発用ブランチの作成

1. テスト用の環境を構築およびデプロイするコードを Git にプッシュ

ストアを開発、テスト、デプロイするための詳細な手順と手順については、次の節を参照してください。

- [スターターの開発およびデプロイワークフロー](starter-develop-deploy-workflow.md)

- [Dockerローカル開発](../dev-tools/cloud-docker.md) （Cloud Docker for Commerceで有効化された開発環境）

- [ブランチの管理](../project/console-branches.md)

- [ストアのデプロイ](../deploy/staging-production.md)

- [サイトの起動](../launch/overview.md)
