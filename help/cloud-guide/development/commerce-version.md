---
title: Commerceのバージョンのアップグレード
description: クラウドインフラストラクチャプロジェクトでAdobe Commerceのバージョンをアップグレードする方法を説明します。
feature: Cloud, Upgrade
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# Commerceのバージョンのアップグレード

Adobe Commerceのコードベースを新しいバージョンにアップグレードできます。 プロジェクトをアップグレードする前に、最新のソフトウェア バージョン要件については [ インストール ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) ガイドの _システム要件_ を確認してください。

プロジェクトの設定に応じて、アップグレードタスクには次のものが含まれる場合があります。

- 新しいAdobe Commerce バージョンとの互換性を確保するために、MariaDB （MySQL）、OpenSearch、RabbitMQ、Redis などの更新サービスを提供しています。
- 古い構成管理ファイルを変換します。
- フックと環境変数の新しい設定で `.magento.app.yaml` ファイルを更新します。
- サードパーティの拡張機能をサポートされている最新バージョンにアップグレードします。
- `.gitignore` ファイルを更新します。

{{upgrade-tip}}

{{pro-update-service}}

## 古いバージョンからのアップグレード

2.1 より前のCommerce バージョンからアップグレードを開始する場合、Adobe Commerce コードベースの一部の制限が、特定の ECE-Tools リリースへの _アップデート_ や、次のサポートされるCommerce バージョンへの _アップグレード_ の機能に影響を与える可能性があります。 次の表を使用して、最適なパスを決定します。

