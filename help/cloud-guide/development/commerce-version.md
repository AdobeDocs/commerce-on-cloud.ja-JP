---
title: Commerce バージョンのアップグレード
description: クラウドインフラストラクチャ環境でAdobe Commerce バージョンをアップグレードする方法について説明します。
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 770b0cbb98fccc1bb2d6791297b98e186c38fea3
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Commerce バージョンのアップグレード

Adobe Commerce コードベースを新しいバージョンにアップグレードできます。 環境をアップグレードする前に、_インストール_ ガイドの[必要システム構成](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)を参照して、最新のソフトウェアバージョン要件を確認してください。

環境タイプ（開発、ステージング、実稼動）に応じて、アップグレード タスクには次のものが含まれます。

- サードパーティ製の拡張機能を、サポートされている最新バージョンにアップグレードします。
- Pro プロジェクトの場合、ステージング環境と実稼動環境でサービスをインストールまたは更新するには、Adobe Commerce サポートチケットを送信する必要があります。
- 開発/統合/PR ブランチの場合：
   - 新しいバージョンのAdobe Commerceとの互換性を確保するために、MariaDB （MySQL）、OpenSearch、RabbitMQ、およびRedisの新しいバージョンで`.magento/services.yaml` ファイルを更新します。
   - フックと環境変数の新しい設定で`.magento.app.yaml` ファイルを更新します。

{{upgrade-tip}}

{{pro-update-service}}

## 設定ファイル

