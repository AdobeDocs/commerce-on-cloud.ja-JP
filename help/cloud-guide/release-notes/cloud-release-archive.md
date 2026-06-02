---
title: ece-toolsのリリースノートのアーカイブ
description: e ツールのアーカイブされた改善点について説明します。
hide: true
hidefromtoc: yes
recommendations: noDisplay, noCatalog
exl-id: 3ba39fa6-88e9-4177-956d-f3e382bf59e3
TQID: https://experienceleague.adobe.com/oO7wTN1rGRxx-34M19dUgivQl9xdmUPtpRycVWQxJ4Y
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: c1579802-ddd4-4214-8a91-97b2066abe11id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 7737
ht-degree: 0%

---

# ece-toolsのリリースノートのアーカイブ

>[!NOTE]
>
>これらのリリースノートには、`ece-tools` v2002.0.22以降の情報とアップデートが記載されています。 `ece-tools`およびその他のCloud パッケージの最新のアップデートについては、「[Cloud Tools Suiteのリリースノート ](cloud-tools-suite.md)」を参照してください。

## v2002.0.22

`ece-tools` 2002.0.22 リリースでは、`ece-tools` パッケージの構造が変更され、`Adobe Commerce on cloud infrastructure` パッチのリリースがECE-Tools リリースから切り離されます。 このリリース以降、パッチと重大な修正は、`ece-tools` パッケージの新しい依存関係である[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) パッケージを使用して配信されます。 これらの変更は、リリース更新のスケジュール設定やコミュニティの貢献に関する作業の複雑さを軽減するために行いました。

- ![新しいアイコン ](../../assets/new.svg) **ECE-Tools パッケージの変更**

   - ![新しいアイコン ](../../assets/new.svg) Adobe Commerce パッチを`ece-tools` パッケージから新しい[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) コンポーザーパッケージに移動しました。

   - ![新しいアイコン ](../../assets/new.svg) `magento/magento-cloud-patches` v1.0.0 パッケージの依存関係を追加するために、`ece-tools` パッケージの`composer.json` ファイルを更新しました。

   - ![ アイコンを修正](../../assets/fix.svg) バージョン 2.3.2-p2以降からセキュリティのみのリリースにパッチセットを適用すると、`ece-tools`のパッチプロセスが壊れる問題を修正しました。 この問題は、[ セキュリティのみのパッチ ](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview)に対して採用された新しいバージョン管理スキームによって導入されました。<!--MAGECLOUD-4661-->

- ![修正アイコン ](../../assets/fix.svg) **パッチと重大な修正** – 次のパッチと重大な修正を適用するには、`ece-tools` バージョン 2002.0.22でクラウド環境を更新します。 これらのパッチは、`magento/magento-cloud-patches` v1.0.0 パッケージに含まれています。

   - ![fix icon](../../assets/fix.svg) **Page Builderの2.3.1.xおよび2.3.2.x リリースに対するセキュリティパッチ** – 未認証のユーザーが一部のテンプレートメソッドにアクセスできるページビルダープレビューの問題を修正しました。このテンプレートメソッドは、ネットワーク（RCE）経由での任意のコード実行をトリガーするために使用でき、グローバルな情報漏洩が発生します。 この問題は、Adobe Commerce バージョン 2.3.1および2.3.2でサポートされていないバージョンのPage Builderを使用している場合に発生する可能性があります。<!--MAGECLOUD-4649-->

   - ![ アイコンを修正](../../assets/fix.svg) **MSI パッチ** – 在庫の管理にデフォルトの在庫設定を使用する際にインデックスエラーやパフォーマンスの問題が発生する問題を修正します。<!--MAGECLOUD-4428-->

   - ![fix icon](../../assets/fix.svg) **新しいメールインターフェイスの後方互換性**-Adobe Commerce v2.3.3で導入された`Magento\Framework\Mail\EmailMessageInterface` PHP インターフェイスによって引き起こされる後方互換性の問題を修正します。 このパッチの適用範囲では、新しい`EmailMessageInterface`は古い`MessageInterface`から継承し、Adobe Commerce コアモジュールは`MessageInterface`に依存するように元に戻されます。<!--MAGECLOUD-4422-->

   - ![ アイコン ](../../assets/fix.svg)を修正&#x200B;**カタログのページネーションがElasticsearch 6.x**&#x200B;で機能しない – Elasticsearch 6.xをカタログ検索エンジンとして使用しているお客様に影響を与える検索結果のページネーションに関する重大な問題を修正しました。<!--MAGECLOUD-4448-->

## v2002.0.21

- ![新しいアイコン ](../../assets/new.svg) **Dockerの更新**—

   - ![新しいアイコン ](../../assets/new.svg) **新しいDocker イメージ** - バージョン 2.3.3以降でサポート <!-- MAGECLOUD-3345 -->

      - PHP バージョン 7.3.<!-- MAGECLOUD-4017 -->

      - Varnish キャッシュ 6.2.0<!-- MAGECLOUD-4017 -->

   - ![新しいアイコン ](../../assets/new.svg) Docker環境の`.magento.app.yaml`で指定されたカスタムフック設定を適用するためのサポートを追加しました。 以前は、Docker環境はデフォルトのフック設定のみをサポートしていました。<!-- MAGECLOUD-3505-->

   - ![新しいアイコン ](../../assets/new.svg) Docker ENV ファイルは、Docker ビルド中に生成されなくなったため、`docker:config:convert` コマンドは非推奨になりました。 対応するデータが`docker-compose.yml` ファイルに保存されるようになりました。<!-- MAGECLOUD-3816-->

   - ![新しいアイコン ](../../assets/new.svg) **PHP イメージ**&#x200B;を更新し、Node.jsをPHP Docker イメージに追加して、node、npm、grunt-cli機能をサポートしました。<!-- MAGECLOUD-3953 -->

- ![新しいアイコン ](../../assets/new.svg) **環境変数の更新**-

   - ![新しいアイコン ](../../assets/new.svg)重複するcron ジョブとcron グループの起動を防ぐロックプロバイダーを設定するために、**LOCK_PROVIDER**&#x200B;のデプロイ変数を追加しました。 変数の説明については、[変数のデプロイ ](../environment/variables-deploy.md#lock_provider) トピックを参照してください。<!-- MAGECLOUD-4052 -->

   - ![新しいアイコン ](../../assets/new.svg) **CONSUMERS_WAIT_FOR_MAX_MESSAGES**&#x200B;環境変数を追加して、`CRON_CONSUMERS_RUNNER`環境変数を使用してcron ジョブを管理する際に、消費者がメッセージキューからメッセージを処理する方法を設定しました。 変数の説明については、[変数のデプロイ ](../environment/variables-deploy.md#consumers_wait_for_max_messages) トピックを参照してください。<!-- MAGECLOUD-4071 -->

   - ![修正アイコン ](../../assets/fix.svg) `consumers_runner` cron ジョブが異なるノードで同じコンシューマーの複数のインスタンスを開始すると、データベースのデッドロックエラーが発生する可能性がある問題を修正しました。 環境で&#x200B;[**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) デプロイ変数を有効にしている場合、`consumers_runner` ジョブは`single-thread` オプションを使用して、1つのノードで各コンシューマーの1つのインスタンスを開始します。<!-- MAGECLOUD-3913 -->

   - ![ アイコンを修正](../../assets/fix.svg) デフォルトのストア URLを使用する&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;機能に影響する問題を修正しました。 これで、`config:show:default-url` コマンドでベース URLを取得できない場合は、MAGENTO_CLOUD_ROUTES変数のURLが使用されます。<!-- MAGECLOUD-3866 -->

- ![新しいアイコン ](../../assets/new.svg) `module:refresh` コマンドによって返されたログ情報を更新しました。 これで、`cloud.log` ファイルに有効なモジュールの詳細なリストが表示されるようになりました。<!-- MAGECLOUD-2514 -->

- ![新しいアイコン ](../../assets/new.svg) Adobe Commerce バージョンと、Elasticsearch、[!DNL RabbitMQ]、Redis、DBなどのインストール済みサービスとの間の互換性の問題に関するバージョンの検証と警告の通知が改善されました。<!-- MAGECLOUD-3535 -->

- ![新しいアイコン ](../../assets/new.svg) RabitMQ バージョン 3.8.<!-- MAGECLOUD-4674-->のサポートを追加しました

- ![新しいアイコン ](../../assets/new.svg)新しいAdobe Commerce 2.3.3および2.2.10 リリースでサポートされているバージョンを反映するように、サービスの互換性に関するインタラクティブな検証を更新しました。 推奨バージョンについては、_インストールガイド_&#x200B;の[必要システム構成](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)を参照してください。<!-- MAGECLOUD-4018 -->

- ![修正アイコン ](../../assets/fix.svg)展開フェーズのcron ジョブ管理プロセスが、既に完了したcron ジョブを停止しようとしたときに返されるログメッセージを改善して、この問題がエラーではないことを確認しました。 ログレベルを`INFO`から`DEBUG`に変更しました。<!-- MAGECLOUD-3653-->

- ![ アイコンを修正](../../assets/fix.svg) `app:config:import` タスク中にエラーが発生したときにデプロイメントプロセスを中断しなかった`setup:upgrade` コマンドを実行する際の問題を修正しました。<!-- MAGECLOUD-3806 -->

- ![新しいアイコン ](../../assets/new.svg) ファイルハンドラーのデフォルトのログレベルを`debug`に変更し、[!DNL Cloud Console]に表示されるログの詳細を減らしながら、デバッグの詳細情報を提供しました。<!-- MAGECLOUD-3871 -->