| 現在のバージョン | アップグレードパス |
| ----------------- | ------------ |
| 2.1.3 以前 | 続行する前に、Adobe Commerceをバージョン 2.1.4 以降にアップグレードしてください。 次に、[1 回限りのアップグレード ](../dev-tools/install-package.md) を実行して、ECE-Tools をインストールします。 |
| 2.1.4 - 2.1.14 | [ECE ツールを更新 ](../dev-tools/update-package.md) パッケージ。<br>[2002.0.9 以降の 2002.0.x リリースのリリースノートを参照してください ](../release-notes/cloud-release-archive.md#v200209)。 |
| 2.1.15 - 2.1.16 | [ECE ツールを更新 ](../dev-tools/update-package.md) パッケージ。<br>2002[0.9 以降のリリースノートを参照してください ](../release-notes/cloud-release-archive.md#v200209)。 |
| 2.2.x 以降 | [ECE ツールを更新 ](../dev-tools/update-package.md) パッケージ。<br>2002[0.8 以降のリリースノートを参照してください ](../release-notes/cloud-release-archive.md#v200208)。 |

{style="table-layout:auto"}

{{ece-tools-package}}

## 設定管理

2.1.4 以降や 2.2.x 以降など、Adobe Commerceの以前のバージョンでは、Configuration Management に `config.local.php` ファイルを使用していました。 Adobe Commerce バージョン 2.2.0 以降では、`config.local.php` ファイルと同じように機能する `config.php` ファイルを使用しますが、有効なモジュールの一覧や追加の設定オプションなど、設定の種類が異なります。

古いバージョンからアップグレードする場合は、新しい `config.php` ファイルを使用するように `config.local.php` ファイルを移行する必要があります。 次の手順を使用して、設定ファイルをバックアップし、作成します。

**一時 `config.php` ファイルを作成するには**:

1. ファイルのコピー `config.local.php` 作成し、`config.php` という名前を付けます。

1. このファイルをプロジェクトの `app/etc` フォルダーに追加します。

1. ブランチにファイルを追加してコミットします。

1. ファイルを統合ブランチにプッシュします。

1. アップグレードプロセスを続行します。

>[!WARNING]
>
>アップグレード後、`config.php` ファイルを削除して新しい完全なファイルを生成することができます。 このファイルを削除して置き換えることができるのは、1 回だけです。 新しい完全なファイル `config.php` 生成した後は、そのファイルを削除して新しいファイルを生成することはできません。 [ 設定管理とパイプラインのデプロイメント ](../store/store-settings.md) を参照してください。

### Zend Framework Composer の依存関係の検証

2.2.x から **2.3.x 以降にアップグレードする場合は** Zend フレームワークの依存関係が `composer.json` ファイルの `autoload` プロパティに追加され、Lamina をサポートしていることを確認してください。 このプラグインは、Laminas プロジェクトに移行された Zend フレームワークの新しい要件をサポートします。 [2}MagentoDevBlog} の ](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251)Zend フレームワークの Laminas プロジェクトへの移行 _を参照してください。_

**`auto-load:psr-4` 設定を確認するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. 統合ブランチを確認します。

1. `composer.json` ファイルをテキストエディターで開きます。

1. Zend プラグインマネージャーを実装してコントローラの依存関係を確認するには、`autoload:psr-4` の節を参照してください。

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Zend 依存関係がない場合は、`composer.json` ファイルを更新します。

   - `autoload:psr-4` セクションに次の行を追加します。

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - プロジェクトの依存関係を更新します。

     ```bash
     composer update
     ```

   - コードの変更を追加、コミットおよびプッシュします。

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - 変更内容をステージング環境に結合してから、実稼動環境に結合します。

## 設定ファイル

アプリケーションをアップグレードする前に、クラウドインフラストラクチャー上のAdobe Commerceまたはアプリケーションのデフォルト設定に対する変更を考慮して、プロジェクト設定ファイルを更新する必要があります。 最新のデフォルトは、[magento-cloud GitHub リポジトリ ](https://github.com/magento/magento-cloud) にあります。

### .magento.app.yaml

[.magento.app.yaml](../application/configure-app-yaml.md) ファイルに含まれる値は、インストールしたバージョンで必ず確認してください。これは、アプリケーションのビルド方法や、クラウドインフラストラクチャへのデプロイ方法を制御しているからです。 次の例はバージョン 2.4.7 で、Composer 2.7.2 を使用します。`build: flavor:` プロパティは Composer 2.x には使用されません。[Composer 2 のインストールと使用 ](../application/properties.md#installing-and-using-composer-2) を参照してください。

**`.magento.app.yaml` ファイルを更新するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. `magento.app.yaml` ファイルを開いて編集します。

1. PHP オプションを更新します。

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. `hooks` プロパティ `build` および `deploy` コマンドを変更します。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 次の環境変数をファイルの末尾に追加します。

   Adobe Commerce 2.2.x から 2.3.x の場合 – 

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Adobe Commerce 2.4.x の場合 – 

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. ファイルを保存します。 リモート環境に対する変更は、まだコミットまたはプッシュしないでください。

1. アップグレードプロセスを続行します。

### composer.json

アップグレードする前に必ず `composer.json` ファイルの依存関係がAdobe Commerceのバージョンと互換性があることを確認してください。

**Adobe Commerce バージョン 2.4.4 以降の `composer.json` ファイルを更新するには**:

1. `config` セクションに次の `allow-plugins` を追加します。

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. `require` セクションに次のプラグインを追加します。

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. `extra:component_paths` セクションに次のコンポーネントを追加します。

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. ファイルを保存します。 ブランチの変更はまだコミットまたはプッシュしないでください。

1. アップグレードプロセスを続行します。

## プロジェクトのバックアップ

アップグレードの前に、プロジェクトのバックアップを作成することをお勧めします。 統合環境、ステージング環境、実稼動環境をバックアップするには、次の手順を使用します。

**統合環境のデータベースとコードをバックアップするには**:

1. リモート・データベースのローカル・バックアップを作成します。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump` コマンドは、`--single-transaction` フラグを指定して [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) コマンドを実行します。これにより、テーブルをロックせずにデータベースをバックアップできます。

1. コードとメディアをバックアップします。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   オプションで、既にソース管理内にある静的ファイルが多数ある場合は、`[--media]` を省略できます。

**デプロイする前にステージング環境または実稼動環境のデータベースをバックアップするには、** の手順に従います。

1. SSH を使用してリモート環境にログインします。

1. [ データベースダンプ ](../storage/database-dump.md) を作成します。 DB ダンプのターゲット・ディレクトリを選択するには、`--dump-directory` オプションを使用します。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   ダンプ操作では、リモート・プロジェクト・ディレクトリに `dump-<timestamp>.sql.gz` アーカイブ・ファイルが作成されます。 [ データベースのバックアップ ](../storage/database-dump.md) を参照してください。

## アプリケーションのアップグレード

アプリケーションをアップグレードする前に、最新のソフトウェアバージョン要件の [ サービスバージョン ](../services/services-yaml.md#service-versions) 情報を確認してください。

**アプリケーションのバージョンをアップグレードするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. [ バージョン制約構文 ](overview.md#cloud-metapackage) を使用して、アップグレードバージョンを設定します。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >`ece-tools` パッケージを正常に更新するには、バージョン制約構文を使用する必要があります。 アップグレードに使用する [ アプリケーションテンプレート ](https://github.com/magento/magento-cloud/blob/master/composer.json) のバージョンのバージョン制約を `composer.json` ファイルで確認できます。

1. プロジェクトを更新します。

   ```bash
   composer update
   ```

1. 現在適用されているパッチを確認します。

   - `m2-hotfixes` ディレクトリにパッチがインストールされている場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) し、Adobe Commerce サポートに連絡して、新しいバージョンに適用可能なパッチを確認します。 該当しないパッチを `m2-hotfixes` ディレクトリから削除します。

   - `.magento.env.yaml` ファイルに [ 品質向上パッチ ] が適用されている場合は、そのパッチを新しいバージョンにも適用できるかどうかを確認します。 `.magento.env.yaml` ファイルの `QUALITY_PATCHES` セクションから、適用できないパッチを削除します。

   **方法 1**:[ 品質パッチのリリースノートで該当するバージョンを確認してください ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法 2**:[ 使用可能なパッチおよびステータスの表示 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法 3**:[ パッチの検索 ](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Composer マーシャリング ベース パッケージの方式により、変更されたすべてのファイルをソース コントロールに追加するには `git add -A` が必要です。 `composer install` と `composer update` の両方で、ベースパッケージ（`magento/magento2-base` と `magento/magento2-ee-base`）からパッケージルートにファイルをマーシャリングします。

   Composer マーシャリングが属するファイルは、古いバージョンのAdobe Commerceを上書きするために新しいバージョンのファイルです。 現在、Adobe Commerceではマーシャリングが無効になっているので、マーシャリングされたファイルをソース管理に追加する必要があります。

1. デプロイメントが完了するまで待ちます。

1. SSH を使用してログインし、バージョンを確認して、統合環境、ステージング環境、実稼動環境でアップグレードを検証します。

   ```bash
   php bin/magento --version
   ```

### config.php ファイルの作成

[ 設定管理 ](#configuration-management) で説明したように、アップグレード後は、更新された `config.php` ファイルを作成する必要があります。 統合環境の管理者を通じて、追加の設定変更を行います。

**システム固有の設定ファイルを作成するには**:

1. ターミナルから、SSH コマンドを使用して、環境用の `/app/etc/config.php` ファイルを生成します。

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   例えば、Pro の場合は、`integration` のブランチで `scd-dump` を実行します。

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. `rsync` または `scp` を使用して、`config.php` ファイルをローカル ワークステーションに転送します。 このファイルはブランチに対してのみローカルに追加できます。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   これにより、モジュールリストと設定を含む、更新された `/app/etc/config.php` ファイルが生成されます。

>[!WARNING]
>
>アップグレードの場合は、`config.php` ファイルを削除します。 このファイルをコードに追加したら、削除 **ない** でください。 設定を削除または編集する必要がある場合は、ファイルを手動で編集します。

### アップグレード拡張機能

Marketplace または他の会社のサイトでサードパーティの拡張機能およびモジュールページを確認し、クラウドインフラストラクチャでのAdobe CommerceおよびAdobe Commerceのサポートを検証します。 サードパーティの拡張機能とモジュールをアップグレードする必要がある場合、Adobeでは、拡張機能を無効にした新しい統合ブランチで作業することをお勧めします。

**拡張機能を検証してアップグレードするには**:

1. ローカルワークステーションにブランチを作成します。

1. 必要に応じて、拡張機能を無効にします。

1. 使用可能な場合は、拡張機能のアップグレードをダウンロードします。

1. サードパーティのドキュメントに記載されているとおりに、アップグレードをインストールします。

1. 拡張機能を有効にしてテストします。

1. コードの変更を追加、コミットし、リモートにプッシュします。

1. 統合環境でにプッシュしてテストします。

1. ステージング環境にプッシュして、実稼動前の環境でテストします。

Adobeでは、アップグレードした拡張機能をサイト起動プロセスに含め _実稼動環境_ 以前）をアップグレードすることを強くお勧めします。

>[!NOTE]
>
>アプリケーションのバージョンをアップグレードすると、アップグレードプロセスが自動的に最新バージョンの [Fastly CDN モジュール ](../cdn/fastly.md#fastly-cdn-module-for-magento-2) に更新されます。

## アップグレードのトラブルシューティング

アップグレードに失敗すると、ストアフロントまたは管理パネルにアクセスできないことを示すエラーメッセージがブラウザーに表示されます。

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**エラーを解決するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. `./app/var/report/<error number>` ファイルを開きます。

1. [ ログを調べて ](../test/log-locations.md) 問題の原因を特定します。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
