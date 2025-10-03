---
title: Commerceのバージョンのアップグレード
description: クラウドインフラストラクチャ環境でAdobe Commerceのバージョンをアップグレードする方法を説明します。
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 7f9aac358effdf200b59678098e6a1635612301b
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 0%

---

# Commerceのバージョンのアップグレード

Adobe Commerceのコードベースを新しいバージョンにアップグレードできます。 環境をアップグレードする前に、最新のソフトウェア バージョン要件について [&#x200B; インストール &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) ガイドの _システム要件_ を確認してください。

環境のタイプ（開発、ステージング、実稼動）に応じて、アップグレードタスクには次のものが含まれます。

- 新しいAdobe Commerce バージョンとの互換性を保つため、MariaDB （MySQL）、OpenSearch、RabbitMQ、Redis の新しいバージョンで `.magento/services.yaml` ファイルをアップデートします。
- フックと環境変数の新しい設定で `.magento.app.yaml` ファイルを更新します。
- サードパーティの拡張機能をサポートされている最新バージョンにアップグレードします。

{{upgrade-tip}}

{{pro-update-service}}

## 設定ファイル

アプリケーションをアップグレードする前に、クラウドインフラストラクチャー上のAdobe Commerceまたはアプリケーションのデフォルト設定に対する変更を考慮して、プロジェクト設定ファイルを更新する必要があります。 最新のデフォルトは、[magento-cloud GitHub リポジトリ &#x200B;](https://github.com/magento/magento-cloud) にあります。

### composer.json

アップグレードする前に必ず `composer.json` ファイルの依存関係がAdobe Commerceのバージョンと互換性があることを確認してください。

Adobe Commerce バージョン 2.4.4 以降の `composer.json` ファイルを更新するには、**の手順を実行します。

1. `allow-plugins` セクションに次の `config` を追加します。

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

## 環境のバックアップ

アップグレードの前に、インスタンスのバックアップを作成することをお勧めします。 統合環境、ステージング環境、実稼動環境をバックアップするには、次の手順を使用します。

**統合環境のデータベースとコードをバックアップするには**:

1. リモート・データベースのローカル・バックアップを作成します。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump` コマンドは、[&#x200B; フラグを指定して &#x200B;](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)mysqldump`--single-transaction` コマンドを実行します。これにより、テーブルをロックせずにデータベースをバックアップできます。

1. コードとメディアをバックアップします。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   オプションで、既にソース管理内にある静的ファイルが多数ある場合は、`[--media]` を省略できます。

**デプロイする前にステージング環境または実稼動環境のデータベースをバックアップするには、** の手順に従います。

1. SSH を使用してリモート環境にログインします。

1. [&#x200B; データベースダンプ &#x200B;](../storage/database-dump.md) を作成します。 DB ダンプのターゲット・ディレクトリを選択するには、`--dump-directory` オプションを使用します。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   ダンプ操作では、リモート・プロジェクト・ディレクトリに `dump-<timestamp>.sql.gz` アーカイブ・ファイルが作成されます。 [&#x200B; データベースのバックアップ &#x200B;](../storage/database-dump.md) を参照してください。

## アプリケーションのアップグレード

アプリケーションをアップグレードする前に、最新のソフトウェアバージョン要件の [&#x200B; サービスバージョン &#x200B;](../services/services-yaml.md#service-versions) 情報を確認してください。

**アプリケーションのバージョンをアップグレードするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. ターゲットのアップグレード バージョンに対して [&#x200B; バージョン制約 &#x200B;](overview.md#cloud-metapackage) を設定します。 この手順は、ターゲットバージョンが既存の制約の範囲外にある場合にのみ必要です。

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >`ece-tools` パッケージを正常に更新するには、バージョン制約構文を使用する必要があります。 アップグレードに使用する `composer.json` アプリケーションテンプレート [&#x200B; のバージョンのバージョン制約を &#x200B;](https://github.com/magento/magento-cloud/blob/master/composer.json) ファイルで確認できます。

1. コアのCommerce アップグレードバージョンで `composer.json` ファイルを更新します。

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. B2B を使用している場合は、Commerceの `composer.json` サポートされているバージョン [&#x200B; で &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions) ファイルをアップデートします。

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. プロジェクトの依存関係を更新します。

   ```bash
   composer update
   ```

1. 現在適用されているパッチを確認します。

   - `m2-hotfixes` ディレクトリにパッチがインストールされている場合は、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) し、Adobe Commerce サポートに連絡して、新しいバージョンに適用可能なパッチを確認します。 該当しないパッチを `m2-hotfixes` ディレクトリから削除します。

   - [ ファイルに ] 品質向上パッチ `.magento.env.yaml` が適用されている場合は、そのパッチを新しいバージョンにも適用できるかどうかを確認します。 `QUALITY_PATCHES` ファイルの `.magento.env.yaml` セクションから、適用できないパッチを削除します。

   **方法 1**:[&#x200B; 品質パッチのリリースノートで該当するバージョンを確認してください &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法 2**:[&#x200B; 使用可能なパッチおよびステータスの表示 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法 3**:[&#x200B; パッチの検索 &#x200B;](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


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

### アップグレード拡張機能

Marketplace または他の会社のサイトでサードパーティの拡張機能およびモジュールページを確認し、クラウドインフラストラクチャでのAdobe CommerceおよびAdobe Commerceのサポートを検証します。 サードパーティの拡張機能およびモジュールをアップグレードする必要がある場合、Adobeでは、拡張機能を無効にした新しい統合ブランチで作業することをお勧めします。

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
>アプリケーションのバージョンをアップグレードすると、アップグレードプロセスが自動的に最新バージョンの [Fastly CDN モジュール &#x200B;](../cdn/fastly.md#fastly-cdn-module-for-magento-2) に更新されます。

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

1. [&#x200B; ログを調べて &#x200B;](../test/log-locations.md) 問題の原因を特定します。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
