---
title: アプリケーションのデプロイメントの設定
description: '[!DNL Commerce] アプリケーションのビルドとクラウド環境へのデプロイ方法を制御するアプリケーション設定ファイルで、プロパティを設定する方法について説明します。'
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: 'https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
subfeature_v2:
  - id: adedf3b3-e153-47a3-ae73-b5d65067b544
    internal-label: Build system
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%
---
# アプリケーションのデプロイメントの設定

`.magento.app.yaml` ファイルは、アプリケーションのビルドおよびデプロイ方法を制御します。 Adobe Commerce on cloud infrastructureは、プロジェクトごとに複数のアプリケーションをサポートしていますが、通常、プロジェクトには、リポジトリのルートに`.magento.app.yaml` ファイルが含まれた1つのアプリケーションがあります。

`.magento.app.yaml`には多くのデフォルト値があります。[ サンプル `.magento.app.yaml` ファイル ](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml)を参照してください。 インストール済みのバージョンについては、常に`.magento.app.yaml`を確認してください。 このファイルは、Adobe Commerce on cloud infrastructureのバージョンによって異なる場合があります。

`.magento.app.yaml` ファイルを使用して、次の設定値を定義します。

- [ プロパティ ](properties.md) - アプリケーションインスタンスのプロパティ値を定義します。
- [変数プロパティ ](variables-property.md) - [!DNL Commerce] アプリケーション バージョンに必要な環境変数を確認します。
- [PHP設定](php-settings.md) - ランタイム PHP オプションを設定します。
- [静的ファイルのキャッシュを設定](set-cache.md) - メディアと静的ファイルのキャッシュ TTLを設定します。

>[!NOTE]
>
>`.magento.app.yaml` ファイルはローカルまたはGit リポジトリで管理されます。 設定は、デプロイメントとビルドプロセスの目的でのみ読み取られ、デプロイメントが完了した後に削除されるので、サーバーで見つかりません。

