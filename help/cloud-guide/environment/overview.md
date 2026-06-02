---
title: 設定ファイルの概要
description: カスタマイズしたAdobe Commerce ストアのデプロイと管理をサポートするためのクラウドインフラストラクチャ環境の設定について説明します。
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: 305380b0-1920-4037-a1db-80e72c6af333
TQID: https://experienceleague.adobe.com/mFjzrTN6R7LC3e9ADnzzulcWAwun4k-g3aCjc9Bo3gQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 280
ht-degree: 0%

---

# 設定ファイルの概要

Adobe Commerce on cloud インフラストラクチャの環境には、Adobe Commerce アプリケーションコードベースとファイル用の包括的なシステムを提供するアプリケーション、サービス、データベースを備えたコンテナが含まれます。

次の設定ファイルを使用して、アプリケーション設定、ルート、ビルドとデプロイのアクション、および通知をプロジェクト環境をサポートするように設定できます。

| 設定 | ファイル名 | 説明 |
| ------------- | -------- | ----------- |
| [&#x200B; アプリケーション &#x200B;](../application/configure-app-yaml.md) | `.magento.app.yaml` | サービス、フック、cron ジョブなど、Adobe Commerceの構築とデプロイの方法を定義します。 |
| [環境](configure-env-yaml.md) | `.magento.env.yaml` | 環境変数を使用して、プロステージングと実稼動環境を含むあらゆる環境でのビルドとデプロイのアクションの管理を一元化します。 |
| [&#x200B; ルート &#x200B;](../routes/routes-yaml.md) | `.magento/routes.yaml` | キャッシュ、リダイレクト、サーバーサイドのインクルードを設定します。 |
| [&#x200B; サービス &#x200B;](../services/services-yaml.md) | `.magento/services.yaml` | Adobe Commerceが使用するサービスを名前とバージョンで定義します。 例えば、このファイルには、MariaDB、PHP拡張機能、Redis、RabbitMQ、ElasticsearchまたはOpenSearchのバージョンが含まれます。 これらの変更をPro プランのステージング環境と実稼動環境にプッシュするには、サポートチケットを開く必要があります。 |
| [PHP設定](../application/php-settings.md#configure-php) | `php.ini` | プロジェクトに追加できるオプションのファイル。 このファイルに含まれる設定は、クラウドインフラストラクチャによって維持される設定に追加されます。 |

{style="table-layout:auto"}

## Pro環境の設定の更新

Adobe Commerce on cloud infrastructure Proのステージング環境および実稼動環境では、ローカル開発環境の多くの設定オプションを更新し、変更を確定してこれらの環境に適用できます。 ただし、次の設定オプションを更新するには、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。

- `.magento/services.yaml` ファイル内のサービスをインストールまたは更新します。
- `.magento.app.yaml` ファイルの`mounts`および`disk` プロパティの設定を変更します。

{{pro-self-service-warning}}