- ![ アイコンを修正](../../assets/fix.svg) ビルド中に静的コンテンツのデプロイメントでエラーが発生する問題を修正しました。 インストールと`ece-tools`設定ダンプの後、`config.php` ファイルに管理者ユーザーのロケールが指定されていない場合にエラーが発生しました。 これで、`config.php` ファイルに管理者ユーザーのデフォルトのロケールがあります。<!-- MAGECLOUD-3957 -->

- ![修正アイコン ](../../assets/fix.svg)安全なURL （https）で設定されていない環境で`magento-cloud` CLI コマンドが失敗した場合に発生する`Undefined index error`を修正しました。 現在、ECE-Tools パッケージは、セキュア URLが利用できない場合にベース URL （http）を使用します。<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![新しいアイコン ](../../assets/new.svg) **Dockerの更新**—

   - ![新しいアイコン ](../../assets/new.svg) Docker環境で`ece-tools` パッケージを使用して機能テストを実行できるようになりました。 [ アプリケーションテスト ](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing)を参照してください。<!-- MAGECLOUD-3129/3684 -->

   - ![新しいアイコン ](../../assets/new.svg) `.magento.app.yaml` ファイルを使用したPHP モジュールの設定のサポートを追加しました。 `.magento.app.yaml` ファイル ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions)で指定された[PHP拡張機能は、Docker PHP コンテナで使用できるようになります。<!-- MAGECLOUD-3357 -->

   - ![新しいアイコン ](../../assets/new.svg) Docker コマンドラインエクスペリエンスを向上させるために使用できる新しいコマンドがあります。 Docker リファレンス ](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli)の[`bin/magento-docker` セクションを参照してください。<!-- MAGECLOUD-3569 -->

   - ![新しいアイコン ](../../assets/new.svg) Mutagen.ioを使用して、ローカルホストとDocker間の開発中にファイルを同期する機能を追加しました。<!-- MAGECLOUD-3559 -->

   - ![修正アイコン ](../../assets/fix.svg) Docker環境を使用する際のデフォルトのパスを修正しました。 現在、SSHを使用してDocker コンテナにログインする場合、想定どおり、`/app` ディレクトリのプロジェクト ルートにあります。<!-- MAGECLOUD-3582 -->

   - ![修正アイコン ](../../assets/fix.svg) Sodium ライブラリをバージョン 1.0.11からバージョン 1.0.18に更新し、Sodium PHP拡張機能を更新しました。<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Adobe Commerce on cloud infrastructureのお客様は、Adobe Commerce 2.3.2にアップグレードする前に、Pro実稼動環境およびステージング環境でlibsodium パッケージをアップグレードするには、[Adobe Commerce サポートチケット ](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket)を送信する必要があります。 現在、Starter環境をAdobe Commerce 2.3.2にアップグレードすることはできません。

   - ![fix icon](../../assets/fix.svg)すべてのDocker イメージに`analysis-icu`と`analysis-phonetic` Elasticsearch プラグインを追加しました。<!-- MAGECLOUD-3446 -->

   - ![ アイコンを修正](../../assets/fix.svg)検証改善：`docker:build` コマンドのオプションを使用する場合は、オプションを使用する際に値を指定する必要があります。 また、`docker:build run` コマンドを使用する際に、Node バージョンの検証を追加しました。<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![新しいアイコン ](../../assets/new.svg) **環境変数の更新**—

   - ![新しいアイコン ](../../assets/new.svg) [DATABASE_CONFIGURATION環境変数](../environment/variables-deploy.md#database_configuration)を使用したデータベーステーブルのプレフィックスのサポートを追加しました。<!-- MAGECLOUD-2901 -->

   - ![新しいアイコン ](../../assets/new.svg) ProおよびStarterの実稼動環境とステージング環境にデプロイする際に、ベース URLを更新する&#x200B;**FORCE_UPDATE_URLS** デプロイ変数を追加しました。 [変数のデプロイ ](../environment/variables-deploy.md#force_update_urls) コンテンツで定義を参照してください。<!-- MAGECLOUD-3602 -->

   - ![新しいアイコン ](../../assets/new.svg) **TTFB _TESTED_PAGES**&#x200B;のデプロイ後の変数を追加して、_最初のバイトまでの時間_のページテストを設定し、クラウドインフラストラクチャにデプロイされたサイトのアプリケーションパフォーマンスを確認できるようにしました。 変数の説明については、[ デプロイ後の変数](../environment/variables-post-deploy.md)を参照してください。<!-- MAGECLOUD-3643 -->

   - ![ アイコンを修正](../../assets/fix.svg)静的コンテンツのデプロイメントでランダムなエラーが発生する、マルチスレッド SCDの問題を修正しました。 この回避策では、**SCD_THREADS**&#x200B;変数を`1`に設定しました。 必要に応じてカウントを増やすことができます。 [ デプロイ変数](../environment/variables-deploy.md#scd_threads)および[ ビルド変数](../environment/variables-build.md#scd_threads)の定義を参照してください。<!-- MAGECLOUD-3611 -->

   - ![ アイコンを修正](../../assets/fix.svg) **WARM_UP_PAGES**&#x200B;環境変数を設定して、単一ページ、複数ドメイン、複数ページをキャッシュできます。 [ デプロイ後の変数](../environment/variables-post-deploy.md#warm_up_pages) コンテンツで、拡張された定義を参照してください。<!-- MAGECLOUD-3258 -->

- ![修正アイコン ](../../assets/fix.svg)除外リストに`pub/static/.htaccess` ファイルを追加しました。 [Phoenix MEDIA GmbH](https://github.com/magento/ece-tools/pull/455)のBjörn Krausによって送信された修正<!-- MAGECLOUD-3545/Github#455 -->

- ![修正アイコン ](../../assets/fix.svg)少なくとも1人のクリティカルレベルのバリデータがエラーを返した場合、すべての検証メッセージが`Critical`として表示されるエラーを修正しました。<!-- MAGECLOUD-3178 -->

- ![修正アイコン ](../../assets/fix.svg) データベースにベース URLが存在しない場合にデプロイメント失敗が発生する問題を修正しました。<!-- MAGECLOUD-3075 -->

- ![新しいアイコン ](../../assets/new.svg)環境サービス、ルート、または変数を表示する`ece-tools` パッケージに新しい&#x200B;**`env:config:show`コマンド**&#x200B;を追加しました。 [ サービス、ルート、変数](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables)を参照してください。 [Vladimir Kerkhoffによって送信された機能](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![fix icon](../../assets/fix.svg) シェルリファクタリング後に`ece-tools`現像でAdobe Commerce 2.2.6以前をインストールしようとしたときに重大なエラーが発生する問題を修正しました。<!-- MAGECLOUD-3665 -->

- ![ アイコンの修正](../../assets/fix.svg)非推奨バージョンのCarbonの使用に関する警告が表示され、Adobe Commerce 2.1.xおよび2.2.xのインストールが失敗する問題を修正しました。<!-- MAGECLOUD-3704 -->

- ![修正アイコン ](../../assets/fix.svg) シェル出力の`cloud.log` ログレベルを`info`から`debug`に減少しました。<!-- MAGECLOUD-3277 -->

- ![修正アイコン ](../../assets/fix.svg) ダンプ ファイルから定義を削除するために、`ece-tools db-dump` コマンドに`--remove-definers (-d)` オプションを追加しました。<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![ アイコンの修正](../../assets/fix.svg) デプロイ中に`env.php` ファイルが上書きされ、カスタム設定が失われる問題を修正しました。 このアップデートにより、カスタム設定を維持しながら、Adobe Commerce on cloud infrastructureがデプロイメントごとに`env.php` ファイルを更新できるようになります。<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![新しいアイコン ](../../assets/new.svg) **Dockerの更新**—

   - ![新しいアイコン ](../../assets/new.svg)これで、Docker環境は、.magento.app.yaml ファイル ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property)の[crons プロパティで定義されたcron設定をサポートするようになりました。<!-- MAGECLOUD-3150 -->

   - ![新しいアイコン ](../../assets/new.svg) **新しいDocker Container** - [TLS終了プロキシコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container)を追加して、HTTPSでのVarnish SSL終了を容易にしました。<!-- MAGECLOUD-2890 -->

   - ![新しいアイコン ](../../assets/new.svg) **新しいDocker Image** - Gulpやその他の機能（Jasmine JS ユニットテストなど）をサポートするNode.js イメージを追加しました。<!-- MAGECLOUD-3345 -->

   - ![新しいアイコン ](../../assets/new.svg) **Docker ビルドモード** – これで、[実稼動モードまたは開発者モード ](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode)でDocker環境を起動できます。 開発者モードは、書き込み可能な完全なファイルシステム権限を持つアクティブな開発をサポートしています。<!-- MAGECLOUD-3152/3511 -->

   - ![ アイコンを修正](../../assets/fix.svg)使用できないサービスに対してキャッシュが設定されている場合に`Name or service not known` エラーが発生し、Docker デプロイが失敗する問題を修正しました。 これで、[`.magento/services.yaml` ファイル ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)からサービスを削除できます。 Docker設定ジェネレーターは、`docker/config.php.dist` ファイルのサービスを自動的に更新します。<!-- MAGECLOUD-3369 -->

   - ![新しいアイコン ](../../assets/new.svg) サービスの互換性を確認するためのインタラクティブな検証を追加しました。 現在、要求されたサービスがAdobe Commerce バージョンまたはその他のサービスと互換性がない場合、_インタラクティブモード_&#x200B;は、メッセージと続行の選択肢をユーザーに求めます。 Dockerで使用可能な[ サービスバージョン ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers)を参照してください。 `-n` オプションを使用して、CICD目的のインタラクティブ機能をスキップします。<!-- MAGECLOUD-3251 -->

   - ![ アイコンの修正](../../assets/fix.svg)既存のダンプを消去するDocker compose `db-dump` コマンドの問題を修正しました。<!-- MAGECLOUD-3366 -->

   - ![修正アイコン ](../../assets/fix.svg)同じデータベース IDにRedis `session`、`default`および`page_cache` キャッシュ ストレージを割り当てた問題を修正しました。<!-- MAGECLOUD-3172 -->

- ![新しいアイコン ](../../assets/new.svg) **環境変数の更新**—

   - ![新しいアイコン ](../../assets/new.svg)新しい&#x200B;**ELASTICSUITE\_CONFIGURATION**&#x200B;環境変数は、デプロイメント間でカスタマイズされたサービス設定を保持します。 [変数のデプロイ ](../environment/variables-deploy.md#elasticsuite_configuration) コンテンツで定義を参照してください。<!-- MAGECLOUD-3205 -->

   - ![新しいアイコン ](../../assets/new.svg) **SCD_MAX_EXECUTION_TIMEOUT**&#x200B;環境変数を追加しました。これにより、`.magento.env.yaml` ファイルから静的コンテンツのデプロイメントを完了する時間を増やすことができます。 [ デプロイ変数](../environment/variables-deploy.md#scd_max_execution_time)、[ ビルド変数](../environment/variables-build.md#scd_max_execution_time)、[ グローバル変数](../environment/variables-global.md#scd_max_execution_time)の定義を参照してください。<!-- MAGECLOUD-2822 -->

      - ![新しいアイコン ](../../assets/new.svg) **MAGENTO_CLOUD_LOCKS_DIR**&#x200B;環境変数を追加して、クラウドインフラストラクチャ上のロックプロバイダーのマウントポイントへのパスを設定しました。 ロックプロバイダーは、重複したcron ジョブとcron グループの起動を防ぎます。 この変数は、Adobe Commerce バージョン 2.2.5以降でサポートされており、自動的に設定されます。 [ クラウド変数](../environment/variables-cloud.md)の定義を参照してください。<!-- MAGECLOUD-3135 -->

      - ![fix icon](../../assets/fix.svg)検出されたCPU スレッド数に基づいて最適な値を自動的に決定するように、**SCD_THREADS**&#x200B;環境変数のデフォルト値を変更しました。 更新された定義については、[ デプロイ変数](../environment/variables-deploy.md#scd_threads)および[ ビルド変数](../environment/variables-build.md#scd_threads)を参照してください。<!-- MAGECLOUD-3382 -->

- ![fix icon](../../assets/fix.svg) Cloud Infrastructure バージョン 2002.0.16でAdobe Commerceにアップグレードする際にエラーが発生するDB Isolation Mechanismのパッチの問題を修正しました。<!-- MAGECLOUD-3383 -->

- ![修正アイコン ](../../assets/fix.svg) _Google Image Charts_&#x200B;を&#x200B;_Image-Charts_&#x200B;に置き換えるパッチを追加しました。 M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006)のDevBlog記事[Google Image Chartsの非推奨化と更新を参照してください。<!-- MAGECLOUD-3456 -->

- ![修正アイコン ](../../assets/fix.svg) [SEARCH_CONFIGURATION変数](../environment/variables-deploy.md#search_configuration)の検証を追加しました。 「エンジン」オプションが設定されておらず、`_merge`が必要ない場合、デプロイに失敗します。<!-- MAGECLOUD-3470 -->

- ![ アイコンの修正](../../assets/fix.svg)例外が発生した後に機密データが公開される問題を修正しました。 これで、機密情報が適切にマスクされるようになりました。<!-- MAGECLOUD-3525 -->

- ![fix icon](../../assets/fix.svg) Magento Open Source パッケージのフォールトトレラント設定を改善しました。 Adobe CommerceがRedis `slave` インスタンスからデータを読み取れない場合、Redis `master` インスタンスから読み取りが行われます。 [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection)を参照してください。<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>`ece-tools` バージョン 2002.0.17には、重要なセキュリティ パッチが含まれています。 [技術リソース：Magento Open Source パッチ ](https://magento.com/tech-resources/download#download2288)を参照してください。

- ![新しいアイコン ](../../assets/new.svg) **サービスの更新** – 次のAdobe Commerce バージョンでサポートされています：2.2.8以降2.2.x、2.3.1以降2.3.x

   - Elasticsearch バージョン 6.x.<!-- MAGECLOUD-3196 -->のサポートを追加しました

   - Redis バージョン 5.0のサポートを追加しました。

- ![新しいアイコン ](../../assets/new.svg) **新しいDocker イメージ** - Docker ビルドに次のサービスを追加しました。

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![新しいアイコン ](../../assets/new.svg) **新しい環境変数** – 以前は、SCD圧縮のハードコードされたタイムアウトがありました。 これで、**SCD_COMPRESSION_TIMEOUT**&#x200B;環境変数を使用してSCD圧縮タイムアウトを設定できます。 [ ビルド変数](../environment/variables-build.md#scd_compression_timeout)および[ デプロイ変数](../environment/variables-deploy.md#scd_compression_timeout) コンテンツの定義を参照してください。<!-- MAGECLOUD-2870 -->

- ![修正アイコン ](../../assets/fix.svg) インストールコマンドに`--use-rewrites` オプションを追加し、ストアフロントおよび管理者アクセスで生成されたリンクにweb サーバーの書き換えを使用して、セキュリティと顧客体験を向上させました。<!-- MAGECLOUD-3246 -->

- ![ アイコンを修正](../../assets/fix.svg) `var/log/install_upgrade.log` ファイルにタイムスタンプを追加して、インストールおよびアップグレード イベントの日付を表示しました。<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![新しいアイコン ](../../assets/new.svg) **Dockerの更新**—

   - 現在、Docker環境で生成される既定のサービス設定は、クラウド テンプレートの既定の設定と同じです。<!-- MAGECLOUD-3025 -->

   - `sendmail` サービスを使用して、Docker環境からメールを送信できます。<!-- MAGECLOUD-2907 -->

   - Cloud Docker環境でデバッグするために[configure Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug)の機能を追加しました。<!-- MAGECLOUD-2891 -->

   - `docker-compose.yml` ファイルを生成する際のweb サービスの権限に関する問題を修正しました。<!-- MAGECLOUD-2883 -->

- ![新しいアイコン ](../../assets/new.svg) **アップグレードの改善** - Adobe Commerce v2.3にアップグレードする前に、`composer.json` ファイルの`autoload` プロパティに必要な設定変更が含まれていることを確認するための検証を追加しました。 [ アップグレードのバージョン ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version)を参照してください。<!-- MAGECLOUD-2392 -->

- ![新しいアイコン ](../../assets/new.svg)静的コンテンツをデプロイする際の圧縮プロセスに、ネイティブ生成またはカスタマイズされたすべてのアセットが含まれるようになり、[`build:transfer` セクション ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property)の最初のビルド フェーズで実行されます。 以前は、静的アセットのカスタム縮小とバンドルを適用する前に、圧縮プロセスが実行されていました。 [Tryzens Limited](https://github.com/magento/ece-tools/pull/413)からRafael Garcia Lepperによって送信された修正<!-- MAGECLOUD-3104 -->

- ![修正アイコン ](../../assets/fix.svg)追加のデータベースとサービスの関係を設定した直後にデプロイメント中に発生したデータベース接続エラーを修正しました。 また、この修正プログラムは、Commerce Reporting for Starterの設定プロセス中に発生した問題を解決します。 Starterの場合、このアップグレードは、Commerce Reportingを使用する際に「必須」です。<!-- MAGECLOUD-3035 -->

- ![ アイコンを修正](../../assets/fix.svg) デプロイプロセスが失敗する原因となったデータベース設定の検証問題を修正しました。<!-- MAGECLOUD-3003 -->

- ![修正アイコン ](../../assets/fix.svg) [PHP定数](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)で使用する`symfony/yaml` パッケージの適切なバージョンで制約を更新しました。 3.2より前のバージョンの`symfony/yaml` パッケージを使用する場合、定数解析は機能しません。 [Vladimir Kerkhoffによって送信された修正](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![新しいアイコン ](../../assets/new.svg) **環境設定チェック** - PHP バージョンを確認し、最新の推奨バージョンを使用していない場合にユーザーに警告する検証を追加しました。<!--MAGECLOUD-2903-->

- ![修正アイコン ](../../assets/fix.svg)形式が正しくないJSON変数の処理に関する問題を修正しました。 JSON変数が構文エラーを引き起こした場合、`cloud.log` ファイルに警告が表示され、デプロイメントはデフォルト変数を使用して続行されます。<!-- MAGECLOUD-2851 -->

- ![修正アイコン ](../../assets/fix.svg) Redis サービスを無効にした直後にデプロイメント中に発生した接続エラーを修正しました。<!-- MAGECLOUD-2747 -->

- ![新しいアイコン ](../../assets/new.svg) **変更のログ記録** – 次のビルドおよびデプロイ プロセス イベントの[ ログレベル ](../environment/log-handlers.md#log-levels)を`Info`から`Notice`に更新しました：<!--MAGECLOUD-2925-->

   - `composer.json`にインストールされたモジュールを、`app/etc/config.php` ファイルの共有構成設定と照合するプロセスの開始と終了

   - 設定検証プロセスの開始と終了

   - クラスを生成するための`setup:di:compile` プロセスの開始と終了

- ![新しいアイコン ](../../assets/new.svg) **新しい環境変数**—

   - **[RESOURCE_CONFIGURATIONのデプロイ変数](../environment/variables-deploy.md#resource_configuration)**：この変数を使用して、リソース名をデータベース接続にマッピングします。<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION グローバル変数](../environment/variables-global.md#x_frame_configuration)**：この変数を使用して、`<frame>`、`<iframe>`、または`<object>`でAdobe Commerce ページをレンダリングするための`X-Frame-Options` ヘッダー設定を変更します。<!-- MAGECLOUD-3048 -->

- ![ アイコンの修正](../../assets/fix.svg) **環境変数の更新** – 次の環境変数を変更しました。

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** - Adobe Commerce ストア用に定義されたすべてのドメインで、指定されたページのキャッシュをプリロードする機能を追加しました。 以前は、サイトが複数のドメインで構成されていた場合、デプロイ後のプロセスで、デフォルト以外のドメインで指定されたページのキャッシュのプリロードに失敗し、デプロイ後のログに次のエラーが返されました：`ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** - SCD圧縮レベルの正しいデフォルト値を使用して、ドキュメントとサンプル `.magento.env.yaml` ファイルを更新しました。 [ ビルド変数](../environment/variables-build.md#scd_compression_level)および[ デプロイ変数](../environment/variables-deploy.md#scd_compression_level) コンテンツの定義を参照してください。<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** – この環境変数は非推奨です。 [SCD_MATRIX](../environment/variables-build.md#scd_matrix)を使用して、テーマ設定を制御します。<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** - SCD_MATRIXが異なる文字ケースを含むテーマ値を無視した場合に発生する問題を回避するための検証プロセスを修正しました。 [ ビルド変数](../environment/variables-build.md#scd_matrix)および[ デプロイ変数](../environment/variables-deploy.md#scd_matrix) コンテンツの定義を参照してください。<!-- MAGECLOUD-2904 -->

   - **管理者変数**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 環境変数を使用して管理者ユーザーの資格情報を管理する際のセキュリティが向上しました。 アップグレード中にADMIN_EMAIL、ADMIN_USERNAME、およびADMIN_PASSWORD環境変数を使用して管理者資格情報を上書きすることはできなくなりました。 管理パネルにアクセスできない場合は、_パスワードを忘れた_&#x200B;機能または`admin:user:create` CLI コマンドを使用して、新しい管理者ユーザーを作成します。 [管理者パネルへのアクセス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin)を参照してください。

      - パッチをアップグレードまたは適用する際に、ADMIN_EMAILは不要になりました。

## v2002.0.15

- ![新しいアイコン ](../../assets/new.svg) **Dockerの更新**—

   - これで、Docker ジェネレーターは、[Docker環境の構築時](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)に`.magento.app.yaml`および`.magento/services.yaml`設定ファイルで指定されたサービスを使用します。 ビルド パラメーターを使用して、別のサービス バージョンを選択できます。<!-- MAGECLOUD-2888 -->

   - PHP 7.2 imageを追加 – Cloud DockerでのPHP 7.2のサポートを追加しました。[Launch Docker設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)を更新し、お使いのAdobe Commerceのバージョンと互換性のあるPHPのバージョンを指定するための`docker:build --php` オプションを含めました。<!-- MAGECLOUD-2799 -->

   - PHP-CLI イメージに基づいて[Cron コンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli#cron-container)を追加しました。<!-- MAGECLOUD-2565 -->

   - Docker ビルドに次のサービスを追加しました。

      - [!DNL RabbitMQ] 3.5および3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7、2.4、および5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2および4.0<!-- MAGECLOUD-2886 -->

- ![新しいアイコン ](../../assets/new.svg) **PHP定数を使用した設定**-`.magento.env.yaml`設定ファイルに[PHP定数](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)のサポートを追加しました。<!-- MAGECLOUD- 2575 -->

- ![new icon](../../assets/new.svg) **New environment variable** – デフォルトでは、実稼動環境でのみGoogle Analyticsが有効になっています。 [ENABLE_GOOGLE_ANALYTICS環境変数](../environment/variables-deploy.md#enable_google_analytics)を使用して、ステージング環境および統合環境でGoogle Analyticsを有効にできます。<!--MAGECLOUD-2879-->

- ![ アイコンを修正](../../assets/fix.svg)再デプロイ後に`env.php` ファイルからカスタマイズされたcron設定を削除する問題を修正しました。 これで、カスタム cron設定は安全に`env.php` ファイルに残りました。<!-- MAGECLOUD-2923 -->

- ![修正アイコン ](../../assets/fix.svg) ビルド、デプロイ、デプロイ後のフェーズのメッセージと[ ログレベル ](../environment/log-handlers.md#log-levels)の不整合を修正しました。 すべてのフェーズとサブフェーズで、開始と終了のログメッセージレベルが&#x200B;**info**&#x200B;から&#x200B;**notice**&#x200B;に増加しました。 適切な場合に、開始と終了のログメッセージを追加しました。<!-- MAGECLOUD-2919 -->

- ![ アイコンを修正](../../assets/fix.svg)構成時にデプロイ後のフェーズの開始を妨げるcron プロセスに関する問題を修正しました。 これで、デプロイ後のフックが有効になっている場合、デプロイ後のフェーズの開始時にcron プロセスが再度有効になります。<!-- MAGECLOUD-2862 -->

- ![fix icon](../../assets/fix.svg) カスタムデータベース設定を指定する際にAdobe Commerceを正常にインストールできなかった問題を解決しました。 以前は、[DATABASE_CONFIGURATION環境変数](../environment/variables-deploy.md#database_configuration)でカスタマイズされた接続情報を指定した場合でも、[MAGENTO_CLOUD_RELATIONSHIP変数](../environment/variables-cloud.md)のデータベース設定を使用していました。<!--MAGECLOUD-2736-->

- ![修正アイコン ](../../assets/fix.svg) `config.php` ファイルの`system` セクションに各web サイトのロケールが含まれるように、`config:dump` コマンドを修正しました。<!--MAGECLOUD-2740-->

- ![修正アイコン ](../../assets/fix.svg) ソース ベース URL参照を修正することで、デプロイ後のフェーズで&#x200B;_ウォームアップ_ エラーが発生する問題を修正しました。<!--MAGECLOUD-2797-->

- ![fix icon](../../assets/fix.svg) Amazon Pay モジュールに影響を与えた`setup:di:compile` プロセス中にファイルが正しく生成されない問題を修正しました。<!--MAGECLOUD-2850-->

## v2002.0.14

- ![新しいアイコン ](../../assets/new.svg) **理想的な状態の確認** - `ideal-state` ウィザードは、デプロイメントごとに現在の設定を確認し、より高速でダウンタイムのないデプロイメントを実現するために設定を更新するための明確な手順を提供するようになりました。<!--MAGECLOUD-2372-->

- ![fix icon](../../assets/fix.svg) **PCI Compliance**：サードパーティのメッセージングサービスと接続する際にTransport Layer Security （TLS）バージョン 1.2が必要になるように、クラウドインフラストラクチャ上のAdobe Commerceのメッセージングプロトコルを更新しました。 TLS バージョン 1.2をサポートしていないメッセージサービスを使用している場合は、サービスをアップグレードする必要があります。 それ以外の場合は、Adobe Commerce アプリケーションがメッセージサーバーに接続して電子メールを送信しようとすると、次のエラーメッセージが表示されます。`Unable to connect via TLS`。<!--MAGECLOUD-2521-->

- ![新しいアイコン ](../../assets/new.svg) **デプロイメントの改善** - ステージング環境または実稼動環境で`dev`、`debug`、または`debug_logging` オプションが有効になっている場合に顧客に警告するための検証を追加し、過剰なログ活動によるパフォーマンスの問題を防ぎます<!--MAGECLOUD-2517-->。

- ![ アイコンの修正](../../assets/fix.svg) **展開の修正**—

   - これで、デプロイフェーズの開始時にメンテナンスモードが有効になり、終了時に無効になりました。 デプロイメントに失敗した場合、デプロイメントの問題が解決されるまで、サイトはメンテナンスモードのままになります。 以前は、デプロイメントに失敗した場合でも、サイトが実稼動モードに戻っていました。<!--MAGECLOUD-2603-->

      - 展開フェーズの検証チェックを見直し、展開が完了するように、次の展開の問題のエラーレベルを`CRITICAL`から`WARNING`にダウングレードしました。 以前は、これらの問題によりデプロイメントが失敗していました。

      - 環境設定に、デプロイ変数またはクラウド変数の誤った値が含まれています。

   - クラウドインフラストラクチャ上のElasticsearch バージョンは、クラウドインフラストラクチャ上のAdobe Commerceでサポートされているelasticsearch/elasticsearch モジュールのバージョンと互換性がありません。 Adobe Commerce サポート ナレッジベースの[Elasticsearchのトラブルシューティングに関する記事](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento)を参照してください。<!--MAGECLOUD-2600-->

   - デプロイメント中に`recursion detected` エラーが発生した`app/etc/config.php` ファイルの共有構成設定の問題を修正しました。<!--MAGECLOUD-2173-->

- ![ アイコンの修正](../../assets/fix.svg) **Cron関連の修正**—

   - 既定（1分）以外のcron頻度を指定すると、ジョブが実行できないcron スケジュールの問題を修正しました。<!--MAGECLOUD-2602-->

   - デプロイメントフェーズで、デプロイメント中にcron ジョブを実行し続けることができる問題を修正しました。これにより、データベースのロックやその他の重大な問題が発生する可能性があります。 これで、デプロイメントフェーズが開始される前にすべてのcron ジョブが停止し、デプロイメント完了後に再起動されます。&lt;!—MAGECLOUD—2537—>

   - バージョン 2.2.xのcron ジョブワークフローを修正して、固定cron ジョブのロックを解除し、デプロイメントを開始する前に停止できるようにしました。 以前は、固定cron ジョブによってデプロイメントが停止していました。<!--MAGECLOUD-2501-->

- ![fix icon](../../assets/fix.svg) `vendor/bin/ece-tools config:dump` コマンドによって生成された`config.php` ファイルのフォーマットを変更して、Adobe Commerce コーディング標準に準拠するために、短縮配列構文と4 スペースのインデントを使用するようにしました。<!--MAGECLOUD-2527-->

- ![fix icon](../../assets/fix.svg) `.magento.env.yaml`に、Adobe Commerce on cloud infrastructure プロジェクトのデフォルト URL設定ではなく、web設定の`{{ base_url }}`と`{{ unsecure_base_url }}`のプレースホルダーが含まれている場合に発生するデプロイメントエラーを修正しました。/<!--MAGECLOUD-2607-->

## v2002.0.13

- ![新しいアイコン ](../../assets/new.svg) **ダウンタイムなしのデプロイメントを有効にする** – デプロイメント中に必要なデータベースの変更を含むAdobe Commerce オンクラウドインフラストラクチャのリクエストをキューに入れ、デプロイメントが完了するとすぐに変更を適用します。 リクエストは、セッションが失われないように、最大5分間保持できます。 クラウド ](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud)でのデプロイメントのダウンタイムを短縮するには、[静的コンテンツのデプロイメントオプションを参照してください。<!--MAGECLOUD-2169-->

- ![新しいアイコン ](../../assets/new.svg) **Docker Compose for Cloud**-[Dockerの設定と設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) プロセスに次の改善を加えました。

   - 環境設定を簡素化するために、PHP設定ファイルをDocker ENV形式に変換するコマンド `docker:config:convert`を追加しました。 次に、PHP設定ファイルをDocker ディレクトリにコピーし、Docker ENV ファイルに変換します。 [Dockerの起動](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)を参照してください。<!--MAGECLOUD-2359-->

   - Adobe Commerce オンクラウドインフラストラクチャのインストールプロセスで、読み取り専用および読み取り/書き込み両方のファイルシステムへのデプロイがサポートされ、クラウドファイルシステムをより詳細にエミュレートできるようになりました。 [Configure Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)を参照してください。&lt;!—MAGECLOUD—2357—>

   - **Redis サービスのサポート**—Docker コンテナにデプロイされ、Docker インストールと連携するように自動的に設定されるRedis イメージを追加しました。&lt;!—MAGECLOUD—2442—>

   - これで、Cloud Docker [ データベースコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#database-container)を使用する際のDB ダンプ機能を使用できるようになりました。 また、`docker/mnt` ディレクトリを使用して、ホストマシンとコンテナの間でファイル ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container)を[共有できます。<!-- MAGECLOUD-2577 -->

   - **Varnish サービスのサポート**— Varnish イメージを追加し、Docker コンテナに自動的にデプロイされました。 デプロイメント後、Adobe Commerceのベストプラクティスに従って、Varnishを手動で設定できます。 [Varnish](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish)の設定と使用を参照してください。&lt;!—MAGECLOUD—2358—>

   - 安全なサイトアクセス - Adobe Commerce ストアと管理パネルにアクセスするためのSSL サポートを追加しました。&lt;!—MAGECLOUD—2360—>

- ![fix icon](../../assets/fix.svg) **クラウドインフラストラクチャ拡張機能のサポート**&#x200B;の改善 – Adobe Commerce クラウドインフラストラクチャ上のAdobe Commerceのguzzlehttp/guzzle パッケージの最小バージョン要件[composer.json ファイル ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview)をバージョン 6.2にダウングレードし、`ece-tools` パッケージがより多くの拡張機能と互換性を持つようにしました。<!--MAGECLOUD-2205-->

- ![新しいアイコン ](../../assets/new.svg) **ビルド フェーズ中にAdobe Commerce アプリケーションにカスタム変更を適用** – ビルド フェーズを2つの個別のプロセスに分割し、フックを使用して、生成された静的コンテンツにカスタム変更を適用してから、アプリケーションをデプロイメント用にパッケージ化できるようにしました。 _build :generate_プロセスは、コードの生成、パッチの適用、静的コンテンツの生成を行います。 _build:transfer_ プロセスは、生成されたコードと静的コンテンツを最終宛先に転送します。 [ アプリケーションフック ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property)を参照してください。<!--MAGECLOUD-2363-->

- ![fix icon](../../assets/fix.svg) **環境設定チェック** - Adobe Commerceをクラウドインフラストラクチャに構築してデプロイする前に、バージョンの非互換性と設定エラーについて顧客に警告するための環境設定の検証を改善しました。

   - サポートされていない、または非推奨の環境変数と値を識別するためのバージョン固有の検証を追加しました。<!--MAGECLOUD-2183-->

   - Elasticsearchの互換性チェックを追加し、Elasticsearch設定の問題についてユーザーに警告しました。 サーバー上のElasticsearch サービスのバージョンがAdobe Commerceと互換性がない場合、デプロイメントは失敗します。 以前は、Elasticsearchのバージョンが互換性がない場合でも、デプロイメントが成功し、サイトのデプロイメント後に製品カタログの問題が発生していました。<!--MAGECLOUD-2389-->

     非互換性を解決するには、[ サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)を送信してElasticsearchを互換性のあるバージョンにアップグレードするか、Adobe Commerce設定を変更してElasticsearch PHP クライアントの互換性のあるバージョンを指定します。

      - Adobe Commerce バージョン 2.1.xから2.2.2の場合は、Elasticsearchをバージョン 2.4にアップグレードします。

      - Adobe Commerce バージョン 2.2.3以降の場合は、Elasticsearchをバージョン 5.2にアップグレードします。

      - Elasticsearch 1.xまたは2.xを使用しており、アップグレードしない場合は、composer.jsonのAdobe Commerce Elasticsearch PHP クライアントのバージョン要件を`"elasticsearch/elasticsearch": "~2.0"`に更新してください。

   - 環境変数の検証を改善して、ビルド、デプロイ、デプロイ後のフェーズで競合を引き起こす可能性のある設定設定を特定しました。 例えば、静的コンテンツ展開のグローバル設定がビルドまたはデプロイ フェーズの設定と競合する場合、インストールおよびアップグレード プロセス中に警告メッセージが表示されます。<!--MAGECLOUD-2156-->

- ![ アイコンの修正](../../assets/fix.svg) **環境変数の更新** – 次の環境変数を変更しました。

   - **[SKIP_HTML_MINIFICATION グローバル変数](../environment/variables-global.md#skip_html_minification)** - オンデマンドのHTML コンテンツの縮小を有効にするために、デフォルト値を`true`に変更しました。これにより、ステージング環境と実稼動環境へのデプロイ時のダウンタイムを最小限に抑えることができます。 この設定は、ダウンタイムなしのデプロイメントに必要です。<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES デプロイ変数](../environment/variables-deploy.md#clean_static_files)** - CLEAN_STATIC_FILES環境変数設定に基づいて、ビルド段階で生成された静的コンテンツのクリーンな静的ファイル処理を管理する機能を追加しました。 以前は、ビルド フェーズで生成された静的コンテンツ ファイルは、常にクリーニングされていました。<!--MAGECLOUD-1506-->

- ![ アイコンを修正](../../assets/fix.svg) **ログ** - ログメッセージを改善し、ログサイズを削減するために、次の変更を行いました。

   - デプロイメント失敗ログエントリには、環境設定でデバッグレベルのログを指定しない場合でも、失敗の原因となる操作からのコマンド出力が含まれるようになりました。 [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level)を参照してください。<!--MAGECLOUD-2489-->

   - ファイルシステムが読み取り専用の状態であるため、一部の拡張機能で必要な生成工場が正しく生成できないときに発生するデプロイメントエラーのログを追加しました。<!--MAGECLOUD-2209-->

   - インタラクティブな進行状況バーを使用するセットアップ コマンドによって引き起こされるデプロイ ログのサイズと修正された書式設定の問題を削減しました。<!--MAGECLOUD-2402-->

   - 不要な冗長性を排除し、一部のログステートメントの優先度レベルを更新しました。<!--MAGECLOUD-2227-->

- ![ アイコンの修正](../../assets/fix.svg) **Cron固有の修正**—

   - cron キューの入力速度が速すぎる場合に発生する可能性のあるパフォーマンスの問題とデプロイメントのエラーを防ぐために、履歴の有効期間の既定のcron ジョブ設定設定を3d （4320分）から1h （60分）に変更しました。<!--MAGECLOUD-2427-->

   - データベースのロックやその他の重要な問題を防ぐために、デプロイフェーズ中のcron ジョブ管理プロセスを改善しました。 これで、デプロイメントフェーズ中にすべてのcron ジョブが停止し、デプロイメント完了後に再起動します。<!--MAGECLOUD-2445-->

   - Adobe Commerce バージョン 2.2.0以降でcron ジョブによって起動されたコンシューマーのスケジュール設定メカニズムで、cron ジョブが重複するコンシューマーを起動しない問題を修正しました。<!--MAGECLOUD-2464-->

- ![修正アイコン ](../../assets/fix.svg)展開プロセス中に圧縮ファイルを参照する際に`not overwritten`と`no such file or directory`のエラーが発生する[静的コンテンツ圧縮プロセス ](../environment/variables-intro.md) （`gzip`）の問題を修正しました。<!-- MAGECLOUD-2182-->

- ![ アイコンを修正](../../assets/fix.svg) ストアロケールが指定されていない場合、`php ./vendor/bin/ece-tools config:dump` コマンドがダンププロセス中に`config.php` ファイルから冗長セクションを削除できない問題を修正しました。 環境間で設定ファイルを簡単に移動できるようになりました。 `ece-tools` v2002.0.13に更新した後、改善された`config:dump` コマンドを使用して古い`config.php` ファイルを再生成します。 ストア設定の[構成管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings)を参照してください。<!--MAGECLOUD-2444-->

- ![修正アイコン ](../../assets/fix.svg) `.magento/routes.yaml` ファイルのルート設定が[apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) ドメインから`www` ドメインにリダイレクトされる場合、デプロイフェーズ中にエラーが発生する問題を修正しました。<!--MAGECLOUD-2556-->

- ![修正アイコン ](../../assets/fix.svg)更新された`.magento.env.yaml`設定ファイルに`engine` パラメーターを含めない場合に誤った結合結果が発生する[`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration)変数の`_merge` オプションの問題を修正しました。 これで、結合操作では、更新された`.magento.env.yaml`で指定した値のみが正しく上書きされ、`engine` パラメーターを設定する必要がなくなりました。<!--MAGECLOUD-2520-->

- ![ アイコンを修正](../../assets/fix.svg) クラウドインフラストラクチャのバージョン 2.2.1以降で、Adobe Commerceのセッションロックが誤って有効になり、パフォーマンスとタイムアウトが遅くなる可能性があるRedis設定の問題を修正しました。 セッションロックはデフォルトで無効になっています。 この問題は、Redis セッション ハンドラーパッケージのv1.3.4で導入された`disable_locking` パラメーターのデフォルト動作の変更が原因で発生しました。 [colinmollenhour/php-redis-session-abstract パッケージ ](https://github.com/colinmollenhour/php-redis-session-abstract)を参照してください。<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![新しいアイコン ](../../assets/new.svg) **Docker Compose for Cloud** – クラウド `ece-tools` リポジトリから[Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)設定を生成するコマンド `docker:build`を追加しました。<!-- MAGECLOUD-2250 -->

- ![新しいアイコン ](../../assets/new.svg) **ロケールの変更** – これで、書き出しと読み込みの設定プロセスを実行せずに、ストアのロケールを変更できます。 アプリケーションが実稼動環境にあり、SCD_ON_DEMANDが有効になっている間は、ストアと管理者のロケールオプションを使用できます。<!-- MAGECLOUD-2019 -->

- ![新しいアイコン ](../../assets/new.svg) <!-- MAGECLOU-1998 -->**サイトマップとロボット** - [ ワークフロー](../store/robots-sitemap.md)を作成して`robots.txt` ファイルを追加し、インフラストラクチャに変更を加えることなく、単一ドメイン構成用の`sitemap.xml` ファイルを生成しました。

- ![新しいアイコン ](../../assets/new.svg) **ウィザード** - クラウド設定に役立つ2つの[ ウィザード ](../deploy/smart-wizards.md)を追加しました：<!-- MAGECLOUD-1910 -->

   - `ideal-state`：デプロイメントのダウンタイムを最小限に抑えるための理想的な状態を設定します

   - `master-slave` - データベースとRedisの負荷分散の設定

- ![新しいアイコン ](../../assets/new.svg) **モジュールの更新** - クラウドコマンドを追加しました – `module:refresh` – 無効にされているか、明示的に有効になっていないモジュールを有効にします。これは、ビルド中に自動的に行われるのと同様です。<!-- MAGECLOUD-1521 -->

- ![新しいアイコン ](../../assets/new.svg) [CACHE](../environment/variables-deploy.md#cache_configuration)、[SESSION](../environment/variables-deploy.md#session_configuration)、[QUEUE](../environment/variables-deploy.md#queue_configuration)、[SEARCH](../environment/variables-deploy.md#search_configuration)の設定で`_merge` オプションを使用して、サービスの構成を結合または上書きする機能を追加しました。<!-- MAGECLOUD-2105 -->

- ![新しいアイコン ](../../assets/new.svg) **環境設定サンプルファイル** – 各環境変数の詳細な説明と可能な値を含む`.magento.env.yaml` サンプルファイルをECE-Tools パッケージに追加しました。<!-- MAGECLOUD-1908 -->

   - また、予期しない値によって引き起こされるデプロイメントプロセスのエラーを防ぐ`.magento.env.yaml`設定の詳細な検証も追加しました。 エラーが発生すると、次で始まる詳細なエラーメッセージが表示されるようになりました：`Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![新しいアイコン ](../../assets/new.svg)次の&#x200B;[**環境変数**](../environment/variables-intro.md)&#x200B;を追加しました：

   - 新しい[SCD_MATRIX](../environment/variables-deploy.md#scd_matrix)環境変数を使用して、各テーマに複数のロケールを定義できるようになりました。これにより、デプロイするテーマファイルの量を減らすことができます。<!-- MAGECLOUD-1501 -->

   - デプロイメント用にデータベース接続をカスタマイズするために、[DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration)環境変数を追加しました。<!-- MAGECLOUD-2047 -->

   - 新しい[MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level)変数は、コードを変更することなく、すべての出力ストリームの最小ログレベルを上書きします。<!-- MAGECLOUD-2129 -->

- ![ アイコンを修正](../../assets/fix.svg) デプロイとデプロイ後のフェーズの間でダウンタイムが発生する問題を修正しました。 これで、デプロイ後のフェーズは、デプロイフェーズの終了後&#x200B;_すぐに_&#x200B;開始されます。

- ![修正アイコン ](../../assets/fix.svg)正常に実行されたcron ジョブ（`status = success`を含む）をスケジュールから削除しなかった問題を修正しました。<!-- MAGECLOUD-2268 -->

- ![ アイコンの修正](../../assets/fix.svg) プロジェクトのデプロイ後フェーズではなく、デプロイ フェーズでキャッシュをクリアした`post_deploy` フックの問題を修正しました。<!-- MAGECLOUD-2113 -->

- ![修正アイコン ](../../assets/fix.svg)複数のロケールでSCDを使用する際に、各ロケールに同じ`js-translation.json` ファイルが生成される問題を修正しました。<!-- MAGECLOUD-2034 -->

- ![修正アイコン ](../../assets/fix.svg) テーブルのロックを回避し、速度を向上させるために、`ece-tools` パッケージの`db:dump` コマンドを最適化しました。<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>2.2.4との互換性を確保するには、ECE-Tools バージョン 2002.0.11が必要です。

- ![新しいアイコン ](../../assets/new.svg) **非マスターノードへの読み取り専用接続の設定** – このリリースでは、非マスターノードに読み取り専用の接続を設定して、読み取り専用トラフィックを受信する機能が追加されました（[MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)の場合）。<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection)および
<!--MAGECLOUD-143 -->

- ![新しいアイコン ](../../assets/new.svg) **設定ウィザード** – 静的コンテンツのデプロイメント用に設定を検証するためのウィザードを追加しました。 [ スマートウィザード ](../deploy/smart-wizards.md)を参照してください。<!--MAGECLOUD-1910 -->

- ![新しいアイコン ](../../assets/new.svg) **Symfony Consoleのサポート** - Adobe Commerce 2.3とSymfony Console 4のサポートを追加しました。<!-- MAGECLOUD-1966 -->

- ![修正アイコン ](../../assets/fix.svg) **Cron スケジュールの最適化** - キュー管理を改善し、ログを強化して、cron関連の問題のデバッグに役立てました。<!-- MAGECLOUD-1607 -->

- `ADMIN_EMAIL`または`ADMIN_USERNAME`の値が既存の管理者アカウントと同じ場合、![修正アイコン ](../../assets/fix.svg)の展開の検証が失敗します。<!-- MAGECLOUD-1221 -->

- ![ アイコンを修正](../../assets/fix.svg) 2.2.x バージョンのSOLR サポートを削除しました。 2.1.x バージョンでは、SOLRを有効にする機能が保持されます。<!-- MAGECLOUD-1282 -->

- ![fix icon](../../assets/fix.svg) PRO プロジェクトのステージングおよび実稼動環境の最初のインストールには、Elasticsearchの異なるインデックスプレフィックスが含まれるようになり、各環境に属するレコードを識別する際に競合が発生する可能性を防ぐことができます。<!-- MAGECLOUD-1489 -->

- ![修正アイコン ](../../assets/fix.svg)静的コンテンツのデプロイメント中にレガシーアーキテクチャのビルド フェーズが中断する問題を修正しました。<!-- MAGECLOUD-2021 -->

- ![ アイコンの修正](../../assets/fix.svg) **Cron固有の機能強化**- cronの実装を再作業しました：<!-- MAGECLOUD-1607 -->

   - cron キューがすばやく入力される問題を修正しました。 これにより、古くなったcron ジョブをより確実な方法でクリアできます。

   - cron ジョブのシーケンスを再編成して、別のスレッドのすべてのジョブが一般グループの前に起動されるようにしました。

   - cronの問題のデバッグをより支援するために、ログを改善しました。

   - **メモ** – このリリースでは、cronに関連する多くの問題を解決します。 現在、_m2-hotfixes_&#x200B;でcron関連のパッチを使用している場合は、それらを削除します。

- ![ アイコンを修正](../../assets/fix.svg) **SCD固有の改善**—

   - _ビルド_&#x200B;とデプロイの両方のフェーズで、`VERBOSE_COMMANDS`と`SCD_COMPRESSION_LEVEL`環境変数を使用できます。<!-- MAGECLOUD-1819 -->

   - `SCD_COMPRESSION_LEVEL`環境変数の予期しない値が検出された場合に、デプロイメントがランダムエラーで失敗する問題を修正しました。 有意義な通知を提供するための設定検証を改善しました。 許容可能な値については、[`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level)を参照してください。<!-- MAGECLOUD-2043 -->

   - `SCD_COMPRESSION_LEVEL`環境変数設定フローの動作を修正し、オーバーライドが期待どおりに機能するようにしました。<!-- MAGECLOUD-2044 -->

   - `.magento.env.yaml` ファイル _デプロイ_ ステージの`SCD_THREADS`環境変数の設定を妨げる問題を修正しました。<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![新しいアイコン ](../../assets/new.svg) **静的コンテンツのデプロイメント （SCD）** – 要求された場合（オンデマンド）に静的コンテンツを生成する、新しい代替デプロイメントプロセスがあります。 これにより、最も重要なアセットを生成することで、ダウンタイムが削減され、キャッシュ処理が改善されます。<!-- MAGECLOUD-1285 -->

   - **新しい環境変数** – 要求されたときに静的コンテンツを生成する`SCD_ON_DEMAND` グローバル環境変数を追加しました。<!-- MAGECLOUD-1738 -->

   - **デプロイ後のフック** - キャッシュをクリアし、コンテナが接続の受け入れを開始した&#x200B;_後にキャッシュを事前に読み込む（ウォーム）ファイル `.magento.app.yaml` ファイルの`post_deploy` フックを追加しました。_[!DNL Cloud Console]のステージング環境と実稼動環境を含むPro プロジェクトと、スタータープロジェクトでのみ使用できます。 これは必須ではありませんが、`SCD_ON_DEMAND`環境変数と連動して動作します。<!-- MAGECLOUD-1788 -->

- ![新しいアイコン ](../../assets/new.svg) **最適化** - デプロイメント時にファイルの移動またはコピーを最適化して、デプロイメントの速度を向上させ、ファイルシステムの負荷を軽減します。<!-- MAGECLOUD-1842 -->

- ![新しいアイコン ](../../assets/new.svg) **デプロイメント ログ** - デプロイメント プロセス中にログを出力するためのSyslogおよびGraylog Extended Log Format （GELF）ハンドラーを有効にする機能を追加しました。 [ ログハンドラー](../environment/log-handlers.md)を参照してください。<!-- MAGECLOUD-1751 -->

- ![新しいアイコン ](../../assets/new.svg)次の&#x200B;[**環境変数**](../environment/variables-intro.md)&#x200B;を追加しました：

   - `CRYPT_KEY` - データベースを移動する際に、暗号化キーを別の環境に提供します。<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_グローバル_&#x200B;環境変数。静的ビューファイルの`var/view_preprocessed` ディレクトリへのコピーをスキップし、要求されると縮小HTMLを生成します。<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_グローバル_&#x200B;環境変数。要求されたときに静的コンテンツを生成します。<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - キャッシュのプリロードに使用するページを一覧表示できます。 新しい[ デプロイ後の変数](../environment/variables-post-deploy.md)で使用できます。

- ![ アイコンを修正](../../assets/fix.svg) インスタンスのデプロイメントが破損し、ローカルに適用されたパッチが含まれる問題を修正しました。 ECE-Toolsは、パッチが適用されたことを検出できるようになりました。<!-- MAGECLOUD-982 -->

- ![ アイコンの修正](../../assets/fix.svg) JavaScript バンドルとGZIP機能の競合を修正しました。 これらの機能は正しく連携するようになりました。<!-- MAGECLOUD-1735 -->

- ![ アイコンの修正](../../assets/fix.svg)以前のバージョンのPHP 7.0.xを使用する際にECE-Tools CLI コマンドが失敗する問題を修正しました。<!-- MAGECLOUD-1744 -->

- ![ アイコンを修正](../../assets/fix.svg)複数のスレッドでコンパクトな戦略を使用すると、静的コンテンツのデプロイメントが妨げられる問題を修正しました。<!-- MAGECLOUD-1822 -->

- ![ アイコンを修正](../../assets/fix.svg)管理者のログイン遅延の原因となるRedis セッション ロックの問題を修正しました。 また、この修正は2.1.x.<!-- MAGECLOUD-1853 -->で利用できます

## v2002.0.9

>[!NOTE]
>
>これとすべての今後の更新を取得するには、[ クラウド インフラストラクチャ上のAdobe Commerce メタパッケージ ](../dev-tools/install-package.md#update-the-metapackage)をアップグレードする必要があります。

- ![new icon](../../assets/new.svg) **ece-tools** - `ece-tools` パッケージでAdobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->がサポートされるようになりました。

- ![新しいアイコン ](../../assets/new.svg) **Redis設定** – 環境変数を使用して、Redis](../environment/variables-deploy.md#cache_configuration) ページとデフォルトのキャッシュおよびRedis セッションストレージを[設定できるようになりました。<!-- MAGECLOUD-1552 -->

- ![新しいアイコン ](../../assets/new.svg) **検索、AMQP、およびRedis サービスの改善** - サービス設定フローを統合して、すべてのサービスで同じように動作するようにしました。 `env.php` ファイルを手動で編集してサービスを設定することはサポートされなくなりました。 代わりに、環境変数または`.magento.env.yaml` ファイルを使用する必要があります。<!-- MAGECLOUD-1437 -->

- ![ アイコンを修正](../../assets/fix.svg) **環境変数**—

   - `env:STATIC_CONTENT_THREADS`の使用は推奨されません。今後のリリースで削除されます。 代わりに[SCD_THREADS](../environment/variables-deploy.md#scd_threads)を使用してください。<!-- MAGECLOUD-1507 -->

   - `STATIC_CONTENT_EXCLUDE_THEMES`環境変数は非推奨（廃止予定）になりました。 代わりに`SCD_EXCLUDE_THEMES`環境変数を使用する必要があります。<!-- MAGECLOUD-1640 -->

- ![ アイコンの修正](../../assets/fix.svg) **ログ** – 組み込みのパッチ適用操作に関するログを簡単に記録できるようになりました。<!-- MAGECLOUD-1674 -->

- ![修正アイコン ](../../assets/fix.svg) `developer` モードのサポートと`APPLICATION_MODE`環境変数が予期しない動作を引き起こしていたため、削除されました。<!-- MAGECLOUD-1615 -->

- ![ アイコンを修正](../../assets/fix.svg)Redisに関連する静的コンテンツ展開エラーの原因となっている問題を修正しました。 これで、マルチスレッドの静的コンテンツのデプロイメントが設計どおりに実行されます。<!-- MAGECLOUD-1630 -->

- ![ アイコンを修正](../../assets/fix.svg) ユーザーが管理者の設定フィールドに変更を保存できない問題を修正しました。管理者は、`app:config:dump` コマンドを実行した後、機密性が高いとマークされています。<!-- MAGECLOUD-1175 -->

- ![修正アイコン ](../../assets/fix.svg)一部のパッケージとの競合を修正するために、以前のバージョンの`symfony/yaml`のサポートが追加されました。このパッケージは、最新バージョンと互換性がありません。<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>このリリースでは、`vendor/magento/ece-patches`を`vendor/magento/ece-tools`と統合しました。 `vendor/magento/ece-patches` パッケージを個別に更新する必要はありません。

**新機能：**

- **ログの改善**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - ビルドまたはデプロイのプロセスが環境変数を上書きする際に、より適切な説明を提供できるように、ログメッセージが改善されました。
   - インストールとアップグレードの進行状況をリアルタイムで確認できるようになりました。 進行状況を表示するには、`install_update.log` ファイルをテールします。 以下に例を挙げます。

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **新しいcron コマンド** - [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) コマンドですべてのcron ジョブを停止して再起動するのではなく、特定のスタックしたcron ジョブのロックを解除できるようになりました。 2.1では利用できません。<!-- MAGECLOUD-1367 -->

- **統合設定ファイル** - [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml) ファイルを使用して、ビルドとデプロイのステージを設定できるようになりました。<!-- MAGECLOUD-1369 -->

- **設定ファイルのバックアップ** – デプロイメント プロセスは、デプロイメント後に`app/etc/env.php`および`app/etc/config.php`設定ファイルのバックアップを自動的に作成するようになりました。 また、[新しいCLI コマンド ](https://support.magento.com/hc/en-us/articles/360033182871)を追加して、これらの設定ファイルをバックアップから復元しました。<!-- MAGECLOUD-1372 -->

- **検証エラーのトラブルシューティング** - `config.php`に静的コンテンツのデプロイメントに十分なデータが含まれていない場合に検証エラーを解決するために使用するコマンドを変更しました。 以前は、エラーメッセージで`bin/magento app:config:dump`を実行するように指示されていました。 次に、`php ./vendor/bin/ece-tools config:dump`を実行する必要があります。<!-- MAGECLOUD-1491 -->

- **新しい環境変数** – 環境変数を使用して、カスタム [検索](../environment/variables-deploy.md#search_configuration)および[AMQP ベースの](../environment/variables-deploy.md#queue_configuration) サービスをサイトに接続できるようになりました。<!-- MAGECLOUD-1410 -->

- スマートパッチを導入しました。 これで、パッケージは、クラウドインフラストラクチャバージョンのAdobe Commerceではなく、パッケージバージョンに基づいてパッチを適用します。<!--MAGECLOUD-1090-->

**解決済みの問題：**

- ビルド エラーの原因となっているログの問題を修正しました。<!-- MAGECLOUD-1162 -->

- インタラクティブモードでデプロイメントを実行すると、タイムアウト例外が発生する問題を修正しました。<!-- MAGECLOUD-1389 -->

- 静的コンテンツ生成にコンパクト戦略を使用する際にエラーが発生する問題を修正しました。 2.1では利用できません。<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- デプロイメントスクリプトがステージング環境と実稼動環境を正しく識別できない問題を修正しました。<!-- MAGECLOUD-1493 -->

- ネットワークの問題により、データベース接続が中断され、インストールおよびアップグレードのプロセス中にエラーが発生する問題を修正しました。<!-- MAGECLOUD-1520 -->

- `app:config:dump`を使用して設定ファイルを複数回エクスポートできない問題を修正しました。 2.1では利用できません。<!--  MAGECLOUD-1567  -->

- Redis セッション _ロック_&#x200B;の問題を修正しました。この問題により、_管理者_ ログイン遅延が発生しました。 2.1では利用できません。<!--  MAGECLOUD-1582  -->

- バージョン管理に関連する実装の問題を修正しました。この問題は、他のComposer ベースのパッチモジュールと競合していました。<!-- MAGECLOUD-1450 -->

- インポート中にPHP メモリの問題が発生していた問題を修正しました。<!-- MAGECLOUD-1310 -->

- パッチを削除しました。Cloud Infrastructure 2.2.1でAdobe Commerceのサポートを有効にするため、`colinmollenhour/credis` v1.6のバグを修正しました。 2.1では利用できません。<!-- MAGECLOUD-1033 -->

## v2002.0.7

**解決済みの問題：**

- JavaScriptの縮小の競合を引き起こしていた問題を修正するために、`var/view_preprocessed`のシンボリックリンクを削除しました。<!-- MAGECLOUD-1454 -->

## v2002.0.6

**解決済みの問題：**

- ファイルまたはディレクトリ名にスペースが含まれている場合に`gzip` エラーが発生する問題を修正しました。<!-- MAGECLOUD-1413 -->

- デプロイメントスクリプトがモジュールの依存関係を正しく認識し、有効にできない問題を修正しました。<!-- MAGECLOUD-1424 -->

## v2002.0.5

**新機能：**

- **環境変数を使用してcron コンシューマーを設定** – 新しい`CRON_CONSUMERS_RUNNER`環境変数を使用してcron コンシューマーを設定できるようになりました。

- **設定のスキャン** – ビルド/デプロイプロセス中に重要なコンポーネントをスキャンし、スキャンが失敗した場合はプロセスを停止します。これにより、サイトがメンテナンスモードになっているために不要なダウンタイムを防ぐことができます。

- **ビルド/デプロイ通知** – すべての環境でビルド/デプロイ アクションを行うための[Slackやメール通知](../environment/set-up-notifications.md)の設定に使用できる設定ファイルを追加しました。

- **静的コンテンツ圧縮** - ビルドおよびデプロイのフェーズで[gzip](https://www.gnu.org/software/gzip/)を使用して、静的コンテンツを圧縮するようになりました。 この圧縮とFastlyの圧縮は、ストアのサイズを縮小し、展開の速度を向上させるのに役立ちます。 必要に応じて、[ ビルドオプション ](../environment/variables-build.md)または[ デプロイ変数](../environment/variables-deploy.md)を使用して圧縮を無効にできます。 詳しくは、次のトピックを参照してください。

   - [アプリケーション環境変数](../application/variables-property.md)

   - [静的なコンテンツ展開のパフォーマンス](../deploy/static-content.md)

   - [デプロイメントプロセス](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **構成管理** – ビルド フェーズ中に`app/etc/config.php` ファイルが既に存在しない場合、Git リポジトリで自動生成されるようになりました。 自動生成ファイルには、モジュールと拡張機能のリストのみが含まれます。 ファイルが既に存在する場合、ビルド フェーズは通常どおり続行されます。 後で[構成管理](../store/store-settings.md)に従う場合、コマンドは追加の手順を必要とせずにファイルを更新します。 詳しくは、[ デプロイメントプロセス ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)を参照してください。

- **データベースダンプ** – すべての環境でデータベースダンプを作成するための`magento/ece-tools` CLI コマンドを追加しました。 Pro プランの実稼動環境の場合、このコマンドは3つの高可用性ノードのうちの1つからのみダンプされるため、ダンプ中に別のノードに書き込まれた実稼動データはコピーされない場合があります。 実稼動環境でデータベースダンプを実行する前に、アプリケーションをメンテナンスモードにすることをお勧めします。 詳しくは、[ バックアップ管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots)を参照してください。

- **Cron間隔の制限が解除されました**- us-3、eu-3、およびap-3地域でプロビジョニングされたすべての環境のデフォルトのcron間隔は1分です。 その他のすべてのリージョンのデフォルトのcron間隔は、Pro統合環境では5分、Pro ステージング環境および実稼動環境では1分です。 既存のcron ジョブを変更するには、`.magento.app.yaml`で設定を編集するか、実稼動/ステージング環境用のサポートチケットを作成します。 詳しくは、[cron ジョブの設定](../application/crons-property.md#set-up-cron-jobs)を参照してください。

**解決済みの問題：**

- 静的コンテンツのデプロイメントの前に`cache-clean`操作を呼び出すデプロイプロセスが原因で、デプロイ時間が長くなる問題を修正しました。<!-- MAGECLOUD-1327 -->

- 実稼動環境でのデプロイメントの静的コンテンツ生成ステップ中にエラーが発生する問題を修正しました。<!-- MAGECLOUD-1322 -->

- 一部の`magento/ece-tools` コマンドが`stderr`に出力をログ記録できない問題を修正しました。<!-- MAGECLOUD-1264 -->

- `env.php`のベース URL値がフォークされた分岐で更新されない問題を修正しました。<!-- MAGECLOUD-1242 -->

- セキュリティで保護されていないプレフィックス （`http://`）をセキュリティで保護されたベース URLに追加する`magento setup:install` コマンドの問題を修正しました。<!-- MAGECLOUD-1171 -->

- パッチ エラーによって展開エラーが発生しない問題を修正しました。<!-- MAGECLOUD-1170 -->

- パッチを適用できない場合、`ece-tools`が実行を停止し、例外をスローするのを防ぐ問題を修正しました。<!-- MAGECLOUD-1152 -->

- 管理者でHTMLの縮小を有効にした後、ストアフロントを読み込む際にエラーが発生する問題を修正しました。<!-- MAGECLOUD-1138 -->

## v2002.0.4

**解決済みの問題：**

- SSH アクセスを介して、すべての環境でCLI コマンドを使用して、スタックしたcron ジョブを[手動でリセットできるようになりました](https://support.magento.com/hc/en-us/articles/360033099451)。 展開プロセスは、cron ジョブを自動的にリセットします。<!-- MAGECLOUD-1355 -->

## v2002.0.3

**解決済みの問題：**

- Redisの読み取り/書き込みに時間がかかりすぎたため、ページがタイムアウトする問題を修正しました。 Redis設定で`disable_locking` パラメーターを使用して、この問題を回避できるようになりました。<!-- MAGECLOUD-1311 -->

## v2002.0.2

**解決済みの問題：**

- [!DNL RabbitMQ]設定プロセスは、必要なすべてのパラメーターを自動的に取得するようになりました。<!-- MAGECLOUD-1246 -->

## v2002.0.1

**新機能：**

- クラウド インフラストラクチャ上のAdobe Commerceで、スコープと[静的コンテンツ デプロイメント戦略](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy)がサポートされるようになりました。 静的コンテンツ展開戦略のデフォルト設定が`quick`の`–s` パラメーターを追加しました。 環境変数[SCD_STRATEGY](../environment/variables-deploy.md)を使用して、ビルドおよびデプロイのアクションでこれらの戦略をカスタマイズして使用できます。 この変数は、オプション `standard`、`quick`または`compact`をサポートしています。 `compact`を選択すると、`STATIC_CONTENT_THREADS`の値が`1`で上書きされ、特に実稼動環境ではデプロイメントが遅くなる可能性があります。 2.1では利用できません。<!--- MAGECLOUD-1057 -->

- 環境に関するログファイルを作成して、ビルドおよびデプロイのアクションをキャプチャおよびコンパイルしました。 `var/log/cloud.log` ファイルはルート アプリケーション ディレクトリにあります。<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**解決済みの問題：**

- `ece-tools` パッケージをリファクタリングして、クラウドインフラストラクチャ 2.2.0以降のAdobe Commerceと互換性を持たせました。<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- パッチを適用できない場合、`ece-tools`が実行を停止し、例外をスローするのを妨げる問題を修正しました。<!-- MAGECLOUD-1186 -->

- ビルド中に依存関係インジェクション （di） コンパイルがスキップされると、例外がスローされる問題を修正しました。<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- デプロイプロセスが`env.php` ファイルのカスタム Redis設定を上書きする問題を修正しました。<!-- MAGECLOUD-1019 -->

- デフォルトのセキュアな管理者によって無効にされたため、リダイレクトループが発生していた問題を修正しました。<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>このパッケージは、クラウドインフラストラクチャ上の他のバージョンのAdobe Commerceとの互換性がなくなったため、**は使用できません**。

### 初回リリース

Adobe Commerce on cloud infrastructure 2.2.0の`ece-tools`の最初のリリース。
