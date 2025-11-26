---
title: 後方互換性のない変更
description: 既存のクラウドプロジェクトをアップグレードする際の後方互換性について説明します。
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# 後方互換性のない変更

下位互換性のない変更を行うには、Cloud パッケージの最新リリースまたはCommerce パッケージ用の他の Cloud Tools Suite にアップグレードする際に、既存の `ece-tools` ラウドプロジェクトのクラウド設定とプロセスを調整する必要がある場合があります。

## パッケージ `ece-tools` 変更

以前 `ece-tools` パッケージに含まれていた機能の一部は、現在は別のパッケージで提供されています。 これらのパッケージは、ece-tools をインストールまたは更新すると自動的にインストールおよび更新される、`ece-tools` 用の composer の依存関係です。

新しいアーキテクチャは、インストールや更新のプロセスには影響を与えません。 ただし、クラウドインフラストラクチャプロジェクトでAdobe Commerceを使用する場合、コマンドの構文とプロセスをいくつか変更する必要が生じる場合があります。 詳しくは、次の後方互換性のない変更情報と [Cloud Tools Suite リリースノート &#x200B;](cloud-tools-suite.md) を参照してください。

### サービス バージョン要件の変更

`ece-tools` v2002.1.0 以降を使用するクラウドプロジェクトの場合、PHP の最小バージョン要件を 7.0.x から 7.1.x に変更しました。 環境設定で PHP 7.0 を指定している場合は、[&#x200B; ファイルの &#x200B;](../application/php-settings.md)php 設定 `.magento.app.yaml` を更新します。

>[!WARNING]
>
>PHP のバージョン要件が変わったので、`ece-tools` 2002.1.0 では、Adobe Commerce 2.1.15 以降を実行しているクラウドインフラストラクチャプロジェクトでのAdobe Commerceのみをサポートしています。 プロジェクトで以前のリリースを使用している場合、[&#x200B; 2002.1.0 に更新する前に &#x200B;](../development/commerce-version.md) アップグレード `ece-tools` する必要があります。

### 環境設定の変更

次の表は、`ece-tools` v2002.1.0 で削除または非推奨になった環境変数およびその他の環境設定ファイルの情報を示しています。

| 項目 | 置き換え |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` 変数 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` 変数 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` 変数 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` 変数 | なし。 現在は、ビルドでは常に、静的コンテンツディレクトリ `pub/static` へのシンボリックリンクが作成されます。 |
| `build_options.ini` ファイル | [`.magento.env.yaml`](../application/configure-app-yaml.md) ファイルを使用して環境変数を設定し、すべての環境でビルドおよびデプロイアクションを管理します。<p>`build_options.ini` ファイルを含むクラウド環境をビルドすると、ビルドに失敗します。 |

### CLI コマンドの変更点

次の表に、コマンドまたはスクリプトの更新が必要になる可能性のある ECE-Tools v2002.1.0 の CLI コマンドの変更点を要約します。

| コマンド | 置き換え |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

以前の ECE ツールリリースでは、`m2-ece-build` および `m2-ece-deploy` コマンドを使用して、`.magento.app.yaml` ファイルにデプロイメントフックを設定できました。 v2002.1.0 に更新する場合は、`hooks` ファイルの `.magento.app.yaml` 設定で古いコマンドを確認し、必要に応じて置き換えます。

## クラウドパッチの変更

- **ダウンロードしたパッチを削除**-`magento/magento-cloud-patches` パッケージには、「[&#x200B; ソフトウェアのダウンロード &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html?lang=ja)」ページから使用可能なすべてのパッチがバンドルされており、クラウドへのデプロイ時に自動的に適用されます。 ECE-Tools 2002.1.0 以降にアップグレードした後にパッチの競合が発生しないようにするには、ダウンロードしてプロジェクトに追加した、Adobe提供のパッチを手動で削除します。

- **apply patches コマンドの更新** - パッチを適用するためのコマンドを `vendor/bin/ece-tools` ディレクトリから `vendor/bin/ece-patches` ディレクトリに移動しました。 このコマンドを使用してパッチを手動で適用する場合は、新しいパスを使用します。

  > パッチを手動で適用

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker の変更点

- **PHP の最小バージョンは PHP 7.1 です**- Cloud Docker for Commerceで以前のバージョンが使用されている場合は、PHP v7.1 以降にアップグレードしてください。

- **Cloud Docker for Commerceのコマンドの変更点**-

   - **Docker ビルド操作用の Cloud Docker for Commerce コマンドの更新**- Cloud Docker for Commerce コマンドを `vendor/bin/ece-tools` ディレクトリから `vendor/bin/ece-docker` ディレクトリに移動しました。 新しいパスを使用するようにスクリプトとコマンドを更新します。

     `ece-tools` 2002.1.0 にアップグレードした後、次のコマンドを使用して、使用可能な `ece-docker` コマンドを表示します。

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Cloud docker-compose コマンドの更新**- コマンドファイルのパスを `./bin/docker` から `./bin/magento-docker` に変更しました。 新しいパスを使用するようにスクリプトとコマンドを更新します。

   - **Cron コンテナはデフォルトの Docker 設定に含まれなくなりました** – ここでは、`--with-cron` コマンドに `ece-docker build:compose` オプションを追加して、Docker 環境設定に Cron コンテナを含める必要があります。 [Cloud Docker for Commerce](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs) ガイドの _Cron ジョブの管理_ を参照してください。

     以前に cron ジョブでコンテナを生成したスクリプトは、cron コンテナを使用しなくなりました。

   - **一時コンテナの使用** – 以前のバージョンでは、`bin/magento-docker` のコマンド操作で作成されたコンテナは削除されなかったので、他の操作に使用できます。 `magento-docker` のコマンドは、コマンドが完了した後に作成したコンテナをすべて削除するようになりました。

     Docker-compose 操作で作成されたコンテナを保持する場合は、`docker-compose run` コマンドではなく `bin/magento-docker` コマンドを使用します。

   - **デプロイ後フックの実行**- `cloud-deploy` コマンドは、デプロイ後フックを実行しなくなりました。 新しい `cloud-post-deploy` コマンドを使用して、デプロイ後にデプロイ後のフックを実行します。 スクリプトを更新して、コマンドを追加し、デプロイ後のフックを実行します。

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     または、`docker-compose` コマンドを直接使用する場合は、deploy コマンドの後に `docker-compose run deploy cloud-post-deploy` コマンドを実行します。

- **データベースの更新** - データベースコンテナは、`magento-db` の永続 Docker ボリュームに保存されるようになりました。 Docker 環境を更新すると、データベースは自動的には削除されなくなります。 必要に応じて、次のいずれかのコマンドを使用して手動で削除します。

   - `magento-db` コンテナを削除します。

     ```bash
     docker volume rm magento-db
     ```

   - Docker コンテナをシャットダウンする際に、関連するボリュームをすべて削除します。

     ```bash
     docker-compose down -v
     ```

- **アーカイブおよびバックアップファイルのファイル同期設定の上書き**-Docker-sync または mutagen:SQL、GZ、ZIP、BZ2 を使用する場合、次の拡張子を持つアーカイブおよびバックアップファイルは同期されなくなります。 これらのファイルタイプのデフォルトのファイル同期を上書きするには、ファイル名を別の拡張子に変更します。 例：`synchronize-me.zip-backup`
