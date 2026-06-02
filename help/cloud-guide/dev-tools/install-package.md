---
title: ECE-Toolsを使用するようにプロジェクトをアップグレード
description: ECE-Tools パッケージを使用し、最新の修正と機能を活用するために、Adobe Commerce on cloud インフラストラクチャプロジェクトをアップグレードする方法について説明します。
feature: Cloud, Install
exl-id: 164c47e4-c871-41a3-b268-581d426e7a7f
TQID: https://experienceleague.adobe.com/CH-wgIk-5aM6qIO7tdHI2jHlx1YgaEbLvfp4cEpSqTc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 376
ht-degree: 0%

---

# ECE-Tools パッケージを使用するようにプロジェクトをアップグレード

Adobeでは、`magento/magento-cloud-configuration`および`magento/ece-patches` パッケージが非推奨となり、多くのクラウドプロセスを簡素化する`ece-tools` パッケージが優先されました。 _not_&#x200B;に`ece-tools` パッケージが含まれている古いAdobe Commerce on cloud infrastructure プロジェクトを使用する場合は、プロジェクトに対して1回限りの手動&#x200B;_アップグレード_&#x200B;処理を実行する必要があります。

>[!WARNING]
>
>プロジェクトに`ece-tools` パッケージが含まれている場合は、次のアップグレードをスキップできます。 確認するには、ローカル プロジェクトのルート ディレクトリで`php vendor/bin/ece-tools -V` コマンドを使用して[!DNL Commerce] バージョンを取得します。

このプロジェクトのアップグレードプロセスでは、ルートディレクトリの`composer.json` ファイルの`magento/magento-cloud-metapackage` バージョン制約を更新する必要があります。 この制約により、現在のAdobe Commerce バージョンをアップグレードすることなく、非推奨のパッケージの削除など、Adobe Commerce on cloud infrastructure メタパッケージのアップデートが可能になります。

{{upgrade-tip}}

## 非推奨パッケージの削除

`ece-tools` パッケージを使用するようにアップグレードを実行する前に、次の非推奨パッケージについて`composer.lock` ファイルを確認してください。

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## メタパッケージの更新

Adobe Commerceの各バージョンには、次の要素に基づいて異なる制約が必要です。

```
>=current_version <next_version
```

- `current_version`に、インストールするAdobe Commerce バージョンを指定します。
- `next_version`の場合、`current_version`で指定した値の後に次のパッチバージョンを指定します。

Adobe Commerce `2.3.5-p2`をインストールする場合は、`current_version`を`2.3.5`に、`next_version`を`2.3.6`に設定します。 制約`">=2.3.5 <2.3.6"`は、2.3.5の利用可能な最新のパッケージをインストールします。

最新のメタパッケージ制約は、[`magento-cloud` テンプレート &#x200B;](https://github.com/magento/magento-cloud/blob/master/composer.json)にいつでも見つけることができます。

次の例では、Adobe Commerce on cloud infrastructure メタパッケージの制約を、現在のバージョン 2.4.8以上のバージョンと次のバージョン 2.4.9未満のバージョンに設定します。

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
},
```

## プロジェクトのアップグレード

プロジェクトをアップグレードして`ece-tools` パッケージを使用するには、メタパッケージと`.magento.app.yaml` フックのプロパティを更新し、Composerの更新を実行する必要があります。

**ece-tools**&#x200B;を使用するようにプロジェクトをアップグレードするには：

1. `composer.json` ファイルの`magento/magento-cloud-metapackage` バージョン制約を更新します。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.8 <2.4.9" --no-update
   ```

1. メタパッケージを更新します。

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. `magento.app.yaml` ファイルのフックコマンドを変更します。

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

1. 非推奨パッケージ [を確認して削除します](#remove-deprecated-packages)。 非推奨パッケージは、アップグレードの成功を妨げる可能性があります。

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. `ece-tools` パッケージを更新する必要がある場合があります。

   ```bash
   composer update magento/ece-tools
   ```

1. コードの変更を追加してコミットします。 この例では、次のファイルが更新されました。

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. コードの変更をリモート サーバーにプッシュし、このブランチを`integration` ブランチと結合します。

   ```bash
   git push origin <branch-name>
   ```
