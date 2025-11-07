---
title: ECE-Tools リリースノート
description: ECE-Tools パッケージの最新の改善点のリストを確認してください。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 4f96ed89edbbc148c5558050368d8366bd89053a
workflow-type: tm+mt
source-wordcount: '3286'
ht-degree: 0%

---

# ECE-Tools リリースノート

[ece-tools](https://github.com/magento/ece-tools) パッケージは、クラウドプロジェクトを管理およびデプロイするために設計された一連のスクリプトとツールです。 これらのリリースノートでは、[Commerce用 Cloud Tools Suite](cloud-tools-suite.md) の一部であるこのパッケージの最新の改善点について説明します。

>[!NOTE]
>
>[&#x200B; パッケージの最新リリースへの更新について詳しくは、](../dev-tools/update-package.md)ECE ツールのアップグレード `ece-tools` を参照してください。

`ece-tools` パッケージでは、次のリリースバージョン管理シーケンスを使用します。`200<major>.<minor>.<patch>`

リリースノートには次のものが含まれます。

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 新機能
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) 修正点および改善点

<!--Add release notes below-->

## v2002.2.8 {#latest}

リリース日：2025 年 10 月 8 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**ActiveMQ** - ActiveMQ のサポートが追加されました。<!-- MCLOUD-13770 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**ActiveMQ** – 追加された機能テスト。<!-- MCLOUD-13813 -->


## v2002.2.7

リリース日：2025 年 8 月 7 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **PHP 8.4 の修正** 型の互換性を追加しました。<!-- MCLOUD-13965 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**EOL バリデーター** – 提供終了（EOL）サービスの日付を更新。<!-- MCLOUD-13929 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Valkey**-PHP 8.2 および PHP 8.3 の機能テストを追加しました。<!-- MCLOUD-13610 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**バリデーター**- ECE ツールの警告メッセージを修正しました。<!-- MCLOUD-13896 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**ECE ツール** – 追加された単体テストの改善。<!-- MCLOUD-13838 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**サービスのバリデーター**-Opensearch、MariaDB、PHP の新しいバージョンのサポートが追加されました。<!-- MCLOUD-13923 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Opensearch3**-Opensearch3.<!-- MCLOUD-13763 --> のサポートを追加
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**2.4.4-p7/p12 の Opensearch サポート** – バリデータースクリプトを更新。<!-- MCLOUD-13945 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Opensearch3 テスト** – 追加された機能テスト。<!-- MCLOUD-13769 -->

## v2002.2.6

リリース日：2025 年 6 月 3 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**2.4.8 との互換性の向上** – サードパーティライブラリを更新し、2.4.8 との互換性を向上 <!-- MCLOUD-13707 -->

## v2002.2.5

リリース日：2025 年 5 月 27 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Extended Valkey compatibility** Adobe Commerceの Extended Valkey compatibility<!-- MCLOUD-13595 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**更新された RabbitMQ バリデーター**-RabbitMQ の更新されたバリデーター <!-- MCLOUD-13589 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **MariaDB バリデーターを更新**-MariaDB 10.11 用 ece-tools バリデーターを更新。<!-- MCLOUD-13593 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**拡張 Opensearch2 互換性** 最新の 2.4.4 バージョンと互換性のある Opensearch2 を実現。<!-- MCLOUD-13710 -->

## v2002.2.4

リリース日：2025 年 4 月 24 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**2.4.4/2.4.5の Opensearch2** - Adobe Commerce バージョン 2.4.4/2.4.5での `opensearch2` のサポートに関連する問題を修正しました。<!-- MCLOUD-13607 -->

## v2002.2.3

リリース日：2025 年 4 月 9 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Valkey** Valkey カスタム設定の問題を修正しました。<!-- MCLOUD-13569 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**修正バリデーター**-RabbitMQ 4.0 の修正バリデーター <!-- MCLOUD-13560 -->

## v2002.2.2

リリース日：2025 年 4 月 7 日（PT）

## v2002.2.2

リリース日：2025 年 4 月 7 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Valkey** - Redis に代わる新しいサービス（Valkey）のサポートを追加しました。<!-- MCLOUD-13455 -->
- ![fix icon](../../assets/fix.svg)**Opensearch2 for 2.4.4/2.4.5**—Adobe Commerce バージョン 2.4.4/2.4.5で `opensearch2` のサポートが追加されました。<!-- MCLOUD-13493 -->

## v2002.2.1

リリース日：2024 年 2 月 6 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP 8.4**—PHP 8.4 のサポートを追加しました。<!-- MCLOUD-13145 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Opensearch のバリデーター** – 間違ったバージョンのサービスに関して誤解を招くメッセージを生成したバリデーターを修正しました。<!-- MCLOUD-13184 -->

## v2002.2.0

リリース日：2024 年 10 月 7 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**MariaDB 11.4**- MariaDB 11.4 のサポートを追加しました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**リファクタリングされたコード** – 古い PHP バージョン 7.4、7.3、7.2 および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**アップグレードされた Monolog バージョン** Monolog 3.6.<!-- MCLOUD-12855 --> のサポートが追加されました
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**RabbitMQ、MariaDB、PHP のバリデーター** – 誤ったバージョンのサービスに関して誤解を招くようなメッセージを表示していたバリデーターを修正しました。

## v2002.1.19

リリース日：2024 年 5 月 21 日（PT）

- ![new icon](../../assets/new.svg) **Lua**—CACHE_CONFIGURATION に useLua オプションを追加しました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**バリデーター** – 新しいバージョンの Redis と RabbitMQ のバリデーターを更新しました。

## v2002.1.18

リリース日：2024 年 4 月 8 日（PT）

- ![new icon](../../assets/new.svg) **PHP** — PHP 8.3 のサポートを追加しました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**バリデーター** - EOL バリデーターを更新しました。

## v2002.1.17

リリース日：2024 年 1 月 16 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Elasticsearchおよび OpenSearch のバリデーター** - LiveSearch が有効な場合に、検索サービスをインストールする際に誤解を招くメッセージが表示されるバリデーターを修正しました。<!-- MCLOUD-10167 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **配置警告** – 空でないフォルダに関する配置警告が発生する問題を修正しました。<!-- MCLOUD-8958 -->

## v2002.1.16

リリース日：2023 年 10 月 16 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **ENABLE_WEBHOOK グローバル環境変数** - Commerce Webhook で使用する [ENABLE_WEBHOOK](../environment/variables-global.md#enable_webhooks) グローバル変数が追加され、App Builder ランタイムアクションやサードパーティの在庫管理システムなどの外部エンドポイントに接続できるようになりました。

## v2002.1.15

リリース日：2023 年 7 月 31 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**エラーコード** - エラーコードのスキーマとエラーコードドキュメントジェネレーターを更新しました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**カスタム Redis モデルのバリデーター** – カスタム Redis バックエンドモデルのバリデーターを更新しました。 [&#x200B; キャッシュ設定の例を参照 &#x200B;](../environment/variables-deploy.md#cache_configuration)。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**RabbitMQ のバリデーター**-RabbitMQ 3.11 のサポートを追加
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **間違ったリンクを修正** – お知らせメールテンプレートのオンボーディングドキュメントへの間違ったリンクを修正しました。

## v2002.1.14

リリース日：2023 年 3 月 10 日（PT）

- ![new icon](../../assets/new.svg) **PHP**—PHP 8.2 のサポートを追加しました。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**サービスのバリデータ**—Commerce 2.4.6 のバリデータを更新しました。必要なサービスは、MariaDB 10.6、Redis 7.0、PHP 8.2、OpenSearch 2.x、RabbitMQ 3.9 です。
- ![fix icon](../../assets/fix.svg)**ece-tools db-dump** - `db-dump` 操作が途中で停止する問題を修正しました。

## v2002.1.13

リリース日：2022 年 10 月 27 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Adobe I/O Events for Adobe Commerceのサポートを追加**。 拡張機能の開発者は、[Adobe I/O Events](https://developer.adobe.com/events/docs/) フレームワークを使用して、クラウドインスタンスから [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/) 用に作成されたアプリケーションにCommerce イベント情報を送信できるようになりました。 Adobe CommerceのAdobe I/O Eventsはパートナープレビュー版です。<!-- CEXT-932 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**OPcache 設定のバリデーター** – 除外されたパスの OPcache 設定を確認するバリデーターを追加しました。<!-- MCLOUD-9485 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**GraphQL キャッシュ設定の問題を修正** - ECE-Tools は、`id_salt` ファイルの `cache` 設定にGraphQL `app/etc/env.php` 値を保持するようになりました。<!-- MCLOUD-9486 -->

## v2002.1.12

リリース日：2022 年 9 月 13 日（PT）

- ![new icon](../../assets/new.svg) **Enable`synchronous_replication`**—ECE-Tools は、`synchronous_replication=>true` が有効な場合に `app/etc/env.php` ファイルの `MYSQL_USE_SLAVE_CONNECTION` を設定します。 この設定は、Commerce 2.4.6 以降にのみ影響します。 `MYSQL_USE_SLAVE_CONNECTION` 変数のデプロイ [&#x200B; の &#x200B;](../environment/variables-deploy.md#mysql_use_slave_connection) 変数の説明を参照してください。<!-- MCLOUD-9142 -->
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**OpenSearch** – 次回のAdobe Commerce リリース 2.4.6 用に `opensearch` エンジンを設定および設定する機能が追加されました。[OpenSearch サービスの設定 &#x200B;](../services/opensearch.md) を参照してください <!-- MCLOUD-9236 -->。

## v2002.1.11

リリース日：2022 年 8 月 4 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**ElasticSuite バリデーターと OpenSearch**—OpenSearch がインストールされている場合の ElasticSuite 整合性チェックバリデーターの問題を修正しました。<!-- MCLOUD-8767 -->
- ![fix icon](../../assets/fix.svg)**デプロイ・コマンドの戻り値タイプ** – デプロイ・コマンドの戻り値タイプを修正しました。<!-- AC-3208 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) 新しいCommerce 2.4.5 のインストールに関する **[!DNL RabbitMQ]の問題** – 新しいCommerce 2.4.5 の [!DNL RabbitMQ] のクラッシュの問題を修正しました。インストール。<!-- MCLOUD-9059 -->

## v2002.1.10

リリース日：2022 年 3 月 31 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **Elasticsearch 7.10** - Elasticsearch 7.10 バージョンをサポートするようにバリデーターを更新しました。<!-- MCLOUD-8548 -->

## v2002.1.9

リリース日：2022 年 3 月 10 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**OpenSearch**—Adobe Commerce バージョン 2.4.4、2.4.3-p2 および 2.3.7-p3.<!-- MCLOUD-8296 --> で OpenSearch がサポートされるようになりました。
- ![new icon](../../assets/new.svg) **PHP**—PHP 8.1 のサポートを追加しました。
- ![fix icon](../../assets/fix.svg) **symfony/process** - symfony/process ^5.3 との互換性が追加されました。<!-- MCLOUD-8283 -->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**コンシューマーの複数のプロセス** - コンシューマーごとに発生するプロセスの数を指定できる `multiple_processes` オプションが追加されました。 `CRON_CONSUMERS_RUNNER` 変数のデプロイ [&#x200B; の &#x200B;](../environment/variables-deploy.md#cron_consumers_runner) 変数の説明を参照してください。<!-- MCLOUD-8295 -->
- ![new icon](../../assets/new.svg)**OpenSearch scheme and full host path**—Elasticsearchスキームとフルホストパスを設定する機能が追加されました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **AWS S3** - AWS S3 のイネーブルメント方法を変更しました。
- ![fix icon](../../assets/fix.svg) **Fix driver_options reader**—`env.php` ファイルから DB 接続用の driver_options 設定を読み取り、バリデータを `ece-tools` び出す機能を追加しました。<!-- MCLOUD-8420 -->

## v2002.1.8

リリース日：2021 年 10 月 25 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**代替ダンプ場所** - `--dump-directory` オプションが追加され、DB ダンプのターゲットディレクトリを選択できるようになりました。 `/app/var/dump-main` は、DB ダンプのデフォルトのターゲットディレクトリになりました。 [&#x200B; バックアップ管理：データベースのダンプ &#x200B;](../storage/database-dump.md)<!-- MCLOUD-8063 --> を参照してください。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**モノログを更新** - `monolog` パッケージに必要な最小バージョンを `^2.3` に更新しました。<!-- ACMP-1263 -->
- ![fix icon](../../assets/fix.svg)**Update Symfony** - Adobe Commerce 2.4.4 と互換性を持たせるために Symfony 依存関係を更新しました。<!-- ACMP-1533 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**機能/自動読み込みを解決** – 統合環境にデプロイして `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` エラーが表示される際の問題を修正しました。<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

リリース日：2021 年 7 月 29 日（PT）

**設定の更新**—

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)Composer 2.0.<!--MCLOUD-8003--> のサポートを追加

- ![fix icon](../../assets/fix.svg) **`symphony/console`** のコンポーザー要件の更新 – `composer.json` パッケージの ECE-Tools `symphony/console` バージョン要件を更新し、`di:compile` コマンドが次のエラーで失敗する問題を修正しました。`Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![fix icon](../../assets/fix.svg) 提供終了のソフトウェアチェック（`eol.yaml`）が更新され、Elasticsearch 7.9.x が含まれるようになりました。<!--MCLOUD-7938-->

## v2002.1.6

リリース日：2021 年 4 月 20 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Redis 認証資格情報** - デプロイフェーズで `relationships` プロパティから Redis 認証資格情報を読み取る機能が追加されました。<!--MCLOUD-7694-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Elasticsearch認証資格情報** - デプロイフェーズで `relationships` プロパティからElasticsearch認証資格情報を読み取る機能が追加されました。<!--MCLOUD-7695-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**専用セッションストレージサービス** - セッションストレージの 2 つ目のオプションとして `redis-session` を追加しました。 `redis-session` サービスを使用してセッション情報を保存し、キャッシュに `redis` サービスを使用して、パフォーマンスを向上させることができます。<!--MCLOUD-7698-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**非推奨（廃止予定）の SPLIT_DB メッセージ** - Adobe Commerce 2.4.2 の非推奨（廃止予定）の `SPLIT_DB` オプションと、Adobe Commerce 2.5.0 での削除に関するバリデーターの警告と重要なメッセージを追加しました。<!--MCLOUD-7806-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **関係からのElasticsearch バージョン** - サービスバリデーターを修正して、Cloud Docker および統合環境の `relationships` プロパティから正しいバージョンのElasticsearchを取得しました。<!--MCLOUD-7572-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**柔軟な Redis ポート検証**—`server` URL からカスタムキャッシュ接続のポートを検証できるようになりました。 例えば、`server: 'tcp://rfs-store-simple-page-cache:26379'` のようにポート番号をサーバー URL に追加できます。 これは、`port` オプションがない、または間違っている場合の検証エラーを防ぐのに役立ちます。<!--MCLOUD-7722-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Adobe Commerce 2.4.2 へのアップグレード** - Adobe Commerce 2.4.2 にアップグレードした後、サイトを動作させるために `bin/magento setup:upgrade` を手動で実行する必要がある問題を修正しました。<!--MCLOUD-7776-->

## v2002.1.5

リリース日：2021 年 2 月 1 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**リモートストレージ** - AWS S3 などのストレージサービスを使用してメディアファイルをリモートストレージする Cloud Projects を有効にするための `REMOTE_STORAGE` 環境変数が追加されました。 この設定オプションは ECE-Tools パッケージの一部ですが、クラウドインフラストラクチャー上のAdobe Commerceではサポートされていません。<!--MCLOUD-7153-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新規 `cloud:config:validate` コマンド** – 変更をリモートクラウド環境にプッシュする前に `php vendor/bin/ece-tools cloud:config:validate` 設定を検証するコマンド `.magento.env.yaml` を追加しました。<!--MCLOUD-7120-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**opcache のフラッシュ** – デプロイフックを実行する前に OPcache をフラッシュする `opcache.enable_cli` PHP オプションのサポートが追加されました。 この設定では、キャッシュ設定がリセットされ、各デプロイメントに現在の設定が確実に適用されます。<!--MCLOUD-7015-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **Aurora DB の検証** - Aurora データベースと互換性を持たせるために、データベースサービスの検証を更新しました。<!--MCLOUD-7269-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新規 SCD_NO_PARENT 環境変数** – 親テーマの静的コンテンツの生成を管理するための `SCD_NO_PARENT` 環境変数（Adobe Commerce >=2.4.2 用）が追加されました。<!--MCLOUD-7284-->

- ![fix icon](../../assets/fix.svg)**メモリ制限とコマンド**—`php vendor/bin/ece-tools` ファイルのサイズが PHP の memory_limit を超えると `cloud.log` コマンドが動作しない問題を修正しました。 `cloud.log` ファイル全体をメモリに読み込む代わりに、ログファイルから小さなサブセットのデータのみを読み込むようになりました。<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**カスタムデータベース接続** - `.magento.env.yaml` 用に定義されたカスタムデータベース接続が使用されなかった `DATABASE_CONFIGURATION` 設定の問題を修正しました。 接続設定が `app/etc/env.php`.<!--MCLOUD-7426--> に追加されませんでした

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**空のエラーログ** - `cloud.error.log` が空の場合にデプロイメントが失敗する問題を修正しました。<!--MCLOUD-7296-->

- ![fix icon](../../assets/fix.svg) **MariaDB 10.3 validation**—Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416--> の MariaDB 10.3 の検証を修正しました。

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**キャッシュ :flush ログ** -`cache:flush` の手順の開始と終了を示すようにログエントリを改善しました。<!--MCLOUD-7503-->

## v2002.1.4

リリース日：2020 年 11 月 19 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `SEARCH_CONFIGURATION` 環境変数で指定された検索エンジンが `elasticsearch` 以外の値の場合に、デプロイメントエラーが発生する問題を修正しました。<!--MCLOUD-7283-->

## v2002.1.3

リリース日：2020 年 11 月 9 日（PT）

**インフラストラクチャの更新**—

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) ビルドステージで静的コンテンツがデプロイされるように設定されている場合に、読み取り専用の `pub/static` ディレクトリが ECE ツールでサポートされるようになりました。<!--MC-37699-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 今後のAdobe Commerce リリースとの互換性のために、Elasticsearch 7.9 および Redis 6 のサポートを追加。<!--MCLOUD-7191-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) ECE-Tools `composer.json` を更新し、品質向上パッチツールに必要な依存関係を追加しました。 これにより、ECE-Tools パッケージと magento-cloud-patches パッケージの間に存在していた循環依存関係が修正されます。<!--MCLOUD-6910-->

**検証とログの改善**—

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 検索エンジンの検証が追加され、Cloud Infrastructure 2.4 以降のAdobe Commerceに対して `elasticsearch` が設定されていることを確認できるようになりました。 検証に失敗した場合、デプロイメントは停止され、問題の修正を示唆する重要なエラーメッセージが表示されます。 [&#x200B; 重大なエラー、デプロイステージ &#x200B;](../dev-tools/error-reference.md#deploy-stage) を参照してください。<!--MCLOUD-6937-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) Elasticsearch サービスのバージョンとAdobe Commerceのバージョンの間の互換性を確認するためのElasticsearch検証が追加されました。<!--MCLOUD-7193-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) Adobe Commerce Elasticsearch モジュールと互換性のあるElasticsearchのバージョンを示すように、Elasticsearch互換性エラーメッセージを更新しました。 お使いのバージョンのElasticsearchで使用されるElasticsearch モジュールと互換性を持たせるために、エラーメッセージに、クラウドインフラストラクチャにインストールする特定のAdobe Commerce バージョンが表示されるようになりました。 [&#x200B; 警告エラー、ステージのデプロイ &#x200B;](../dev-tools/error-reference.md#deploy-stage-1) を参照してください <!--MCLOUD-6698-->。

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) 無効な `2026` 環境変数の設定に対する警告エラー `2027` と `MAGE_MODE` を追加しました。 有効な値は `production` のみです。 この修正前は、読み取り専用ファイルに書き込もうとした場合に後でエラーが発生するように、`MAGE_MODE` を配置エラーなしで `developer` に設定できました。 [&#x200B; 警告エラー &#x200B;](../dev-tools/error-reference.md#warning-errors) を参照してください <!--MCLOUD-6708-->。

- ![fix icon](../../assets/fix.svg)Redis、RabbitMQ および MySQL サービスの検証を修正し、これらのバージョンがAdobe Commerceのバージョンと互換性があることを確認しました。 これらのサービスの有効なバージョンが `cloud.log`.<!--MCLOUD-7098--> に書き込まれるようになりました。

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) キャッシュのウォームアップ中に送信リクエストの同時リクエスト制限を含むように `cloud.log` を更新しました。 この値は、デプロイ後の変数 [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) で設定され <!--MCLOUD-5563--> す。

**CLI コマンドの更新**—

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)1 つ以上のビルド、デプロイ、デプロイ後変数を含めることができる設定で `cloud:config:create` ファイルを作成および更新するための CLI コマンド（`cloud:config:update` および `.magento.env.yaml`）が追加されました。 [CLI からの構成ファイルの作成 &#x200B;](../environment/configure-env-yaml.md#create-configuration-file-from-cli) を参照してください <!--MCLOUD-7072-->。

**環境変数の更新**—

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) ビルド変数を追加しました。 変数を `true` に設定すると、Cloud Docker for Commerceのインストール中に、アプリケーションで `composer dump-autoload` コマンドが実行されなくなります。 変数は、書き込み可能なファイルシステム（`./vendor/bin/ece-docker build:compose --with-test` を使用したテストおよび開発用に作成されたもの）を持つ Cloud Docker for Commerce コンテナにのみ関連します。 このようなインストールでは、`composer dump-autoload` コマンドをスキップすると、削除された `generated` ディレクトリからファイルにアクセスしようとする他のコマンドを実行する際にエラーが発生するのを防ぐことができます。<!--MCLOUD-6939-->

## v2002.1.2

リリース日：2020 年 8 月 5 日（PT）

**検証とログの改善**—

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) ビルド、デプロイ、デプロイ後のプロセス中に発生する可能性のあるすべてのエラーおよび警告通知と、エラーを解決するための推奨事項を含む、`schema.error.yaml` ファイルを追加しました。 このファイルの情報は、_Commerceのクラウドガイド_ でも参照できます。 [ece-tools のエラーメッセージのリファレンス &#x200B;](../dev-tools/error-reference.md) を参照してください <!--MCLOUD-5878-->。

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) クラウドエラーログ（`/var/log/cloud.error.log`）のエントリを JSON 形式に変更して、ログをプログラムで解析しやすくしました。<!--MCLOUD-5879-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) ビルド、デプロイ、デプロイ後処理にエラーチェックを追加し、既存のチェックを改善しました。

   - エラーコード 2026 - ビルドフェーズで生成された一部のデータを、マウントされたディレクトリに復元できませんでした

   - エラーコード 3004 - バックアップファイルを作成できない

   - エラーコード 102 - `env.php` ファイルが書き込み可能でない場合に発生する問題に対する追加チェックを追加しました <!--MCLOUD-6221-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **QUALITY_PATCHES** 環境変数を追加し、デプロイメントプロセス中に適用する 1 つ以上の品質向上パッチを指定しました。 [&#x200B; ビルド変数 &#x200B;](../environment/variables-build.md#quality_patches) を参照してください。<!--MCLOUD-6375-->

## v2002.1.1

リリース日：2020 年 6 月 25 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**インフラストラクチャの更新**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**ログの改善** – 重要なデプロイ・エラーに終了コードを割り当て、エラー・メッセージ通知とログ・イベントに終了コードを公開することで、ログ追跡機能を向上しました。 [ece-tools のエラーメッセージのリファレンス &#x200B;](../dev-tools/error-reference.md) を参照してください <!-- MCLOUD-5637, 5531-->。

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) データベースダンプ（`vendor/bin/ece-tools db-dump`）のプロセスを改善し、ログメッセージを更新して、データベースダンプ操作がアプリケーションをメンテナンスモードに切り替え、コンシューマーキュープロセスを停止し、ダンプが開始される前に cron ジョブを無効化することを明確にしました。<!--MCLOUD-5324, MCLOUD-2062-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) ステージング環境と実稼動環境にデプロイする際に、プロジェクト URL が正しく更新されるように問題を修正しました。 `ece-tools` では、プロジェクトのルート設定で設定された `primary:true` 属性を持つルートの URL を使用するようになりました。 [&#x200B; 変数のデプロイ &#x200B;](../environment/variables-deploy.md#update_urls) を参照してください。<!--MCLOUD-5883-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) パッチを適用するための `generate.xml` ビルドシナリオワークフローを更新しました。 `di:compile` および `module:refresh` の手順が失敗する可能性のある問題を修正するには、Adobe Commerceを更新するパッチを事前に適用する必要があります。<!--MCLOUD-5941-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) インストールプロセスで、`Crypt key missing` エラーが誤って返される問題を修正しました。 `crypt/key` の値は、インストール時に自動的に生成されます。<!--MCLOUD-6120-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**サービスのアップデート**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)PHP 7.4 および MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144--> のサポートを追加

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**環境変数の更新**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) クラウドインフラストラクチャー上のAdobe Commerceのビルドプロセス中にJavaScriptのバンドル用に Baler モジュールを有効にするための **SCD_USE_BALER** 変数が追加されました。 [&#x200B; ビルド変数 &#x200B;](../environment/variables-build.md#scd_use_baler) の変数の説明を参照してください。<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![new icon](../../assets/new.svg) Adobe Commerce 2.3.5 以降の Redis キャッシュ用 Redis バックエンドモデルを設定する **REDIS_BACKEND** 環境変数を追加しました。 [&#x200B; 変数のデプロイ &#x200B;](../environment/variables-deploy.md#redis_backend) の変数の説明を参照してください。<!--MCLOUD-5721, MCLOUD-5865-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**CLI コマンドの更新**—

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 以下の CLI コマンドを、より詳細なログを記録するオプションを追加して更新しました。

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     各呼び出しのログレベルは、[`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) ファイルの `.magento.env.yaml` 変数の設定によって決まります。<!--MCLOUD-3503-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**検証の改善**—

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Elasticsearch 7.x 互換性チェック** - Elasticsearch 7.x ソフトウェア互換性チェックのElasticsearch検証を更新しました。<!--MCLOUD-5542-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**更新されたサービスバージョンと EOL 検証チェック** – インストールされているサービスバージョンをAdobe Commerce 2.4 と照合するように検証を更新しました。要件 <!--MCLOUD-6144-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) 検証の問題を修正して、デプロイ後の警告メッセージが `post-deploy` ファイルに `.magento.app.yaml` フック設定がない場合にのみ表示されるようにしました。

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Zend フレームワークの依存関係の検証を追加**—Laminas プロジェクトに移行した Zend フレームワークのコンポーザーの依存関係の検証を追加しました。 必要な依存関係が見つからない場合は、ビルドプロセス中に次のエラーメッセージが表示されます。

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     [Zend フレームワークの依存関係の検証 &#x200B;](../development/commerce-version.md#verify-zend-framework-composer-dependencies) を参照してください <!--MCLOUD-4094-->。

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**ファイルおよびデータの検証 `env.php` 追加** - インストールおよびアップグレードプロセス中に `env.php` ファイルおよびデータのチェックを追加しました。<!--MCLOUD-5991-->

      - `env.php` ファイルがインストールに存在せず、`crypt/key` の値が `.magento.app.yaml` ファイルで指定されていない場合、デプロイメントは次の通知で失敗します。

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - インストールに `env.php` ファイルが含まれていない場合、または構成にキャッシュ タイプが 1 つしか含まれていない場合は、アップグレード プロセス中に `cron:enable` コマンドが実行され、すべての `cache_types` を使用してファイルがリストアされます。 次の通知がログに追加されます。

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

リリース日：2020 年 2 月 6 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**インフラストラクチャの更新**—

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Cloud Docker for Commerce用に個別のパッケージが追加されました**—Docker パッケージを `ece-tools` パッケージから切り離して、コード品質を維持し、独立したリリースを提供します。 `ece-tools` に関連する更新と修正は、[magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub リポジトリ <!--MAGECLOUD-2927--> から管理されます。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**更新されたパッチ適用機能** - パッチ適用機能を ECE-Tools パッケージから別の [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) パッケージに移動しました。 デプロイ時に、`ece-tools` は新しいパッケージを使用してパッチを適用します。 [&#x200B; クラウドパッチのリリースノート &#x200B;](cloud-patches.md) を参照してください。<!--MAGECLOUD-4567-->

   - ![new icon](../../assets/new.svg)**更新された Composer の依存関係** - `composer.json` パッケージの依存関係を使用して、クラウドインフラストラクチャー上のAdobe Commerceの `magento/magento-cloud-docker` ファイルを更新しました。 現在は、`ece-tools` ージ内のすべて [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md) パッケージの依存関係が含まれています。 これらのパッケージは、のインストールまたはアップデート時に自動的にインストールおよびアップデートさ `ece-tools` ます。

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**シナリオベースのデプロイメントのサポート**—<!--MAGECLOUD-4101-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)XML 設定ファイルを使用して、ビルド、デプロイ、デプロイ後のプロセスをカスタマイズし、デフォルトの設定を上書きまたはカスタマイズできるようになりました。

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **`hooks` の`.magento.app.yaml`** 設定を変更 – シナリオベースのデプロイメントをサポートするために、`hooks` 設定形式を更新しました。 以前の ECE-Tools 2002.0.x リリースのレガシー形式は、引き続きサポートされます。 ただし、シナリオベースのデプロイメント機能を使用するには、新しい形式に更新する必要があります。 [&#x200B; シナリオベースのデプロイメント &#x200B;](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks) を参照してください。

>[!NOTE]
>
>ECE-Tools バージョン 2002.1.0 に更新する前に、[backward   互換性のない変更 &#x200B;](backward-incompatible-changes.md) 次の操作が必要になる可能性のある変更について説明します   クラウドインフラストラクチャプロジェクトの設定またはプロセスに関するAdobe Commerceの更新。

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**サービスのアップデート**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)PHP 7.3.<!--MAGECLOUD-4022--> のサポートを追加

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)RabbitMQ 3.8.<!--MAGECLOUD-4674--> のサポートを追加

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) 各サービスの EOL 日付に対してインストール済みのサービスバージョンを確認する検証が追加されました。 現在は、サービスのバージョンが提供終了（EOL）日から 3 か月以内の場合は通知が届き、提供終了（EOL）日が過去の場合は警告が届きます。<!--MAGECLOUD-4076-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)Elasticsearch設定の問題を修正して、すべての環境でElasticsearchが正しく設定されるようにしました。<!--MAGECLOUD-4474-->

>[!NOTE]
>
>クラウドインフラストラクチャ上のAdobe Commerceで使用されるサービスのリストと、クラウドテンプレートとバージョンの互換性については、[&#x200B; サービスバージョン &#x200B;](../services/services-yaml.md#service-versions) を参照してください。

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**環境変数の更新**—

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 特定の製品ページのキャッシュのプリロードをサポートするために、`WARM_UP_PAGES` 環境変数の機能が拡張されました。 「[&#x200B; デプロイ後変数 &#x200B;](../environment/variables-post-deploy.md#warm_up_pages) トピックの展開された定義を参照してください。<!--MAGECLOUD-4444-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) `ERROR_REPORT_DIR_NESTING_LEVEL` 環境変数を追加して、`<magento_root>/var/report/` ディレクトリでのエラーレポートデータ管理を簡素化しました。 「[&#x200B; ビルド変数 &#x200B;](../environment/variables-build.md#error_report_dir_nesting_level)」トピックの変数の説明を参照してください。

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `SCD_EXCLUDE_THEMES`、`STATIC_CONTENT_THREADS`、`DO_DEPLOY_STATIC_CONTENT` および `STATIC_CONTENT_SYMLINK` 環境変数を削除しました。 [&#x200B; 後方互換性のない変更 &#x200B;](backward-incompatible-changes.md#environment-configuration-changes) を参照してください。<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)Elastic Suite 設定プロセスの問題を修正して、`ELASTICSUITE_CONFIGURATION` オプションを使用せずに `_merge` デプロイ変数を設定した場合に、デフォルト設定が期待どおりに上書きされるようにしました。<!--MAGECLOUD-4388-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**CLI コマンドの更新**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新規 cron コマンド** - `cron:disable` および `cron:enable` コマンドを使用して、クラウドインフラストラクチャ環境のAdobe Commerceで cron 処理を手動で管理できるようになりました。 disable コマンドを使用して、アクティブな cron プロセスをすべて停止し、すべての cron ジョブを無効にします。 準備が整ったら enable コマンドを使用して、cron ジョブを再度有効にします。 [cron ジョブの無効化 &#x200B;](../application/crons-property.md#disable-cron-jobs) を参照してください。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**エラー・レポート作成機能の向上** - ECE ツールの処理中に発生する CLI コマンドのエラーに関するログ作成機能が向上しました。<!--MAGECLOUD-4849-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**非推奨のビルドコマンドを削除** – 次のビルドコマンドを削除しました。`m2-ece-build`、`m2-ece-deploy`、`m2-ece-scd-dump`、および名前を変更した `ece-tools docker` コマンドを `ece-docker` に変更しました。 [&#x200B; 後方互換性のない変更 &#x200B;](backward-incompatible-changes.md)<!--MAGECLOUD-4392--> を参照してください。

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) 非推奨の `build_options.ini` ファイルを削除し、ファイルが存在する場合にビルドに失敗する検証を追加しました。 [.magento.env.yaml](../environment/configure-env-yaml.md) ファイルを使用して、ビルドオプションを設定します。

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `config.php` ファイルが空の場合にビルドプロセスが失敗する問題を修正しました。<!--MAGECLOUD-4127-->

## 2002.0.23

リリース日：2020 年 2 月 27 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `ece-tools` 2002.0.x リリースとの互換性の問題を修正しました。実稼動モードでオンデマンドの静的コンテンツ生成が正常に完了しませんでした。

## 以前のリリース

バージョン 2002.0.22 以前については、[&#x200B; リリースノートアーカイブ &#x200B;](cloud-release-archive.md) を参照してください。
