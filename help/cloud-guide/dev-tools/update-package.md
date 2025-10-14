---
title: ECE-Tools パッケージの更新
description: ECE-Tools パッケージをアップグレードして、クラウドインフラストラクチャ上のAdobe Commerceに適用された最新の修正と機能を活用する方法について説明します。
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# ECE-Tools パッケージの更新

`ece-tools` パッケージのアップデートは、`ece-tools` の依存関係である他の [Cloud Tools Suite for Commerce パッケージ &#x200B;](../release-notes/cloud-tools-suite.md) もアップデートします。 そのため、`ece-tools` パッケージをサポートするバージョンのAdobe Commerce on cloud infrastructure を使用する必要があります。

{{ece-tools-package}}

**前提条件**:

- `ece-tools` を更新する前に、[Commerce Cloud Tools Suite リリースノート &#x200B;](../release-notes/cloud-tools-suite.md) を確認してください。
- `ece-tools` 2002.0.22 以前から 2002.1.0 にアップデートする場合は、[&#x200B; 後方互換性のない変更点 &#x200B;](../release-notes/backward-incompatible-changes.md) を確認し、クラウドインフラストラクチャプロジェクトのAdobe Commerceに必要な変更を加えます。
- [&#x200B; アップグレードとパッチ &#x200B;](../development/commerce-version.md#upgrade-from-older-versions) を確認して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceと互換性のある ECE-Tools のバージョンを判断します。

{{upgrade-tip}}

**`ece-tools` パッケージを更新するには**:

1. ローカル ワークステーションで、Composer を使用して更新を実行します。

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >バージョン 2002.0.8 以降 `ece-tools` 更新できない場合は、[ECE-Tools パッケージを使用するようにプロジェクトをアップグレードする &#x200B;](install-package.md) を参照してください。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. テスト検証後、このブランチを統合ブランチに結合します。
