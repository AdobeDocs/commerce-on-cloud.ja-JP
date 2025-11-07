---
title: Cloud Docker パッケージ
description: Cloud Docker パッケージの最新の改善点のリストを確認します。
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: 562fd6e1dcd09600e00d034a94509b2dfd69d1ef
workflow-type: tm+mt
source-wordcount: '3806'
ht-degree: 0%

---

# Cloud Docker パッケージ

[`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) パッケージは、Adobe Commerceをローカルクラウド環境にデプロイする機能と Docker イメージを提供します。 これらのリリースノートでは、[Commerce用 Cloud Tools Suite](cloud-tools-suite.md) のコンポーネントであるこのパッケージの最新の改善点について説明します。

`magento/magento-cloud-docker` パッケージでは、次のバージョン シーケンスを使用します：`<major>.<minor>.<patch>`

リリースノートには次のものが含まれます。

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) 新機能
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) 修正点および改善点

<!--Add release notes below-->

## v1.4.6 {#latest}

リリース日：2025 年 11 月 6 日（PT）

- ![fix icon](../../assets/fix.svg)**Symfony パッケージ** – 最新の Symfony YAML パッケージのサポートを追加しました。<!-- MCLOUD-14020 -->

## v1.4.5

リリース日：2025 年 10 月 8 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**ActiveMQ** – 機能テストを含む Cloud-docker で ActiveMQ のサポートを追加しました。<!-- MCLOUD-13771 -->

## v1.4.4

リリース日：2025 年 8 月 7 日（PT）

- ![fix icon](../../assets/fix.svg) **PHP 8.4**—PHP 8.4 のテストを追加しました。<!-- MCLOUD-13311 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**FTP 拡張機能**- FTP 拡張機能に対して追加された修正。<!-- MCLOUD-13843 -->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Opensearch3 画像**—Opensearch3.<!-- MCLOUD-13766 --> のサポートを追加
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Opensearch3 tests**—Opensearch3.<!-- MCLOUD-13768 --> 用に PHP 8.4 テストを追加しました。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Valkey**—Valkey のサポートを追加しました。<!-- MCLOUD-13558 -->

## v1.4.3

リリース日：2025 年 6 月 3 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**2.4.8 との互換性の向上** – サードパーティライブラリを更新し、2.4.8 との互換性を向上 <!-- MCLOUD-13707	 - -->

## v1.4.2

リリース日：2025 年 4 月 7 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.4**—8.4 および `php-cli` 8.4 の画像 `php-fpm` 追加されました。


## v1.4.1

リリース日：2025 年 2 月 6 日（PT）

- ![new icon](../../assets/new.svg) **PHP 8.4**—PHP 8.4 のサポートを追加しました。


## v1.4.0

リリース日：2024 年 10 月 7 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**リファクタリングされたコード** – 古い PHP バージョン（7.4、7.3、7.2）および関連するライブラリと画像のサポートを削除しました。

## v1.3.7

リリース日：2024 年 4 月 8 日（PT）

- ![new icon](../../assets/new.svg) **PHP** — PHP 8.3 および PHP 8.3 のイメージのサポートを追加しました。
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **Nginx** – 画像 nginx v. 1.24 を追加しました。
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Opensearch** – 画像 OpenSearch v. 2.12, 1.3 を追加。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Composer** - Composer バージョンを 2.2.23 に更新しました。

## v1.3.6

リリース日：2023 年 7 月 31 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新しいサービスバージョンを追加**—OpenSearch 2.5
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Composer キャッシュの有効化** — Docker コンテナを起動する際に、Docker 設定を拡張して Composer のキャッシュのクリアを有効にできるようになりました。 [Cloud Docker for Commerce](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) ガイドの _Docker 設定を拡張_ を参照してください。

## v1.3.5

リリース日：2023 年 3 月 10 日（PT）

- ![new icon](../../assets/new.svg) **ionCube**—PHP 8.1 の画像に ionCube 拡張を追加しました。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**新しいサービスバージョンを追加**—OpenSearch 2.3 と 2.4、PHP 8.2、Varnish 7.1.1
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP 8.2 のサポートの強化**—Commerce 2.4.6 をサポートするために、特定の PHP 8.2.x バージョンとの互換性の問題を修正しました。
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Composer の問題** - Docker コンテナ内で Composer バージョンを更新した後に発生した問題を修正しました。

## v1.3.4

リリース日：2022 年 10 月 27 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **新しいワニス画像を追加** - ワニス 6.5、7.0、7.1.<!-- MCLOUD-7879 --> の画像を追加しました。

## v1.3.3

リリース日：2022 年 9 月 13 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Apple M1 （ARM64）のサポート** - Apple M1 （ARM64）アーキテクチャのサポートを有効にするために、Docker イメージに変更が追加されました。<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Mailhog** – 開発者モードの間に Mailhog サービスがメールを取得できない問題を修正しました。<!-- MCLOUD-8643 -->
- ![fix icon](../../assets/fix.svg) **init-docker.sh** - `init-docker.sh` スクリプトのサービスバージョンバリデーターを修正しました。<!-- MCLOUD-8765 -->

## v1.3.2

リリース日：2022 年 3 月 31 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Elasticsearch 7.10 の画像を追加**<!-- MCLOUD-8548 -->

## v1.3.1

リリース日：2022 年 3 月 10 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP 8.1 のサポート**—PHP 8.1 のサポートを追加しました。
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**OpenSearch** - OpenSearch バージョン 1.1 および 1.2 の画像を追加しました。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Composer 2.1**—PHP 8.x イメージで composer 2.1.x をデフォルトで設定します。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP イメージの改善**—

   - PHP 8.1 イメージを追加しました。
   - xDebug バージョン 3.1.2 のアップグレード
   - Xmlrpc 1.0.0RC3 をアップグレードしました

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **Elasticsearchと OpenSearch の改善** - Elasticsearchと OpenSearch Dockerfiles の改善。Elasticsearch 5.2 の画像を削除しました。
- ![fix icon](../../assets/fix.svg)**Sodium extension** – すべての PHP イメージでデフォルトで `sodium` 拡張を有効にします。
- ![fix icon](../../assets/fix.svg)**Composer cache volume** – キャッシュされた Composer パッケージを持つ Composer キャッシュ ボリュームの固定パス。
- ![fix icon](../../assets/fix.svg)**nginx のメモリ制限** - NGINX イメージのメモリ制限を修正しました。

## v1.3.0

リリース日：2021 年 10 月 25 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**開発者モードワークフローの改善** – 以前は、ビルド手順とデプロイ手順でモードを指定する必要がありました。 これで、`--mode` の手順の `build` オプションによって、後の `deploy` の手順のモードが決まります。 デプロイメント後のモードの設定は不要になりました。 [&#x200B; 開発者モード &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) を参照してください。<!-- ACMP-1086 -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**読み取り専用ファイルシステムの改善点**—<!-- ACMP-1106 -->
   - メール設定用の PHP コンテナを起動する際の問題を修正しました。
   - INI ファイルで環境変数を使用できます。
   - PHP エントリポイントに書き込み権限が必要ないことを確認します。
- ![fix icon](../../assets/fix.svg)**Update Node** – バンドルされている Node のバージョンを更新します。PHP-CLI イメージに Node をインストールする場合、現在の LTS バージョンが使用されます。<!-- ACMP-1539 -->
- ![fix icon](../../assets/fix.svg)**Update Symfony** - Adobe Commerce 2.4.4 と互換性を持たせるために Symfony config 依存関係を更新しました。<!-- ACMP-1533 -->

## v1.2.4

リリース日：2021 年 7 月 29 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新規 `Zookeeper` コンテナ** - Cloud インフラストラクチャ上のAdobe Commerceにデプロイされていないプロジェクトのロックプロバイダー設定を管理するための [Zookeeper コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container) を追加しました。<!--MCLOUD-8000-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Composer 2.0.** のサポートが追加されました – 提供終了が近づいている Composer 1.0 のアップグレードをサポートするために、Composer バージョン 2.0 が Composer 設定ファイルに追加されました。<!--MCLOUD-8003-->

## v1.2.3

リリース日：2021 年 6 月 14 日

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.0 を追加**—PHP をバージョン 8.0 に更新し、PHP 8.0 に含まれるすべての新機能と最適化を活用できるようにしました。<!--MCLOUD-7941-->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Varnish 6.6 およびElasticsearch 7.11.2 に更新** – 次のリンクは、[Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) およびElasticsearch 7.11.2.<!--MCLOUD-7921--> に関するリリース情報を提供します。
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **PHP 7.4 のイメージ用に `ioncube` 拡張モジュールが追加されました**—`ioncube` 拡張モジュールは、最初に PHP 7.3 から PHP 7.4 へのアップグレードから除外された後、PHP 7.4 のイメージに再度追加されました。 *[mattskr により送信 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **ファイル同期オプションを追加：`manual-native`** – 「`manual-native`」ファイル同期オプションを使用すると、同期を手動で制御でき、macOS環境と Windows 環境で最高のパフォーマンスが得られます。 `manual-native` 開発者モード [&#x200B; での &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) オプションの使用と [Docker 開発環境でのデータの同期 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options) を参照してください <!--MCLOUD-7977-->。
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**`up` および `down` コマンドから削除されたボリュームの削除** - 「`--volume`」オプションが `bin/magento-docker up` および `bin/magento-docker down` コマンドから削除され、新しい `bin/magento-docker init` コマンドに置き換えられ、データ損失の警告が表示されました。 この変更により、誤ってデータが失われるのを防ぐことができます。 *[寄稿者：joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**生成された証明書の `CN` 値を更新** - Dockerfile からハードコードされた `CN` 値を削除しました。 この値により、証明書エラー（`NET::ERR_CERT_INVALID`）が発生し、`--host` コマンドの `ece-docker build:compose` オプションが無視されました。<!--MCLOUD-7934-->

## v1.2.2

リリース日：2021 年 4 月 20 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**更新された `host.docker.internal` はプラットフォームに依存しません**—Ubuntu、Windows、macOS用に同じ Docker Compose スクリプトを作成できるようになりました。 Ubuntu で Xdebug を使用する際に、個別の環境変数が必要なくなりました。 [Igor Vitol による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![new icon](../../assets/new.svg) **Updated init-docker.sh**—`mounts` オブジェクトを `MAGENTO_CLOUD_APPLICATION` 環境変数に追加しました。 [Chiranjeevi による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![new icon](../../assets/new.svg) **Updated init-docker.sh**—`init-docker.sh` スクリプトを PHP 7.4 および Cloud Docker 1.2.1 バージョンで更新しました。 [Adarsh Manickam によって修正が送信されました &#x200B;](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**デフォルトで有効な Sodium** - PHP Docker イメージ内でデフォルトで `sodium` PHP 拡張機能を有効にします。<!--MCLOUD-7548-->
- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **`custom-registry`オプション** – 独自の画像レジストリを使用する `--custom-registry` めのコマンドに `php ./vendor/bin/ece-docker build:compose` しいオプションを追加しました。<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**古いElasticsearch バージョンを削除** - Elasticsearch バージョン 1.7 および 2.4 をElasticsearch イメージから削除しました。<!--MCLOUD-7504-->
- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**NGINX 証明書の自動生成** - NGINX イメージから既存の証明書を削除しました。 NGINX 証明書は、セキュリティを向上させるために、新しいデプロイメントのたびに自動生成されるようになりました。<!--MCLOUD-7396-->
- ![fix icon](../../assets/fix.svg) **Enabled`opcache.validate_timestamps`** – 開発者モードでデフォルトで `opcache.validate_timestamps` PHP 設定を有効にします。 この設定を有効にすると、Docker でファイルシステムに対する変更が認識されない問題が修正されました。<!--MCLOUD-7466-->
- ![fix icon](../../assets/fix.svg) **Fixed`build:custom:compose`** – ビルド処理中にファイルを上書きできない場合にエラーをスローする `build:custom:compose` コマンドを修正しました。 エラーをスローすると、間違 `docker-compose up` たファイルを使用している可能性のある状況を防ぐことができます。<!--MCLOUD-7457-->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**修正 `--sync_engine="native"` オプション** – 実稼動モード（`--mode="production"`）で、`--sync_engine="native"` オプションを使用しても `docker.composer.yml` ファイル内のローカルフォルダーのエントリが作成されない問題を修正しました。<!--MCLOUD-7254-->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**修正されたサービスバージョン検証エラー** - [!DNL RabbitMQ]、Elasticsearch、その他のサービスのサービスバージョンを `type` 変数の `MAGENTO_CLOUD_RELATIONSHIP` プロパティに追加しました。 これらのバージョンを `relationships` 変数に追加すると、デプロイフェーズで発生した検証エラーが修正されました。<!--MCLOUD-7572-->

## v1.2.1

リリース日：2020 年 12 月 21 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**NGINX コマンド オプション**—TLS および Web サービスの NGINX `worker_processes` および NGINX `worker_connections` の数を変更するビルド コマンド オプションが追加されました。 `worker_process` パラメーターは、値を `auto` に設定する機能を保持します。 例：<!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**TLS コマンドオプション**—TLS サービスを使用せずに設定を作成するためのビルドコマンドオプションを追加しました。 例：<!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**NGINX メモリ消費** - TLS および Web サービスに対する NGINX プロセスによって消費されるメモリを削減しました。<!--MCLOUD-7259-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **Blackfire** - Cloud Docker イメージで、Blackfire PHP 拡張機能をデフォルトで無効にしました。

- ![fix icon](../../assets/fix.svg) **PHP-FPM container**—`WEB_PORT` を `80` から `8080` に変更することにより、PHP-FPM コンテナのヘルスチェックを修正しました。<!--MCLOUD-7232-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**無効なボリュームの命名** – 開発者モードで無効なボリュームの命名に関するエラーを修正しました。<!--MCLOUD-7442-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**NGINX アップストリームポート** – 無限ループを回避するために、ポート 8080 を使用するように Docker NGINX 1.19 イメージを更新しました。 [Adarsh Manickam によって修正が送信されました &#x200B;](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

リリース日：2020 年 11 月 9 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**コンテナの更新 –**

   - ![new icon](../../assets/new.svg) **PHP-FPM container**—gnupg PHP 拡張モジュールのサポートを追加しました。 [G Arvind が Zilker Technology から送信した修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**データベースコンテナ** - ヘルスチェックコマンドに必要なデータベースパスワードを追加することで、データベースコンテナのヘルスチェックを修正しました。<!--MCLOUD-7122-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **Elasticsearch コンテナ**

      - 今後のElasticsearch リリースとの互換性のために、Adobe Commerce 7.9 がサポートされるようになりました。<!--MCLOUD-7190-->

      - **Elasticsearch プラグイン設定** - `services.yaml` ファイルからElasticsearch プラグイン設定情報を使用して、Commerce環境用の Cloud Docker の `docker-compose.yaml` ファイルを生成できるようになりました。 [Elasticsearch プラグイン &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789--> を参照

      - **Elasticsearch プラグインのサポート** - Elasticsearch プラグイン `analysis-icu`、`analysis-phonetic`、`analysis-stempel`、`analysis-nori` のサポートを追加しました。 `analysis-icu` プラグインと `analysis-phonetic` プラグインがデフォルトでインストールされます。 必要に応じて、`analysis-stempel` と `analysis-nori` プラグインを追加または削除できます。<!--MCLOUD-2789-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**CLI コンテナ**

      - **Docker PHP コンテナ内でコマンドを実行** - ホストに PHP をインストールしなくても、Cloud Docker CLI を使用して、Docker 環境の PHP コンテナ内でコマンドを実行できるようになりました。 例えば、次のコマンドは、設定をビルドします。`./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`。 [Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli) を参照してください。 [G Arvind が Zilker Technology から送信した修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - OpenSSH-client を PHP CLI コンテナに追加しました。 これで、`composer.json` ファイルに Composer コマンドを使用するために ssh クライアントを必要とするプライベート Git リポジトリが含まれている場合、Composer に ssh エージェント転送を使用できるようになりました。<!--MCLOUD-6008-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **TLS コンテナ** - [TLS コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) は、CentOS イメージではなく `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker イメージをベースにするようになりました。 この変更により、Cloud Docker 環境のコンテナ間で HTTPS リクエストを送信する際にエラーが発生していた問題が修正されました。<!--MCLOUD-6469-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**テストコンテナ** - アプリケーションテスト用のテストコンテナを追加し、Docker 環境でテストする場合にのみコンテナを作成するように Docker `--with-test` コマンドに `build:compose` オプションを追加しました。 [&#x200B; アプリケーションテスト &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) を参照してください <!--MCLOUD-6394-->。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**FPM-XDEBUG コンテナ**

      - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Linux で Xdebug を設定** - `--set-docker-host` コマンドに `ece-docker build:compose` オプションを追加して、Xdebug コンテナに `host.docker.internal` 値を設定しました。 このオプションは、Linux システムで Xdebug を使用する場合に必要です。 [Docker の Xdebug の設定 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) を参照してください <!--MCLOUD-6430-->。

      - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)Docker エントリポイントの Xdebug 変数設定を修正して、ログのエラー `uninitialized "with_xdebug" variable` 解決しました。 [Florent Olibaud による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Docker 設定の変更**

   - **MailHog 構成**：次の `ece-docker build:compose` コマンド・オプションを使用して、MailHog を無効にし、ポートを指定できるようになりました。`--no-mailhog`、`--mailhog-http-port`、`--mailhog-smtp-port`。 [&#x200B; メールの設定 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email) を参照してください。<!--MCLOUD-6898, MCLOUD-6660-->

   - Cloud Docker for Commerce 1.2.0 以降では、Adobeは、各パッチバージョンの Docker イメージを提供するようになりました。Docker configuration generator は、最新のバージョンを使用する代わりに、指定されたパッチバージョンで Docker 設定を作成します。 以前は、Docker Configuration Generator は、以前のバージョンでビルドされたCommerce環境用の Cloud Docker を破損する可能性がある最新のパッチバージョンを使用して設定をビルドしていました。<!--MCLOUD-7093-->

   - **カスタムの Cloud Docker 設定でカスタムの画像とバージョンを指定** - カスタム Docker コンポーズ設定ファイル（`build:custom:compose`）を生成する際に、カスタムの画像とバージョンを指定するオプションを追加して `docker-compose.yaml` コマンドを更新しました。 [&#x200B; カスタム Docker Compose 設定の作成 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/) を参照してください。<!--MCLOUD-7089-->

   - ポート 443 を公開するように Docker ホスト設定を更新して、すべての CLI コンテナからAdobe Commerce（`https://magento2.docker`）へのアクセスを有効にしました。 デフォルトのポートは、Docker 設定ファイルの生成時に `--tls-port` オプションを追加することで変更できます。<!--MCLOUD-6806-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `app/etc/env.php` ファイルが存在する場合に、Cloud Docker for Commerceのビルドが失敗する問題を修正しました。<!--MCLOUD-6732-->

- ![fix icon](../../assets/fix.svg) Linux または Windows Subsystem for Commerce（WSL2）に Cloud Docker for Linux をデプロイする際の問題を防ぐために、指定したボリュームを通常のボリュームに置き換えるようにビルド設定を更新しました。<!--MCLOUD-6732-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) Composer 2.0 をサポートするように Cloud Docker for Commerce機能テストを更新しました。<!--MCLOUD-7183-->

## v1.1.2

リリース日：2020 年 9 月 9 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)Elasticsearch 7.7 がサポートされるようになりました <!--MCLOUD-6219-->

## v1.1.1

リリース日：2020 年 8 月 5 日（PT）

- ![fix icon](../../assets/fix.svg)**更新されたメール設定** - SendMail を使用する代わりに MailHog サービスをサポートするために、デフォルトの Cloud Docker for Commerce設定を更新しました。 [&#x200B; メールの設定 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email) を参照してください。<!--MCLOUD-5624-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) エラーを修正するために PS ライブラリを Cloud Docker 環境設定に復元 `ps:  command not found` ました。<!--MCLOUD-6621-->

- ![fix icon](../../assets/fix.svg) データベースエントリポイントと MariaDB ボリュームの自動マウントを削除して、Commerce Docker の起動時に発生する可能性のあるエラーを修正するため `Cannot create container for service db`、デフォルトの Cloud Docker for Cloud Docker 設定を更新しました。

  これで、`ece-docker build:compose` コマンドに `--with-entry-point` および `with-mariadb-conf` オプションを追加することで、データベースディレクトリをマウントするように Cloud Docker 環境を設定できるようになりました。 [&#x200B; サービス設定オプション &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options) を参照してください。<!--MCLOUD-6424-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**CLI コマンドの更新**

| アクション | コマンド |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| データベース コンテナーにエントリ ポイントを追加して、バックアップからデータベースを復元します | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| MariaDB 構成ボリュームの追加 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

リリース日：2020 年 6 月 25 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**分割データベースパフォーマンスソリューションのサポートが追加されました**—Cloud Docker 環境で分割データベースパフォーマンスソリューションを使用して、ストアを設定およびデプロイできるようになりました。<!--MCLOUD-3740-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Adobe CommerceとMagento Open Sourceのデプロイメントのサポート** - Cloud Docker for Commerceを使用して、クラウドインフラストラクチャ上のAdobe Commerceでホストされていないプロジェクトのローカル開発環境をデプロイできるようになりました。<!--MCLOUD-5667-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Blackfire.io のサポート** - [Blackfire.io 拡張機能 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/) を使用して自動パフォーマンステストを実行できるようになりました。 [Zilker Technology の Adarsh Manickam による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**コンテナの更新**

   - **Varnish** - サポートされているバージョンの Cloud Application テンプレートを使用して Cloud Docker 環境にAdobe Commerceをデプロイする場合、Varnish がデフォルトのキャッシュになりました。 [&#x200B; ニス コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) を参照してください。<!--MCLOUD-2634-->

   - Cloud Docker 設定ファイルを生成する際に、Varnish サービスのインストールをスキップする `--no-varnish` オプションを追加しました。<!--MCLOUD-2634-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **データベース**

      - MySQL データベースのサポートを追加しました。 これで、MariaDB または MySQL を使用して Cloud Docker 環境を設定できます。 [&#x200B; サービス設定オプション &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options) を参照してください。<!--MCLOUD-5691-->

      - Docker コンポーズファイルを生成する際に、データベースレプリケーションの増分とオフセットの設定を指定する機能が追加されました。 [&#x200B; サービスコンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers) を参照してください。<!--MCLOUD-5735-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**PHP-FPM**

      - PHP 7.4 のサポートを追加しました。[Zilker Technology の Mohanela Murugan が修正を送信しました &#x200B;](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - ルートプロジェクトディレクトリの `php.ini` ファイルを Cloud Docker 環境にコピーし、カスタム PHP 設定を PHP-FPM および CLI コンテナに適用する機能が追加されました。 [PHP 設定のカスタマイズ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings) を参照してください。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - コンテナヘルスチェックを追加しました。 [Zilker Technology の Visanth Sampath によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Node.js** - セキュリティを強化するために、デフォルトの Node.js バージョンをバージョン 8 からバージョン 10 に更新しました。 Node.js バージョン 8 は非推奨で、バグ修正やセキュリティパッチによる更新はなくなりました。 [Zilker Technology の Mohan Elamurugan が提供した修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **Elasticsearch**

      - Elasticsearch 6.8、7.2、7.5、7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860--> がサポートされるようになりました。

      - Docker コンポーズ設定ファイルを生成する際に [&#128279;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container)Elasticsearch コンテナ設定をカスタマイズする機能が追加されました。<!--MCLOUD-3059-->

      - Docker Compose 設定ファイルを生成するためのサービス設定オプションに `--no-es` オプションを追加しました。 Elasticsearch コンテナのインストールをスキップして MySQL 検索を使用する場合は、このオプションを使用します。 このオプションは、Adobe Commerce バージョン 2.3.5 以前でのみサポートされています。<!--MCLOUD-3766-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**FPM-XDEBUG コンテナ**—Cloud Docker 環境で PHP をデバッグするために Xdebug をインストールして設定するサービス設定オプションを追加しました。 [Xdebug の設定 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) を参照してください <!--MCLOUD-4098-->。

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Docker 設定の変更**

   - PHP-FPM、Redis、Elasticsearch、および MySQL Docker サービスコンテナのヘルスチェックを追加しました。<!--MCLOUD-3335 and MCLOUD-5856-->

   - 開発者モードで、デフォルトのファイル同期モードを `native` に変更しました。<!--MCLOUD-3890 -->

   - `docker-compose.yml` ファイルを生成する際の汎用 Docker サービスコンテナイメージにバージョン情報を追加しました。<!--MCLOUD-3878-->

   - Nginx サーバの `fastcgi_buffers` 値を増やすことで、アップストリームの PHP-FPM コンテナからの大きな応答を処理する機能を改善しました。<!--MCLOUD-5980-->

   - `vendor` ディレクトリ内のファイルを同期するための 2 番目の同期セッションを追加することで、変更ファイル同期のパフォーマンスを向上しました。 この変更により、ファイルの同期処理中に変更が停止するのを防ぎます。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**CLI コマンドの更新**

| アクション | コマンド |
| -------- | --------------- |
| Redis キャッシュのクリア | `bin/magento-docker flush-redis` |
| Varnish キャッシュのクリア | `bin/magento-docker flush-varnish` |
| 既定の Varnish インストールをスキップする | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Elasticsearch オプションのカスタマイズ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Elasticsearch設定の削除 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| MySQL バージョン 5.6 または 5.7 で DB コンテナを設定する | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| カスタムベース URL を指定 | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Xdebug 設定用のコンテナの追加 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![fix icon](../../assets/fix.svg)mutagen が古いセッションを作成するのを防ぐために、mutagen ファイル同期の設定を修正しました。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![fix icon](../../assets/fix.svg) PHP-FPM コンテナを起動する際に Docker の作成ログに構文エラーが発生する設定の問題を修正しました。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) 複数の Docker 環境を使用する際に発生することがあったボリューム競合エラーを修正しました。 [Zilker Technology の G Arvind による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/168)。

- ![fix icon](../../assets/fix.svg) 設定にBlackfire.io が含まれている場合に、`ece-docker build:compose` コマンドが失敗する問題を修正しました。 [Zilker Technology の G Arvind 氏により修正が提出されました &#x200B;](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![fix icon](../../assets/fix.svg) Cloud Docker for Commerceを使用して複数のパッケージをインストールする際に発生するメモリ不足エラーを防ぐために、PHP CLI イメージ設定を更新しました。 [Zilker Technology の Mohan Elamurugan による修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/197)。*<!--MCLOUD-5818-->

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)Cloud Docker 環境で複数の MySQL ユーザーをサポートするようになりました。 以前のリリースでは、`build:compose` ファイルで複数のデータベース ユーザが指定されている場合、`magento.app.yaml` 操作は失敗していました。 [G Arvind が Zilker Technology から送信した修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![fix icon](../../assets/fix.svg) デプロイ中に警告を表示する原因となった互換性の問題を解決するために、Cloud Docker for Commerce PHP コンテナから `rsyslog` を削除しました。 Cloud Docker は rsyslog ユーティリティを使用しません。<!--MCLOUD-6173-->

## v1.0.0

リリース日：2020 年 2 月 5 日（PT）

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **`Cloud Docker for Commerce`** を配信するための個別のパッケージを作成 – Commerce用 Cloud Docker を配信するためのソースコードを `ece-tools` リポジトリーから [&#x200B; 新しい `magento-cloud-docker` リポジトリー &#x200B;](https://github.com/magento/magento-cloud-docker) に移動して、コード品質を維持し、独立したリリースを提供するようになりました。 新しいパッケージは ECE-Tools v2002.1.0 以降の依存関係です。

  ece-tools を更新すると、`magento/magento-cloud-docker` パッケージもバージョン 1.0.0 に更新されます。以前の `ece-tools` リリース（2002.0.x）で Cloud Docker for Commerceを使用していた場合は、[&#x200B; 下位非互換性 &#x200B;](backward-incompatible-changes.md) を確認し、必要に応じてスクリプト、コマンド、プロセスとしてプロジェクトを更新します。

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Docker イメージへのバージョン管理の追加** – 更新されたイメージを取得するには、`magento/magento-cloud-docker` パッケージを更新する必要があります。<!--MAGECLOUD-4737-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**コンテナの更新**—

   - ![new icon](../../assets/new.svg)**PHP-FPM コンテナ**—

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Node.js のサポートの追加**—PHP コンテナ内の node、npm、および grunt-cli 機能をサポートするように PHP-FPM イメージを更新しました。<!--MAGECLOUD-3953-->

      - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **[ionCube](https://www.ioncube.com/)** のサポートの追加 – ローカル Docker 開発環境で ionCube をサポートするようにデフォルトの Docker 設定を更新しました。<!--MAGECLOUD-4354-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Web コンテナ**—

      - ![new icon](../../assets/new.svg)**Customize NGINX configuration** – カスタム `nginx.conf` ファイルを Cloud Docker for Commerce環境にマウントできるようになりました。 [Web コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container) を参照してください。<!--MAGECLOUD-4204-->

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**自動生成された NGINX 証明書** - Docker 設定ファイルに、Web コンテナ用に NGINX 証明書を自動生成するための設定が含まれるようになりました。<!--MAGECLOUD-4258-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**新規 Selenium コンテナ** - Magento Functional Testing Framework （MFTF）を使用したAdobe Commerce アプリケーションテストをサポートするために、[Selenium コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container) が追加されました。<!--MAGECLOUD-4040-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **[!DNL RabbitMQ]バージョンのサポート** - [!DNL RabbitMQ] コンテナ設定を更新して、[!DNL RabbitMQ] バージョン 3.8.<!--MAGECLOUD-4674--> をサポートするようになりました

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) **永続的なデータベースコンテナ** - Docker 設定を停止して削除した後も、`magento-db: /var/lib/mysql` データベースボリュームが保持され、Docker 設定を再起動すると復元されるようになりました。 ここで、データベース・ボリュームを手動で削除する必要があります。 [ データベースコンテナ ] を参照してください <!--MAGECLOUD-3978-->。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**TLS コンテナ**—

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) **公式の画像を使用するようにコンテナベース画像を更新**—[&#x200B; クラウド TLS コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) 画像は、公式の `debian:jessie` Docker 画像に基づくようになりました。—<!--MAGECLOUD-4163-->

      - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **[ ポンド TLS 終了プロキシのサポートの追加]** - [&#x200B; ポンド設定ファイル &#x200B;](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) には、次の ENV 変数が追加され、TLS コンテナ用の Docker 設定がカスタマイズされています。

         - **`TimeOut`** - Time to First Byte （TTFB）タイムアウト値を設定します。 デフォルト値は 300 秒です。

         - **`RewriteLocation`** - ポンド・プロキシによってデフォルトで場所が要求 URL に書き換えられるかどうかを決定します。 デフォルトでは `0` に設定されており、外部の SSO サイトなど、外部の web サイトへのリダイレクトが書き換えによって中断されるのを防ぎます。 [&#x200B; ソリン・シュガーによる修理 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg) TLS コンテナ設定のタイムアウト値を 15 秒から 300 秒に増加しました。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**ニス コンテナ**—

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**公式の画像を使用するようにコンテナベース画像を更新**—[Cloud Varnish コンテナ &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) は、公式の `centos` Docker 画像をベースにするようになりました。<!--MAGECLOUD-4163-->

      - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**改善されたデフォルトのタイムアウト設定** Varnish コンテナに `.first_byte_timeout` と `.between_bytes_timeout` の設定が追加されました。 両方のタイムアウト値はデフォルトで `300s` （5 分）になります。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**Xdebug セッション中にワニスをスキップ** - Xdebug が有効な場合に受信したリクエストに `pass` を返すように、ワニスのコンテナ設定を更新しました。 以前のリリースでは、Docker 環境に Varnish が含まれている場合、Xdebug を使用できませんでした。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**Docker 設定の変更**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**プロジェクトのマウントとボリュームの管理** - Docker 環境を起動してローカル開発する際にマウントとボリュームを管理する機能が追加されました。 [ プロジェクトデータの共有 ].<!--MAGECLOUD-3248--> を参照してください。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**ネットワークブリッジモードのサポート** - ローカルネットワーク上の Docker コンテナ間の接続を有効にする、ネットワークブリッジモードのサポートを追加しました。<!--MAGECLOUD-4165-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**デフォルトで Cron コンテナが無効** - パフォーマンスを向上させるために、Docker 環境のビルド時に Cron コンテナはデフォルトでは設定されなくなりました。 Docker ビルドコマンドの `--with-cron` オプションを使用して、Cron コンテナを環境に追加できます。 [cron ジョブの管理 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) を参照してください <!--MAGECLOUD-5181-->。

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**大きなバックアップファイルの同期を停止**—DB ダンプとアーカイブファイル（ZIP、SQL、GZ、BZ2）を `dist/docker-sync.yml` ファイルと `dist/mutagen.sh` ファイルの除外リストに追加しました。 大きなファイル（1 GB 超）を同期すると、非アクティブな状態が長時間続く可能性があり、バックアップファイルは再生成できるので、通常は同期は必要ありません。<!--MAGECLOUD-3979-->

- ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg)**コマンドの変更**—

   - ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg) `./bin/docker` ファイルが既存の Docker バイナリファイルを上書きするので、一部の Docker 環境が壊れる問題を修正するために、`./bin/magento-docker` ファイルの名前を `./bin/docker` に変更しました。 これは [&#x200B; 後方互換性のない変更 &#x200B;](backward-incompatible-changes.md) であり、スクリプトやコマンドを更新する必要があります。<!-- MAGECLOUD-4038 -->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **データベース・ポートをホストに公開するサービス構成オプションが追加されました**—`--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` のオプションを使用して、`docker-compose.yml` ファイルの構築時にデータベース・ポートをホストに公開します：`bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) **新しいデプロイ後コマンド** – 以前は、`.magento.app.yaml` コマンドを使用してAdobe Commerceを Cloud Docker コンテナにデプロイした後、`cloud-deploy` ファイルで定義されたデプロイ後フックが自動的に実行されていました。 次に、デプロイ後にデプロイ後のフックを実行するには、別の `cloud-post-deploy` コマンドを発行する必要があります。 [&#x200B; 開発者 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) モードと [&#x200B; 実稼動 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/) モードの、更新されたローンチ手順を参照してください。<!--MAGECLOUD-3996-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) ビルドコンテナおよびデプロイコンテナのコマンドを `--rm` ストするための `./bin/magento-docker` オプションが追加されました。 これにより、タスクが完了した後にコンテナが削除されます。<!--MAGECLOUD-4205-->

   - ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**コマンド `build:compose` 更新**—

      - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) デベロッパーモードで Docker Compose 設定ファイルを生成する際のファイル同期を無効にするために、`--sync-engine="native"` コマンドに「`docker-build`」オプションを追加しました。 このオプションは、ローカル Docker 開発用のファイル同期を必要としない Linux システムで開発する場合に使用します。 [Docker 環境でのデータの同期 &#x200B;](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/) を参照してください <!--MCLOUD-3231, MCLOUD-3890-->。

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) デフォルトのファイル同期設定を `docker-sync` から `native` に変更しました。 [Zilker Technology から Mathew Beane によって送信された修正 &#x200B;](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**検証の改善**—

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) ローカル Docker 開発環境のデプロイメントプロセスに検証を追加し、クラウド環境設定にデータベースの復号化に必要な暗号化キーが含まれていることを確認しました。 これで、環境設定で暗号化キーの値が指定されていない場合、ログにエラーメッセージが表示されるようになりました。<!--MAGECLOUD-4423-->

   - ![&#x200B; 新規アイコン &#x200B;](../../assets/new.svg) Elasticsearch サービスにコンテナヘルスチェックを追加して、ビルドおよびデプロイ処理を続行する前にサービスの準備が整っていることを確認しました。 ヘルスチェックがエラーを返した場合、コンテナは自動的に再起動します。<!--MAGECLOUD-4456-->
