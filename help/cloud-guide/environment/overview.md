---
title: 設定ファイルの概要
description: カスタマイズしたAdobe Commerce ストアのデプロイと管理をサポートするためのクラウドインフラストラクチャ環境の設定について説明します。
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: 305380b0-1920-4037-a1db-80e72c6af333
TQID: 'https://experienceleague.adobe.com/mFjzrTN6R7LC3e9ADnzzulcWAwun4k-g3aCjc9Bo3gQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: 00451af3-7b97-5414-9992-3a6c269e413f
    internal-label: Paas
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
    internal-label: IaaS
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '286'
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
| [&#x200B; サービス &#x200B;](../services/services-yaml.md) | `.magento/services.yaml` | Adobe Commerceが使用するサービスを名前とバージョンで定義します。 例えば、このファイルには、MariaDB、PHP拡張機能、RedisまたはValkey、RabbitMQ、ElasticsearchまたはOpenSearchのバージョンが含まれます。 これらの変更をPro プランのステージング環境と実稼動環境にプッシュするには、サポートチケットを開く必要があります。 |
| [PHP設定](../application/php-settings.md#configure-php) | `php.ini` | プロジェクトに追加できるオプションのファイル。 このファイルに含まれる設定は、クラウドインフラストラクチャによって維持される設定に追加されます。 |

{style="table-layout:auto"}

## Pro環境の設定の更新

Adobe Commerce on cloud infrastructure Proのステージング環境および実稼動環境では、ローカル開発環境の多くの設定オプションを更新し、変更を確定してこれらの環境に適用できます。 ただし、次の設定オプションを更新するには、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)を送信する必要があります。

- `.magento/services.yaml` ファイル内のサービスをインストールまたは更新します。
- `.magento.app.yaml` ファイルの`mounts`および`disk` プロパティの設定を変更します。

{{pro-self-service-warning}}
