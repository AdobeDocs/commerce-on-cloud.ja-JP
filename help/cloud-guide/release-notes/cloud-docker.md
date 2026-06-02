---
title: Cloud Docker パッケージ
description: Cloud Docker パッケージの最新の改善点の一覧を参照してください。
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00.000Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
TQID: https://experienceleague.adobe.com/H-A-2jStZ7GuPn2oE-OrZWhScp1GsjEUU1NHDQKhRBU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 4496
ht-degree: 0%

---

# Cloud Docker パッケージ

[`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) パッケージには、Adobe Commerceをローカル Cloud環境にデプロイするための機能とDocker イメージが用意されています。 このリリースノートでは、[Cloud Tools Suite for Commerce](cloud-tools-suite.md)のコンポーネントであるこのパッケージの最新の機能強化について説明します。

`magento/magento-cloud-docker` パッケージでは、次のバージョン シーケンスが使用されています：`<major>.<minor>.<patch>`

リリースノートには以下が含まれます。

- ![新しいアイコン ](../../assets/new.svg)新機能
- ![修正アイコン ](../../assets/fix.svg)修正と機能強化

<!--Add release notes below-->

## v1.4.8 {#latest}

リリース日：2026年5月6日（PT）

- ![新しいアイコン ](../../assets/new.svg) **サービステスト**&#x200B;を更新 – MariaDB、RabbitMQ、ActiveMQ、Opensearch、Valkeyのテストを更新しました。<!-- MCLOUD-14821 -->
- ![新しいアイコン ](../../assets/new.svg) **Valkey** - Valkey 8.1のサポートを追加しました。<!-- MCLOUD-14784 -->
- ![新しいアイコン ](../../assets/new.svg) **Opensearch image** - Opensearch 2.19および3.5.<!-- MCLOUD-14790/MCLOUD-14785 -->を追加しました
- ![新しいアイコン ](../../assets/new.svg) **ActiveMQ** - ActiveMQ 2.51のサポートを追加しました。<!-- MCLOUD-14683 -->
- ![新しいアイコン ](../../assets/new.svg) **MariaDB** - MariaDB 11.8および12.2のサポートを追加しました。<!-- MCLOUD-14635 -->
- ![ アイコンを修正](../../assets/fix.svg) **MailHog** - MailHog画像を修正しました。<!-- MCLOUD-14663 -->

## v1.4.7

リリース日：2026年3月5日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.5** - PHP 8.5のサポートを追加しました。<!-- MCLOUD-14180 -->
- ![新しいアイコン ](../../assets/new.svg) **追加`php-cli`および`php-fpm` 8.5画像** - PHP 8.5をサポートする新しいCloud Docker イメージ（CLIおよびFPM）を追加しました。<!-- MCLOUD-14178 -->
- ![新しいアイコン ](../../assets/new.svg) **PHP 8.5およびOpensearch 3.0のDocker イメージ生成コマンドを追加**&#x200B;解決済みDocker ネットワークの分離（ARM64を含む）、固定統合テスト、Docker イメージ生成コマンドに対するPHP 8.5およびOpenSearch 3.0のサポートを追加<!-- MCLOUD-14523 -->

## v1.4.6

リリース日：2025年11月13日（PT）

- ![ アイコンを修正](../../assets/fix.svg) **Symfony パッケージ** – 最新のSymfony YAML パッケージのサポートを追加しました。<!-- MCLOUD-14020 -->

## v1.4.5

リリース日：2025年10月8日（PT）

- ![新しいアイコン ](../../assets/new.svg) **ActiveMQ** – 機能テストを使用したcloud-dockerでのActiveMQ サポートを追加しました。<!-- MCLOUD-13771 -->

## v1.4.4

リリース日：2025年8月7日（PT）

- ![修正アイコン ](../../assets/fix.svg) **PHP 8.4** - PHP 8.4 テストを追加しました。<!-- MCLOUD-13311 -->
- ![修正アイコン ](../../assets/fix.svg) **FTP拡張機能**-FTP拡張機能の修正が追加されました。<!-- MCLOUD-13843 -->
- ![新しいアイコン ](../../assets/new.svg) **Opensearch3 イメージ** - Opensearch3のサポートを追加しました。<!-- MCLOUD-13766 -->
- ![新しいアイコン ](../../assets/new.svg) **Opensearch3 テスト** - Opensearch3用のPHP 8.4 テストを追加しました。<!-- MCLOUD-13768 -->
- ![新しいアイコン ](../../assets/new.svg) **Valkey** - Valkeyのサポートを追加しました。<!-- MCLOUD-13558 -->

## v1.4.3

リリース日：2025年6月3日（PT）

- ![ アイコンの修正](../../assets/fix.svg) **2.4.8**&#x200B;との互換性を向上させるために、2.4.8<!-- MCLOUD-13707- -->との互換性を向上させるために、サードパーティ製ライブラリを更新しました

## v1.4.2

リリース日：2025年4月7日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.4** - `php-cli`個の8.4画像と`php-fpm`個の8.4画像を追加しました。


## v1.4.1

リリース日：2025年2月6日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.4** - PHP 8.4のサポートを追加しました。


## v1.4.0

リリース日：2024年10月7日（PT）

- ![fix icon](../../assets/fix.svg) **リファクタリングされたコード** – 古いPHP バージョン（7.4、7.3、7.2）および関連するライブラリと画像のサポートを削除しました。

## v1.3.7

リリース日：2024年4月8日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP** — PHP 8.3およびPHP 8.3 イメージのサポートを追加しました。
- ![new icon](../../assets/new.svg) **Nginx** – 画像nginx vを追加しました。 1.24.
- ![新しいアイコン ](../../assets/new.svg) **Opensearch** – 画像OpenSearch vを追加しました。 2.12, 1.3.
- ![新しいアイコン ](../../assets/new.svg) **Composer** - Composer バージョンを2.2.23に更新しました。

## v1.3.6

リリース日：2023年7月31日（PT）

- ![新しいアイコン ](../../assets/new.svg) **新しいサービスバージョン**—OpenSearch 2.5を追加しました。
- ![新しいアイコン ](../../assets/new.svg) **コンポーザーのキャッシュを有効にする**—Docker コンテナの起動時にコンポーザーのクリア キャッシュを有効にするようにDocker設定を拡張できるようになりました。 _Cloud Docker for Commerce_ ガイドの「[Docker設定を拡張](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)」を参照してください。

## v1.3.5

リリース日：2023年3月10日（PT）

- ![new icon](../../assets/new.svg) **ionCube** - PHP 8.1 イメージのionCube拡張機能を追加しました。
- ![新しいアイコン ](../../assets/new.svg) **新しいサービスバージョン**&#x200B;を追加しました – OpenSearch 2.3および2.4、PHP 8.2、Varnish 7.1.1。
- ![新しいアイコン ](../../assets/new.svg) **PHP 8.2**&#x200B;の拡張サポート – 特定のPHP 8.2.x バージョンでCommerce 2.4.6をサポートする際の互換性の問題を修正しました。
- ![ アイコンを修正](../../assets/fix.svg) **コンポーザーの問題** - Docker コンテナ内のコンポーザーのバージョンを更新した後に発生した問題を修正しました。

## v1.3.4

リリース日：2022年10月27日（PT）

- ![新しいアイコン ](../../assets/new.svg) **新しいVarnish画像を追加** - Varnish 6.5、7.0、および7.1の画像を追加しました。<!-- MCLOUD-7879 -->

## v1.3.3

リリース日：2022年9月13日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Apple M1 （ARM64）のサポート** - Docker イメージに変更を追加して、Apple M1 （ARM64） アーキテクチャのサポートを有効にしました。<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![ アイコンを修正](../../assets/fix.svg) **Mailhog** – 開発者モードでメールホグサービスが電子メールをキャッチしない問題を修正しました。<!-- MCLOUD-8643 -->
- ![fix icon](../../assets/fix.svg) **init-docker.sh**-`init-docker.sh` スクリプトのサービスバージョン検証ツールを修正しました。<!-- MCLOUD-8765 -->

## v1.3.2

リリース日：2022年3月31日（PT）

- ![新しいアイコン ](../../assets/new.svg) **Elasticsearch 7.10の画像を追加**<!-- MCLOUD-8548 -->

## v1.3.1

リリース日：2022年3月10日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.1**&#x200B;をサポート - PHP 8.1のサポートを追加しました。
- ![新しいアイコン ](../../assets/new.svg) **OpenSearch** - OpenSearch バージョン 1.1および1.2の画像を追加しました。
- ![新しいアイコン ](../../assets/new.svg) **Composer 2.1** - PHP 8.x イメージでデフォルトでcomposer 2.1.xを設定します。
- ![新しいアイコン ](../../assets/new.svg) **PHP画像の改善**—

   - PHP 8.1の画像を追加
   - アップグレードされたxDebug バージョン 3.1.2
   - アップグレードされたxmlrpc 1.0.0RC3

- ![fix icon](../../assets/fix.svg) **ElasticsearchとOpenSearchの機能強化** - ElasticsearchとOpenSearch Dockerfilesの機能強化。Elasticsearch 5.2 イメージが削除されました。
- ![fix icon](../../assets/fix.svg) **ナトリウム拡張機能** – すべてのPHP画像で`sodium`拡張機能をデフォルトで有効にしました。
- ![ アイコンを修正](../../assets/fix.svg) **Composer キャッシュ ボリューム** - Composer パッケージをキャッシュするComposer キャッシュ ボリュームのパスを修正しました。
- ![ アイコンを修正](../../assets/fix.svg) **nginx**&#x200B;のメモリ制限 – NGINX イメージのメモリ制限を修正しました。

## v1.3.0

リリース日：2021年10月25日（PT）

- ![ アイコンの修正](../../assets/fix.svg) **開発者モードの改善ワークフロー** – 以前は、ビルドとデプロイの手順でモードを指定する必要がありました。 ここで、`build` ステップの`--mode` オプションによって、後の`deploy` ステップのモードが決定されます。 デプロイメント後のモードの設定は不要になりました。 [開発者モード ](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode)を参照してください。<!-- ACMP-1086 -->
- ![ アイコンの修正](../../assets/fix.svg) **読み取り専用のファイルシステムの改善**—<!-- ACMP-1106 -->
   - メール設定用のPHP コンテナを開始する際の問題を修正しました。
   - INI ファイルで環境変数を使用できます。
   - PHPのエントリポイントに書き込み権限が必要ないことを確認します。
- ![fix icon](../../assets/fix.svg) **Update Node** – バンドルされたNode バージョンを更新します。PHP-CLI イメージにNodeをインストールする場合、現在のLTS バージョンが使用されるようになりました。<!-- ACMP-1539 -->
- ![fix icon](../../assets/fix.svg) **Symfony**&#x200B;を更新 – Symfony設定の依存関係を更新して、Adobe Commerce 2.4.4と互換性を持たせました。<!-- ACMP-1533 -->

## v1.2.4

リリース日：2021年7月29日（PT）

- ![新しいアイコン ](../../assets/new.svg) **新しい`Zookeeper` コンテナ** - [Zookeeper コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#zookeeper-container)を追加して、Cloud Infrastructure上のAdobe Commerceにデプロイされていないプロジェクトのロックプロバイダー設定を管理しました。<!--MCLOUD-8000-->

- ![新しいアイコン ](../../assets/new.svg) **Composer 2.0のサポートを追加しました。** - Composer設定ファイルにComposer バージョン 2.0を追加し、サポート終了に近づいているComposer 1.0からのアップグレードをサポートしました。<!--MCLOUD-8003-->

## v1.2.3

リリース日：2021年6月14日（PT）

- ![新しいアイコン ](../../assets/new.svg) **PHP 8.0**&#x200B;を追加 – PHPをバージョン 8.0に更新し、PHP 8.0に含まれるすべての新機能と最適化を活用できるようにしました。<!--MCLOUD-7941-->
- ![新しいアイコン ](../../assets/new.svg) **Varnish 6.6およびElasticsearch 7.11.2**&#x200B;に更新 – 次のリンクは、[Varnish キャッシュ 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0)およびElasticsearch 7.11.2.<!--MCLOUD-7921-->に関するリリース情報を提供します
- ![新しいアイコン ](../../assets/new.svg) **PHP 7.4画像**&#x200B;用の`ioncube`拡張機能を追加しました – `ioncube`拡張機能が、PHP 7.3からPHP 7.4へのアップグレードから最初に除外された後、PHP 7.4画像に再追加されました。 *[mattskrによって送信されました](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![新しいアイコン ](../../assets/new.svg) **ファイル同期オプションを追加しました：`manual-native`** - `manual-native` ファイル同期オプションは、手動で同期を制御し、macOSおよびWindows環境に最適なパフォーマンスを提供します。 [開発者モード ](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode)および[Docker開発者環境でのデータの同期](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data#file-synchronization-options)で`manual-native` オプションを使用する方法について説明します。<!--MCLOUD-7977-->
- ![新しいアイコン ](../../assets/new.svg) **ボリューム削除を`up`および`down` コマンド**&#x200B;から削除しました – `--volume` オプションが`bin/magento-docker up`および`bin/magento-docker down` コマンドから削除され、新しい`bin/magento-docker init` コマンドがデータ損失警告に置き換えられました。 この変更は、偶発的なデータ損失を防ぐのに役立ちます。 *[送信者joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![修正アイコン ](../../assets/fix.svg) **生成された証明書の`CN`値を更新** - ハードコードされた`CN`値をDockerfileから削除しました。 この値により、証明書エラー（`NET::ERR_CERT_INVALID`）が作成され、`ece-docker build:compose` コマンドの`--host` オプションが無視されました。<!--MCLOUD-7934-->

## v1.2.2

リリース日：2021年4月20日（PT）

- ![新しいアイコン ](../../assets/new.svg) **プラットフォームに依存しないように`host.docker.internal`を更新しました**- Ubuntu、Windows、macOS用に同じDocker Compose スクリプトを作成できるようになりました。 UbuntuでXdebugを使用する場合、別の環境変数は必要なくなりました。 [Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299)によって送信された修正<!--Issue #298-->
- ![new icon](../../assets/new.svg) **init-docker.sh**&#x200B;を更新 – `mounts` オブジェクトを`MAGENTO_CLOUD_APPLICATION`環境変数に追加しました。 [Chiranjeeviによって送信された修正](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![新しいアイコン ](../../assets/new.svg) **init-docker.sh**&#x200B;を更新 – PHP 7.4およびCloud Docker 1.2.1のバージョンで`init-docker.sh` スクリプトを更新しました。 [Adarsh Manickamによって送信された修正](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![新しいアイコン ](../../assets/new.svg) **Sodiumがデフォルトで有効** - PHP Docker イメージ内で`sodium` PHP拡張機能をデフォルトで有効にしました。<!--MCLOUD-7548-->
- ![新しいアイコン ](../../assets/new.svg) **`custom-registry`オプション** – 独自の画像レジストリを使用するための`php ./vendor/bin/ece-docker build:compose` コマンドに`--custom-registry` オプションを追加しました。<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![新しいアイコン ](../../assets/new.svg) **古いElasticsearch バージョン**&#x200B;を削除 – Elasticsearch バージョン 1.7および2.4をElasticsearch イメージから削除しました。<!--MCLOUD-7504-->
- ![新しいアイコン ](../../assets/new.svg) **自動生成NGINX証明書** - NGINX イメージから既存の証明書を削除しました。 NGINX証明書は、セキュリティを向上させるために、新しいデプロイメントごとに自動生成されるようになりました。<!--MCLOUD-7396-->
- ![修正アイコン ](../../assets/fix.svg) **有効`opcache.validate_timestamps`** – 開発者モードでデフォルトで`opcache.validate_timestamps` PHP設定を有効にしました。 この設定を有効にすると、Dockerでファイルシステムへの変更が認識されない問題が修正されました。<!--MCLOUD-7466-->
- ![修正アイコン ](../../assets/fix.svg) **修正`build:custom:compose`** - ビルドプロセス中にファイルを上書きできない場合にエラーをスローする`build:custom:compose` コマンドを修正しました。 エラーをスローすると、`docker-compose up`が間違ったファイルを使用する可能性がある状況を防ぐことができます。<!--MCLOUD-7457-->
- ![ アイコンを修正](../../assets/fix.svg) **修正`--sync_engine="native"` オプション** – 実稼動モード （`--mode="production"`）で、`--sync_engine="native"` オプションで`docker.composer.yml` ファイルのローカルフォルダーのエントリが作成されない問題を修正しました。<!--MCLOUD-7254-->
- ![fix icon](../../assets/fix.svg) **固定サービスバージョン検証エラー** - [!DNL RabbitMQ]、Elasticsearch、およびその他のサービスのサービスバージョンを`MAGENTO_CLOUD_RELATIONSHIP`変数の`type` プロパティに追加しました。 これらのバージョンを`relationships`変数に追加すると、デプロイ フェーズで発生した検証エラーが修正されました。<!--MCLOUD-7572-->

## v1.2.1

リリース日：2020年12月21日（PT）

- ![新しいアイコン ](../../assets/new.svg) **NGINX コマンド オプション** - TLSおよびWeb サービスのNGINX `worker_processes`とNGINX `worker_connections`の数を変更するためのビルド コマンド オプションを追加しました。 `worker_process` パラメーターは、値を`auto`に設定する機能を保持します。 例：<!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![新しいアイコン ](../../assets/new.svg) **TLS コマンドオプション** - TLS サービスを使用せずに設定を作成するためのビルドコマンドオプションを追加しました。 例：<!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![新しいアイコン ](../../assets/new.svg) **NGINX メモリ消費量** - TLSおよびWeb サービスのNGINX プロセスで消費されるメモリを削減しました。<!--MCLOUD-7259-->

- ![新しいアイコン ](../../assets/new.svg) **Blackfire** - Cloud Docker イメージでBlackfire PHP拡張機能をデフォルトで無効にしました。

- ![ アイコンの修正](../../assets/fix.svg) **PHP-FPM コンテナ** - `WEB_PORT`を`80`から`8080`に変更して、PHP-FPM コンテナのヘルスチェックを修正しました。<!--MCLOUD-7232-->

- ![修正アイコン ](../../assets/fix.svg) **無効なボリューム命名** – 開発者モードでの無効なボリューム命名に関するエラーを修正しました。<!--MCLOUD-7442-->

- ![fix icon](../../assets/fix.svg) **NGINX アップストリームポート** - Docker NGINX 1.19 イメージを更新して、ポート 8080を使用して無限ループを回避しました。 [Adarsh Manickamによって送信された修正](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

リリース日：2020年11月9日（PT）

- ![新しいアイコン ](../../assets/new.svg) **コンテナの更新 –**

   - ![新しいアイコン ](../../assets/new.svg) **PHP-FPM コンテナ** - gnupg PHP拡張機能のサポートを追加しました。 [Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210)からG Arvindによって送信された修正<!--MCLOUD-5981-->

   - ![fix icon](../../assets/fix.svg) **データベースコンテナ** – 必要なデータベースパスワードをヘルスチェックコマンドに追加して、データベースコンテナのヘルスチェックを修正しました。<!--MCLOUD-7122-->

   - ![新しいアイコン ](../../assets/new.svg) **Elasticsearch コンテナ**

      - 今後のAdobe Commerce リリースとの互換性を保つため、Elasticsearch 7.9のサポートが追加されました。<!--MCLOUD-7190-->

      - **Elasticsearch プラグイン設定** - `services.yaml` ファイルのElasticsearch プラグイン設定情報を使用して、Cloud Docker for Commerce環境の`docker-compose.yaml` ファイルを生成するサポートを追加しました。 [Elasticsearch プラグイン ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-plugins)を参照してください。<!--MCLOUD-2789-->

      - **Elasticsearch プラグインのサポート** – 次のElasticsearch プラグインのサポートが追加されました：`analysis-icu`、`analysis-phonetic`、`analysis-stempel`、および`analysis-nori`。 `analysis-icu`と`analysis-phonetic` プラグインはデフォルトでインストールされます。 必要に応じて、`analysis-stempel`および`analysis-nori` プラグインを追加または削除できます。<!--MCLOUD-2789-->

   - ![新しいアイコン ](../../assets/new.svg) **CLI コンテナ**

      - **Docker PHP コンテナ内でコマンドを実行** – これで、Cloud Docker CLIを使用して、ホストにPHPをインストールすることなく、Docker環境のPHP コンテナ内でコマンドを実行できます。 例えば、次のコマンドは設定をビルドします：`./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`。 [Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli)を参照してください。 [Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209)からG Arvindによって送信された修正<!--MCLOUD-5982-->

      - OpenSSH-clientをPHP CLI コンテナに追加しました。 これで、`composer.json` ファイルにComposer コマンドを使用するためにssh クライアントを必要とするプライベート Git リポジトリが含まれている場合は、Composerに対してssh エージェント転送を使用できます。<!--MCLOUD-6008-->

   - ![修正アイコン ](../../assets/fix.svg) **TLS コンテナ** – 現在、[TLS コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#tls-container)はCentOS イメージではなく`https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker イメージに基づいています。 この変更は、Cloud Docker環境のコンテナ間でHTTPS リクエストを送信する際にエラーが発生する問題を修正します。<!--MCLOUD-6469-->

   - ![新しいアイコン ](../../assets/new.svg) **テストコンテナ** - アプリケーションテスト用のテストコンテナを追加し、Docker `build:compose` コマンドに`--with-test` オプションを追加して、Docker環境でテストする場合にのみコンテナを作成しました。 [ アプリケーションテスト ](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing)を参照してください。<!--MCLOUD-6394-->

   - ![新しいアイコン ](../../assets/new.svg) **FPM-XDEBUG コンテナ**

      - ![新しいアイコン ](../../assets/new.svg) **Linux**&#x200B;でのXdebugの設定 – `ece-docker build:compose` コマンドに`--set-docker-host` オプションを追加して、Xdebug コンテナで`host.docker.internal`値を設定しました。 このオプションは、Linux システムでXdebugを使用する場合に必要です。 [Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug)のXdebugの設定<!--MCLOUD-6430-->を参照してください。

      - ![ アイコンを修正](../../assets/fix.svg) Docker ENTRYPOINTのXdebug変数設定を修正して、ログの`uninitialized "with_xdebug" variable`個のエラーを解決しました。 [Florent Olivaudによって送信された修正](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![新しいアイコン ](../../assets/new.svg) **Docker設定の変更**

   - **MailHog設定** – 次の`ece-docker build:compose` コマンドオプションを使用して、MailHogを無効にし、ポートを指定できます：`--no-mailhog`、`--mailhog-http-port`、および`--mailhog-smtp-port`。 [電子メールの設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email)を参照してください。<!--MCLOUD-6898, MCLOUD-6660-->

   - Cloud Docker for Commerce 1.2.0以降では、Adobeは各パッチバージョンのDocker イメージを提供するようになり、Docker コンフィギュレーションジェネレーターは、最新バージョンを使用する代わりに、指定されたパッチバージョンのDocker コンフィギュレーションを作成します。 以前は、Docker設定ジェネレーターは、以前のバージョンを使用して構築されたCommerce環境用Cloud Dockerを壊す可能性のある最新のパッチバージョンを使用して設定を構築していました。<!--MCLOUD-7093-->

   - **カスタム Cloud Docker構成でカスタム画像とバージョンを指定** - カスタム Docker構成構成ファイル （`docker-compose.yaml`）を生成する際に、カスタム画像とバージョンを指定するオプションを使用して`build:custom:compose` コマンドを更新しました。 [ カスタム Docker Compose設定の構築](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose)を参照してください。<!--MCLOUD-7089-->

   - すべてのCLI コンテナからAdobe Commerce （`https://magento2.docker`）へのアクセスを有効にするために、ポート 443を公開するようにDocker ホスト設定を更新しました。 Docker設定ファイルを生成する際に`--tls-port` オプションを追加することで、デフォルトポートを変更できます。<!--MCLOUD-6806-->

- ![fix icon](../../assets/fix.svg) `app/etc/env.php` ファイルが存在する場合にCloud Docker for Commerce ビルドが失敗する問題を修正しました。<!--MCLOUD-6732-->

- ![fix icon](../../assets/fix.svg) ビルド設定を更新して、Cloud Docker for Commerce LinuxまたはWindows Subsystem for Linux （WSL2）をデプロイする際の問題を防ぐために、名前付きボリュームを通常のボリュームに置き換えました。<!--MCLOUD-6732-->

- ![fix icon](../../assets/fix.svg) Composer 2.0をサポートするように、Commerce機能テスト用のCloud Dockerを更新しました。<!--MCLOUD-7183-->

## v1.1.2

リリース日：2020年9月9日（PT）

- ![新しいアイコン ](../../assets/new.svg) Elasticsearch 7.7<!--MCLOUD-6219-->のサポートを追加しました

## v1.1.1

リリース日：2020年8月5日（PT）

- ![fix icon](../../assets/fix.svg) **メール設定を更新** - SendMailを使用する代わりにMailHog サービスをサポートするように、Commerce用のデフォルト Cloud Docker設定を更新しました。 [電子メールの設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email)を参照してください。<!--MCLOUD-5624-->

- ![修正アイコン ](../../assets/fix.svg) PS ライブラリをCloud Docker環境設定に復元して、`ps:  command not found`個のエラーを修正しました。<!--MCLOUD-6621-->

- ![fix icon](../../assets/fix.svg) Cloud Docker for Commerceのデフォルト設定を更新して、データベースのエントリーポイントとMariaDB ボリュームの自動マウントを削除し、Cloud Docker環境の起動時に発生する可能性のある`Cannot create container for service db` エラーを修正しました。

  これで、次のオプションを`ece-docker build:compose` コマンド `--with-entry-point`および`with-mariadb-conf`に追加して、データベースディレクトリをマウントするようにCloud Docker環境を設定できます。 [ サービス設定オプション ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options)を参照してください。<!--MCLOUD-6424-->

- ![新しいアイコン ](../../assets/new.svg) **CLI コマンドの更新**

| アクション | コマンド |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| データベースコンテナにエントリポイントを追加して、バックアップからデータベースを復元します | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| MariaDB設定ボリュームの追加 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

リリース日：2020年6月25日（PT）

- ![新しいアイコン ](../../assets/new.svg) **分割データベースのパフォーマンスソリューションのサポートを追加** – これで、Cloud Docker環境の分割データベースのパフォーマンスソリューションを使用してストアを設定およびデプロイできるようになりました。<!--MCLOUD-3740-->

- ![新しいアイコン ](../../assets/new.svg) **Adobe CommerceおよびMagento Open Sourceのデプロイメントのサポート**—Cloud Docker for Commerceを使用して、Adobe Commerce上のクラウドインフラストラクチャでホストされていないプロジェクトにローカル開発環境をデプロイできるようになりました。<!--MCLOUD-5667-->

- ![新しいアイコン ](../../assets/new.svg) **Blackfire.io サポート** - [Blackfire.io拡張機能](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire)を自動パフォーマンステストに使用するためのサポートを追加しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->からAdarsh Manickamによって送信された[修正

- ![新しいアイコン ](../../assets/new.svg) **コンテナの更新**

   - **Varnish** - サポートされているバージョンのCloud アプリケーションテンプレートを使用してCloud Docker環境にAdobe Commerceをデプロイすると、Varnishがデフォルトのキャッシュになります。 [ ニス コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container)を参照してください。<!--MCLOUD-2634-->

   - Cloud Docker設定ファイルを生成する際にVarnish サービスのインストールをスキップする`--no-varnish` オプションを追加しました。<!--MCLOUD-2634-->

   - ![新しいアイコン ](../../assets/new.svg) **データベース**

      - MySQL データベースのサポートを追加しました。 これで、MariaDBまたはMySQLを使用してCloud Docker環境を設定できます。 [ サービス設定オプション ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options)を参照してください。<!--MCLOUD-5691-->

      - Docker構成ファイルを生成する際に、データベースレプリケーションの増分設定とオフセット設定を行う機能を追加しました。 [ サービスコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers)を参照してください。<!--MCLOUD-5735-->

   - ![新しいアイコン ](../../assets/new.svg) **PHP-FPM**

      - PHP 7.4のサポートを追加しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->からMohanela Muruganによって送信された[修正

      - ルートプロジェクトディレクトリの`php.ini` ファイルをCloud Docker環境にコピーし、PHP-FPMおよびCLI コンテナにカスタム PHP設定を適用する機能を追加しました。 [PHP設定のカスタマイズ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#customize-php-settings)を参照してください。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130)からMathew Beaneによって送信された[修正<!--MCLOUD-6012-->

      - コンテナのヘルスチェックを追加しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188)からVisanth Sampathによって送信された[修正<!--MCLOUD-5752-->

   - ![fix icon](../../assets/fix.svg) **Node.js** - セキュリティを向上させるために、デフォルトのNode.js バージョンをバージョン 8からバージョン 10に更新しました。 Node.js バージョン 8は非推奨（廃止予定）となり、バグ修正やセキュリティパッチによる更新は行われなくなりました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183)から[Mohan Elamuruganによって送信された修正<!--MCLOUD-5586-->

   - ![新しいアイコン ](../../assets/new.svg) **Elasticsearch**

      - Elasticsearch 6.8、7.2、7.5、7.6のサポートを追加しました。<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Docker コンポーズ設定ファイルを生成する際に、[Elasticsearch コンテナ設定](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container)をカスタマイズする機能を追加しました。<!--MCLOUD-3059-->

      - Docker Compose設定ファイルを生成するためのサービス設定オプションに`--no-es` オプションを追加しました。 このオプションを使用すると、Elasticsearch コンテナのインストールをスキップし、代わりにMySQL検索を使用できます。 このオプションは、Adobe Commerce バージョン 2.3.5以前でのみサポートされています。<!--MCLOUD-3766-->

   - ![新しいアイコン ](../../assets/new.svg) **FPM-XDEBUG コンテナ** - Cloud Docker環境でPHPをデバッグするためのXdebugをインストールおよび設定するためのサービス設定オプションを追加しました。 [Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug)の設定を参照してください。<!--MCLOUD-4098-->

- ![新しいアイコン ](../../assets/new.svg) **Docker設定の変更**

   - PHP-FPM、Redis、Elasticsearch、およびMySQL Docker サービスコンテナのヘルスチェックを追加しました。<!--MCLOUD-3335 and MCLOUD-5856-->

   - デベロッパーモードで、デフォルトのファイル同期モードを`native`に変更しました。<!--MCLOUD-3890 -->

   - `docker-compose.yml` ファイルを生成する際に、汎用Docker サービスコンテナイメージにバージョン情報を追加しました。<!--MCLOUD-3878-->

   - Nginx サーバーの`fastcgi_buffers`値を増やすことにより、アップストリーム PHP-FPM コンテナからの大きな応答を処理する機能を改善しました。<!--MCLOUD-5980-->

   - 2回目の同期セッションを追加して、`vendor` ディレクトリ内のファイルを同期することで、mutagen ファイルの同期パフォーマンスを向上しました。 この変更により、ファイル同期プロセス中に突然変異が停止するのを防ぐことができます。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127)からMathew Beaneによって送信された[修正<!--MCLOUD-6010-->

   - ![新しいアイコン ](../../assets/new.svg) **CLI コマンドの更新**

| アクション | コマンド |
| -------- | --------------- |
| Redis キャッシュをクリア | `bin/magento-docker flush-redis` |
| Varnish キャッシュをクリア | `bin/magento-docker flush-varnish` |
| デフォルトのVarnish インストールをスキップ | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Elasticsearch オプションのカスタマイズ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Elasticsearch設定の削除](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| MySQL バージョン 5.6または5.7を使用したDB コンテナの設定 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| カスタムベース URLの指定 | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Xdebug設定のコンテナを追加](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![ アイコンを修正](../../assets/fix.svg) mutagen ファイル同期の設定を修正して、mutagenによる古いセッションの作成を防止しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127)からMathew Beaneによって送信された[修正<!--MCLOUD-6010-->

- ![ アイコンの修正](../../assets/fix.svg) PHP-FPM コンテナの起動時にDocker構成ログで構文エラーが発生する設定の問題を修正しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->からMathew Beaneによって送信された[修正

- ![修正アイコン ](../../assets/fix.svg)複数のDocker環境を使用する際に発生することがあるボリューム競合エラーを修正しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168)からG Arvindによって送信された[修正。

- ![fix icon](../../assets/fix.svg)設定にBlackfire.ioが含まれている場合、`ece-docker build:compose` コマンドが失敗する問題を修正しました。 [Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199)からG Arvindによって送信された修正。<!--MCLOUD-5797-->

- ![fix icon](../../assets/fix.svg) Cloud Docker for Commerceを使用して複数のパッケージをインストールする際に発生するメモリ不足エラーを防ぐために、PHP CLI イメージ設定を更新しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197)から[Mohan Elamuruganによって送信された修正*<!--MCLOUD-5818-->

- ![修正アイコン ](../../assets/fix.svg) Cloud Docker環境で複数のMySQL ユーザーをサポートするようになりました。 以前のリリースでは、`magento.app.yaml` ファイルで複数のデータベースユーザーが指定されている場合、`build:compose`操作は失敗しました。 [Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181)からG Arvindによって送信された修正<!--MCLOUD-5670-->

- ![fix icon](../../assets/fix.svg) デプロイメント中に警告の通知が発生した互換性の問題を解決するために、Cloud Docker for Commerce PHP コンテナから`rsyslog`を削除しました。 Cloud Dockerはrsyslog ユーティリティを使用しません。<!--MCLOUD-6173-->

## v1.0.0

リリース日：2020年2月5日（PT）

- ![新しいアイコン ](../../assets/new.svg) **別のパッケージを作成して`Cloud Docker for Commerce`**&#x200B;を配信しました。ソースコードを移動して、`ece-tools` リポジトリから[新しい`magento-cloud-docker` リポジトリ ](https://github.com/magento/magento-cloud-docker)にCommerce用Cloud Dockerを配信し、コードの品質を維持し、独立したリリースを提供しました。 新しいパッケージは、ECE-Tools v2002.1.0以降の依存関係です。

  ece-toolsを更新すると、`magento/magento-cloud-docker` パッケージもバージョン 1.0.0に更新されます。 以前の`ece-tools` リリース（2002.0.x）でCloud Docker for Commerceを使用した場合は、[後方非互換性](backward-incompatible-changes.md)を確認し、必要に応じてスクリプト、コマンド、プロセスとしてプロジェクトを更新します。

- ![新しいアイコン ](../../assets/new.svg) **Docker イメージにバージョン管理を追加** – 更新されたイメージを取得するには、`magento/magento-cloud-docker` パッケージを更新する必要があります。<!--MAGECLOUD-4737-->

- ![新しいアイコン ](../../assets/new.svg) **コンテナの更新**—

   - ![新しいアイコン ](../../assets/new.svg) **PHP-FPM コンテナ**—

      - ![新しいアイコン ](../../assets/new.svg) **Node.js サポート**&#x200B;を追加 – PHP コンテナ内のノード、npm、grunt-cli機能をサポートするようにPHP-FPM イメージを更新しました。<!--MAGECLOUD-3953-->

      - ![新しいアイコン ](../../assets/new.svg) **[ionCube](https://www.ioncube.com/)**&#x200B;のサポートを追加 – ローカル Docker開発環境でionCubeをサポートするように、デフォルトのDocker設定を更新しました。<!--MAGECLOUD-4354-->

   - ![新しいアイコン ](../../assets/new.svg) **Web コンテナ**—

      - ![new icon](../../assets/new.svg) **Customize NGINX configuration** - カスタム `nginx.conf` ファイルをCloud Docker for Commerce環境にマウントする機能を追加しました。 [Web コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#web-container)を参照してください。<!--MAGECLOUD-4204-->

      - ![新しいアイコン ](../../assets/new.svg) **自動生成されたNGINX証明書** - Docker設定ファイルに、Web コンテナのNGINX証明書を自動生成するための設定が含まれるようになりました。<!--MAGECLOUD-4258-->

   - ![新しいアイコン ](../../assets/new.svg) **新しいSelenium コンテナ** - [Selenium コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#selenium-container)を追加して、Magento Functional Testing Framework （MFTF）を使用したAdobe Commerce アプリケーションテストをサポートしました。<!--MAGECLOUD-4040-->

   - ![新しいアイコン ](../../assets/new.svg) **[!DNL RabbitMQ]バージョン サポート** - [!DNL RabbitMQ] コンテナ設定を更新して、[!DNL RabbitMQ] バージョン 3.8.<!--MAGECLOUD-4674-->をサポートしました

   - ![fix icon](../../assets/fix.svg) **永続データベースコンテナ** - Docker設定を停止および削除した後に`magento-db: /var/lib/mysql` データベースボリュームが保持され、Docker設定を再起動すると復元されるようになりました。 次に、データベースボリュームを手動で削除する必要があります。 [ データベースコンテナ ]を参照してください。<!--MAGECLOUD-3978-->

   - ![新しいアイコン ](../../assets/new.svg) **TLS コンテナ**—

      - ![新しいアイコン ](../../assets/new.svg) **公式の画像**&#x200B;を使用するようにコンテナベースの画像を更新しました – [Cloud TLS container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#tls-container)の画像が、公式の`debian:jessie` Docker イメージに基づいています – <!--MAGECLOUD-4163-->。

      - ![新しいアイコン ](../../assets/new.svg) **[Pound TLS終了プロキシ]**&#x200B;のサポートを追加 – [Pound設定ファイル ](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/)は、次のENV変数を追加して、TLS コンテナのDocker設定をカスタマイズします。

         - **`TimeOut`** - Time to First Byte （TTFB）タイムアウト値を設定します。 デフォルト値は300秒です。

         - **`RewriteLocation`** - Pound プロキシがデフォルトで場所をリクエスト URLに書き換えるかどうかを決定します。 書き換えが外部SSO サイトなどの外部Web サイトへのリダイレクトを壊さないように、デフォルトは`0`です。 [Sorin Sugarによって送信された修正](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![新しいアイコン ](../../assets/new.svg) TLS コンテナ設定のタイムアウト値が15秒から300秒に増加しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->からMathew Beaneによって送信された[修正

   - ![新しいアイコン ](../../assets/new.svg) **ニス コンテナ**—

      - ![新しいアイコン ](../../assets/new.svg) **公式の画像**&#x200B;を使用するようにコンテナベースの画像を更新しました – [Cloud Varnish コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container)は、公式の`centos` Docker イメージに基づいています。<!--MAGECLOUD-4163-->

      - ![新しいアイコン ](../../assets/new.svg) **デフォルトのタイムアウト設定を改善** - Varnish コンテナに`.first_byte_timeout`と`.between_bytes_timeout`の設定を追加しました。 両方のタイムアウト値のデフォルト値は`300s` （5分）です。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->からMathew Beaneによって送信された[修正

      - ![ アイコンを修正](../../assets/fix.svg) **Xdebug セッション中にVarnishをスキップ** - Xdebugが有効になっているときに受信したリクエストに対して`pass`を返すようにVarnish コンテナ設定を更新しました。 以前のリリースでは、Docker環境にVarnishが含まれている場合、Xdebugを使用できませんでした。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111)からMathew Beaneによって送信された[修正<!--MAGECLOUD-4873-->

- ![新しいアイコン ](../../assets/new.svg) **Docker設定の変更**—

   - ![新しいアイコン ](../../assets/new.svg) **プロジェクトのマウントとボリュームを管理** - ローカル開発用のDocker環境を起動する際に、マウントとボリュームを管理する機能を追加しました。 [ プロジェクトデータの共有]を参照してください。<!--MAGECLOUD-3248-->

   - ![新しいアイコン ](../../assets/new.svg) **ネットワークブリッジモード**&#x200B;のサポート – ローカルネットワーク経由でDocker コンテナ間の接続を有効にするためのネットワークブリッジモードのサポートを追加しました。<!--MAGECLOUD-4165-->

   - ![新しいアイコン ](../../assets/new.svg) **デフォルトで無効になっているCron コンテナ** - パフォーマンスを向上させるために、Docker環境を構築する際に、Cron コンテナがデフォルトで設定されなくなりました。 Docker build コマンドの`--with-cron` オプションを使用して、Cron コンテナを環境に追加できます。 [cron ジョブの管理](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#manage-cron-jobs)を参照してください。<!--MAGECLOUD-5181-->

   - ![新しいアイコン ](../../assets/new.svg) **大きなバックアップ ファイル**&#x200B;の同期を停止 – DB ダンプとアーカイブ ファイル（ZIP、SQL、GZ、BZ2）を`dist/docker-sync.yml`および`dist/mutagen.sh` ファイルの除外リストに追加しました。 大きなファイル（>1 GB）を同期すると、非アクティブな期間が発生する可能性があり、バックアップファイルを再生成できるため、通常は同期は必要ありません。<!--MAGECLOUD-3979-->

- ![新しいアイコン ](../../assets/new.svg) **コマンドの変更**—

   - ![修正アイコン ](../../assets/fix.svg) `./bin/docker` ファイルの名前を`./bin/magento-docker`に変更して、`./bin/docker` ファイルが既存のDocker バイナリファイルを上書きするため、一部のDocker環境が破損する問題を修正しました。 これは[下位互換性のない変更](backward-incompatible-changes.md)で、スクリプトとコマンドの更新が必要です。<!-- MAGECLOUD-4038 -->

   - ![新しいアイコン ](../../assets/new.svg) **データベース ポートをホスト**&#x200B;に公開するサービス構成オプションを追加しました。`--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` オプションを使用して、`docker-compose.yml` ファイルを作成する際にデータベース ポートをホストに公開します。`bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![新しいアイコン ](../../assets/new.svg) **新しいデプロイ後コマンド** – 以前は、`cloud-deploy` コマンドを使用してAdobe CommerceをCloud Docker コンテナにデプロイした後、`.magento.app.yaml` ファイルで定義されたデプロイ後フックが自動的に実行されていました。 これで、デプロイ後にデプロイ後のフックを実行するには、別の`cloud-post-deploy` コマンドを発行する必要があります。 [developer](https://developer.adobe.com/commerce/cloud-tools/docker/deploy)および[実稼動](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode) モードの最新の起動手順を参照してください。<!--MAGECLOUD-3996-->

   - ![新しいアイコン ](../../assets/new.svg) ビルドコンテナとデプロイコンテナの`./bin/magento-docker` コマンドに`--rm` オプションを追加しました。 これにより、タスクが完了した後にコンテナが削除されます。<!--MAGECLOUD-4205-->

   - ![新しいアイコン ](../../assets/new.svg) **を`build:compose` コマンドに更新**—

      - ![新しいアイコン ](../../assets/new.svg)開発者モードでDocker Compose設定ファイルを生成する際に、ファイルの同期を無効にする`--sync-engine="native"` オプションを`docker-build` コマンドに追加しました。 このオプションは、ローカルのDocker開発にファイル同期を必要としないLinux システムで開発する場合に使用します。 「[Docker環境でのデータの同期](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data)」を参照してください。<!--MCLOUD-3231, MCLOUD-3890-->

   - ![新しいアイコン ](../../assets/new.svg)既定のファイル同期設定を`docker-sync`から`native`に変更しました。 Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124)からMathew Beaneによって送信された[修正<!--MAGECLOUD-5066-->

- ![新しいアイコン ](../../assets/new.svg) **検証の改善**—

   - ![新しいアイコン ](../../assets/new.svg) クラウド環境設定にデータベースの復号に必要な暗号化キーが含まれていることを確認するために、ローカル Docker開発環境のデプロイメントプロセスに検証を追加しました。 環境設定で暗号化キーの値が指定されていない場合、ログにエラーメッセージが表示されます。<!--MAGECLOUD-4423-->

   - ![新しいアイコン ](../../assets/new.svg) Elasticsearch サービスにコンテナヘルスチェックを追加して、ビルドとデプロイの処理を続行する前にサービスの準備ができていることを確認しました。 ヘルスチェックでエラーが返された場合、コンテナは自動的に再起動します。<!--MAGECLOUD-4456-->
