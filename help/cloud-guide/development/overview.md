---
title: 開発概要
description: Adobe Commerceクラウド基盤プロジェクトでローカル開発に備える。
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
TQID: https://experienceleague.adobe.com/VrOBK4bPYTxzRz10ZaHYDOdISw0lSl6WPAHcnEI1J-w
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 580
ht-degree: 0%

---

# 開発概要

クラウドインフラストラクチャ上のAdobe Commerce リモート環境は&#x200B;**読み取り専用**&#x200B;です。これには、すべてのStarter環境とすべてのPro統合、ステージング、実稼動環境が含まれます。 ローカル開発環境では、統合環境にプッシュする前にコードを記述してテストし、ステージング環境と実稼動環境にさらにテストとデプロイメントを行うことができます。

ローカル ワークスペースを準備する前に、[資格情報](../../get-started/prepare-workspace.md)があることを確認してください。 [Cloud Docker for Commerce](#docker-environment)を使用しない限り、ローカル開発にはPHPとComposerのインストールが必要です。

## 必要なパッケージ

Adobe Commerce on cloud infrastructureでは、Composerを使用してプロジェクトの依存関係とアップグレードを管理します。 ローカル開発するには、Cloud プロジェクトと互換性のあるPHPとComposerのバージョンをインストールする必要があります。 例えば、[!DNL Commerce] 2.4.8 クラウドテンプレートを使用している場合、[`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml)設定ファイルで&#x200B;**PHP 8.4**&#x200B;と&#x200B;**Composer 2.8.4**&#x200B;が使用されていることがわかります。

Composerは、プロジェクトに必要なライブラリと依存関係を`vendor` ディレクトリにインストールします。 次の必要なComposer ファイルは、プロジェクトのルートディレクトリにあります。

- `composer.json` - `composer.json` ファイルを使用して、製品のインストールとアップグレードを管理します。
- `composer.lock` - `composer.lock` ファイルには、プロジェクトの依存関係ツリー内の各パッケージの要件のバージョン制約を満たす、正確なバージョン依存関係のセットが格納されます。

**共通コマンド：**

| コマンド | 説明 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | `composer.json` ファイルに反映されている依存関係の最新バージョンを更新します。 これにより、`composer.lock` ファイルが更新されます。 |
| `composer install` | 依存関係をダウンロードするために`composer.lock` ファイルを読み取ります。 プロジェクトリポジトリに`composer.lock`の最新コピーを保持することをお勧めします。 |

{style="table-layout:auto"}

更新されたコードを追加、コミット、プッシュすると、デプロイメントプロセスは[&#x200B; ビルド段階](../deploy/process.md#build-phase-build-phase)中に`composer install` コマンドを自動的に実行します。

### クラウドメタパッケージ

Adobe Commerce on cloud infrastructureは、`magento/product-enterprise-edition`を必要とするメタパッケージを使用します。 最新バージョンのCommerceの最新のアップデートを入手するには、次の制約の構文を使用します。

```text
>=current_version <next_version
```

例えば、最新のAdobe Commerce バージョン 2.4.9を使用するには、`2.4.8`を「現在の」バージョンとして、`2.4.9`を`composer.json` ファイルの「次の」バージョンとして設定します。

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

このメタパッケージの主なパッケージは次のとおりです。

- **vendor/magento/ece-tools** - `ece-tools` パッケージは、Adobe Commerce バージョン 2.1.4以降と互換性があり、Adobe Commerce on cloud infrastructure プロジェクトの管理に使用できる豊富な機能を提供します。 コードの管理やプロジェクトの自動ビルドとデプロイに役立つスクリプトとAdobe Commerce on cloud infrastructure コマンドが含まれています。 [`ece-tools` パッケージの概要](../dev-tools/package-overview.md)を参照してください。
- **vendor/magento/product-enterprise-edition**：このメタパッケージには、モジュール、フレームワーク、テーマなどのアプリケーションコンポーネントが必要です。
- **vendor/fastly2/magento2**：このモジュールは、プロステージング環境および実稼動環境とスターター実稼動環境のFastly CDNとサービスを管理します。 [Fastly サービス &#x200B;](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2)を参照してください。
- **vendor/magento/module-paypal-on-boarding**：このモジュールは、PayPal加盟店アカウントに接続することで、PayPal支払いゲートウェイのチェックアウトを提供します。 [PayPal オンボーディングツール &#x200B;](../store/paypal.md)を参照してください。
- **vendor/aem/rum**：このモジュールは、[運用上のテレメトリ &#x200B;](../monitor/operational-telemetry.md) データ収集ツールを管理します。

>[!TIP]
>
>依存関係とサードパーティライセンスの一覧については、_Adobe Commerce リリースノート_&#x200B;の[Commerce用クラウドパッケージ &#x200B;](/help/cloud-guide/release-notes/cloud-packages.md)を参照してください。

## Docker環境

Cloud Docker for Commerce ツールを使用して、Adobe Commerce on cloud インフラストラクチャの実稼動環境と開発環境をローカル開発用にエミュレートできます。 Cloud Docker for Commerceでは、PHPとComposerをローカルにインストールする必要はありません。

- Adobe Developer サイトの[Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)とのローカル開発
- [Docker アーキテクチャと共通コマンド](../dev-tools/cloud-docker.md)
- [Cloud Docker リリースノート](../release-notes/cloud-docker.md)

>[!TIP]
>
>Adobe Commerce クラウドインフラストラクチャでのGit ベースのホスティングサービスの使用について詳しくは、[統合](../integrations/overview.md)を参照してください。
