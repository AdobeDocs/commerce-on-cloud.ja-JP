---
title: ECE-Tools を使用するようにプロジェクトをアップグレード
description: クラウドインフラストラクチャプロジェクトのAdobe Commerceをアップグレードして、ECE-Tools パッケージを使用し、最新の修正点および機能を活用する方法について説明します。
feature: Cloud, Install
exl-id: 164c47e4-c871-41a3-b268-581d426e7a7f
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# ECE-Tools パッケージを使用するようにプロジェクトをアップグレード

Adobeでは、`magento/magento-cloud-configuration` パッケージと `magento/ece-patches` パッケージを非推奨（廃止予定）にし、多くのクラウドプロセスを簡素化する `ece-tools` パッケージを優先しました。 `ece-tools` パッケージを含 _ない_ 古いAdobe Commerce on cloud infrastructure プロジェクトを使用する場合は、プロジェクトに対して 1 回限りの手動 _アップグレード_ プロセスを実行する必要があります。

>[!WARNING]
>
>プロジェクトに `ece-tools` パッケージが含まれている場合は、次のアップグレードをスキップできます。 検証するには、ローカルプロジェクトのルートディレクトリで `php vendor/bin/ece-tools -V` コマンドを使用して [!DNL Commerce] バージョンを取得します。

このプロジェクトのアップグレードプロセスでは、ルートディレクトリにある `composer.json` ファイルの `magento/magento-cloud-metapackage` バージョン制約を更新する必要があります。 この制約により、現在のAdobe Commerce バージョンをアップグレードすることなく、クラウドインフラストラクチャメタパッケージ（非推奨パッケージの削除など）上のAdobe Commerceを更新できます。

{{upgrade-tip}}

## 非推奨（廃止予定）のパッケージを削除

`ece-tools` パッケージを使用するようにアップグレードを実行する前に、`composer.lock` ファイルで以下の非推奨パッケージを確認してください。

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## メタパッケージの更新

Adobe Commerceの各バージョンには、次に基づいて異なる制約が必要です。

```
>=current_version <next_version
```

- `current_version` しくは、インストールするAdobe Commerceのバージョンを指定します。
- `next_version`:`current_version` で指定した値の後の次のパッチバージョンを指定します。

Adobe Commerce `2.3.5-p2` をインストールする場合は、`current_version` を `2.3.5` に、`next_version` を `2.3.6` に設定します。 制約 `">=2.3.5 <2.3.6"` は、2.3.5 で利用可能な最新のパッケージをインストールします。

最新のメタパッケージ制約は、[`magento-cloud` のテンプレートで常に見つけることができ ](https://github.com/magento/magento-cloud/blob/master/composer.json) す。

次の例では、クラウドインフラストラクチャメタパッケージ上のAdobe Commerceを、現在のバージョン 2.4.8 以上、次のバージョン 2.4.9 以下の任意のバージョンに制限します。

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
},
```

## プロジェクトのアップグレード

`ece-tools` パッケージを使用するようにプロジェクトをアップグレードするには、メタパッケージおよび `.magento.app.yaml` フック プロパティを更新し、Composer の更新を実行する必要があります。

**ece-tools を使用するようにプロジェクトをアップグレードするには**:

1. `composer.json` ファイルの `magento/magento-cloud-metapackage` バージョン制約を更新します。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.8 <2.4.9" --no-update
   ```

1. メタパッケージを更新します。

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. `magento.app.yaml` ファイルのフック コマンドを修正します。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. [ 非推奨パッケージ ](#remove-deprecated-packages) を確認して削除します。 非推奨パッケージを使用すると、アップグレードが正常に行われなくなる可能性があります。

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. 場合によっては、`ece-tools` パッケージを更新する必要があります。

   ```bash
   composer update magento/ece-tools
   ```

1. コードの変更内容を追加してコミットします。 この例では、次のファイルが更新されました。

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. コードの変更をリモートサーバーにプッシュし、このブランチを `integration` ブランチとマージします。

   ```bash
   git push origin <branch-name>
   ```
