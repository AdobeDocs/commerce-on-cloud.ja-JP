---
title: 設定ファイルの概要
description: カスタマイズしたAdobe Commerce ストアのデプロイと管理をサポートするためのクラウドインフラストラクチャ環境の設定について説明します。
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 設定ファイルの概要

クラウドインフラストラクチャー上のAdobe Commerceの環境には、Adobe Commerce アプリケーションのコードベースとファイルに対して包括的なシステムを提供するための、アプリケーション、サービス、データベースを備えたコンテナが含まれます。

以下の設定ファイルを使用して、アプリケーション設定、ルート、ビルドおよびデプロイのアクション、プロジェクト環境をサポートする通知を設定できます。

| 設定 | ファイル名 | 説明 |
| ------------- | -------- | ----------- |
| [ 適用 ](../application/configure-app-yaml.md) | `.magento.app.yaml` | サービス、フック、cron ジョブなど、Adobe Commerceのビルドおよびデプロイ方法を定義します。 |
| [ 環境 ](configure-env-yaml.md) | `.magento.env.yaml` | 環境変数を使用して、ステージング環境および実稼動環境を含むすべての環境で、ビルドアクションおよびデプロイアクションの管理を一元化します。 |
| [ ルート ](../routes/routes-yaml.md) | `.magento/routes.yaml` | キャッシュ、リダイレクト、サーバーサイドインクルードを設定します。 |
| [ サービス ](../services/services-yaml.md) | `.magento/services.yaml` | Adobe Commerceが使用するサービスを名前とバージョンで定義します。 例えば、このファイルには、MariaDB、PHP Extensions、Redis、RabbitMQ、およびElasticsearchまたは OpenSearch のバージョンが含まれている場合があります。 これらの変更を Pro プランのステージング環境および実稼動環境にプッシュするには、サポートチケットを開く必要があります。 |
| [PHP 設定 ](../application/php-settings.md#configure-php) | `php.ini` | プロジェクトに追加できるオプションのファイル。 このファイルに含まれる設定は、クラウドインフラストラクチャで管理される設定に追加されます。 |

{style="table-layout:auto"}

## Pro 環境の設定アップデート

Cloud infrastructure Pro 上のAdobe Commerceのステージング環境および実稼動環境では、ローカル開発環境の多くの設定オプションを更新し、変更内容をコミットしてこれらの環境に適用することができます。 ただし、次の設定オプションを更新するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) する必要があります。

- `.magento/services.yaml` ファイルのサービスをインストールまたは更新します。
- `.magento.app.yaml` ファイルの `mounts` および `disk` プロパティの設定を変更します。

{{pro-self-service-warning}}
