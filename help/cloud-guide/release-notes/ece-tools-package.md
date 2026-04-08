---
title: ECE-Tools リリースノート
description: ECE-Tools パッケージの最新の機能強化の一覧を参照してください。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: b3d634838e562ceba4221a69e87eda377d8f9363
workflow-type: tm+mt
source-wordcount: '3485'
ht-degree: 0%

---

# ECE-Tools リリースノート

[ece-tools](https://github.com/magento/ece-tools) パッケージは、クラウドプロジェクトの管理とデプロイ用に設計されたスクリプトとツールのセットです。 このリリースノートでは、Commerce向け[Cloud Tools Suite ](cloud-tools-suite.md)に含まれるこのパッケージの最新の機能強化について説明します。

>[!NOTE]
>
>`ece-tools` パッケージの最新リリースへの更新について詳しくは、[ECE-Toolsのアップグレード ](../dev-tools/update-package.md)を参照してください。

`ece-tools` パッケージでは、次のリリースバージョン管理シーケンスが使用されています：`200<major>.<minor>.<patch>`

リリースノートには以下が含まれます。

- ![新しいアイコン ](../../assets/new.svg)新機能
- ![修正アイコン ](../../assets/fix.svg)修正と機能強化

<!--Add release notes below-->

## v2002.2.10 {#latest}

リリース日：2026年3月5日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.5** - PHP 8.5のサポートを追加しました。<!-- MCLOUD-14179 -->
- ![ アイコンを修正](../../assets/fix.svg) **機能テストを更新**-Valkey 8.1、8,2およびRedis 8.4 テストを更新<!-- MCLOUD-13983 -->
- ![ アイコンを修正](../../assets/fix.svg) **MariaDB、PHPおよびOpensearch バリデーター**&#x200B;を更新しました。バリデーターのスクリプトを更新しました。<!-- MCLOUD-14574/MCLOUD-14466 -->
- ![ アイコンを修正](../../assets/fix.svg) **EOL バリデーター** – 更新済み提供終了（EOL） サービスの日付。<!-- MCLOUD-14272 -->

## v2002.2.9

リリース日：2025年11月13日（PT）

- ![ アイコンを修正](../../assets/fix.svg) **Symfony パッケージ** – 最新のSymfony YAML パッケージのサポートを追加しました。<!-- MCLOUD-14020 -->
- ![修正アイコン ](../../assets/fix.svg) **アクティブなサービスのキャッシュのクリーニングを修正** - アクティブなサービスの検証を追加しました。<!-- MCLOUD-14166 -->

## v2002.2.8

リリース日：2025年10月8日（PT）

- ![新しいアイコン ](../../assets/new.svg) **ActiveMQ**-ActiveMQのサポートが追加されました。<!-- MCLOUD-13770 -->
- ![新しいアイコン ](../../assets/new.svg) **ActiveMQ** – 追加された機能テスト。<!-- MCLOUD-13813 -->


## v2002.2.7

リリース日：2025年8月7日（PT）

- ![修正アイコン ](../../assets/fix.svg) **PHP 8.4の修正** – 追加されたタイプの互換性<!-- MCLOUD-13965 -->
- ![ アイコンを修正](../../assets/fix.svg) **EOL バリデーター** – 更新済み提供終了（EOL） サービスの日付。<!-- MCLOUD-13929 -->
- ![新しいアイコン ](../../assets/new.svg) **Valkey**&#x200B;がPHP 8.2とPHP 8.3の機能テストを追加しました。<!-- MCLOUD-13610 -->
- ![ アイコンの修正](../../assets/fix.svg) **Valkey validator**-ECE ツールの警告メッセージを修正しました。<!-- MCLOUD-13896 -->
- ![ アイコンの修正](../../assets/fix.svg) **ECE ツール** – 追加された単体テストの改善。<!-- MCLOUD-13838 -->
- ![新しいアイコン ](../../assets/new.svg) **サービス用バリデーター**-Opensearch、MariaDB、PHPの新しいバージョンのサポートを追加しました。<!-- MCLOUD-13923 -->
- ![新しいアイコン ](../../assets/new.svg) **Opensearch3**-Opensearch3のサポートが追加されました。<!-- MCLOUD-13763 -->
- ![ アイコンを修正](../../assets/fix.svg) **2.4.4-p7/p12**&#x200B;のOpensearch サポート – 検証スクリプトを更新しました。<!-- MCLOUD-13945 -->
- ![新しいアイコン ](../../assets/new.svg) **Opensearch3 テスト** – 追加された機能テスト。<!-- MCLOUD-13769 -->

## v2002.2.6

リリース日：2025年6月3日（PT）

- ![ アイコンの修正](../../assets/fix.svg) **2.4.8**&#x200B;との互換性を向上させるために、2.4.8<!-- MCLOUD-13707 -->との互換性を向上させるために、サードパーティ製ライブラリを更新しました

## v2002.2.5

リリース日：2025年5月27日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Adobe CommerceでのValkeyの互換性の拡張**&#x200B;とValkeyの互換性の拡張<!-- MCLOUD-13595 -->
- ![ アイコンの修正](../../assets/fix.svg) **更新されたRabbitMQ バリデーター** – 更新されたRabbitMQ バリデーター<!-- MCLOUD-13589 -->
- ![ アイコンを修正](../../assets/fix.svg) **MariaDB バリデーター**&#x200B;を更新 – MariaDB 10.11のece-tools バリデーター<!-- MCLOUD-13593 -->を更新しました
- ![ アイコンを修正](../../assets/fix.svg) **拡張Opensearch2互換性**&#x200B;最新の2.4.4 バージョンと互換性のあるOpensearch2を作成しました。<!-- MCLOUD-13710 -->

## v2002.2.4

リリース日：2025年4月24日（PT）

- ![fix icon](../../assets/fix.svg) **Opensearch2 for 2.4.4/2.4.5**—Adobe Commerce バージョン 2.4.4/2.4.5の`opensearch2`のサポートに関する問題を修正しました。<!-- MCLOUD-13607 -->

## v2002.2.3

リリース日：2025年4月9日（PT）

- ![ アイコンを修正](../../assets/fix.svg) **Valkeyを修正** Valkey カスタム設定の問題を修正しました。<!-- MCLOUD-13569 -->
- ![ アイコンを修正](../../assets/fix.svg) **検証ツールを修正**-RabbitMQ 4.0の検証ツールを修正しました。<!-- MCLOUD-13560 -->

## v2002.2.2

リリース日：2025年4月7日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Valkey** - Redisの代替となる新しいサービス（Valkey）のサポートを追加しました。<!-- MCLOUD-13455 -->
- ![fix icon](../../assets/fix.svg) **Opensearch2 for 2.4.4/2.4.5** - Adobe Commerce バージョン 2.4.4/2.4.5の`opensearch2`のサポートを追加しました。<!-- MCLOUD-13493 -->

## v2002.2.1

リリース日：2024年2月6日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.4** - PHP 8.4のサポートを追加しました。<!-- MCLOUD-13145 -->
- ![ アイコンを修正](../../assets/fix.svg) **Opensearch用バリデーター** – 間違ったバージョンのサービスに関する誤解を招くメッセージを生成するバリデーターを修正しました。<!-- MCLOUD-13184 -->

## v2002.2.0

リリース日：2024年10月7日（PT）

- ![新しいアイコン ](../../assets/new.svg) **MariaDB 11.4** - MariaDB 11.4のサポートを追加しました。
- ![修正アイコン ](../../assets/fix.svg) **リファクタリングされたコード** – 古いPHP バージョン 7.4、7.3、7.2および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 -->
- ![ アイコンを修正](../../assets/fix.svg) **Monolog バージョン**&#x200B;をアップグレードしました – monolog 3.6.<!-- MCLOUD-12855 -->のサポートを追加しました
- ![ アイコンを修正](../../assets/fix.svg) **RabbitMQ、MariaDB、およびPHP**&#x200B;のバリデーター – 誤ったバージョンのサービスに関する誤解を招くメッセージを生成するバリデーターを修正しました。

## v2002.1.19

リリース日：2024年5月21日（PT）

- ![new icon](../../assets/new.svg) **Lua** - CACHE_CONFIGURATIONにuseLua オプションを追加しました。
- ![fix icon](../../assets/fix.svg) **Validator** - RedisおよびRabbitMQの新しいバージョンのバリデータを更新しました。

## v2002.1.18

リリース日：2024年4月8日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP** — PHP 8.3のサポートを追加しました。
- ![ アイコンを修正](../../assets/fix.svg) **バリデーター** - EOL バリデーターを更新しました。

## v2002.1.17

リリース日：2024年1月16日（PT）

- ![fix icon](../../assets/fix.svg) **Validator for Elasticsearch &amp; OpenSearch** - LiveSearchが有効になっている場合に検索サービスをインストールするための誤解を招くようなメッセージを生成するバリデーターを修正しました。<!-- MCLOUD-10167 -->
- ![ アイコンを修正](../../assets/fix.svg) **展開の警告** – 空でないフォルダーに関する展開の警告が発生する問題を修正しました。<!-- MCLOUD-8958 -->

## v2002.1.16

リリース日：2023年10月16日（PT）

- ![new icon](../../assets/new.svg) **ENABLE_WEBHOOKS グローバル環境変数** - Commerce Webhookで使用して、App Builder ランタイムアクションやサードパーティの在庫管理システムなどの外部エンドポイントに接続するための[ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) グローバル変数を追加しました。

## v2002.1.15

リリース日：2023年7月31日（PT）

- ![修正アイコン ](../../assets/fix.svg) **エラーコード** - エラーコードスキーマとエラーコードドキュメントジェネレーターを更新しました。
- ![修正アイコン ](../../assets/fix.svg) **カスタム Redis モデルのバリデータ** – カスタム Redis バックエンドモデルのバリデータを更新しました。 [ キャッシュ設定の例を参照](../environment/variables-deploy.md#cache_configuration)。
- ![ アイコンを修正](../../assets/fix.svg) **RabbitMQ用バリデーター** - RabbitMQ 3.11のサポートを追加
- ![ アイコンを修正](../../assets/fix.svg) **間違ったリンクを修正** – ようこそ電子メールテンプレートのオンボーディングドキュメントへの間違ったリンクを修正しました。

## v2002.1.14

リリース日：2023年3月10日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP** - PHP 8.2のサポートを追加しました。
- ![新しいアイコン ](../../assets/new.svg) **サービス用バリデータ** - Commerce 2.4.6に必要なサービス（MariaDB 10.6、Redis 7.0、PHP 8.2、OpenSearch 2.x、RabbitMQ 3.9）のバリデータを更新しました。
- ![ アイコンを修正](../../assets/fix.svg) **ece-tools db-dump** - `db-dump`操作が早期に停止する問題を修正しました。

## v2002.1.13

リリース日：2022年10月27日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Adobe Commerce**&#x200B;のAdobe I/O Eventsのサポートを追加しました。 拡張機能の開発者は、[Adobe I/O Events](https://developer.adobe.com/events/docs/) フレームワークを使用して、[Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/)用に作成されたアプリケーションにCommerce イベント情報をCloud インスタンスから送信できるようになりました。 Adobe Commerce用Adobe I/O Eventsは、パートナープレビューで表示されています。<!-- CEXT-932 -->
- ![新しいアイコン ](../../assets/new.svg) **OPcache設定のバリデーター** – 除外されたパスのOPcache設定を確認するバリデーターを追加しました。<!-- MCLOUD-9485 -->
- ![fix icon](../../assets/fix.svg) **GraphQL キャッシュ設定**&#x200B;の問題を修正しました。ECE-Toolsは`app/etc/env.php` ファイルの`cache`設定にGraphQL `id_salt`の値を保持するようになりました。<!-- MCLOUD-9486 -->

## v2002.1.12

リリース日：2022年9月13日（PT）

- ![新しいアイコン ](../../assets/new.svg) **有効化`synchronous_replication`** - `MYSQL_USE_SLAVE_CONNECTION`が有効になっている場合、ECE-Toolsは`app/etc/env.php` ファイルに`synchronous_replication=>true`を設定します。 この設定は、Commerce 2.4.6以降にのみ影響します。 [変数のデプロイ ](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->の「`MYSQL_USE_SLAVE_CONNECTION`変数の説明」を参照してください
- ![new icon](../../assets/new.svg) **OpenSearch**：次のAdobe Commerce リリース 2.4.6の`opensearch` エンジンを設定および設定する機能を追加しました。 [OpenSearch サービスの設定](../services/opensearch.md)を参照してください。<!-- MCLOUD-9236 -->

## v2002.1.11

リリース日：2022年8月4日（PT）

- ![ アイコンを修正](../../assets/fix.svg) **ElasticSuite ValidatorとOpenSearch** - OpenSearchがインストールされたときのElasticSuite整合性チェックの検証に関する問題を修正しました。<!-- MCLOUD-8767 -->
- ![ アイコンを修正](../../assets/fix.svg) **デプロイ コマンドの戻り値の型** – デプロイ コマンドの戻り値の型を修正しました。<!-- AC-3208 -->
- 新しいCommerce 2.4.5のインストール **に関する![ アイコン ](../../assets/fix.svg)の**[!DNL RabbitMQ]&#x200B;の問題 – [!DNL RabbitMQ]の新しいCommerce 2.4.5でのクラッシュの問題を修正しました。 インストール。<!-- MCLOUD-9059 -->

## v2002.1.10

リリース日：2022年3月31日（PT）

- ![ アイコンを修正](../../assets/fix.svg) **Elasticsearch 7.10** - 7.10 バージョンのElasticsearchをサポートするようにバリデータを更新しました。<!-- MCLOUD-8548 -->

## v2002.1.9

リリース日：2022年3月10日（PT）

- ![新しいアイコン ](../../assets/new.svg) **OpenSearch** - Adobe Commerce バージョン 2.4.4、2.4.3-p2、および2.3.7-p3のOpenSearchのサポートを追加しました。<!-- MCLOUD-8296 -->
- ![新しいアイコン ](../../assets/new.svg) **PHP** - PHP 8.1のサポートを追加しました。
- ![fix icon](../../assets/fix.svg) **symfony/process** - symfony/process ^5.3.<!-- MCLOUD-8283 -->との互換性を追加しました

- ![新しいアイコン ](../../assets/new.svg) **消費者の複数のプロセス** - `multiple_processes` オプションを追加して、消費者ごとに生成するプロセスの数を指定できるようにしました。 [変数のデプロイ ](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->の「`CRON_CONSUMERS_RUNNER`変数の説明」を参照してください
- ![新しいアイコン ](../../assets/new.svg) **OpenSearch スキームと完全なホストパス** - Elasticsearch スキームと完全なホストパスを設定する機能を追加しました。
- ![fix icon](../../assets/fix.svg) **AWS S3** - AWS S3のイネーブルメントの方式を変更しました。
- ![fix icon](../../assets/fix.svg) **Fix driver_options reader** – バリデータ用に`ece-tools`が`env.php` ファイルからDB接続のdriver_options設定を読み取れるようにしました。<!-- MCLOUD-8420 -->

## v2002.1.8

リリース日：2021年10月25日（PT）

- ![新しいアイコン ](../../assets/new.svg) **代替ダンプの場所** - DB ダンプのターゲットディレクトリを選択できるように`--dump-directory` オプションを追加しました。 これで、`/app/var/dump-main`はDB ダンプのデフォルトのターゲットディレクトリになります。 [ バックアップ管理：データベースをダンプする](../storage/database-dump.md)<!-- MCLOUD-8063 -->を参照してください
- ![修正アイコン ](../../assets/fix.svg) **モノログの更新**-`monolog` パッケージに必要な最小バージョンを`^2.3`に更新しました。<!-- ACMP-1263 -->
- ![fix icon](../../assets/fix.svg) **Symfony**&#x200B;の更新 – Symfonyの依存関係をAdobe Commerce 2.4.4と互換性があるように更新しました。<!-- ACMP-1533 -->
- ![ アイコン ](../../assets/fix.svg) **機能/解決オートロード** – 統合環境にデプロイし、`CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` エラーが表示される場合の問題を修正しました。<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

リリース日：2021年7月29日（PT）

**設定の更新**—

- ![新しいアイコン ](../../assets/new.svg) Composer 2.0のサポートを追加しました。<!--MCLOUD-8003-->

- ![ アイコンを修正](../../assets/fix.svg) **次のエラーで`di:compile` コマンドが失敗した問題を修正するために、`symphony/console`**&#x200B;のコンポーザー要件を更新しました。`symphony/console` パッケージのECE-Tools `composer.json` バージョン要件を更新しました：`Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![fix icon](../../assets/fix.svg) Elasticsearch 7.9.x.<!--MCLOUD-7938-->が含まれるように、提供終了のソフトウェアチェック （`eol.yaml`）を更新しました

## v2002.1.6

リリース日：2021年4月20日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Redis認証資格情報** - デプロイフェーズ中に`relationships` プロパティからRedis認証資格情報を読み取る機能を追加しました。<!--MCLOUD-7694-->

- ![新しいアイコン ](../../assets/new.svg) **Elasticsearch認証資格情報** - デプロイフェーズ中に`relationships` プロパティからElasticsearch認証資格情報を読み取る機能を追加しました。<!--MCLOUD-7695-->

- ![新しいアイコン ](../../assets/new.svg) **専用セッションストレージサービス** - セッションストレージの2番目のオプションとして`redis-session`を追加しました。 `redis-session` サービスを使用してセッション情報を保存し、キャッシュに`redis` サービスを使用してパフォーマンスを向上させることができます。<!--MCLOUD-7698-->

- ![新しいアイコン ](../../assets/new.svg) **非推奨のSPLIT_DB メッセージ** - Adobe Commerce 2.4.2の非推奨`SPLIT_DB` オプションと、Adobe Commerce 2.5.0での削除に関するバリデーター警告とクリティカルメッセージを追加しました。<!--MCLOUD-7806-->

- ![ アイコンを修正](../../assets/fix.svg) **関係**&#x200B;からのElasticsearch バージョン – 修正されたサービス バリデーターを使用して、Cloud Dockerおよび統合環境の`relationships` プロパティから正しいバージョンのElasticsearchを取得します。<!--MCLOUD-7572-->

- ![修正アイコン ](../../assets/fix.svg) **柔軟なRedis ポート検証** - Redisは、`server` URLからカスタムキャッシュ接続でポートを検証できるようになりました。 例えば、次のようにサーバーURLにポート番号を追加できます：`server: 'tcp://rfs-store-simple-page-cache:26379'`。 これにより、`port` オプションが見つからないか正しくない検証エラーを防ぐことができます。<!--MCLOUD-7722-->

- ![ アイコン ](../../assets/fix.svg) **Adobe Commerce 2.4.2**&#x200B;へのアップグレード - Adobe Commerce 2.4.2<!--MCLOUD-7776-->にアップグレードした後、サイトを稼働させるために`bin/magento setup:upgrade`を手動で実行する必要がある問題を修正しました。

## v2002.1.5

リリース日：2021年2月1日（PT）

- ![新しいアイコン ](../../assets/new.svg) **リモートストレージ** - AWS S3などのストレージサービスを使用してメディアファイルをリモートストレージするためにCloud Projectsを有効にする`REMOTE_STORAGE`環境変数を追加しました。 このコンフィギュレーション オプションはECE-Tools パッケージの一部ですが、Adobe Commerce クラウド インフラストラクチャではサポートされていません。<!--MCLOUD-7153-->

- ![新しいアイコン ](../../assets/new.svg) **新しい`cloud:config:validate` コマンド** - リモート クラウド環境に変更をプッシュする前に`.magento.env.yaml`設定を検証するコマンド `php vendor/bin/ece-tools cloud:config:validate`を追加しました。<!--MCLOUD-7120-->

- ![新しいアイコン ](../../assets/new.svg) **opcache**&#x200B;のフラッシュ – デプロイフックを実行する前にOPcacheをフラッシュする`opcache.enable_cli` PHP オプションのサポートを追加しました。 この構成は、現在の構成設定が各デプロイメントに適用されるように、キャッシュ設定をリセットします。<!--MCLOUD-7015-->

- ![新しいアイコン ](../../assets/new.svg) **Aurora DB**&#x200B;の検証 – Aurora データベースと互換性があるようにデータベースサービスの検証を更新しました。<!--MCLOUD-7269-->

- ![新しいicon](../../assets/new.svg) **新しいSCD_NO_PARENT環境変数** – 親テーマの静的コンテンツの生成を管理するために`SCD_NO_PARENT`環境変数（Adobe Commerce >=2.4.2）を追加しました。<!--MCLOUD-7284-->

- ![ アイコン ](../../assets/fix.svg)を修正&#x200B;**メモリ制限とコマンド** - `cloud.log` ファイルのサイズがPHP memory_limitを超えると、`php vendor/bin/ece-tools` コマンドが機能しない問題を修正しました。 `cloud.log` ファイル全体をメモリに読み込む代わりに、ログ ファイルからデータの小さいサブセットのみを読み取るようになりました。<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![修正アイコン ](../../assets/fix.svg) **カスタムデータベース接続** - `DATABASE_CONFIGURATION`に対して定義されたカスタムデータベース接続が使用されなかった`.magento.env.yaml`設定の問題を修正しました。 接続設定が`app/etc/env.php`に追加されませんでした。<!--MCLOUD-7426-->

- ![ アイコンを修正](../../assets/fix.svg) **空のエラーログ** - `cloud.error.log`が空の場合にデプロイメントが失敗する問題を修正しました。<!--MCLOUD-7296-->

- ![fix icon](../../assets/fix.svg) **MariaDB 10.3検証**—Adobe Commerce 2.3.6-p1に対するMariaDB 10.3の検証を修正しました。<!--MCLOUD-7416-->

- ![ アイコンを修正](../../assets/fix.svg) **キャッシュ :flush ログ** - ログ エントリを改善して、`cache:flush` ステップの開始と終了を示しました。<!--MCLOUD-7503-->

## v2002.1.4

リリース日：2020年11月19日（PT）

- ![修正アイコン ](../../assets/fix.svg)環境変数`SEARCH_CONFIGURATION`で指定された検索エンジンが`elasticsearch`以外の値の場合にデプロイメントが失敗する問題を修正しました。<!--MCLOUD-7283-->

## v2002.1.3

リリース日：2020年11月9日（PT）

**インフラストラクチャの更新**—

- ![新しいアイコン ](../../assets/new.svg)静的コンテンツがビルド段階でデプロイするように設定されている場合に、読み取り専用`pub/static` ディレクトリに対するECE-Tools サポートを追加しました。<!--MC-37699-->

- ![新しいアイコン ](../../assets/new.svg)今後のAdobe Commerce リリースとの互換性を保つため、Elasticsearch 7.9とRedis 6のサポートが追加されました。<!--MCLOUD-7191-->

- ![修正アイコン ](../../assets/fix.svg) ECE-Tools `composer.json`を更新して、品質パッチツールに必要な依存関係を追加しました。 これにより、ECE-Tools パッケージとmagento-cloud-patches パッケージの間に存在していた循環依存関係が修正されます。<!--MCLOUD-6910-->

**検証とログの改善**—

- ![新しいアイコン ](../../assets/new.svg) クラウドインフラストラクチャ 2.4以降で`elasticsearch`がAdobe Commerce用に設定されていることを確認するために、検索エンジンの検証を追加しました。 検証が失敗した場合、デプロイメントは停止され、問題の修正を示唆する重要なエラーメッセージが表示されます。 [ クリティカルエラー、デプロイステージ ](../dev-tools/error-reference.md#deploy-stage)を参照してください。<!--MCLOUD-6937-->

- ![新しいアイコン ](../../assets/new.svg) Elasticsearch サービス版とAdobe Commerce版の互換性を確認するためのElasticsearch検証を追加しました。<!--MCLOUD-7193-->

- ![新しいアイコン ](../../assets/new.svg) Adobe Commerce Elasticsearch モジュールと互換性のあるElasticsearchのバージョンを表示するように、Elasticsearchの互換性に関するエラーメッセージを更新しました。 これで、お使いのバージョンのAdobe Commerceで使用されているElasticsearch モジュールと互換性を持つように、クラウドインフラストラクチャにインストールする特定のElasticsearch バージョンがエラーメッセージに表示されました。 [警告エラー、デプロイステージ ](../dev-tools/error-reference.md#deploy-stage-1)を参照してください。<!--MCLOUD-6698-->

- ![新しいアイコン ](../../assets/new.svg)無効な`MAGE_MODE`環境変数設定に対する警告エラー`2026`と`2027`を追加しました。 有効な値は`production`のみです。 この修正を行う前は、デプロイメントエラーなしで`MAGE_MODE`を`developer`に設定できましたが、読み取り専用ファイルへの書き込み時に後でエラーが発生する可能性がありました。 [警告エラー](../dev-tools/error-reference.md#warning-errors)を参照してください。<!--MCLOUD-6708-->

- ![fix icon](../../assets/fix.svg)これらのバージョンがAdobe Commerce バージョンと互換性があることを確認するために、Redis、RabbitMQ、およびMySQL サービスの検証を修正しました。 これらのサービスの有効なバージョンが`cloud.log`.<!--MCLOUD-7098-->に書き込まれるようになりました

- ![修正アイコン ](../../assets/fix.svg) キャッシュ ウォームアップ中にリクエストを送信するための同時要求の制限を含めるように`cloud.log`を更新しました。 この値は、[WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency)のデプロイ後の変数で設定されます。<!--MCLOUD-5563-->

**CLI コマンドの更新**—

- ![新しいアイコン ](../../assets/new.svg) 1つ以上のビルド、デプロイ、デプロイ後の変数を含めることができる設定で`.magento.env.yaml` ファイルを作成および更新するためのCLI コマンド （`cloud:config:create`および`cloud:config:update`）を追加しました。 「[CLIから構成ファイルを作成](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->」を参照してください

**環境変数の更新**—

- ![新しいアイコン ](../../assets/new.svg) [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) ビルド変数を追加しました。 変数を`true`に設定すると、Cloud Docker for Commerceのインストール中に`composer dump-autoload` コマンドが実行されなくなります。 変数は、書き込み可能なファイルシステムを持つCommerce コンテナのCloud Dockerにのみ関連します（`./vendor/bin/ece-docker build:compose --with-test`を使用してテストおよび開発用に作成）。 このようなインストールでは、`composer dump-autoload` コマンドをスキップすると、削除された`generated` ディレクトリからファイルにアクセスしようとする他のコマンドを実行する際のエラーを防ぐことができます。<!--MCLOUD-6939-->

## v2002.1.2

リリース日：2020年8月5日（PT）

**検証とログの改善**—

- ![新しいアイコン ](../../assets/new.svg) ビルド、デプロイ、デプロイ後のプロセス中に発生する可能性のあるすべてのエラーと警告の通知と、エラーを解決するための提案を含む`schema.error.yaml` ファイルを追加しました。 このファイルの情報は、_Commerce向けクラウドガイド_&#x200B;でも入手できます。 e-tools](../dev-tools/error-reference.md)の[ エラーメッセージ参照を参照してください。<!--MCLOUD-5878-->

- ![新しいアイコン ](../../assets/new.svg) Cloud エラーログ （`/var/log/cloud.error.log`）のエントリをJSON形式に変更し、ログをプログラムで解析しやすくしました。<!--MCLOUD-5879-->

- ![新しいアイコン ](../../assets/new.svg)処理のビルド、デプロイ、デプロイ後に追加のエラーチェックを追加し、既存のチェックを改善しました。

   - エラーコード 2026：ビルド フェーズで生成された一部のデータをマウントされたディレクトリに復元できませんでした

   - エラーコード 3004 - バックアップ ファイルを作成できません

   - エラーコード 102 - `env.php` ファイルが書き込み可能でない場合に発生する問題を確認するための追加チェックを追加しました<!--MCLOUD-6221-->

- ![新しいアイコン ](../../assets/new.svg) **QUALITY_PATCHES**&#x200B;環境変数を追加して、デプロイメントプロセス中に適用する1つ以上の品質パッチを指定しました。 [ ビルド変数](../environment/variables-build.md#quality_patches)を参照してください。<!--MCLOUD-6375-->

## v2002.1.1

リリース日：2020年6月25日（PT）

- ![新しいアイコン ](../../assets/new.svg) **インフラストラクチャの更新**—

   - ![新しいアイコン ](../../assets/new.svg) **改善のログ記録** – 終了コードを重大なデプロイエラーに割り当て、エラーメッセージ通知とログイベントで終了コードを公開することで、ログ追跡機能を改善しました。 e-tools](../dev-tools/error-reference.md)の[ エラーメッセージ参照を参照してください。<!-- MCLOUD-5637, 5531-->

   - ![新しいアイコン ](../../assets/new.svg) データベース ダンプ （`vendor/bin/ece-tools db-dump`）のプロセスを改善し、ログ メッセージを更新して、データベース ダンプ操作がアプリケーションをメンテナンスモードに切り替え、コンシューマーのキュー処理を停止し、ダンプが開始される前にcron ジョブを無効にすることを明確にしました。<!--MCLOUD-5324, MCLOUD-2062-->

   - ![ アイコンを修正](../../assets/fix.svg) ステージング環境と実稼動環境にデプロイする際に、プロジェクト URLが正しく更新されるように問題を修正しました。 現在、`ece-tools`は、プロジェクトのルート設定で`primary:true`属性が設定されたルートのURLを使用しています。 [変数のデプロイ ](../environment/variables-deploy.md#update_urls)を参照してください。<!--MCLOUD-5883-->

   - ![修正アイコン ](../../assets/fix.svg) パッチを適用するための`generate.xml` ビルドシナリオワークフローを更新しました。 Adobe Commerceを更新して、`di:compile`と`module:refresh`の手順が失敗する可能性がある問題を修正するには、パッチを早めに適用する必要があります。<!--MCLOUD-5941-->

   - ![ アイコンの修正](../../assets/fix.svg) インストールプロセスで、`Crypt key missing` エラーが誤って返される問題を修正しました。 `crypt/key`値は、インストール中に自動的に生成されます。<!--MCLOUD-6120-->

- ![新しいアイコン ](../../assets/new.svg) **サービスの更新**—

   - ![新しいアイコン ](../../assets/new.svg) PHP 7.4およびMariaDB 10.4のサポートを追加しました。<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![新しいアイコン ](../../assets/new.svg) **環境変数の更新**—

   - ![新しいアイコン ](../../assets/new.svg) **SCD_USE_BALER**&#x200B;変数を追加して、Adobe Commerce on cloud インフラストラクチャのビルドプロセス中にJavaScript バンドル用のBaler モジュールを有効にしました。 [ ビルド変数](../environment/variables-build.md#scd_use_baler)の変数の説明を参照してください。<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![新しいアイコン ](../../assets/new.svg) Adobe Commerce 2.3.5以降のRedis キャッシュ用のRedis バックエンドモデルを設定するために、**REDIS_BACKEND**&#x200B;環境変数を追加しました。 変数の説明については、[変数のデプロイ ](../environment/variables-deploy.md#redis_backend)を参照してください。<!--MCLOUD-5721, MCLOUD-5865-->

- ![新しいアイコン ](../../assets/new.svg) **CLI コマンドの更新**—

   - ![新しいアイコン ](../../assets/new.svg)次のCLI コマンドを、詳細なログ記録のオプション付きで更新しました。

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     各呼び出しのログレベルは、`.magento.env.yaml` ファイルの[`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands)変数の設定によって決まります。<!--MCLOUD-3503-->

- ![新しいアイコン ](../../assets/new.svg) **検証の改善**—

   - ![新しいアイコン ](../../assets/new.svg) **Elasticsearch 7.xの互換性チェック** - Elasticsearch 7.x ソフトウェアの互換性チェック用のElasticsearch検証を更新しました。<!--MCLOUD-5542-->

   - ![新しいアイコン ](../../assets/new.svg) **サービスのバージョンとEOL検証チェック**&#x200B;を更新しました。インストール済みのサービスのバージョンをAdobe Commerce 2.4に照らし合わせてチェックするための検証を更新しました。 要件。<!--MCLOUD-6144-->

   - ![ アイコンの修正](../../assets/fix.svg)次のデプロイ後の警告メッセージが`.magento.app.yaml` ファイルに`post-deploy` フック設定がない場合にのみ表示されるように、検証の問題を修正しました。

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![新しいアイコン ](../../assets/new.svg) **Zend Framework依存関係の検証を追加** - Laminas プロジェクトに移行したZend Frameworkのコンポーザー依存関係の検証を追加しました。 必要な依存関係がない場合は、ビルドプロセス中に次のエラーメッセージが表示されます。

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     [Zend Frameworkの依存関係の確認](../development/commerce-version.md#verify-zend-framework-composer-dependencies)を参照してください。<!--MCLOUD-4094-->

   - ![新しいアイコン ](../../assets/new.svg) **追加された`env.php` ファイルとデータ**&#x200B;の検証 – インストールおよびアップグレードのプロセス中に`env.php` ファイルとデータのチェックを追加しました。<!--MCLOUD-5991-->

      - インストールに`env.php` ファイルが見つからず、`crypt/key`値が`.magento.app.yaml` ファイルで指定されていない場合、デプロイメントは次の通知で失敗します。

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - インストールに`env.php` ファイルが含まれていない場合、または設定に1つのキャッシュの種類しか含まれていない場合は、`cron:enable` コマンドがアップグレードプロセス中に実行され、すべての`cache_types`を含むファイルが復元されます。 次の通知がログに追加されます。

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

リリース日：2020年2月6日（PT）

- ![新しいアイコン ](../../assets/new.svg) **インフラストラクチャの更新**—

   - ![新しいアイコン ](../../assets/new.svg) **Commerce用Cloud Docker用の個別のパッケージを追加しました** - Docker パッケージを`ece-tools` パッケージから切り離して、コードの品質を維持し、独立したリリースを提供しました。 `ece-tools`に関連する更新と修正は、[magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub リポジトリから管理されます。<!--MAGECLOUD-2927-->

   - ![新しいアイコン ](../../assets/new.svg) **パッチ機能を更新** - パッチ機能をECE-Tools パッケージから別の[magento-cloud-patches](https://github.com/magento/magento-cloud-patches) パッケージに移動しました。 デプロイメント中、`ece-tools`は新しいパッケージを使用してパッチを適用します。 [Cloud パッチリリースノート ](cloud-patches.md)を参照してください。<!--MAGECLOUD-4567-->

   - ![新しいアイコン ](../../assets/new.svg) **Composerの依存関係**&#x200B;を更新 – `magento/magento-cloud-docker` パッケージの依存関係を持つクラウドインフラストラクチャ上のAdobe Commerceの`composer.json` ファイルを更新しました。 現在、`ece-tools`には、[`Cloud Tools Suite for Commerce`](cloud-tools-suite.md)内のすべてのパッケージの依存関係が含まれています。 これらのパッケージは、`ece-tools`をインストールまたは更新すると、自動的にインストールおよび更新されます。

- ![新しいアイコン ](../../assets/new.svg) **シナリオベースのデプロイメントのサポート**—<!--MAGECLOUD-4101-->

   - ![新しいアイコン ](../../assets/new.svg) XML設定ファイルを使用して、ビルド、デプロイ、デプロイ後のプロセスをカスタマイズし、デフォルト設定を上書きまたはカスタマイズできるようになりました。

   - ![新しいアイコン ](../../assets/new.svg) **`.magento.app.yaml`**&#x200B;の`hooks`設定を変更しました。シナリオベースのデプロイメントをサポートするように`hooks`設定形式を更新しました。 以前のECE-Tools 2002.0.x リリースの従来の形式は、引き続きサポートされています。 ただし、シナリオベースのデプロイメント機能を使用するには、新しい形式に更新する必要があります。 [ シナリオベースのデプロイメント ](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks)を参照してください。

>[!NOTE]
>
>ECE-Tools バージョン 2002.1.0に更新する前に、[を前のバージョンで確認してください   互換性のない変更](backward-incompatible-changes.md)を使用して、次の操作が必要になる可能性のある変更について学習する   クラウドインフラストラクチャプロジェクトの設定またはプロセスでAdobe Commerceを更新します。

- ![新しいアイコン ](../../assets/new.svg) **サービスの更新**—

   - ![新しいアイコン ](../../assets/new.svg) PHP 7.3のサポートを追加しました。<!--MAGECLOUD-4022-->

   - ![新しいアイコン ](../../assets/new.svg) RabbitMQ 3.8.<!--MAGECLOUD-4674-->のサポートを追加しました

   - ![新しいアイコン ](../../assets/new.svg)各サービスのインストール済みサービスのバージョンをEOL日に照らし合わせて確認するための検証を追加しました。 現在、サービスのバージョンが終了日から3か月以内の場合は通知が表示され、終了日が過去の場合は警告が表示されます。<!--MAGECLOUD-4076-->

   - ![ アイコンの修正](../../assets/fix.svg) Elasticsearchの設定に関する問題を修正し、すべての環境で正しいElasticsearch設定が行われるようにしました。<!--MAGECLOUD-4474-->

>[!NOTE]
>
>クラウドインフラストラクチャ上のAdobe Commerceで使用されるサービスの一覧と、Cloud テンプレートとのバージョン互換性については、[ サービスバージョン ](../services/services-yaml.md#service-versions)を参照してください。

- ![新しいアイコン ](../../assets/new.svg) **環境変数の更新**—

   - ![新しいアイコン ](../../assets/new.svg)特定の製品ページのキャッシュのプリロードをサポートするために、`WARM_UP_PAGES`環境変数の機能を拡張しました。 展開された定義については、[展開した後の変数](../environment/variables-post-deploy.md#warm_up_pages) トピックを参照してください。<!--MAGECLOUD-4444-->

   - ![新しいアイコン ](../../assets/new.svg) `<magento_root>/var/report/` ディレクトリのエラーレポート データ管理を簡素化するために、`ERROR_REPORT_DIR_NESTING_LEVEL`環境変数を追加しました。 [ ビルド変数](../environment/variables-build.md#error_report_dir_nesting_level) トピックの変数の説明を参照してください。

   - ![修正アイコン ](../../assets/fix.svg) `SCD_EXCLUDE_THEMES`、`STATIC_CONTENT_THREADS`、`DO_DEPLOY_STATIC_CONTENT`および`STATIC_CONTENT_SYMLINK`環境変数を削除しました。 [下位互換性のない変更](backward-incompatible-changes.md#environment-configuration-changes)を参照してください。<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![修正アイコン ](../../assets/fix.svg) `_merge` オプションを使用せずに`ELASTICSUITE_CONFIGURATION` デプロイ変数を設定すると、デフォルト設定が期待どおりに上書きされるように、Elastic Suite設定プロセスの問題を修正しました。<!--MAGECLOUD-4388-->

- ![新しいアイコン ](../../assets/new.svg) **CLI コマンドの更新**—

   - ![新しいアイコン ](../../assets/new.svg) **新しいcron コマンド** - `cron:disable`および`cron:enable` コマンドを使用して、Adobe Commerce on cloud infrastructure environmentでcron処理を手動で管理できるようになりました。 disable コマンドを使用して、アクティブなすべてのcron プロセスを停止し、すべてのcron ジョブを無効にします。 準備ができたら、enable コマンドを使用してcron ジョブを再度有効にします。 「[cron ジョブを無効にする](../application/crons-property.md#disable-cron-jobs)」を参照してください。

   - ![新しいアイコン ](../../assets/new.svg) **エラーレポートの改善** - ECE-Tools処理中に発生するCLI コマンドのエラーに関するログの改善が追加されました。<!--MAGECLOUD-4849-->

   - ![新しいアイコン ](../../assets/new.svg) **非推奨のビルドコマンドを削除** – 次のビルドコマンドを削除しました：`m2-ece-build`、`m2-ece-deploy`、`m2-ece-scd-dump`、および`ece-tools docker` コマンドの名前を`ece-docker`に変更しました。 [下位互換性のない変更](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->を参照してください

- ![新しいアイコン ](../../assets/new.svg)非推奨の`build_options.ini` ファイルを削除し、ファイルが存在する場合にビルドを失敗する検証を追加しました。 ビルドオプションを設定するには、[.magento.env.yaml](../environment/configure-env-yaml.md) ファイルを使用します。

- ![修正アイコン ](../../assets/fix.svg) `config.php` ファイルが空の場合にビルド プロセスが失敗する問題を修正しました。<!--MAGECLOUD-4127-->

## 2002.0.23

リリース日：2020年2月27日（PT）

- ![ アイコンを修正](../../assets/fix.svg) オンデマンドの静的コンテンツ生成が実稼動モードで正常に完了しなかった`ece-tools` 2002.0.x リリースの互換性の問題を修正しました。

## 古いリリース

バージョン 2002.0.22以前については、[ リリースノートのアーカイブ ](cloud-release-archive.md)を参照してください。
