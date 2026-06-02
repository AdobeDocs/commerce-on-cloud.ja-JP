---
title: ECE-Tools パッケージの更新
description: Adobe Commerce on cloud infrastructureに適用される最新の修正と機能を活用するために、ECE-Tools パッケージをアップグレードする方法を説明します。
feature: Cloud, Upgrade
exl-id: acb4fd0d-6ffb-4094-8dd4-83bb7735d64f
TQID: https://experienceleague.adobe.com/WqcVa0BJN0ot6yg2unhzeEB8nkjKXM7o26tOwmrg5mU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 172
ht-degree: 0%

---

# ECE-Tools パッケージの更新

`ece-tools` パッケージを更新すると、`ece-tools`の依存関係である他の[Cloud Tools Suite for Commerce パッケージ &#x200B;](../release-notes/cloud-tools-suite.md)も更新されます。 したがって、`ece-tools` パッケージをサポートするAdobe Commerce on cloud infrastructureのバージョンを使用する必要があります。

{{ece-tools-package}}

**前提条件**:

- `ece-tools`を更新する前に、[Cloud Tools Suite for Commerce リリースノート &#x200B;](../release-notes/cloud-tools-suite.md)を確認してください。
- 2002.0.22以前の`ece-tools`から2002.1.0にアップデートする場合は、[後方互換性のない変更](../release-notes/backward-incompatible-changes.md)を確認し、Adobe Commerce on cloud infrastructure プロジェクトに必要な変更を加えます。
- [&#x200B; アップグレードとパッチ &#x200B;](../development/commerce-version.md#upgrade-from-older-versions)を確認して、Adobe Commerce on cloud インフラストラクチャプロジェクトと互換性のあるECE-Tools バージョンを判断します。

{{upgrade-tip}}

**`ece-tools` パッケージ**&#x200B;を更新するには：

1. ローカルワークステーションで、Composerを使用して更新を実行します。

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >バージョン 2002.0.8以降を更新できない場合は、[&#x200B; プロジェクトをアップグレードしてECE-Tools パッケージ &#x200B;](install-package.md)を使用してください。`ece-tools`

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. テスト検証後、このブランチを統合ブランチにマージします。
