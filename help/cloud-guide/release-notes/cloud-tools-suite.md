---
title: Cloud Tools Suiteのリリースノート
description: Adobe Commerce向けCloud Tools スイートの最新の改善点について説明します。
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Commerce Cloud Tools Suiteのリリースノート

このリリース情報では、Cloud PlatformでのAdobe Commerceのインストールとアップグレードのデプロイと管理を目的としたCloud Tools Suite for Commerce パッケージの最新の機能強化について説明します。

| リリースノート | バージョン | 説明 | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ece-tools パッケージ ](ece-tools-package.md) | 2002.2.11 | クラウドプロジェクトを管理およびデプロイするために設計された一連のスクリプトとツール | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| Commerceの[Cloud パッチ ](cloud-patches.md) | 1.1.14 | すべてのAdobe Commerce バージョンとCloud環境の連携を改善する一連のパッチ。 このパッケージには、`ece-tools`を使用してデプロイする際に適用されるAdobe Commerce パッチと利用可能なホットフィックスが含まれています | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.14) |
| [Commerce用Cloud Docker](cloud-docker.md) | 1.4.8 | Adobe Commerceをローカルクラウド環境にデプロイするためのDocker イメージの機能および設定ファイル | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| [Commerceのクラウドコンポーネント ](cloud-components.md) | 1.1.4 | クラウドインフラストラクチャにデプロイされたサイト用の拡張Adobe Commerce コア機能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

ECE-Tools 2002.1.0以降に更新すると、`ece-tools` パッケージの依存関係である他のパッケージの最新バージョンに自動的に更新されます。 依存関係のリストについては、[Cloud メタパッケージ ](../development/overview.md#cloud-metapackage)を参照してください。
