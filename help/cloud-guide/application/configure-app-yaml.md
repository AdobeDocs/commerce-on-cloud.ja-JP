---
title: アプリケーションのデプロイメントの設定
description: ' [!DNL Commerce]  アプリケーションのビルドとクラウド環境へのデプロイ方法を制御するアプリケーション設定ファイルのプロパティを設定する方法について説明します。'
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 190
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
