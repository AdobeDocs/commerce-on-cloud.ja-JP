---
title: ece-tools のリリースノートアーカイブ
description: ece ツールのアーカイブされた機能強化について説明します。
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# ece-tools のリリースノートアーカイブ

>[!NOTE]
>
>これらのリリースノートは、`ece-tools` v2002.0.22 以降に関する情報とアップデートを提供します。 `ece-tools` およびその他のクラウドパッケージの最新のアップデートを入手するには、[ クラウドツールスイートのリリースノート ](cloud-tools-suite.md) を参照してください。

## v2002.0.22

`ece-tools` 2002.0.22 リリースでは、`ece-tools` パッケージの構造が変更され、`Adobe Commerce on cloud infrastructure` パッチのリリースが ECE-Tools リリースから切り離されます。 このリリース以降、パッチおよび重要な修正は、`ece-tools` パッケージの新しい依存関係である [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) パッケージを使用して配布されます。 リリースアップデートのスケジュール設定やコミュニティの貢献度の操作に関する複雑さを軽減するために、これらの変更を行いました。

- ![ 新規アイコン ](../../assets/new.svg)**ECE ツールパッケージの変更点**

   - ![ 新規アイコン ](../../assets/new.svg) Adobe Commerceのパッチを `ece-tools` パッケージから新しい [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) composer パッケージに移動しました。

   - ![ 新規アイコン ](../../assets/new.svg)`ece-tools` パッケージの `composer.json` ファイルを更新して、`magento/magento-cloud-patches` v1.0.0 パッケージの依存関係を追加しました。

   - ![ 修正アイコン ](../../assets/fix.svg) バージョン 2.3.2-p2 以降のセキュリティ専用リリースにパッチセットを適用すると、`ece-tools` のパッチ適用プロセスが中断する問題を修正しました。 この問題は、[ セキュリティのみのパッチ ](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview) に採用された新しいバージョン管理スキームによって導入されました <!--MAGECLOUD-4661-->

- ![ 修正アイコン ](../../assets/fix.svg)**パッチと重要な修正**-`ece-tools` バージョン 2002.0.22 でクラウド環境を更新し、次のパッチと重要な修正を適用します。 これらのパッチは、`magento/magento-cloud-patches` v1.0.0 パッケージに含まれています。

   - ![ 修正アイコン ](../../assets/fix.svg)**Page Builder 2.3.1.x および 2.3.2.x リリースのセキュリティパッチ** – 未認証のユーザーがネットワーク（RCE）上での任意のコード実行をトリガーするために使用でき、グローバルな情報漏洩を引き起こすテンプレートメソッドにアクセスできるようにする Page Builder プレビューの問題を修正しました。 この問題は、Adobe Commerce バージョン 2.3.1 および 2.3.2.<!--MAGECLOUD-4649--> で、サポートされていないバージョンのページビルダーを使用している場合に発生する可能性があります

   - ![ 修正アイコン ](../../assets/fix.svg)**MSI パッチ** – 在庫を管理するためにデフォルトのインベントリ設定を使用した際に、インデックス作成エラーおよびパフォーマンスの問題が発生する問題を修正しました。<!--MAGECLOUD-4428-->

   - ![fix icon](../../assets/fix.svg) **新しいメールインターフェイスの後方互換性**-Adobe Commerce v2.3.3 で導入された `Magento\Framework\Mail\EmailMessageInterface` PHP インターフェイスに起因する後方非互換性の問題を修正しました。このパッチの適用範囲では、新しい `EmailMessageInterface` は古い `MessageInterface` から継承され、Adobe Commerce コアモジュールは `MessageInterface`.<!--MAGECLOUD-4422--> に依存するように戻されます。

   - ![ 修正アイコン ](../../assets/fix.svg)**Elasticsearch 6.x でカタログのページネーションが機能しない**-Elasticsearch 6.x をカタログ検索エンジンとして使用するお客様に影響を与える、検索結果のページネーションに関する重要な問題を修正しました。<!--MAGECLOUD-4448-->

## v2002.0.21

- ![ 新規アイコン ](../../assets/new.svg)**Docker のアップデート**—

   - ![ 新規アイコン ](../../assets/new.svg)**新規 Docker イメージ** - バージョン 2.3.3 以降でサポート <!-- MAGECLOUD-3345 -->

      - PHP バージョン 7.3.<!-- MAGECLOUD-4017 -->

      - ワニス キャッシュ 6.2.0<!-- MAGECLOUD-4017 -->

   - ![ 新規アイコン ](../../assets/new.svg)Docker 環境の `.magento.app.yaml` で指定されたカスタムフック設定を適用するためのサポートを追加しました。 以前は、Docker 環境はデフォルトのフック設定のみをサポートしていました。<!-- MAGECLOUD-3505-->

   - ![ 新規アイコン ](../../assets/new.svg)Docker のビルド中に Docker 環境ファイルが生成されなくなり、`docker:config:convert` コマンドが非推奨（廃止予定）になりました。 対応するデータが `docker-compose.yml` ファイルに保存されます。<!-- MAGECLOUD-3816-->

   - ![new icon](../../assets/new.svg)**PHP イメージの更新**-Node.js を PHP Docker イメージに追加して、node、npm、grunt-cli の機能をサポートしました。<!-- MAGECLOUD-3953 -->

