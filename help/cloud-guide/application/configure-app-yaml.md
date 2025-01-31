---
title: アプリケーションのデプロイメントの設定
description: アプリケーションによるクラウド環境のビルドおよびデプロイ方法を制御する  [!DNL Commerce]  プリケーション設定ファイルのプロパティを設定する方法について説明します。
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# アプリケーションのデプロイメントの設定

`.magento.app.yaml` ファイルは、アプリケーションのビルドおよびデプロイ方法を制御します。 クラウドインフラストラクチャー上のAdobe Commerceでは、プロジェクトごとに複数のアプリケーションをサポートしていますが、通常、プロジェクトにはリポジトリのルートに `.magento.app.yaml` ファイルを持つ単一のアプリケーションがあります。

`.magento.app.yaml` には多くのデフォルト値があります。[ サンプル `.magento.app.yaml` ファイル ](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml) を参照してください。 インストールしたバージョンの `.magento.app.yaml` を常に確認してください。 このファイルは、クラウドインフラストラクチャー上のAdobe Commerceのバージョンによって異なる場合があります。

`.magento.app.yaml` ファイルを使用して、次の設定値を定義します。

- [ プロパティ ](properties.md) - アプリケーション・インスタンスのプロパティ値を定義します。
- [Variables プロパティ ](variables-property.md) - [!DNL Commerce] アプリケーション・バージョンに必要な環境変数を確認します。
- [PHP 設定 ](php-settings.md) – 実行時の PHP オプションを設定します。
- [ 静的ファイルのキャッシュの設定 ](set-cache.md) - メディアと静的ファイルのキャッシュ TTL を設定します。

>[!NOTE]
>
>`.magento.app.yaml` ファイルは、ローカルまたは Git リポジトリで管理されます。 設定は、デプロイメントおよびビルドプロセスの目的でのみ読み取られ、デプロイメントが完了すると削除されるので、サーバー上では見つかりません。
