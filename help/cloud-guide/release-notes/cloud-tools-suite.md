---
title: Cloud Tools Suite のリリースノート
description: Adobe Commerce用 Cloud Tools スイートの最新の改善点について説明します。
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: fdd3c4a8c33e5c44fa258d2ca87cd03911ca0b92
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 1%

---

# Commerce Cloud ツールスイートのリリースノート

このリリース情報では、Cloud Platform でのAdobe Commerceのインストールとアップグレードのデプロイと管理を目的としたCommerce パッケージの Cloud Tools Suite の最新の改善点について詳しく説明します。

| リリースノート | バージョン | 説明 | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ece-tools パッケージ &#x200B;](ece-tools-package.md) | 2002.2.9 | クラウドプロジェクトの管理とデプロイを行うために設計された一連のスクリプトとツール | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.9) |
| [Commerceのクラウドパッチ &#x200B;](cloud-patches.md) | 1.1.12 | すべてのAdobe Commerce バージョンとクラウド環境の統合を改善するパッチセット。 このパッケージには、Adobe Commerceのパッチと、`ece-tools` を使用してデプロイする際に適用される使用可能なホットフィックスが含まれています | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.12) |
| [Commerce用の Cloud Docker](cloud-docker.md) | 1.4.6 | Adobe Commerceをローカルクラウド環境にデプロイするための Docker イメージの機能および設定ファイル | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.6) |
| [Commerceのクラウドコンポーネント &#x200B;](cloud-components.md) | 1.1.3 | クラウドインフラストラクチャにデプロイされたサイト向けに拡張されたAdobe Commerce コア機能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.3) |

ECE-Tools 2002.1.0 以降に更新すると、`ece-tools` パッケージの依存関係である他のパッケージの最新バージョンに自動的に更新されます。 依存関係のリストについては、[&#x200B; クラウドメタパッケージ &#x200B;](../development/overview.md#cloud-metapackage) を参照してください。
