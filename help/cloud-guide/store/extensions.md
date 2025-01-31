---
title: 拡張機能の管理
description: クラウドインフラストラクチャー上のAdobe Commerceで拡張機能をインストールおよび管理する方法について説明します。
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 拡張機能の管理

[Commerce Marketplace](https://marketplace.magento.com) から拡張機能を追加して、Adobe Commerce アプリケーションの機能を拡張できます。 例えば、テーマを追加してストアフロントのルックアンドフィールを変更したり、言語パッケージを追加してストアフロントと管理者をローカライズしたりできます。

>[!NOTE]
>
>インストールの問題を回避するには、すべての Marketplace での購入を、クラウドプロジェクトを所有するのと同じアカウント（MAGEID）を使用して完了する必要があります。

## 拡張機能のコンポーザー名

このセクションでは、Commerce Marketplaceから Composer 名と拡張機能のバージョンを取得する方法について説明していますが、モジュールの Composer ファイル内で _any_ モジュールの名前とバージョンを見つけることができます。 `composer.json` ファイルをテキストエディターで開き、`"name"` と `"version"` の値をメモします。

**Commerce Marketplaceからモジュールの Composer 名を取得するには**:

1. コンポーネントの購入に使用したユーザー名とパスワードを使用して ](https://marketplace.magento.com)[Commerce Marketplaceにログインします。

1. 右上隅でユーザー名をクリックし、「**マイプロファイル**」を選択します。

   ![Marketplace アカウントにアクセス ](../../assets/marketplace/my-profile.png)

1. _マイアカウント_ ページで **購入履歴** をクリックします。

   ![Marketplace の購入履歴 ](../../assets/marketplace/my-purchases.png)

1. _購入_ ページで、購入したモジュールを選択し、「技術的な詳細 **をクリック** ます。

1. **コピー** をクリックして、[!UICONTROL Component name] をクリップボードにコピーします。

1. テキストエディターを開き、コンポーネント名を貼り付け、コロン文字（`:`）を追加します。

1. **技術的な詳細** で **コピー** をクリックして、[!UICONTROL Component version] をクリップボードにコピーします。

1. テキストエディターで、コンポーネント名のコロンの後にバージョン番号を追加します。 例：

   ```text
   extension-name/magento2:1.0.1
   ```

## 拡張機能のインストール

Adobeでは、実装に拡張機能を追加する際に、開発ブランチで作業することをお勧めします。 拡張機能をインストールすると、拡張機能名（`<VendorName>_<ComponentName>`）が [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html) ファイルに自動的に挿入されます。 ファイルを直接編集する必要はありません。

**拡張機能をインストールするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. 開発ブランチを作成またはチェックアウトします。 [ ブランチ ](../development/cli-branches.md) を参照してください。

1. Composer の名前とバージョンを使用して、`composer.json` ファイルの `require` セクションに拡張子を追加します。

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. プロジェクトの依存関係を更新します。

   ```bash
   composer update
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >拡張機能をインストールするときに、コードの変更をリモート環境にプッシュする際に、`composer.lock` ファイルを含める必要があります。 `composer install` コマンドは、`composer.lock` ファイルを読み取って、リモート環境で定義された依存関係を有効にします。

1. ビルドおよびデプロイが完了したら、SSH を使用してリモート環境にログインし、インストールされている拡張機能を確認します。

   ```bash
   bin/magento module:status <extension-name>
   ```

   拡張機能名には、`<VendorName>_<ComponentName>` の形式を使用します。

   応答の例：

   ```
   Module is enabled
   ```

   デプロイメントエラーが発生した場合は、[ 拡張機能のデプロイメントの失敗 ](../deploy/recover-failed-deployment.md) を参照してください。

## 拡張機能の管理

Composer を使用して拡張機能を追加すると、デプロイメント プロセスによって拡張機能が自動的に有効になります。 拡張機能が既にインストールされている場合は、CLI を使用して拡張機能を有効または無効にできます。 拡張機能を管理する場合、`<VendorName>_<ComponentName>` の形式を使用します。

リモート環境にログインしている間は、拡張機能を有効または無効にしないでください。

**拡張機能を有効または無効にするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. モジュールを有効または無効にします。 `module` コマンドは、要求されたモジュールのステータスで `config.php` ファイルを更新します。

   >モジュールを有効にします。

   ```bash
   bin/magento module:enable <module-name>
   ```

   >モジュールを無効にします。

   ```bash
   bin/magento module:disable <module-name>
   ```

1. モジュールを有効にした場合は、`ece-tools` を使用して設定を更新します。

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. モジュールのステータスを確認します。

   ```bash
   bin/magento module:status <module-name>
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## 拡張機能のアップグレード

続行する前に、拡張機能の Composer 名とバージョンが必要です。 また、拡張機能がプロジェクトおよびAdobe Commerceのバージョンと互換性があることを確認します。 特に、開始する前に [ 必要な PHP のバージョンを確認してください ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)。

**拡張機能を更新するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. 開発ブランチを作成またはチェックアウトします。 [ ブランチ ](../development/cli-branches.md) を参照してください。

1. `composer.json` ファイルをテキストエディターで開きます。

1. 拡張機能を探して、バージョンを更新します。

1. 変更を保存し、テキストエディターを終了します。

1. プロジェクトの依存関係を更新します。

   ```bash
   composer update
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

エラーが発生した場合は、[ コンポーネントの障害からの回復 ](../deploy/recover-failed-deployment.md) を参照してください。 Adobe Commerceでの拡張機能の使用について詳しくは、『 _管理者ガイド_ 』の [ 拡張機能 ](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html) を参照してください。
