---
title: パッチの適用
description: Adobe Commerce on cloud infrastructure プロジェクトにパッチを適用する方法を説明します。
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# パッチの適用

[Cloud Patches for Commerce](https://github.com/magento/magento-cloud-patches) および [Quality Patches Tool](https://github.com/magento/quality-patches) は、インストールされているAdobe Commerce アプリケーションにパッチを提供します。

- Commerce用クラウドパッチ パッケージは、重要な修正を含む必要なパッチを提供します
- 品質パッチは、後方互換性のない変更を含まない [ 個別パッチ ](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html?lang=ja#individual-patch) として、オプションの影響の少ない品質修正を提供します

リリース済みのパッチの完全なリストを確認するには、_Commerce運用ツールガイド_ の [ 使用可能なパッチ ](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=ja) を参照してください。

どちらのパッケージも、すべてのAdobe Commerce バージョンとクラウド環境の統合を強化し、重要な修正、オプションの修正、カスタムの修正の迅速な配信をサポートします。 これらのパッケージを使用して、Commerceで使用可能な個々のパッチに関する一般情報を適用、元に戻し、表示できます。

>[!TIP]
>
>Magento Open SourceプロジェクトとAdobe Commerce プロジェクトのスタンドアロンパッケージとして、[ 品質向上パッチツール ](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=ja) とCommerce用クラウドパッチを使用できます。 クラウド以外のプロジェクトには、品質向上パッチツールを使用することをお勧めします。

リモート環境に変更をデプロイすると、`ece-tools` パッケージは `magento/magento-cloud-patches` と `magento/quality-patches` を使用して保留中のパッチをチェックし、次の順序で自動的に適用します。

1. 「Commerce用クラウドパッチ」パッケージに含まれる、必要なすべてのCommerce パッチを適用します。
1. Quality Patches Tool に含まれているオプションのCommerce パッチを適用します。
1. カスタムパッチをパッチ名のアルファベット順に `/m2-hotfixes` ディレクトリに適用します。

>[!NOTE]
>
>`ece-tools` パッケージまたはCommerce用クラウド修正プログラム パッケージを更新すると、次回プロジェクトをデプロイする際に必要な最新の修正プログラムが適用されます。または、`ece-patches apply` CLI コマンドを使用して即座に修正プログラムをデプロイし、クラウド環境を再デプロイすることもできます。 デプロイメントプロセス中は [ 必須パッチ ](https://github.com/magento/magento-cloud-patches/tree/develop/patches) をスキップすることはできません。

## 前提条件

{{upgrade-tip}}

Quality Patches Tool は、Cloud Patches for Commerceおよび `ece-tools` パッケージの依存関係です。 最新のパッチを適用するには、[ 最新バージョンの ECE-Tools](../dev-tools/update-package.md) がインストールされている必要があります。 ECE-Tools の最低限必要なバージョンは 2002.1.2 です。

## 使用可能なパッチとステータスの表示

使用可能な個別パッチのリストを表示するには：

```bash
php ./vendor/bin/ece-patches status
```

応答の例：

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

ステータステーブルには、以下のタイプの情報が表示されます。

- **タイプ**:
   - `Optional` – 品質向上パッチツールおよびクラウドパッチパッケージのすべてのパッチは、Adobe CommerceおよびMagento Open Sourceのインストールではオプションです。 クラウドインフラストラクチャー上のAdobe Commerceの場合、すべてのパッチはオプションです。
   - `Required` - Cloud のお客様には、Cloud Patches for Commerce パッケージのすべてのパッチが必要です。
   - `Deprecated` – 個々のパッチは非推奨としてマークされています。適用した場合は元に戻すことをお勧めします。 非推奨パッチを元に戻すと、そのパッチはステータステーブルに表示されなくなります。
   - `Custom` - 「m2-hotfixes」ディレクトリのすべてのパッチ。

- **ステータス**:
   - `Applied` - パッチが適用されました。
   - `Not applied` - パッチが適用されていません。
   - `N/A` – 競合が原因でパッチのステータスを定義できません。

- **詳細**:
   - `Affected components` – 影響を受けるモジュールのリスト。
   - `Required patches` – 必要なパッチ（依存関係）のリスト。
   - `Recommended replacement` – 非推奨のパッチの代わりとして推奨されるパッチ。

## ローカル環境でのパッチの適用

ローカル環境でパッチを手動で適用し、デプロイ前にテストできます。

**ローカル開発環境で個別のパッチを適用するには**:

1. `.magento.env.yaml` ファイルに「QUALITY_variables」PATCHを追加し、その下に必要なパッチをリストします。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. プロジェクトルートから、パッチを適用します。

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   `ece-patches apply` コマンドは、次の順序でパッチを適用します。
   - 必要なパッチ
   - 個別のパッチ（オプション）
   - `/m2-hotfixes` ディレクトリからのカスタムパッチ

1. キャッシュをクリアします。

   ```bash
   php ./bin/magento cache:clean
   ```

1. パッチをテストし、カスタムパッチに必要な変更を加えます。

## リモート環境へのパッチの適用

>[!WARNING]
>
>実稼動環境にデプロイする前に、統合環境またはステージング環境ですべてのパッチをテストすることを強くお勧めします。

**リモート環境にパッチを適用するには**:

1. `QUALITY_PATCHES` 変数を `.magento.env.yaml` ファイルに追加し、その下に必要なパッチをリストします。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >新しいバージョンのAdobe Commerceにアップグレードした後、パッチが新しいバージョンに含まれていない場合は、パッチを再適用する必要があります。

1. 更新された `.magento.env.yaml` ファイルを追加、コミット、プッシュします。

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## カスタムパッチの適用

デプロイ時に、ECE-Tools は、プロジェクトルートの `/m2-hotfixes` ディレクトリに追加されたすべてのAdobeパッチとカスタムパッチを適用します。

>[!NOTE]
>
>すべてのパッチファイル名は、`.patch` 拡張子で終わる必要があります。

**クラウド環境にカスタムパッチを適用してテストするには**:

1. プロジェクトルートに `m2-hotfixes` というディレクトリが存在しない場合は、作成します。

   ```bash
   mkdir m2-hotfixes
   ```

1. パッチファイルを `/m2-hotfixes` ディレクトリにコピーします。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >必ず、実稼動前の環境ですべてのパッチをテストしてください。 クラウドインフラストラクチャー上のAdobe Commerceの場合は、`magento-cloud environment:branch <branch-name>` CLI コマンドを使用してブランチを作成できます。

## カスタムパッチを元に戻す

以前に適用したカスタムパッチを元に戻すかアンインストールするには：

1. `/m2-hotfixes` ディレクトリからパッチファイルを削除します。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >必ず実稼動前の環境でテストしてください。 クラウドインフラストラクチャー上のAdobe Commerceの場合は、`magento-cloud environment:branch <branch-name>` CLI コマンドを使用してブランチを作成できます。

## クラウド以外のプロジェクトへのパッチの適用

Magento Open SourceプロジェクトとAdobe Commerce プロジェクトには、[ 品質向上パッチツール ](https://github.com/magento/quality-patches) を使用します。

## ローカル環境でのパッチの復帰

`ece-patches` CLI を使用すると、ローカル開発環境で以前に適用したすべてのパッチを元に戻すことができます。

適用したすべてのパッチを元に戻すには、次の手順に従います。

```bash
php ./vendor/bin/ece-patches revert
```

このコマンドは、すべてのパッチを次の順序に戻します。

- 適用されたすべてのカスタム パッチを/m2-hotfixes ディレクトリから元に戻します。
- 適用したすべてのオプションの個別パッチを元に戻します。
- 適用されたすべての必要なパッチを元に戻します。

## ログ

品質向上パッチツールは、すべての操作を `<Project_root>/var/log/patch.log` ファイルに記録します。
