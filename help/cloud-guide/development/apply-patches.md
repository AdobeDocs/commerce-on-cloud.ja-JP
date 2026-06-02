---
title: パッチを適用
description: Adobe Commerce on cloud infrastructure プロジェクトでパッチを適用する方法を説明します。
feature: Cloud, Upgrade
exl-id: 923c1e43-45da-450f-bdfc-de84a901400d
TQID: https://experienceleague.adobe.com/SyS-AIRHp0LW7Z4JwZw2FNtbvy9FVzISUID12MjlMrc
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 910
ht-degree: 0%

---

# パッチを適用

Commerce](https://github.com/magento/magento-cloud-patches)および[Quality Patches Tool](https://github.com/magento/quality-patches)の[Cloud Patchesは、インストール済みのAdobe Commerce アプリケーションにパッチを配信します。

- Cloud Patches for Commerce パッケージは、重要な修正を含む必要なパッチを提供します
- 品質パッチは、下位互換性のない変更を含まない[個別のパッチ ](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html#individual-patch)として、オプションの影響の小さい品質の修正を提供します

リリースされたパッチの完全なリストを確認するには、_Commerce Operations Tools Guide_&#x200B;の[使用可能なパッチ ](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)を参照してください。

どちらのパッケージも、すべてのAdobe Commerce バージョンとCloud環境との統合を改善し、重要な修正、オプションの修正、およびカスタム修正の迅速な配信をサポートします。 これらのパッケージを使用して、Commerceで使用可能なすべての個々のパッチに関する一般的な情報を適用、取り消し、表示できます。

>[!TIP]
>
>Magento Open SourceおよびAdobe Commerce プロジェクト用のスタンドアロンパッケージとして、[Quality Patches Tool](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)およびCommerce用Cloud Patchesを使用できます。 クラウド以外のプロジェクトには、品質パッチツールを使用することをお勧めします。

リモート環境に変更をデプロイすると、`ece-tools` パッケージは`magento/magento-cloud-patches`と`magento/quality-patches`を使用して保留中のパッチを確認し、次の順序で自動的に適用します。

1. Cloud Patches for Commerce パッケージに含まれているすべての必要なCommerce パッチを適用します。
1. 品質パッチツールに含まれている、選択したオプションのCommerce パッチを適用します。
1. パッチ名でアルファベット順に`/m2-hotfixes` ディレクトリにカスタムパッチを適用します。

>[!NOTE]
>
>`ece-tools` パッケージまたはCloud Patches for Commerce パッケージを更新すると、次にプロジェクトをデプロイするときに最新の必要なパッチが適用されるか、`ece-patches apply` CLI コマンドを使用してすばやくデプロイし、Cloud Environmentを再デプロイできます。 デプロイメントプロセス中に[必要なパッチ ](https://github.com/magento/magento-cloud-patches/tree/develop/patches)をスキップすることはできません。

## 前提条件

{{upgrade-tip}}

Quality Patches Toolは、CommerceのCloud Patchesおよび`ece-tools` パッケージの依存関係です。 最新のパッチを適用するには、[最新バージョンのECE-Tools](../dev-tools/update-package.md)がインストールされている必要があります。 ECE-Toolsの最小必要バージョンは2002.1.2です。

## 使用可能なパッチとステータスの表示

使用可能な個々のパッチのリストを表示するには：

```bash
php ./vendor/bin/ece-patches status
```

回答サンプル：

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

ステータステーブルには、次のタイプの情報が含まれています。

- **種類**:
   - `Optional` – 品質パッチツールとCloud Patches パッケージのすべてのパッチは、Adobe CommerceおよびMagento Open Sourceのインストールではオプションです。 Adobe Commerce on cloud infrastructureの場合、すべてのパッチはオプションです。
   - `Required` - Cloud Patches for Commerce パッケージのすべてのパッチは、Cloudのお客様に必要です。
   - `Deprecated` – 個別のパッチは非推奨とマークされており、適用した場合は元に戻すことをお勧めします。 非推奨（廃止予定）のパッチを元に戻すと、ステータス テーブルに表示されなくなります。
   - `Custom` - 「m2-hotfixes」ディレクトリのすべてのパッチ。

- **ステータス**:
   - `Applied` - パッチが適用されました。
   - `Not applied` - パッチが適用されていません。
   - `N/A` – 競合のため、パッチのステータスを定義できません。

- **詳細**:
   - `Affected components` – 影響を受けるモジュールのリスト。
   - `Required patches` – 必要なパッチ （依存関係）のリスト。
   - `Recommended replacement` – 非推奨パッチの推奨される代替パッチ。

## ローカル環境でのパッチの適用

パッチをローカル環境で手動で適用し、デプロイする前にテストすることができます。

**ローカル開発環境で個別のパッチを適用するには**:

1. `.magento.env.yaml` ファイルに「QUALITY_PATCHES」変数を追加し、その下に必要なパッチをリストします。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. プロジェクトのルートから、パッチを適用します。

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   `ece-patches apply` コマンドは、次の順序でパッチを適用します。
   - 必要なパッチ
   - オプションの個別パッチ
   - `/m2-hotfixes` ディレクトリのカスタムパッチ

1. キャッシュをクリアします。

   ```bash
   php ./bin/magento cache:clean
   ```

1. パッチをテストし、カスタムパッチに必要な変更を加えます。

## リモート環境でのパッチの適用

>[!WARNING]
>
>実稼動環境にデプロイする前に、統合環境またはステージング環境のすべてのパッチをテストすることを強くお勧めします。

**リモート環境でパッチを適用するには**:

1. `QUALITY_PATCHES`変数を`.magento.env.yaml` ファイルに追加し、その下に必要なパッチを一覧表示します。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >新しいバージョンのAdobe Commerceにアップグレードした後、新しいバージョンにパッチが含まれていない場合は、パッチを再適用する必要があります。

1. 更新した`.magento.env.yaml` ファイルを追加、コミット、プッシュします。

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

デプロイすると、ECE-Toolsは、すべてのAdobe パッチと、プロジェクト ルートの`/m2-hotfixes` ディレクトリに追加したカスタム パッチを適用します。

>[!NOTE]
>
>すべてのパッチファイル名は、`.patch`拡張子で終わる必要があります。

**クラウド環境にカスタムパッチを適用してテストするには**:

1. プロジェクトのルートで、`m2-hotfixes`という名前のディレクトリが存在しない場合は作成します

   ```bash
   mkdir m2-hotfixes
   ```

1. パッチファイルを`/m2-hotfixes` ディレクトリにコピーします。

1. コードの変更を追加、コミット、プッシュします。

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
   >必ず、プリプロダクション環境ですべてのパッチをテストしてください。 Adobe Commerce on cloud infrastructureの場合、`magento-cloud environment:branch <branch-name>` CLI コマンドを使用してブランチを作成できます。

## カスタムパッチを元に戻す

以前に適用したカスタムパッチを元に戻すかアンインストールするには：

1. パッチファイルを`/m2-hotfixes` ディレクトリから削除します。

1. コードの変更を追加、コミット、プッシュします。

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
   >必ずプリプロダクション環境でテストしてください。 Adobe Commerce on cloud infrastructureの場合、`magento-cloud environment:branch <branch-name>` CLI コマンドを使用してブランチを作成できます。

## Cloud以外のプロジェクトへのパッチの適用

Magento Open SourceおよびAdobe Commerce プロジェクトに[品質パッチツール ](https://github.com/magento/quality-patches)を使用します。

## ローカル環境でパッチを元に戻す

`ece-patches` CLIを使用して、以前に適用したすべてのパッチをローカル開発環境で元に戻すことができます。

適用したすべてのパッチを元に戻すには：

```bash
php ./vendor/bin/ece-patches revert
```

このコマンドは、すべてのパッチを次の順序で元に戻します。

- 適用されたすべてのカスタムパッチを/m2-hotfixes ディレクトリから元に戻します。
- 適用されたすべてのオプションの個別パッチを元に戻します。
- 適用されたすべての必須パッチを元に戻します。

## ログ

品質パッチツールは、すべての操作を`<Project_root>/var/log/patch.log` ファイルに記録します。
