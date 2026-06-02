---
title: 後方互換性のない変更
description: 既存のCloud プロジェクトをアップグレードする際の下位互換性について説明します。
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
TQID: https://experienceleague.adobe.com/ekS7f5swOsG2xgXP6ybN6hzwYm2xBbPWvl5oabv7Crc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 822
ht-degree: 0%

---

# 後方互換性のない変更

後方互換性のない変更では、`ece-tools` パッケージまたはその他のCloud Tools Suite for Commerce パッケージの最新リリースにアップグレードする際に、既存のCloud プロジェクトのCloud設定とプロセスを調整する必要がある場合があります。

## `ece-tools` パッケージへの変更

以前は`ece-tools` パッケージに含まれていた一部の機能が、個別のパッケージで提供されるようになりました。 これらのパッケージは、`ece-tools`のコンポーザー依存関係です。これらは、e ツールのインストールまたは更新時に自動的にインストールおよび更新されます。

新しいアーキテクチャは、インストールプロセスやアップデートプロセスに影響を与えることはありません。 ただし、Adobe Commerce on cloud infrastructure プロジェクトを使用する場合は、コマンドの構文やプロセスを変更する必要がある場合があります。 詳しくは、後方互換性のない次の変更情報と[Cloud Tools Suite リリースノート &#x200B;](cloud-tools-suite.md)を参照してください。

### サービスバージョン要件の変更

`ece-tools` v2002.1.0以降を使用するクラウドプロジェクトの最小PHP バージョン要件を7.0.xから7.1.xに変更しました。 環境設定でPHP 7.0が指定されている場合は、`.magento.app.yaml` ファイルの[php設定](../application/php-settings.md)を更新してください。

>[!WARNING]
>
>PHPのバージョン要件が変更されたため、`ece-tools` 2002.1.0では、Adobe Commerce 2.1.15以降を実行しているクラウドインフラストラクチャプロジェクト上のAdobe Commerceのみをサポートしています。 プロジェクトで以前のリリースを使用している場合は、`ece-tools` 2002.1.0に更新する前に[&#x200B; アップグレード &#x200B;](../development/commerce-version.md)する必要があります。

### 環境設定の変更

次の表は、`ece-tools` v2002.1.0で削除または非推奨になった環境変数およびその他の環境設定ファイルに関する情報を示しています。

| 項目 | 置き換え |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES`変数 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS`変数 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT`変数 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK`変数 | なし。 これで、ビルドでは常に静的コンテンツディレクトリ `pub/static`へのシンボリックリンクが作成されます。 |
| `build_options.ini` ファイル | [`.magento.env.yaml`](../application/configure-app-yaml.md) ファイルを使用して環境変数を設定し、すべての環境でビルドおよびデプロイのアクションを管理します。<p>`build_options.ini` ファイルを含むクラウド環境をビルドすると、ビルドが失敗します。 |

### CLI コマンドの変更

次の表に、コマンドまたはスクリプトの更新が必要になる場合があるECE-Tools v2002.1.0でのCLI コマンドの変更を示します。

| コマンド | 置き換え |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

以前のECE-Tools リリースでは、`m2-ece-build`および`m2-ece-deploy` コマンドを使用して、`.magento.app.yaml` ファイルのデプロイメントフックを設定できます。 v2002.1.0に更新する場合は、`.magento.app.yaml` ファイルの`hooks`設定で古いコマンドを確認し、必要に応じてそれらを置き換えます。

## クラウドパッチの変更

- **ダウンロード済みのパッチを削除**- `magento/magento-cloud-patches` パッケージは、[&#x200B; ソフトウェアのダウンロード &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) ページから入手できるすべてのパッチをバンドルし、クラウドにデプロイするときに自動的に適用します。 ECE-Tools 2002.1.0以降にアップグレードした後にパッチの競合を防ぐには、Adobeから提供されたパッチのうち、手動でダウンロードしてプロジェクトに追加したものをすべて削除します。

- **パッチ適用コマンドの更新** – パッチを適用するコマンドを`vendor/bin/ece-tools` ディレクトリから`vendor/bin/ece-patches` ディレクトリに移動しました。 このコマンドを使用してパッチを手動で適用する場合は、新しいパスを使用します。

  > パッチを手動で適用

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Dockerの変更

- **PHPの最小バージョン要件はPHP 7.1**&#x200B;になりました。Cloud Docker for Commerce ホストが以前のバージョンを実行している場合は、PHP v7.1以降にアップグレードしてください。

- **Commerce用Cloud Docker コマンドの変更**-

   - **Docker ビルド操作のCommerce コマンド用Cloud Dockerの更新**-Commerce コマンド用Cloud Dockerを`vendor/bin/ece-tools` ディレクトリから`vendor/bin/ece-docker` ディレクトリに移動しました。 新しいパスを使用するようにスクリプトとコマンドを更新します。

     `ece-tools` 2002.1.0にアップグレードした後、次のコマンドを使用して、使用可能な`ece-docker` コマンドを表示します。

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Cloud docker-compose コマンドの更新** – コマンドファイルへのパスの名前を`./bin/docker`から`./bin/magento-docker`に変更しました。 新しいパスを使用するようにスクリプトとコマンドを更新します。

   - **Cron コンテナがデフォルトのDocker設定に含まれなくなりました** – 次に、`ece-docker build:compose` コマンドに`--with-cron` オプションを追加して、Cron コンテナをDocker環境設定に含める必要があります。 _Cloud Docker for Commerce_ ガイドの「[Cron ジョブの管理](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs)」を参照してください。

     cron ジョブを持つ以前に生成されたコンテナを持つスクリプトが、cron コンテナを持たなくなりました。

   - **一時コンテナを使用** – 以前のバージョンでは、`bin/magento-docker` コマンド操作で作成されたコンテナは削除されなかったため、他の操作に使用できました。 これで、`magento-docker` コマンドは、コマンドの完了後に作成したコンテナをすべて削除します。

     docker-compose操作によって作成されたコンテナを保持する場合は、`bin/magento-docker` コマンドの代わりに`docker-compose run` コマンドを使用します。

   - **デプロイ後のフックの実行**- `cloud-deploy` コマンドは、デプロイ後のフックを実行しなくなりました。 デプロイ後にデプロイ後のフックを実行するには、新しい`cloud-post-deploy` コマンドを使用します。 スクリプトを更新して、デプロイ後のフックを実行するコマンドを追加します。

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     または、`docker-compose` コマンドを直接使用する場合は、デプロイ コマンドの後に`docker-compose run deploy cloud-post-deploy` コマンドを実行します。

- **データベースを更新しています**- データベースコンテナは`magento-db`の永続的なDocker ボリュームに保存されるようになりました。 Docker環境を更新すると、データベースは自動的に削除されなくなります。 必要に応じて、次のいずれかのコマンドを使用して手動で削除します。

   - `magento-db` コンテナを削除します。

     ```bash
     docker volume rm magento-db
     ```

   - Docker コンテナをシャットダウンする際に、関連するすべてのボリュームを削除します。

     ```bash
     docker-compose down -v
     ```

- **アーカイブ ファイルとバックアップ ファイルの同期設定を上書きする**-docker-syncまたはmutagenを使用する場合、アーカイブ ファイルとバックアップ ファイルの同期が行われなくなりました（SQL、GZ、ZIP、BZ2）。 これらのファイルタイプのデフォルトのファイル同期は、ファイル名を別の拡張子で終わるように変更することで上書きできます。 例：`synchronize-me.zip-backup`