- ![ 新規アイコン ](../../assets/new.svg)**環境変数の更新**-

   - ![ 新規アイコン ](../../assets/new.svg) **LOCK_PROVIDER** デプロイ変数を追加して、重複する cron ジョブや cron グループの起動を防ぐロックプロバイダーを設定しました。 [ 変数のデプロイ ](../environment/variables-deploy.md#lock_provider) トピックの変数の説明を参照してください。<!-- MAGECLOUD-4052 -->

   - ![ 新規アイコン ](../../assets/new.svg) **CONSUMERS_WAIT_FOR_MAX_MESSAGES** 環境変数を追加しました。コンシューマーが `CRON_CONSUMERS_RUNNER` 環境変数を使用して cron ジョブを管理する際に、メッセージキューからのメッセージを処理する方法を設定します。 [ 変数のデプロイ ](../environment/variables-deploy.md#consumers_wait_for_max_messages) トピックの変数の説明を参照してください。<!-- MAGECLOUD-4071 -->

   - ![ 修正アイコン ](../../assets/fix.svg) `consumers_runner` cron ジョブが異なるノードで同じコンシューマーの複数のインスタンスを開始した場合に、データベースのデッドロックエラーが発生する可能性がある問題を修正しました。 現在は、お使いの環境で [**CRON_CONSUMER_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) デプロイ変数を有効にしている場合、`consumers_runner` ジョブは `single-thread` オプションを使用して 1 つのノードのみで各コンシューマーの 1 つのインスタンスを起動します。<!-- MAGECLOUD-3913 -->

   - ![ 修正アイコン ](../../assets/fix.svg) デフォルトのストア URL を使用する [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) 機能に影響する問題を修正しました。 これで、`config:show:default-url` コマンドでベース URL を取得できない場合は、MAGENTO_CLOUD_ROUTES 変数からの URL が使用されます。<!-- MAGECLOUD-3866 -->

- ![ 新規アイコン ](../../assets/new.svg) `module:refresh` コマンドによって返されたログ情報を更新しました。 これで、有効なモジュールの詳細なリストが `cloud.log` ファイルに表示されました。<!-- MAGECLOUD-2514 -->

- ![ 新しいアイコン ](../../assets/new.svg) Adobe Commerceのバージョンとインストールされているサービス（Elasticsearch、[!DNL RabbitMQ]、Redis、DB など）の互換性の問題に対するバージョン互換性の検証と警告通知を改善しました。<!-- MAGECLOUD-3535 -->

- ![ 新しいアイコン ](../../assets/new.svg)RabitMQ バージョン 3.8.<!-- MAGECLOUD-4674--> のサポートを追加

- ![ 新しいアイコン ](../../assets/new.svg) 新しいAdobe Commerce 2.3.3 および 2.2.10 リリースでサポートされているバージョンを反映するように、サービス互換性のインタラクティブな検証を更新しました。 推奨バージョンについては、『 _インストレーション ガイド_ の [ システム要件 ](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) を参照してください。<!-- MAGECLOUD-4018 -->

- ![ 修正アイコン ](../../assets/fix.svg) デプロイフェーズの cron ジョブ管理プロセスが、この問題がエラーではないことを明確にするために、既に完了した cron ジョブを停止しようとすると返されるログメッセージを改善しました。 ログレベルが `INFO` から `DEBUG`.<!-- MAGECLOUD-3653--> に変更されました

- ![ 修正アイコン ](../../assets/fix.svg) `setup:upgrade` コマンドを実行したときに、`app:config:import` タスク中にエラーが発生した場合にデプロイメントプロセスが中断されなかった問題を修正しました。<!-- MAGECLOUD-3806 -->

- ![ 新規アイコン ](../../assets/new.svg) ファイルハンドラーのデフォルトのログレベルを `debug` に変更して、[!DNL Cloud Console] に表示されるログの詳細量を減らすと同時に、デバッグの詳細情報を提供するようになりました。<!-- MAGECLOUD-3871 -->

- ![ 修正アイコン ](../../assets/fix.svg) ビルド中に静的コンテンツのデプロイメントでエラーが発生する問題を修正しました。 インストールおよび設定ダンプ `ece-tools` 後、`config.php` ファイルで管理者ユーザーのロケールが指定されていない場合、エラーが発生しました。 これで、`config.php` ファイルに admin ユーザーのデフォルトのロケールが含まれるようになりました。<!-- MAGECLOUD-3957 -->

- ![ 修正アイコン ](../../assets/fix.svg) セキュア URL （https）が設定されていない環境で `magento-cloud` CLI コマンドが失敗したときに発生する `Undefined index error` ラーを修正しました。 現在、ECE-Tools パッケージは、セキュア URL が使用できない場合にベース URL （http）を使用します <!-- MAGECLOUD-4009 -->。

## v2002.0.20

- ![ 新規アイコン ](../../assets/new.svg)**Docker のアップデート**—

   - ![ 新規アイコン ](../../assets/new.svg)Docker 環境の `ece-tools` パッケージを使用して機能テストを実行できるようになりました。 [ アプリケーションテスト ](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) を参照してください <!-- MAGECLOUD-3129/3684 -->。

   - ![ 新規アイコン ](../../assets/new.svg) `.magento.app.yaml` ファイルを使用した PHP モジュールの設定をサポートするようになりました。 `.magento.app.yaml` ファイルで指定された [PHP Extensions](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions) は、Docker PHP コンテナで使用できます。<!-- MAGECLOUD-3357 -->

   - ![ 新規アイコン ](../../assets/new.svg) Docker コマンドラインエクスペリエンスを向上させるための新しいコマンドが追加されました。 詳しくは、Docker リファレンスの [`bin/magento-docker` の節を参照してください ](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![ 新規アイコン ](../../assets/new.svg)Mutagen.io を使用して、ローカルホストと Docker 間の開発中にファイルを同期する機能が追加されました。<!-- MAGECLOUD-3559 -->

   - ![ 修正アイコン ](../../assets/fix.svg)Docker 環境を使用する際のデフォルトパスを修正しました。 これで、SSH を使用して Docker コンテナにログインすると、期待どおりに `/app` ディレクトリのプロジェクトルートに移動できます。<!-- MAGECLOUD-3582 -->

   - ![ 修正アイコン ](../../assets/fix.svg) Sodium ライブラリをバージョン 1.0.11 からバージョン 1.0.18 に更新し、Sodium PHP 拡張機能を更新しました。<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Adobe Commerce on cloud infrastructure のお客様がAdobe Commerce 2.3.2 にアップグレードする前に [&#128279;](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket)Adobe Commerce サポートチケットを送信  して、プロ実稼動環境とステージング環境の libsona パッケージをアップグレードする必要があります。現在、スターター環境をAdobe Commerce 2.3.2 にアップグレードすることはできません。

   - ![fix icon](../../assets/fix.svg) すべての Docker イメージに `analysis-icu` プラグインと `analysis-phonetic` Elasticsearchプラグインを追加しました。<!-- MAGECLOUD-3446 -->

   - ![ 修正アイコン ](../../assets/fix.svg) 検証機能の向上：`docker:build` コマンドでオプションを使用する場合、オプションを使用する際に値を指定する必要があります。 また、`docker:build run` コマンドを使用する際のノードバージョンの検証も追加しました。<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![ 新規アイコン ](../../assets/new.svg)**環境変数の更新**—

   - ![ 新規アイコン ](../../assets/new.svg) [DATABASE_CONFIGURATION 環境変数 ](../environment/variables-deploy.md#database_configuration) を使用したデータベーステーブル接頭辞のサポートが追加されました。<!-- MAGECLOUD-2901 -->

   - ![ 新規アイコン ](../../assets/new.svg) **FORCE_UPDATE_URLS** デプロイ変数を追加して、Pro およびスターターの実稼動環境とステージング環境にデプロイする際にベース URL を更新しました。 [ 変数をデプロイ ](../environment/variables-deploy.md#force_update_urls) コンテンツの定義を参照してください。<!-- MAGECLOUD-3602 -->

   - ![ 新規アイコン ](../../assets/new.svg) デプロイ後の **TTFB_TESTED_PAGES** 変数を追加して、_最初のバイトまでの時間_ ページテストを設定し、クラウドインフラストラクチャにデプロイされたサイトのアプリケーションパフォーマンスを確認しました。 [ デプロイ後変数 ](../environment/variables-post-deploy.md) の変数の説明を参照してください。<!-- MAGECLOUD-3643 -->

   - ![ 修正アイコン ](../../assets/fix.svg) マルチスレッド SCD で、静的コンテンツのデプロイメントでランダムなエラーが発生する問題を修正しました。 この問題を回避するために、**SCD_THREADS** 変数を `1` に設定しました。 必要に応じて、カウントを増やすことができるようになりました。 [ 変数のデプロイ ](../environment/variables-deploy.md#scd_threads) の定義および [ 変数のビルド ](../environment/variables-build.md#scd_threads) を参照してください。<!-- MAGECLOUD-3611 -->

   - ![ 修正アイコン ](../../assets/fix.svg) **WARM_UP_PAGES** 環境変数を設定して、単一ページ、複数ドメイン、複数ページをキャッシュできます。 [ デプロイ後の変数 ](../environment/variables-post-deploy.md#warm_up_pages) の内容の展開された定義を参照してください。<!-- MAGECLOUD-3258 -->

- ![ 修正アイコン ](../../assets/fix.svg) 除外リストに `pub/static/.htaccess` ファイルを追加しました。 [PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455) の Björn Kraus による修正 <!-- MAGECLOUD-3545/Github#455 -->

- ![ 修正アイコン ](../../assets/fix.svg) 少なくとも 1 つの重要レベルのバリデーターがエラーを返した場合に、すべての検証メッセージが `Critical` として表示されていたエラーを修正しました。<!-- MAGECLOUD-3178 -->

- ![ 修正アイコン ](../../assets/fix.svg) ベース URL がデータベースに存在しない場合に、デプロイメントが失敗する問題を修正しました。<!-- MAGECLOUD-3075 -->

- ![ 新規アイコン ](../../assets/new.svg) 新しい **`env:config:show`コマンド** を、環境サービス、ルート、変数を表示する `ece-tools` パッケージに追加しました。 [ サービス、ルート、変数 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables) を参照してください。 [Vladimir Kerkhoff が提供した機能 ](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![ 修正アイコン ](../../assets/fix.svg) シェルリファクタリング後に開発された `ece-tools` でAdobe Commerce 2.2.6 以前をインストールしようとすると、重大なエラーが発生する問題を修正しました。<!-- MAGECLOUD-3665 -->

- ![ 修正アイコン ](../../assets/fix.svg) Adobe Commerce 2.1.x および 2.2.x のインストールが失敗し、非推奨（廃止予定）の Carbon バージョンの使用に関する警告が表示される問題を修正しました。<!-- MAGECLOUD-3704 -->

- ![ 修正アイコン ](../../assets/fix.svg) シェル出力の `cloud.log` ログレベルを `info` から `debug` に下げました。<!-- MAGECLOUD-3277 -->

- ![ 修正アイコン ](../../assets/fix.svg) ダンプファイルから定義を削除する `--remove-definers (-d)` オプションを `ece-tools db-dump` コマンドに追加しました。<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![ 修正アイコン ](../../assets/fix.svg) デプロイ中に `env.php` ファイルが上書きされ、カスタム設定が失われる問題を修正しました。 この更新により、カスタム設定を維持しながら、クラウドインフラストラクチャ上のAdobe Commerceによってデプロイメントごとに `env.php` ファイルが更新されます。<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![ 新規アイコン ](../../assets/new.svg)**Docker のアップデート**—

   - ![ 新規アイコン ](../../assets/new.svg)Docker 環境で、.magento.app.yaml ファイルの [cron プロパティで定義された cron 設定がサポートされるようになりました ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property).<!-- MAGECLOUD-3150 -->

   - ![ 新しいアイコン ](../../assets/new.svg)**新しい Docker コンテナ** - HTTPS での Varnish SSL 終了を容易にするために [TLS 終了プロキシコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) を追加しました。<!-- MAGECLOUD-2890 -->

   - ![ 新規アイコン ](../../assets/new.svg)**新しい Docker 画像** - Gulp および Jasmine JS Unit Testing などのその他の機能をサポートするための Node.js 画像を追加しました。<!-- MAGECLOUD-3345 -->

   - ![ 新規アイコン ](../../assets/new.svg)**Docker ビルドモード** - Docker 環境を [ 実稼動モードまたは開発者モード ](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode) で起動するように選択できるようになりました。 開発者モードは、完全な書き込み可能なファイルシステム権限を持つアクティブ開発をサポートします。<!-- MAGECLOUD-3152/3511 -->

   - ![ 修正アイコン ](../../assets/fix.svg) 使用できないサービスに対してキャッシュが設定されている場合、Docker デプロイが `Name or service not known` エラーで失敗する問題を修正しました。 [`.magento/services.yaml` ファイルからサービスを削除できるようになりました ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)。 Docker 設定ジェネレーターが、`docker/config.php.dist` ファイルのサービスを自動的に更新します。<!-- MAGECLOUD-3369 -->

   - ![ 新規アイコン ](../../assets/new.svg) サービスの互換性のインタラクティブな検証を追加しました。 これで、要求されたサービスがAdobe Commerceのバージョンまたは他のサービスと互換性がない場合、_インタラクティブモード_ からメッセージが表示され、続行するかどうかを選択するように求められます。 Docker で使用可能な [ サービスバージョン ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers) を参照してください。 CICD の目的でインタラクティビティをスキップするには、`-n` オプションを使用します。<!-- MAGECLOUD-3251 -->

   - ![ 修正アイコン ](../../assets/fix.svg)Docker compose `db-dump` コマンドで既存のダンプが消去される問題を修正しました。<!-- MAGECLOUD-3366 -->

   - ![ 修正アイコン ](../../assets/fix.svg)Redis `session`、`default` および `page_cache` キャッシュストレージを同じデータベース ID に割り当てた問題を修正しました。<!-- MAGECLOUD-3172 -->

- ![ 新規アイコン ](../../assets/new.svg)**環境変数の更新**—

   - ![ 新規アイコン ](../../assets/new.svg) 新しい **ELASTICSUITE\_CONFIGURATION** 環境変数には、カスタマイズされたサービス設定がデプロイメント間で保持されます。 [ 変数をデプロイ ](../environment/variables-deploy.md#elasticsuite_configuration) コンテンツの定義を参照してください。<!-- MAGECLOUD-3205 -->

   - ![ 新規アイコン ](../../assets/new.svg) **SCD_MAX_EXECUTION_TIMEOUT** 環境変数が追加され、`.magento.env.yaml` ファイルからの静的コンテンツのデプロイメントの完了に要する時間を短縮できるようになりました。 [ 変数のデプロイ ](../environment/variables-deploy.md#scd_max_execution_time)、[ 変数のビルド ](../environment/variables-build.md#scd_max_execution_time)、[ グローバル変数 ](../environment/variables-global.md#scd_max_execution_time) の定義を参照してください。<!-- MAGECLOUD-2822 -->

      - ![ 新規アイコン ](../../assets/new.svg) **environment_CLOUD_LOCKS_DIR** MAGENTO変数を追加して、クラウドインフラストラクチャ上のロックプロバイダーのマウントポイントへのパスを設定しました。 ロックプロバイダーは、重複した cron ジョブや cron グループの起動を防ぎます。 この変数はAdobe Commerce バージョン 2.2.5 以降でサポートされ、自動的に設定されます。 [ クラウド変数 ](../environment/variables-cloud.md) の定義を参照してください。<!-- MAGECLOUD-3135 -->

      - ![fix icon](../../assets/fix.svg) **SCD_THREADS** 環境変数のデフォルト値を変更し、検出されたCPU スレッド数に基づいて最適な値を自動的に決定しました。 [ 変数のデプロイ ](../environment/variables-deploy.md#scd_threads) および [ 変数のビルド ](../environment/variables-build.md#scd_threads) の更新された定義を参照してください。<!-- MAGECLOUD-3382 -->

- ![fix icon](../../assets/fix.svg) クラウドインフラストラクチャバージョン 2002.0.16.<!-- MAGECLOUD-3383 --> 上のAdobe Commerceにアップグレードする際に、DB 分離メカニズムのパッチが原因でエラーが発生していた問題を修正しました

- ![fix icon](../../assets/fix.svg) _Googleの画像グラフ_ を _画像グラフ_ に置き換えるパッチを追加しました。 DevBlog の記事 [Google画像グラフの M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006) の廃止および更新を参照してください。<!-- MAGECLOUD-3456 -->

- ![ 修正アイコン ](../../assets/fix.svg) [SEARCH_CONFIGURATION 変数 ](../environment/variables-deploy.md#search_configuration) の検証を追加しました。 「エンジン」オプションが設定されておらず、`_merge` が不要な場合、デプロイは失敗します。<!-- MAGECLOUD-3470 -->

- ![ 修正アイコン ](../../assets/fix.svg) 例外が発生した後に機密データが公開される問題を修正しました。 これで、機密情報が適切にマスクされます。<!-- MAGECLOUD-3525 -->

- ![fix icon](../../assets/fix.svg)Magento Open Sourceパッケージのフォールトトレラント設定を改善しました。 Adobe Commerceが Redis `slave` インスタンスからデータを読み取れない場合、Redis `master` インスタンスから読み取りが行われます。 [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 --> を参照してください。

## v2002.0.17

>[!NOTE]
>
>`ece-tools` バージョン 2002.0.17 には、重要なセキュリティパッチが含まれています。 [ テクニカルリソース：Magento Open Sourceパッチ ](https://magento.com/tech-resources/download#download2288) を参照してください。

- ![ 新しいアイコン ](../../assets/new.svg)**サービスアップデート** - Adobe Commerce 2.2.8 以降、2.2.x、2.3.1 以降、2.3.x でサポートされています

   - Elasticsearchバージョン 6.x.<!-- MAGECLOUD-3196 --> がサポートされるようになりました。

   - Redis バージョン 5.0 のサポートを追加しました。

- ![ 新規アイコン ](../../assets/new.svg)**新規 Docker イメージ** - Docker ビルドに次のサービスを追加しました。

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![new icon](../../assets/new.svg)**New environment variable** – 以前は、SCD 圧縮のタイムアウトがハードコードされていました。 これで、**SCD_COMPRESSION_TIMEOUT** 環境変数を使用して SCD 圧縮タイムアウトを設定できます。 [ ビルド変数 ](../environment/variables-build.md#scd_compression_timeout) および [ デプロイ変数 ](../environment/variables-deploy.md#scd_compression_timeout) の内容の定義を参照してください。<!-- MAGECLOUD-2870 -->

- ![ 修正アイコン ](../../assets/fix.svg) インストールコマンドに `--use-rewrites` オプションが追加され、ストアフロントおよび管理者アクセスで生成されたリンクの web サーバー書き換えを使用して、セキュリティとカスタマーエクスペリエンスを向上させることができるようになりました。<!-- MAGECLOUD-3246 -->

- ![ 修正アイコン ](../../assets/fix.svg) インストールイベントとアップグレードイベントの日付を表示するように、`var/log/install_upgrade.log` ファイルにタイムスタンプを追加しました。<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![ 新規アイコン ](../../assets/new.svg)**Docker のアップデート**—

   - Docker 環境で生成されたデフォルトのサービス設定は、クラウドテンプレートのデフォルト設定と同じになりました。<!-- MAGECLOUD-3025 -->

   - `sendmail` サービスを使用して、Docker 環境からメールを送信できます。<!-- MAGECLOUD-2907 -->

   - Cloud Docker 環境でデバッグするように [Xdebug を設定 ](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) する機能を追加しました。<!-- MAGECLOUD-2891 -->

   - `docker-compose.yml` ファイルを生成する際の web サービスの権限の問題を修正しました。<!-- MAGECLOUD-2883 -->

- ![ 新規アイコン ](../../assets/new.svg)**アップグレードの改善** - Adobe Commerce v2.3 にアップグレードする前に、`composer.json` ファイルの `autoload` プロパティに必要な設定変更が含まれていることを確認する検証が追加されました。[ アップグレードバージョン ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 --> を参照してください。

- ![ 新しいアイコン ](../../assets/new.svg) 静的コンテンツをデプロイする際の圧縮プロセスには、ネイティブで生成またはカスタマイズされたすべてのアセットが含まれるようになり、[`build:transfer` の節の最初のビルドフェーズで行われます ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property)。 以前は、圧縮プロセスは、静的アセットのカスタム圧縮とバンドルを適用する前に行われていました。 [Tryzens Limited の Rafael Garcia Lepper 氏により修正が提出されました ](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![ 修正アイコン ](../../assets/fix.svg) 追加のデータベースとサービスの関係を設定した直後に、デプロイメント中に発生したデータベース接続エラーを修正しました。 また、この修正は、Starter のCommerce レポートの設定プロセス中に発生した問題に対処します。 スターターの場合、このアップグレードは、Commerce レポートを使用する場合に「必須」です。<!-- MAGECLOUD-3035 -->

- ![ 修正アイコン ](../../assets/fix.svg) データベース設定の検証の問題が修正され、デプロイプロセスが失敗しました。<!-- MAGECLOUD-3003 -->

- ![fix icon](../../assets/fix.svg) [PHP 定数 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) で使用する適切なバージョンの `symfony/yaml` パッケージで制約を更新しました。 3.2 より前の `symfony/yaml` パッケージ バージョンを使用している場合、定数の解析は機能しません。[Vladimir Kerkhoff によって送信された修正 ](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![ 新しいアイコン ](../../assets/new.svg)**環境設定チェック**—PHP のバージョンをチェックし、最新の推奨バージョンを使用していない場合にユーザーに警告する検証を追加しました。<!--MAGECLOUD-2903-->

- ![ 修正アイコン ](../../assets/fix.svg) 不正な形式の JSON 変数を処理する問題を修正しました。 これで、JSON 変数が構文エラーを引き起こした場合、警告が `cloud.log` ファイルに表示され、デフォルトの変数を使用してデプロイメントが続行されます。<!-- MAGECLOUD-2851 -->

- ![ 修正アイコン ](../../assets/fix.svg)Redis サービスを無効にした直後に、デプロイメント中に発生した接続エラーを修正しました。<!-- MAGECLOUD-2747 -->

- ![ 新しいアイコン ](../../assets/new.svg)**変更のログ記録** – 次のビルドおよびデプロイプロセスイベントの [ ログレベル ](../environment/log-handlers.md#log-levels) を `Info` から `Notice` に更新しました：<!--MAGECLOUD-2925-->

   - `composer.json` にインストールされているモジュールを `app/etc/config.php` ファイル内の共有構成設定と紐付けするプロセスの開始と終了

   - 設定検証プロセスの開始と終了

   - クラスを生成する `setup:di:compile` プロセスの開始と終了

- ![ 新規アイコン ](../../assets/new.svg)**新しい環境変数**—

   - **[RESOURCE_CONFIGURATION デプロイ変数](../environment/variables-deploy.md#resource_configuration)** – この変数を使用して、リソース名をデータベース接続にマップします。<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION グローバル変数](../environment/variables-global.md#x_frame_configuration)** - `<frame>`、`<iframe>` または `<object>` でAdobe Commerce ページをレンダリングするための `X-Frame-Options` ヘッダー設定を変更するには、この変数を使用します。<!-- MAGECLOUD-3048 -->

- ![ 修正アイコン ](../../assets/fix.svg)**環境変数の更新** – 次の環境変数を変更しました。

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** - Adobe Commerce ストアに対して定義されたすべてのドメインで、指定されたページのキャッシュをプリロードできるようになりました。 以前は、サイトが複数のドメインで設定されている場合、デプロイ後のプロセスが、デフォルト以外のドメイン上の指定されたページのキャッシュをプリロードできず、デプロイ後のログに次のエラーが返されていました。`ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** - SCD 圧縮レベルの正しいデフォルト値を使用して、ドキュメントとサンプル `.magento.env.yaml` ファイルを更新しました。 [ ビルド変数 ](../environment/variables-build.md#scd_compression_level) および [ デプロイ変数 ](../environment/variables-deploy.md#scd_compression_level) の内容の定義を参照してください。<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** – この環境変数は非推奨です。 テーマの設定を制御するには、[SCD_MATRIX](../environment/variables-build.md#scd_matrix) を使用します。<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** - SCD_MATRIX が異なる文字ケースを含むテーマ値を無視した場合に発生する問題を防ぐために、検証プロセスを修正しました。 [ ビルド変数 ](../environment/variables-build.md#scd_matrix) および [ デプロイ変数 ](../environment/variables-deploy.md#scd_matrix) の内容の定義を参照してください。<!-- MAGECLOUD-2904 -->

   - **管理変数**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 環境変数を使用して管理者ユーザーの資格情報を管理する際のセキュリティが向上しました。 ADMIN_EMAIL、ADMIN_USERNAME および ADMIN_PASSWORD 環境変数を、アップグレード中に管理者の資格情報を上書きするために使用できなくなりました。 管理パネルにアクセスできない場合は、_パスワードを忘れた場合_ 機能または `admin:user:create` CLI コマンドを使用して、新しい管理ユーザーを作成します。 詳しくは [ 管理パネルへのアクセス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin) を参照してください。

      - パッチのアップグレードまたは適用時に、ADMIN_EMAIL は不要になりました。

## v2002.0.15

- ![ 新規アイコン ](../../assets/new.svg)**Docker のアップデート**—

   - これで、Docker Generator は、（Docker 環境の構築 [ 時に、`.magento.app.yaml` および `.magento/services.yaml` 設定ファイルで指定されたサービスを使用す ](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) ようになりました。 ビルドパラメーターを使用して、別のサービスバージョンを選択できます。<!-- MAGECLOUD-2888 -->

   - PHP 7.2 イメージが追加されました – Cloud Docker で PHP 7.2 がサポートされるようになりました。[Launch Docker configuration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) を更新し、Adobe Commerceのバージョンと互換性のある PHP のバージョンを指定する `docker:build --php` オプションを含めました。<!-- MAGECLOUD-2799 -->

   - PHP-CLI イメージに基づく [Cron コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli/#cron-container) を追加しました。<!-- MAGECLOUD-2565 -->

   - Docker ビルドに次のサービスを追加しました。

      - [!DNL RabbitMQ] 3.5 および 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7、2.4、5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 および 4.0<!-- MAGECLOUD-2886 -->

- ![ 新しいアイコン ](../../assets/new.svg)**PHP 定数を使用して設定**—[PHP 定数 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) のサポートを `.magento.env.yaml` 設定ファイルに追加しました。<!-- MAGECLOUD- 2575 -->

- ![new icon](../../assets/new.svg)**New environment variable**：デフォルトでは、実稼動環境でのみGoogle Analyticsが有効になっています。 [ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics) 環境変数を使用して、ステージング環境および統合環境でGoogle Analyticsを有効にできます <!--MAGECLOUD-2879-->

- ![ 修正アイコン ](../../assets/fix.svg) 再デプロイメント後に `env.php` ファイルからカスタマイズされた cron 設定が削除される問題を修正しました。 これで、カスタム cron 設定は `env.php` ファイルに安全に残ります。<!-- MAGECLOUD-2923 -->

- ![ 修正アイコン ](../../assets/fix.svg) ビルド、デプロイ、デプロイ後のフェーズにおけるメッセージと [ ログレベル ](../environment/log-handlers.md#log-levels) の不整合を修正しました。 すべてのフェーズとサブフェーズで、開始および終了ログメッセージレベルが **info** から **notice** に向上しました。 必要に応じて、開始および終了ログメッセージを追加しました。<!-- MAGECLOUD-2919 -->

- ![ 修正アイコン ](../../assets/fix.svg) 設定した場合に、デプロイ後のフェーズの開始が妨げられる cron プロセスに関する問題を修正しました。 これで、デプロイ後のフックを有効にした場合、デプロイ後のフェーズの開始時に、cron プロセスが再び有効になります。<!-- MAGECLOUD-2862 -->

- ![fix icon](../../assets/fix.svg) カスタムデータベース設定を指定した場合に、Adobe Commerceを正常にインストールできない問題を修正しました。 以前は、[DATABASE_CONFIGURATIONMAGENTO変数 ](../environment/variables-deploy.md#database_configuration) でカスタマイズされた接続情報を指定した場合でも [&#128279;](../environment/variables-cloud.md) インストールプロセスは ENVIRONMENT_CLOUD_RELATIONSHIP 変数のデータベース設定を使用していました。<!--MAGECLOUD-2736-->

- ![ 修正アイコン ](../../assets/fix.svg)`config:dump` コマンドを修正して、`config.php` ファイルの `system` セクションに各 web サイトのロケールが含まれるようにしました。<!--MAGECLOUD-2740-->

- ![ 修正アイコン ](../../assets/fix.svg) ソースベース URL の参照を修正することで、デプロイ後のフェーズで _ウォームアップ_ エラーが発生する問題を修正しました。<!--MAGECLOUD-2797-->

- ![ 修正アイコン ](../../assets/fix.svg)`setup:di:compile` プロセス中にファイルが正しく生成されず、Amazon Pay モジュールに影響を与えていた問題を修正しました。<!--MAGECLOUD-2850-->

## v2002.0.14

- ![ 新しいアイコン ](../../assets/new.svg)**理想的な状態の検証**:`ideal-state` のウィザードでは、導入時に現在の構成を検証し、迅速でダウンタイムのない導入を実現するために構成を更新する明確な手順を提供するようになりました。<!--MAGECLOUD-2372-->

- ![fix icon](../../assets/fix.svg)**PCI Compliance**：サードパーティのメッセージングサービスに接続する際に、Transport Layer Security （TLS）バージョン 1.2 が必要になるように、クラウドインフラストラクチャ上のAdobe Commerceのメッセージングプロトコルを更新しました。 TLS バージョン 1.2 をサポートしていないメッセージサービスを使用している場合は、サービスをアップグレードする必要があります。 そうでない場合は、Adobe Commerce アプリケーションがメッセージサーバーに接続してメールを送信しようとすると、次のエラーメッセージが表示されます。`Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![ 新しいアイコン ](../../assets/new.svg) **デプロイメントの向上** - ステージング環境または実稼動環境で `dev`、`debug`、`debug_logging` の各オプションが有効になっている場合に、過度のログアクティビティによって発生するパフォーマンスの問題を防ぐために顧客に警告する検証を追加しました。<!--MAGECLOUD-2517-->

- ![ 修正アイコン ](../../assets/fix.svg)**デプロイメントの修正**—

   - これで、メンテナンスモードがデプロイフェーズの開始時に有効になり、最後に無効になります。 配置に失敗した場合、配置の問題が解決されるまで、サイトはメンテナンス モードのままになります。 以前は、デプロイメントに失敗した場合でも、サイトは実稼動モードに戻っていました。<!--MAGECLOUD-2603-->

      - デプロイフェーズの検証チェックを修正し、デプロイメントが完了するように、次のデプロイメントの問題のエラーレベルを `CRITICAL` から `WARNING` にダウングレードしました。 以前は、これらの問題が原因でデプロイメントが失敗していました。

      - 環境設定に、デプロイまたはクラウド変数の誤った値が含まれています。

   - クラウドインフラストラクチャー上のElasticsearchのバージョンは、クラウドインフラストラクチャー上のAdobe Commerceでサポートされている elasticsearch/elasticsearch モジュールのバージョンと互換性がありません。 Adobe Commerce サポートナレッジベースの [Elasticsearchのトラブルシューティングの記事 ](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) を参照してください。<!--MAGECLOUD-2600-->

   - `app/etc/config.php` ファイルの共有設定で、デプロイ中に `recursion detected` エラーが発生する問題を修正しました。<!--MAGECLOUD-2173-->

- ![ 修正アイコン ](../../assets/fix.svg)**Cron 関連の修正**—

   - デフォルト（1 分）以外の cron 頻度を指定した場合にジョブが実行されない cron スケジュールの問題を修正しました。<!--MAGECLOUD-2602-->

   - デプロイフェーズで、デプロイメント中に cron ジョブを引き続き実行できた問題を修正しました。この問題により、データベースのロックやその他の重要な問題が発生する可能性があります。 これで、デプロイフェーズが開始される前にすべての cron ジョブが停止し、デプロイメントの完了後に再開されます。&lt;!—MAGECLOUD—2537—>

   - バージョン 2.2.x の cron ジョブワークフローを修正し、フリーズされた cron ジョブのロックを解除して、デプロイメント開始前に停止できるようにしました。 以前は、フリーズした cron ジョブが原因でデプロイメントが停止していました。<!--MAGECLOUD-2501-->

- ![fix icon](../../assets/fix.svg) Adobe Commerceのコーディング標準に準拠するために、`vendor/bin/ece-tools config:dump` コマンドで生成される `config.php` ファイルの形式を、短い配列構文と 4 文字のインデントを使用するように変更しました。<!--MAGECLOUD-2527-->

- ![ 修正アイコン ](../../assets/fix.svg) `.magento.env.yaml` に、Cloud Infrastructure プロジェクト上のAdobe Commerceのデフォルト URL 設定ではなく、Web 設定の `{{ base_url }}` および `{{ unsecure_base_url }}` プレースホルダーが含まれる場合に発生するデプロイメントエラーを修正しました。/<!--MAGECLOUD-2607-->

## v2002.0.13

- ![ 新しいアイコン ](../../assets/new.svg)**ダウンタイムなしのデプロイメントを実現** – 現在は、クラウドインフラストラクチャー上のAdobe Commerceがデプロイメント中に必要なデータベース変更を含むリクエストをキューに入れ、デプロイメントが完了するとすぐに変更を適用します。 セッションが失われないように、リクエストを最大 5 分間保持できます。 [Cloud でのデプロイメントのダウンタイムを短縮するための静的コンテンツデプロイメントオプション ](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud) を参照してください。<!--MAGECLOUD-2169-->

- ![ 新規アイコン ](../../assets/new.svg)**Docker Compose for Cloud** - [Docker のセットアップと設定 ](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) プロセスが次のように改善されました。

   - PHP 設定ファイルを Docker ENV 形式に変換して環境設定を簡素化するためのコマンド - `docker:config:convert` を追加しました。 次に、PHP 設定ファイルを Docker ディレクトリにコピーして、Docker ENV ファイルに変換します。 [Docker の起動 ](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) を参照してください。<!--MAGECLOUD-2359-->

   - クラウドインフラストラクチャー上のAdobe Commerceのインストールプロセスで、読み取り専用ファイルシステムと読み取り/書き込み可能ファイルシステムの両方へのデプロイがサポートされるようになりました。これにより、クラウドファイルシステムをより詳細にエミュレートできます。 [Docker の設定 ](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) を参照してください。&lt;!—MAGECLOUD—2357—>

   - **Redis サービスサポート** - Redis イメージを追加しました。Docker コンテナにデプロイされ、Docker インストールと連携するように自動的に設定されます。&lt;!—MAGECLOUD—2442—>

   - これで、Cloud Docker [ データベースコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#database-container) を使用する際に DB ダンプ機能が利用できるようになりました。 また、`docker/mnt` ディレクトリを使用して、ホストマシンとコンテナ間で [ ファイルを共有 ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container) できます <!-- MAGECLOUD-2577 -->。

   - **ワニス サービスのサポート** - Docker コンテナに自動的にデプロイされるワニス画像を追加しました。 デプロイメント後、Adobe Commerceのベストプラクティスに従って、Varnish を手動で設定できます。 [ ワニスの設定と使用 ](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish) を参照してください。&lt;!—MAGECLOUD—2358—>

   - 安全なサイトアクセス - Adobe Commerce ストアと管理パネルにアクセスするための SSL サポートが追加されました。&lt;!—MAGECLOUD—2360—>

- ![ 修正アイコン ](../../assets/fix.svg) **クラウドインフラストラクチャー上のAdobe Commerce拡張機能のサポートの改善** - クラウドインフラストラクチャー上のAdobe Commerce[composer.json ファイル ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview) の guzzlehttp/guzzle パッケージの最小バージョン要件をバージョン 6.2 にダウングレードして、`ece-tools` パッケージがより多くの拡張機能と互換性を持つようにしました。<!--MAGECLOUD-2205-->

- ![ 新規アイコン ](../../assets/new.svg)**ビルドフェーズでAdobe Commerce アプリケーションにカスタムの変更を適用します** - ビルドフェーズを 2 つの個別のプロセスに分割して、フックを使用して、アプリケーションをデプロイメント用にパッケージ化する前に、生成された静的コンテンツにカスタムの変更を適用できます。 _build:generate_ プロセスは、コードを生成し、パッチを適用して、静的コンテンツを生成します。 _build:transfer_ プロセスは、生成されたコードと静的コンテンツを最終的な宛先に転送します。 [ アプリケーションフック ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property) を参照してください <!--MAGECLOUD-2363-->。

- ![ 修正アイコン ](../../assets/fix.svg)**環境設定チェック** - クラウドインフラストラクチャにAdobe Commerceをビルドおよびデプロイする前に、バージョンの非互換性と設定エラーに関する警告を表示するように、環境設定の検証を改善しました。

   - サポートされていない、または非推奨（廃止予定）の環境変数と値を特定するために、バージョン固有の検証を追加しました。<!--MAGECLOUD-2183-->

   - Elasticsearch設定の問題に関してユーザーに警告するためのElasticsearch互換性チェックが追加されました。 現在は、サーバー上のElasticsearchサービスのバージョンがAdobe Commerceと互換性がない場合、デプロイは失敗します。 以前は、Elasticsearchバージョンに互換性がない場合でもデプロイメントが成功し、サイトデプロイメント後に製品カタログの問題が発生していました。<!--MAGECLOUD-2389-->

     この非互換性を解決するには、[ サポートチケットを送信 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) して、Elasticsearchを互換性のあるバージョンにアップグレードします。または、Adobe Commerce設定を変更して、Elasticsearch PHP クライアントの互換性のあるバージョンを指定します。

      - Adobe Commerce バージョン 2.1.x から 2.2.2 の場合は、Elasticsearchをバージョン 2.4 にアップグレードします。

      - Adobe Commerce バージョン 2.2.3 以降では、Elasticsearchをバージョン 5.2 にアップグレードします。

      - Elasticsearch 1.x または 2.x があり、アップグレードしない場合は、composer.json 内のAdobe Commerce Elasticsearch PHP クライアントのバージョン要件を `"elasticsearch/elasticsearch": "~2.0"` に更新します。

   - ビルド、デプロイ、デプロイ後のフェーズで競合を引き起こす可能性のある設定を特定するために、環境変数の検証を改善しました。 例えば、静的コンテンツのデプロイメントのグローバル設定がビルドまたはデプロイフェーズの設定と競合する場合、インストールおよびアップグレードプロセス中に警告メッセージが表示されます。<!--MAGECLOUD-2156-->

- ![ 修正アイコン ](../../assets/fix.svg)**環境変数の更新** – 次の環境変数を変更しました。

   - **[SKIP_ENVIRONMENT_MINIFICATION グローバルHTML](../environment/variables-global.md#skip_html_minification)** - デフォルト値を `true` に変更して、オンデマンドのHTMLコンテンツの縮小を有効にしました。これにより、ステージング環境と実稼動環境にデプロイする際のダウンタイムが最小限に抑えられます。 この構成は、ダウンタイムなしの展開に必要です。<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES デプロイ変数](../environment/variables-deploy.md#clean_static_files)** - CLEAN_STATIC_FILES 環境変数の設定に基づいて、ビルド段階で生成された静的コンテンツに対して処理されるクリーンな静的ファイルを管理する機能が追加されました。 以前は、ビルドフェーズで生成された静的コンテンツファイルは、常にクリーンアップされていました。<!--MAGECLOUD-1506-->

- ![ 修正アイコン ](../../assets/fix.svg)**ログ** - ログメッセージを改善し、ログサイズを小さくするために、次の変更を行いました。

   - 環境設定でデバッグレベルのログが指定されていない場合でも、デプロイメントの失敗ログエントリに、エラーの原因となった操作からのコマンド出力が含まれるようになりました。 [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level) を参照してください。<!--MAGECLOUD-2489-->

   - ファイルシステムが読み取り専用状態にあるため、一部の拡張機能で必要な生成ファクトリが正しく生成されない場合に発生するデプロイメントエラーのログを追加しました。<!--MAGECLOUD-2209-->

   - インタラクティブなプログレスバーを使用するセットアップコマンドによって発生する、デプロイログサイズの縮小とフォーマットの問題の修正。<!--MAGECLOUD-2402-->

   - 不要な冗長さを排除し、一部のログステートメントの優先度レベルを更新しました。<!--MAGECLOUD-2227-->

- ![ 修正アイコン ](../../assets/fix.svg)**Cron 固有の修正**—

   - 履歴有効期間のデフォルトの cron ジョブ設定を 3d （4320 分）から 1 時間（60 分）に変更して、cron キューの量が多すぎると発生するパフォーマンスの問題とデプロイメントエラーを防ぎました。<!--MAGECLOUD-2427-->

   - デプロイフェーズにおける cron ジョブ管理プロセスを改善し、データベースのロックやその他の重大な問題を防ぎました。 これで、すべての cron ジョブはデプロイフェーズで停止し、デプロイメントが完了した後で再起動されます。<!--MAGECLOUD-2445-->

   - Adobe Commerce バージョン 2.2.0 以降の cron ジョブで起動されたコンシューマーをスケジュールするためのロックメカニズムで、cron ジョブで重複したコンシューマーが起動されない問題を修正しました。<!--MAGECLOUD-2464-->

- ![ 修正アイコン ](../../assets/fix.svg) [ 静的コンテンツ圧縮プロセス ](../environment/variables-intro.md) （`gzip`）で、デプロイメントプロセス中に圧縮ファイルを参照する際に `not overwritten` エラーと `no such file or directory` エラーが発生する問題を修正しました。<!-- MAGECLOUD-2182-->

- ![ 修正アイコン ](../../assets/fix.svg) ストアのロケールが指定されていない場合に、ダンププロセス中に `php ./vendor/bin/ece-tools config:dump` コマンドが `config.php` ファイルから冗長なセクションを削除できなかった問題を修正しました。 これで、設定ファイルを環境間で簡単に移動できます。 `ece-tools` v2002.0.13 に更新した後は、改善された `config:dump` コマンドを使用して古い `config.php` ファイルを再生成します。 [ ストア設定の設定の管理 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings) を参照してください。<!--MAGECLOUD-2444-->

- ![ 修正アイコン ](../../assets/fix.svg) `.magento/routes.yaml` ファイルのルート設定が [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) ドメインから `www` ドメインにリダイレクトされる場合、デプロイフェーズ中にエラーが発生する問題を修正しました。<!--MAGECLOUD-2556-->

- ![ 修正アイコン ](../../assets/fix.svg) 更新された `.magento.env.yaml` 設定ファイルに `engine` パラメーターを含めない場合に、[`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) 変数の `_merge` オプションで誤った結合結果が発生する問題を修正しました。 マージ操作では、`engine` パラメーターを設定しなくても、更新された `.magento.env.yaml` で指定した値のみが正しく上書きされるようになりました。<!--MAGECLOUD-2520-->

- ![ 修正アイコン ](../../assets/fix.svg) クラウドインフラストラクチャバージョン 2.2.1 以降で、Adobe Commerceのセッションロックが誤って有効になり、パフォーマンスの低下やタイムアウトの原因となる可能性がある Redis 設定の問題を修正しました。 現在は、セッションロックはデフォルトで無効になっています。 この問題は、Redis セッションハンドラーパッケージ v1.3.4 で導入された `disable_locking` パラメーターのデフォルト動作の変更が原因です。 [colinmollenhour/php-redis-session-abstract package](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515--> を参照ください。

## v2002.0.12

- ![ 新規アイコン ](../../assets/new.svg)**Docker Compose for Cloud**—Cloud `ece-tools` リポジトリから [Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) 設定を生成するためのコマンド – `docker:build` – を追加しました。<!-- MAGECLOUD-2250 -->

- ![ 新規アイコン ](../../assets/new.svg)**ロケールを変更** – 設定のエクスポートおよびインポート処理を実行しなくても、ストアのロケールを変更できるようになりました。 アプリケーションが実稼動環境にあり、SCD_ON_DEMAND が有効になっている間は、格納および管理ロケールオプションを使用できます。<!-- MAGECLOUD-2019 -->

- ![ 新規アイコン ](../../assets/new.svg)<!-- MAGECLOU-1998 -->**サイトマップとロボット** - [ ワークフロー ](../store/robots-sitemap.md) を作成して、インフラストラクチャを変更することなく、`robots.txt` ファイルを追加し、単一ドメイン設定用の `sitemap.xml` ファイルを生成します。

- ![ 新しいアイコン ](../../assets/new.svg)**ウィザード** – クラウド設定に役立つ 2 つの [ ウィザード ](../deploy/smart-wizards.md) を追加しました：<!-- MAGECLOUD-1910 -->

   - `ideal-state`：導入のダウンタイムを最小限に抑えるために理想的な状態を構成

   - `master-slave` - データベースと Redis のロード・バランシングを構成します

- ![ 新しいアイコン ](../../assets/new.svg)**モジュールの更新** - ビルド時に自動的に行われる方法と同様に、無効にされたモジュールや明示的に有効にされなかったモジュールを有効にするクラウドコマンド `module:refresh` を追加しました。<!-- MAGECLOUD-1521 -->

- ![ 新規アイコン ](../../assets/new.svg) [CACHE](../environment/variables-deploy.md#cache_configuration)、[SESSION](../environment/variables-deploy.md#session_configuration)、[QUEUE](../environment/variables-deploy.md#queue_configuration)、[SEARCH](../environment/variables-deploy.md#search_configuration) 設定の `_merge` オプションを使用して、サービスの設定を結合または上書きするように選択する機能が追加されました。<!-- MAGECLOUD-2105 -->

- ![ 新規アイコン ](../../assets/new.svg)**環境設定サンプルファイル** – 各環境変数の詳細な説明と使用可能な値を含む `.magento.env.yaml` サンプルファイルを ECE-Tools パッケージに追加しました。<!-- MAGECLOUD-1908 -->

   - また、予期しない値によってデプロイメントプロセスでエラーが発生するのを防ぐ、`.magento.env.yaml` 設定の詳細な検証を追加しました。 エラーが発生すると、`Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 --> で始まる詳細なエラーメッセージが表示されるようになりました。

- ![ 新規アイコン ](../../assets/new.svg) 次の [**環境変数**](../environment/variables-intro.md) を追加しました。

   - 新しい [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) 環境変数を使用して、テーマごとに複数のロケールを定義できるようになりました。これにより、デプロイするテーマファイルの量を減らすことができます。<!-- MAGECLOUD-1501 -->

   - [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) 環境変数を追加して、デプロイメント用のデータベース接続をカスタマイズしました。<!-- MAGECLOUD-2047 -->

   - 新しい [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) 変数は、コードを変更せずに、すべての出力ストリームの最小ログレベルを上書きします。<!-- MAGECLOUD-2129 -->

- ![ 修正アイコン ](../../assets/fix.svg) デプロイフェーズとデプロイ後フェーズの間でダウンタイムが発生する問題を修正しました。 これで、デプロイフェーズが終了した後、デプロイフェーズが _直ちに_ 開始されます。

- ![ 修正アイコン ](../../assets/fix.svg) 成功した cron ジョブ（`status = success` 付きのジョブ）をスケジュールからクリーンアップしない問題を修正しました。<!-- MAGECLOUD-2268 -->

- ![ 修正アイコン ](../../assets/fix.svg) プロジェクトのデプロイ後フェーズではなくデプロイフェーズでキャッシュをクリアする `post_deploy` フックの問題を修正しました。<!-- MAGECLOUD-2113 -->

- ![ 修正アイコン ](../../assets/fix.svg) 複数のロケールで SCD を使用すると、各ロケールに対して同じ `js-translation.json` ファイルが生成される問題を修正しました。<!-- MAGECLOUD-2034 -->

- ![fix アイコン ](../../assets/fix.svg) テーブルのロックを回避し速度を向上させるために、`ece-tools` パッケージの `db:dump` コマンドを最適化しました。<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>ECE-Tools バージョン 2002.0.11 は、2.2.4 の互換性のために必要です。

- ![ 新規アイコン ](../../assets/new.svg)**マスター以外のノードへの読み取り専用接続の設定** – このリリースでは、（[MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)）読み取り専用トラフィックを受信するように、マスター以外のノードへの読み取り専用接続を設定できるようになりました。<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) と <!--MAGECLOUD-143 -->

- ![ 新しいアイコン ](../../assets/new.svg) **設定ウィザード** – 静的なコンテンツ配置の設定を検証するのに役立つウィザードが追加されました。 [ スマート ウィザード ](../deploy/smart-wizards.md) を参照してください。<!--MAGECLOUD-1910 -->

- ![ 新しいアイコン ](../../assets/new.svg)**Symfony Console のサポート**—Adobe Commerce 2.3 で Symfony Console 4 をサポートするようになりました。<!-- MAGECLOUD-1966 -->

- ![ 修正アイコン ](../../assets/fix.svg)**Cron スケジュールの最適化** - Cron 関連の問題のデバッグに役立つキュー管理と拡張ログを改善しました。<!-- MAGECLOUD-1607 -->

- ![ 修正アイコン ](../../assets/fix.svg)`ADMIN_EMAIL` または `ADMIN_USERNAME` の値が既存の管理者アカウントと同じ場合、デプロイメントの検証が失敗します。<!-- MAGECLOUD-1221 -->

- ![ 修正アイコン ](../../assets/fix.svg) バージョン 2.2.x の SOLR サポートを削除しました。 2.1.x バージョンでは、SOLR.<!-- MAGECLOUD-1282 --> を有効にする機能が保持されています。

- ![ 修正アイコン ](../../assets/fix.svg)PRO プロジェクトのステージング環境と実稼動環境の最初のインストールに、環境に対する異なるインデックスプレフィックスが含まれるようになりました。これにより、各Elasticsearchに属するレコードを特定しながら、競合の可能性を防ぎます。<!-- MAGECLOUD-1489 -->

- ![ 修正アイコン ](../../assets/fix.svg) 静的コンテンツのデプロイメント中にレガシーアーキテクチャのビルドフェーズが中断される問題を修正しました。<!-- MAGECLOUD-2021 -->

- ![ 修正アイコン ](../../assets/fix.svg)**Cron 固有の改善点**—Cron の実装を修正しました。<!-- MAGECLOUD-1607 -->

   - Cron キューがすぐにいっぱいになる問題を修正しました。 これで、古い cron ジョブがより信頼性の高い方法でクリアされます。

   - Cron ジョブのシーケンスを再編成して、個別のスレッド内のすべてのジョブが一般グループの前に起動するようにしました。

   - cron に関する問題のデバッグをより適切に支援するために、ログ機能を改善しました。

   - **メモ** – このリリースでは、cron 関連の多くの問題に対処しています。 現在、_m2-hotfix_ で cron 関連のパッチを使用している場合は、それらを削除します。

- ![ 修正アイコン ](../../assets/fix.svg)**SCD 固有の改善点**—

   - `VERBOSE_COMMANDS` と `SCD_COMPRESSION_LEVEL` の環境変数は、_build_ フェーズと de_ploy フェーズの両方で使用できます <!-- MAGECLOUD-1819 -->。

   - `SCD_COMPRESSION_LEVEL` 環境変数に予期しない値が発生した場合、デプロイメントがランダムエラーで失敗する問題を修正しました。 設定の検証を改善して、意味のある通知を提供するようになりました。 使用可能な値については、[`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) を参照してください。<!-- MAGECLOUD-2043 -->

   - `SCD_COMPRESSION_LEVEL` 環境変数の設定フローの動作を修正して、オーバーライドが期待どおりに動作するようにしました。<!-- MAGECLOUD-2044 -->

   - `.magento.env.yaml` ファイル _deploy_ stage.<!-- MAGECLOUD-2046 --> で `SCD_THREADS` 環境変数を設定できない問題を修正しました

## v2002.0.10

- ![ 新しいアイコン ](../../assets/new.svg)**静的コンテンツ展開（SCD）** – 要求に応じて静的コンテンツを（オンデマンドで）生成する、新しい代替デプロイメントプロセスがあります。 これにより、最も重要なアセットを生成できるので、ダウンタイムが短縮され、キャッシュ処理が改善されます。<!-- MAGECLOUD-1285 -->

   - **新しい環境変数** - `SCD_ON_DEMAND` グローバル環境変数が追加され、要求に応じて静的コンテンツが生成されるようになりました。<!-- MAGECLOUD-1738 -->

   - **デプロイ後のフック** - `.magento.app.yaml` ファイルに `post_deploy` フックが追加され、キャッシュがクリアされて、コンテナが接続の受け入れを開始した _後_ キャッシュがプリロード（warms）されるようになりました。 [!DNL Cloud Console] ージにステージング環境と実稼動環境が含まれている Pro プロジェクトと、スタータープロジェクトでのみ使用できます。 必須ではありませんが、`SCD_ON_DEMAND` 環境変数と連携して機能します。<!-- MAGECLOUD-1788 -->

- ![ 新しいアイコン ](../../assets/new.svg) **最適化** – 導入時にファイルの移動またはコピーを最適化し、導入スピードを向上させ、ファイル・システムへの負荷を軽減 <!-- MAGECLOUD-1842 -->

- ![ 新しいアイコン ](../../assets/new.svg)**配置ログ**：配置プロセス中にログを出力する Syslog および Graylog Extended Log Format （GELF）ハンドラを有効にする機能が追加されました。 [ ログハンドラー ](../environment/log-handlers.md) を参照してください <!-- MAGECLOUD-1751 -->。

- ![ 新規アイコン ](../../assets/new.svg) 次の [**環境変数**](../environment/variables-intro.md) を追加しました。

   - `CRYPT_KEY` - データベースを移動する際に、別の環境に暗号化キーを提供します。<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION` - _グローバル_`var/view_preprocessed` ディレクトリ内の静的ビュー・ファイルのコピーをスキップし、要求されると縮小HTMLを生成する環境変数。<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND` - _グローバル_ 環境変数で、要求に応じて静的コンテンツを生成します。<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - キャッシュのプリロードに使用するページをリストできます。 新しい [ デプロイ後の変数 ](../environment/variables-post-deploy.md) で使用できます。

- ![ 修正アイコン ](../../assets/fix.svg) ローカルに適用されたパッチがインスタンス上のデプロイメントを中断する問題を修正しました。 これで、ECE-Tools はパッチが適用されたことを検出できます。<!-- MAGECLOUD-982 -->

- ![ 修正アイコン ](../../assets/fix.svg) JavaScriptのバンドルと GZIP 機能の間の競合を修正しました。 これで、これらの機能は正しく連携して動作するようになりました。<!-- MAGECLOUD-1735 -->

- ![ 修正アイコン ](../../assets/fix.svg) 以前のバージョンの PHP 7.0.x を使用すると ECE-Tools CLI コマンドが失敗する問題を修正しました。<!-- MAGECLOUD-1744 -->

- ![ 修正アイコン ](../../assets/fix.svg) 複数のスレッドでのコンパクトな戦略で静的コンテンツのデプロイができない問題を修正しました。<!-- MAGECLOUD-1822 -->

- ![ 修正アイコン ](../../assets/fix.svg) 管理者ログインの遅延を引き起こす Redis セッションロックの問題を修正しました。 また、この修正は 2.1.x.<!-- MAGECLOUD-1853 --> で利用可能です

## v2002.0.9

>[!NOTE]
>
>これと今後のすべてのアップデートを取得するには、[ クラウドインフラストラクチャメタパッケージ上のAdobe Commerceをアップグレード ](../dev-tools/install-package.md#update-the-metapackage) する必要があります。

- ![ 新しいアイコン ](../../assets/new.svg)**ece-tools**—`ece-tools` パッケージでAdobe Commerce 2.1.x がサポートされるようになりました。<!-- MAGECLOUD-1086 -->

- ![ 新規アイコン ](../../assets/new.svg)**Redis 設定** – 環境変数を使用して、ページとデフォルトのキャッシュおよび Redis セッションストレージを [Redis 設定 ](../environment/variables-deploy.md#cache_configuration) できるようになりました。<!-- MAGECLOUD-1552 -->

- ![ 新しいアイコン ](../../assets/new.svg)**検索、AMQP、Redis のサービス改善** – サービス設定フローを統合し、すべてのサービスで同じように動作するようになりました。 `env.php` ファイルを手動で編集したサービスの設定はサポートされなくなりました。 代わりに、環境変数または `.magento.env.yaml` ファイルを使用する必要があります。<!-- MAGECLOUD-1437 -->

- ![ 修正アイコン ](../../assets/fix.svg)**環境変数**—

   - `env:STATIC_CONTENT_THREADS` の使用は非推奨（廃止予定）となり、将来のリリースで削除される予定です。 代わりに [SCD_THREADS](../environment/variables-deploy.md#scd_threads) を使用してください。<!-- MAGECLOUD-1507 -->

   - `STATIC_CONTENT_EXCLUDE_THEMES` 環境変数は非推奨（廃止予定）となりました。 代わりに、`SCD_EXCLUDE_THEMES` 環境変数を使用する必要があります。<!-- MAGECLOUD-1640 -->

- ![ 修正アイコン ](../../assets/fix.svg)**ログ** – 組み込みのパッチ適用操作に関するログを簡素化しました。<!-- MAGECLOUD-1674 -->

- ![ 修正アイコン ](../../assets/fix.svg) 予期しない動作を引き起こしていた `developer` モードのサポートと `APPLICATION_MODE` 環境変数を削除しました。<!-- MAGECLOUD-1615 -->

- ![ 修正アイコン ](../../assets/fix.svg) Redis に関連する静的コンテンツのデプロイメント失敗を引き起こす問題を修正しました。 現在は、設計どおりにマルチスレッド静的コンテンツのデプロイメントが実行されます。<!-- MAGECLOUD-1630 -->

- ![ 修正アイコン ](../../assets/fix.svg) 管理コマンドを実行した後に機密としてマークされた管理 `app:config:dump` の設定フィールドにユーザーが変更を保存できなかった問題を修正しました。<!-- MAGECLOUD-1175 -->

- ![ 修正アイコン ](../../assets/fix.svg) 最新バージョンとまだ互換性がない一部のパッケージとの競合を修正するために、以前のバージョンの `symfony/yaml` のサポートを追加しました。<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>このリリースでは、`vendor/magento/ece-patches` と `vendor/magento/ece-tools` を統合しました。 `vendor/magento/ece-patches` パッケージを個別に更新する必要はなくなりました。

**新機能：**

- **ロギングの改善**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - ビルドまたはデプロイプロセスで環境変数が上書きされる際の説明を改善するために、ログメッセージを改善しました。
   - インストールとアップグレードの進行状況をリアルタイムで確認できるようになりました。 進行状況を表示するには、`install_update.log` ファイルに対して「テール」を実行します。 以下に例を挙げます。

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **新しい cron コマンド** - [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) コマンドを使用してすべての cron ジョブを停止して再起動するのではなく、特定の停止した cron ジョブのロックを解除できるようになりました。 2.1.<!-- MAGECLOUD-1367 --> では利用できません。

- **統合設定ファイル** - [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml) ファイルを使用してビルドステージとデプロイステージを設定できるようになりました。<!-- MAGECLOUD-1369 -->

- **設定ファイルのバックアップ** – 導入プロセスでは、導入後に `app/etc/env.php` と `app/etc/config.php` の設定ファイルのバックアップが自動的に作成されるようになりました。 また、これらの構成ファイルをバックアップからリストアするための [ 新しい CLI コマンド ](https://support.magento.com/hc/en-us/articles/360033182871) も追加しました <!-- MAGECLOUD-1372 -->。

- **検証エラーのトラブルシューティング** – 静的コンテンツのデプロイメントに十分なデータが含まれていない場合の検証エラーを解決するために使用する必要が `config.php` るコマンドを変更しました。 以前は、エラーメッセージから `bin/magento app:config:dump` を実行するように指示されていました。 ここで、`php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 --> を実行する必要があります。

- **新しい環境変数** – 環境変数を使用して、カスタム [ 検索 ](../environment/variables-deploy.md#search_configuration) および [AMQP ベース ](../environment/variables-deploy.md#queue_configuration) サービスをサイトに接続できるようになりました。<!-- MAGECLOUD-1410 -->

- 私たちはスマートパッチを実装しました。 これで、クラウドインフラストラクチャー上のAdobe Commerceのバージョンではなく、パッチが適用されたパッケージバージョンに基づいて、パッケージでパッチが適用されます。<!--MAGECLOUD-1090-->

**解決された問題：**

- ビルドエラーの原因となっていたログの問題を修正しました。<!-- MAGECLOUD-1162 -->

- インタラクティブモードでデプロイメントを実行するとタイムアウト例外が発生する問題を修正しました。<!-- MAGECLOUD-1389 -->

- 静的コンテンツ生成にコンパクト戦略を使用するとエラーが発生する問題を修正しました。 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485--> では利用できません。

- デプロイメントスクリプトがステージング環境と実稼動環境を適切に識別しない問題を修正しました。<!-- MAGECLOUD-1493 -->

- ネットワークの問題によってデータベース接続が中断されたり、インストールおよびアップグレードプロセス中にエラーが発生したりする問題を修正しました。<!-- MAGECLOUD-1520 -->

- `app:config:dump` を複数回使用して設定ファイルをエクスポートできない問題を修正しました。 2.1.<!--  MAGECLOUD-1567  --> では利用できません。

- _Admin_ ログインの遅延を引き起こす Redis セッション _ロック_ の問題を修正しました。 2.1.<!--  MAGECLOUD-1582  --> では利用できません。

- 他の Composer ベースのパッチ適用モジュールとの競合を引き起こしていたバージョン管理に関連する実装の問題を修正しました。<!-- MAGECLOUD-1450 -->

- インポート時に PHP のメモリに関する問題を修正しました。<!-- MAGECLOUD-1310 -->

- パッチを削除しました。`colinmollenhour/credis` v1.6 のバグを修正して、Cloud Infrastructure 2.2.1 でのAdobe Commerceのサポートを有効にしました。2.1.<!-- MAGECLOUD-1033 --> では利用できません。

## v2002.0.7

**解決された問題：**

- JavaScript`var/view_preprocessed` 縮小の競合を引き起こしている問題を修正するために、シンボリックリンクを削除しました。<!-- MAGECLOUD-1454 -->

## v2002.0.6

**解決された問題：**

- ファイル名またはディレクトリ名にスペースが含まれている場合に `gzip` エラーが発生する問題を修正しました。<!-- MAGECLOUD-1413 -->

- デプロイメントスクリプトでモジュールの依存関係を適切に認識して有効にできなかった問題を修正しました。<!-- MAGECLOUD-1424 -->

## v2002.0.5

**新機能：**

- **環境変数を使用した cron コンシューマーの設定** – 新しい `CRON_CONSUMERS_RUNNER` 環境変数を使用して cron コンシューマーを設定できるようになりました。

- **構成のスキャン**：構築/展開プロセス中に重要なコンポーネントをスキャンし、スキャンに失敗した場合はプロセスを停止します。これにより、サイトがメンテナンス・モードになっていることが原因で発生する不要なダウンタイムを回避できます。

- **ビルド/デプロイ通知** – すべての環境でビルド/デプロイアクションを行うために [Slackやメール通知の設定 ](../environment/set-up-notifications.md) に使用できる設定ファイルを追加しました。

- **静的コンテンツ圧縮** - ビルドおよびデプロイのフェーズで、[gzip](https://www.gnu.org/software/gzip/) を使用して静的コンテンツを圧縮するようになりました。 この圧縮と Fastly 圧縮を組み合わせると、ストアのサイズを縮小し、デプロイメント速度を向上させることができます。 必要に応じて、[ ビルドオプション ](../environment/variables-build.md) または [ デプロイ変数 ](../environment/variables-deploy.md) を使用して圧縮を無効にできます。 詳しくは、次のトピックを参照してください。

   - [アプリケーション環境変数](../application/variables-property.md)

   - [静的コンテンツのデプロイメントパフォーマンス](../deploy/static-content.md)

   - [ デプロイメントプロセス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **設定管理** - ビルドフェーズ中に、Git リポジトリにまだ存在しない `app/etc/config.php` ファイルを自動生成するようになりました。 自動生成されたファイルには、モジュールと拡張子のリストのみが含まれます。 ファイルが既に存在する場合、ビルドフェーズは通常どおり続行されます。 後で [ 設定管理 ](../store/store-settings.md) に従うと、追加の手順を必要とせずにコマンドによってファイルが更新されます。 詳細は、「[ デプロイメントプロセス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)」を参照してください。

- **データベースダンプ** – すべての環境でデータベースダンプを作成するための `magento/ece-tools` CLI コマンドを追加しました。 Pro Plan の実稼動環境の場合、このコマンドは 3 つの高可用性ノードのうちの 1 つからのみダンプするので、ダンプ中に別のノードに書き込まれた実稼動データはコピーされません。 実稼動環境でデータベースダンプを実行する前に、アプリケーションをメンテナンスモードにすることをお勧めします。 詳しくは、[ バックアップ管理 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots) を参照してください。

- **Cron 間隔の制限の撤廃** - us-3、eu-3、ap-3 の各地域でプロビジョニングされたすべての環境のデフォルトの Cron 間隔は 1 分です。 その他のすべての地域のデフォルトの cron 間隔は、Pro 統合環境の場合は 5 分、Pro ステージング環境と実稼動環境の場合は 1 分です。 既存の cron ジョブを変更するには、`.magento.app.yaml` で設定を編集するか、実稼働/ステージング環境用のサポートチケットを作成します。 詳しくは、[cron ジョブの設定 ](../application/crons-property.md#set-up-cron-jobs) を参照してください。

**解決された問題：**

- 静的コンテンツのデプロイメント前にデプロイプロセスが `cache-clean` 操作を呼び出すことで、デプロイ時間が長くなる問題を修正しました。<!-- MAGECLOUD-1327 -->

- 実稼動環境へのデプロイメントの静的コンテンツ生成手順でエラーが発生する問題を修正しました。<!-- MAGECLOUD-1322 -->

- 一部の `magento/ece-tools` コマンドが `stderr`.<!-- MAGECLOUD-1264 --> に出力をログ記録しない問題を修正しました

- `env.php` のベース URL 値がフォークされたブランチで更新されない問題を修正しました。<!-- MAGECLOUD-1242 -->

- `magento setup:install` コマンドでセキュアでないプレフィックス（`http://`）をセキュアなベース URL に追加する問題を修正しました。<!-- MAGECLOUD-1171 -->

- パッチエラーによってデプロイメントが失敗するのを防ぐ問題を修正しました。<!-- MAGECLOUD-1170 -->

- パッチを適用でき `ece-tools` い場合に、実行が停止して例外がスローされる問題を修正しました。<!-- MAGECLOUD-1152 -->

- 管理でHTMLの縮小を有効にした後にストアフロントを読み込むとエラーが発生する問題を修正しました。<!-- MAGECLOUD-1138 -->

## v2002.0.4

**解決された問題：**

- SSH アクセスを介してすべての環境で CLI コマンドを使用して [ スタックした cron ジョブを手動でリセット ](https://support.magento.com/hc/en-us/articles/360033099451) できるようになりました。 デプロイメントプロセスにより、cron ジョブが自動的にリセットされます。<!-- MAGECLOUD-1355 -->

## v2002.0.3

**解決された問題：**

- Redis が読み取り/書き込みに時間がかかりすぎているので、ページがタイムアウトする問題を修正しました。 これで、Redis 設定で `disable_locking` パラメーターを使用して、この問題を回避できます。<!-- MAGECLOUD-1311 -->

## v2002.0.2

**解決された問題：**

- [!DNL RabbitMQ] 設定プロセスは、必要なすべてのパラメーターを自動的に取得するようになりました。<!-- MAGECLOUD-1246 -->

## v2002.0.1

**新機能：**

- クラウドインフラストラクチャー上のAdobe Commerceで scopes と [ 静的コンテンツのデプロイメント戦略 ](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy) がサポートされるようになりました。 静的コンテンツデプロイメント戦略のデフォルト設定 `quick` を使用して、`–s` パラメーターを追加しました。 環境変数 [SCD_STRATEGY](../environment/variables-deploy.md) を使用して、これらの戦略をカスタマイズし、ビルドおよびデプロイアクションで使用できます。 この変数は、`standard`、`quick`、`compact` のオプションをサポートします。 「`compact`」を選択した場合は、`STATIC_CONTENT_THREADS` 値が `1` で上書きされるので、デプロイメントの速度が低下する可能性があります（特に実稼動環境の場合）。 2.1.<!--- MAGECLOUD-1057 --> では利用できません。

- 環境に関するログファイルを作成して、ビルドアクションとデプロイアクションをキャプチャおよびコンパイルしました。 `var/log/cloud.log` ファイルは、ルートアプリケーションディレクトリにあります。<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**解決された問題：**

- `ece-tools` パッケージをリファクタリングして、Cloud Infrastructure 2.2.0 以降のAdobe Commerceと互換性を持たせました。<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- パッチを適用できない場合に、`ece-tools` の実行が停止して例外が発生する問題を修正しました。<!-- MAGECLOUD-1186 -->

- ビルド中に依存関係の挿入（di）コンパイルがスキップされると例外がスローされる問題を修正しました。<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- デプロイプロセスで `env.php` ファイルのカスタム Redis 設定が上書きされる問題を修正しました。<!-- MAGECLOUD-1019 -->

- デフォルトのセキュア管理者で無効になっていることが原因で、リダイレクトループが発生していた問題を修正しました。<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>このパッケージは、クラウドインフラストラクチャー上で動作するAdobe Commerceの他のバージョンとの互換性がなくなり、**使用しないでください**。

### 初回リリース

Cloud infrastructure 2.2.0 でのAdobe Commerce向け `ece-tools` の初回リリースです。
