---
title: 開発の概要
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceとのローカル開発の準備。
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 開発の概要

クラウドインフラストラクチャー上のAdobe Commerceのリモート環境は、すべてのスターター環境とすべての Pro 統合、ステージングおよび実稼動環境を含む **読み取り専用** です。 ローカル開発環境では、コードを記述してテストしてから統合環境にプッシュし、ステージング環境と実稼動環境へのさらなるテストとデプロイメントを行うことができます。

ローカルワークスペースを準備する前に、[ 資格情報 ](../../get-started/prepare-workspace.md) が揃っていることを確認します。 [Cloud Docker for Commerce](#docker-environment) を使用しない限り、ローカル開発には PHP および Composer のインストールが必要です。

## 必須パッケージ

クラウドインフラストラクチャー上のAdobe Commerceでは、Composer を使用してプロジェクトの依存関係やアップグレードを管理します。 ローカル開発を保つには、Cloud プロジェクトと互換性のある PHP および Composer バージョンをインストールする必要があります。 例えば、[!DNL Commerce] 2.4.7 クラウドテンプレートを使用している場合、[`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.7/.magento.app.yaml) 設定ファイルで **PHP 8.3** と **Composer 2.7.2** が使用されていることがわかります。

Composer は、プロジェクトに必要なライブラリおよび依存関係を `vendor` ディレクトリにインストールします。 次の必須の Composer ファイルは、プロジェクトのルート ディレクトリにあります。

- `composer.json` - `composer.json` ファイルを使用して、製品のインストールとアップグレードを管理します。
- `composer.lock` - `composer.lock` ファイルには、プロジェクトの依存ツリー内のすべてのパッケージのすべての要件のバージョン制約を満たす、正確なバージョンの依存のセットが格納されます。

**共通コマンド：**

| コマンド | 説明 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | `composer.json` ファイルに反映される依存関係の最新バージョンへの更新。 これにより、`composer.lock` ファイルが更新されます。 |
| `composer install` | `composer.lock` ファイルを読み取って依存関係をダウンロードします。 `composer.lock` の最新コピーをプロジェクトリポジトリに保存することをお勧めします。 |

{style="table-layout:auto"}

更新されたコードを追加、コミット、プッシュすると、デプロイメントプロセスにより、[ ビルドフェーズ ](../deploy/process.md#build-phase-build-phase) 中に `composer install` コマンドが自動的に実行されます。

### クラウドメタパッケージ

クラウドインフラストラクチャー上のAdobe Commerceは、`magento/product-enterprise-edition` を必要とするメタパッケージを使用します。 最新バージョンのCommerceの最新のアップデートを取得するには、次の制約構文を使用します。

```text
>=current_version <next_version
```

例えば、最新のAdobe Commerce バージョン 2.4.7 を使用するには、`composer.json` ファイルで、`2.4.7` を「現在」のバージョンとして設定し、`2.4.8` を「次」のバージョンとして設定します。

```text
"magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
```

このメタパッケージの主なパッケージは以下の通りです。

- **vendor/magento/ece-tools** – この `ece-tools` パッケージはAdobe Commerce バージョン 2.1.4 以降と互換性があり、クラウドインフラストラクチャプロジェクトでのAdobe Commerceの管理に使用できる豊富な機能セットを提供します。 コードの管理とプロジェクトの自動ビルドおよびデプロイに役立つスクリプトやAdobe Commerce on cloud infrastructure コマンドが含まれています。 [`ece-tools` パッケージの概要を参照してください ](../dev-tools/package-overview.md)。
- **vendor/magento/product-enterprise-edition**：このメタパッケージには、モジュール、フレームワーク、テーマなどのアプリケーションコンポーネントが必要です。
- **vendor/fastly2/magento2** – このモジュールは、Fastly CDN と、Pro ステージング環境および実稼動環境とスターター実稼動環境用のサービスを管理します。 詳しくは、[Fastly サービス ](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2) を参照してください。
- **vendor/magento/module-paypal-on-boarding** – このモジュールは、PayPal マーチャントアカウントに接続することで、PayPal 支払いゲートウェイチェックアウトを提供します。 [PayPal オンボーディングツール ](../store/paypal.md) を参照してください。

>[!TIP]
>
>依存関係とサードパーティライセンスの一覧については、_Commerce リリースノートの [Adobe Commerce用クラウドパッケージ_ を参照してください ](/help/cloud-guide/release-notes/cloud-packages.md)

## Docker 環境

Cloud Docker for Commerce ツールを使用して、クラウドインフラストラクチャ上のAdobe Commerceの実稼動環境およびローカル開発環境をエミュレートできます。 Cloud Docker for Commerceでは、PHP と Composer をローカルにインストールする必要はありません。

- Adobe Developer サイトでの ](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)0}Cloud Docker とのローカル開発[
- [Docker アーキテクチャと一般的なコマンド](../dev-tools/cloud-docker.md)
- [Cloud Docker リリースノート](../release-notes/cloud-docker.md)

>[!TIP]
>
>Cloud Infrastructure 上のAdobe Commerceで Git ベースのホスティングサービスを使用する方法について詳しくは、「[ 統合 ](../integrations/overview.md)」を参照してください。