アプリケーションをアップグレードする前に、クラウドインフラストラクチャまたはアプリケーション上のAdobe Commerceのデフォルト設定の変更を考慮して、プロジェクト設定ファイルを更新する必要があります。 最新のデフォルトは、[magento-cloud GitHub リポジトリ ](https://github.com/magento/magento-cloud)にあります。

### composer.json

アップグレードする前に、常に`composer.json` ファイルの依存関係がAdobe Commerce バージョンと互換性があることを確認してください。

Adobe Commerce バージョン 2.4.4以降の`composer.json` ファイルを更新するには**

1. 次の`allow-plugins`を`config` セクションに追加します。

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 次のプラグインを`require` セクションに追加します。

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 次のコンポーネントを`extra:component_paths` セクションに追加します。

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. ファイルを保存します。 まだブランチに変更をコミットまたはプッシュしないでください。

1. アップグレードプロセスを続行します。

## 環境バックアップ

アップグレードの前にインスタンスのバックアップを作成することをお勧めします。 統合環境、ステージング環境および実稼動環境をバックアップするには、次の手順を実行します。

**統合環境データベースとコード**&#x200B;をバックアップするには：

1. リモートデータベースのローカルバックアップを作成します。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump` コマンドは[mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) コマンドを`--single-transaction` フラグで実行します。これにより、テーブルをロックせずにデータベースをバックアップできます。

1. コードとメディアをバックアップします。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   ソース管理に既に多数の静的ファイルがある場合は、オプションで`[--media]`を省略できます。

**デプロイする前に、ステージング環境または実稼動環境のデータベースをバックアップするには、**:

1. SSHを使用してリモート環境にログインします。

1. [ データベースダンプ ](../storage/database-dump.md)を作成します。 DB ダンプのターゲットディレクトリを選択するには、`--dump-directory` オプションを使用します。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   ダンプ操作により、リモート プロジェクト ディレクトリに`dump-<timestamp>.sql.gz` アーカイブ ファイルが作成されます。 [ データベースのバックアップ ](../storage/database-dump.md)を参照してください。

## アプリケーションのアップグレード

アプリケーションをアップグレードする前に、最新のソフトウェアバージョン要件について、[ サービスバージョン ](../services/services-yaml.md#service-versions)の情報を確認してください。

**アプリケーションのバージョン**&#x200B;をアップグレードするには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. ターゲット アップグレード バージョンの[ バージョン制約](overview.md#cloud-metapackage)を設定します。 この手順は、ターゲットバージョンが既存の制約の範囲外にある場合にのみ必要です。

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >バージョン制約の構文を使用して、`ece-tools` パッケージを正常に更新する必要があります。 アップグレードに使用している[ アプリケーションテンプレート ](https://github.com/magento/magento-cloud/blob/master/composer.json)のバージョンの`composer.json` ファイルにバージョン制約があります。

1. コア Commerce アップグレード バージョンで`composer.json` ファイルを更新します。

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. B2Bを使用している場合は、`composer.json` ファイルを[ サポートされているバージョン ](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions)のCommerceに更新します。

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. プロジェクトの依存関係を更新する：

   ```bash
   composer update
   ```

1. 現在適用されているパッチを確認します。

   - `m2-hotfixes` ディレクトリにパッチがインストールされている場合、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)を送信し、Adobe Commerce サポートと協力して、新しいバージョンにまだ適用できるパッチを確認します。 該当しないパッチを`m2-hotfixes` ディレクトリから削除します。

   - `.magento.env.yaml` ファイルに適用されている[品質パッチ ]がある場合は、新しいバージョンにまだ適用できるかどうかを確認します。 `.magento.env.yaml` ファイルの`QUALITY_PATCHES` セクションから該当しないパッチを削除します。

   **方法1**: [品質パッチのリリースノートで該当するバージョンを確認する](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法2**: [使用可能なパッチとステータスを表示](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法3**: [ パッチの検索](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Composerが基本パッケージをマーシャルする方法のため、変更されたすべてのファイルをソース管理に追加するには、`git add -A`が必要です。 `composer install`と`composer update`の両方が、基本パッケージ （`magento/magento2-base`と`magento/magento2-ee-base`）からパッケージルートにファイルをマーシャルします。

   コンポーザーのマーシャルが新しいバージョンのAdobe Commerceに属するファイル。これらの同じファイルの古いバージョンを上書きします。 現在、Adobe Commerceではマーシャリングは無効になっているので、マーシャリングされたファイルをソースコントロールに追加する必要があります。

1. デプロイメントが完了するのを待ちます。

1. SSHを使用してログインし、バージョンを確認して、統合環境、ステージング環境、実稼動環境のアップグレードを確認します。

   ```bash
   php bin/magento --version
   ```

### 拡張機能をアップグレード

Marketplaceや他社のサイトで、サードパーティの拡張機能やモジュールページを確認し、Adobe CommerceおよびAdobe Commerce on cloud インフラストラクチャのサポートを確認します。 サードパーティの拡張機能とモジュールをアップグレードする必要がある場合は、拡張機能を無効にして新しい統合ブランチで作業することをお勧めします。

**拡張機能を確認してアップグレードするには**:

1. ローカルワークステーションにブランチを作成します。

1. 必要に応じて拡張機能を無効にします。

1. 利用可能な場合は、拡張機能のアップグレードをダウンロードします。

1. サードパーティのドキュメントに記載されているように、アップグレードをインストールします。

1. 拡張機能を有効にしてテストします。

1. コードの変更を追加し、コミットし、リモートにプッシュします。

1. 統合環境でプッシュしてテストします。

1. ステージング環境にプッシュして、プリプロダクション環境でテストします。

Adobeでは、サイト起動プロセスにアップグレードされた拡張機能を含め、実稼動環境&#x200B;_before_&#x200B;をアップグレードすることを強くお勧めします。

>[!NOTE]
>
>アプリケーションのバージョンをアップグレードすると、アップグレードプロセスが[Fastly CDN モジュール ](../cdn/fastly.md#fastly-cdn-module-for-magento-2)の最新バージョンに自動的に更新されます。

## アップグレードのトラブルシューティング

アップグレードに失敗した場合、ストアフロントまたは管理パネルにアクセスできないことを示すエラーメッセージがブラウザーに表示されます。

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**エラーを解決するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. `./app/var/report/<error number>` ファイルを開きます。

1. [ ログを調べ](../test/log-locations.md)問題の原因を特定します。

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
