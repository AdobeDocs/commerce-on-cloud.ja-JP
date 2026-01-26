---
title: '[!DNL ECE-Tools] パッケージ'
description: パッケージの概要と、パッケ  [!DNL ECE-Tools]  ジを使用してAdobe Commerceを管理およびデプロイする方法について説明します。
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# ECE-Tools パッケージ

[!DNL ECE-Tools] パッケージは、[!DNL Commerce] アプリケーションを管理およびデプロイするために設計された一連のスクリプトとツールです。 `ece-tools` パッケージを使用すると、cron ジョブの管理、プロジェクト設定の検証、Adobeのパッチやホットフィックスの適用など、多くのプロセスを簡単に実行できます。 [GitHub のオープンソース  [!DNL ECE-Tools]  コードリポジトリ ](https://github.com/magento/ece-tools) を表示し、投稿できます。

{{ece-tools-package}}

`ece-tools` パッケージは、バージョン 2.1.4 以降のAdobe Commerceと互換性があり、コードの管理とプロジェクトの自動ビルドおよびデプロイに役立つ、クラウドインフラストラクチャコマンド上のスクリプトおよびAdobe Commerceが含まれています。

使用可能な `ece-tools` コマンドを次に示します。

```bash
php ./vendor/bin/ece-tools list
```

## ビルドとデプロイ

`ece-tools` パッケージには、クラウドインフラストラクチャアプリケーション上でAdobe Commerceを起動する際の、ビルド、デプロイ、およびデプロイ後の段階で操作を実行するコマンドが含まれています。 例えば、`php ./vendor/bin/ece-tools build` コマンドは、アプリケーションビルドプロセスを開始します。

デフォルトでは、これらの `ece-tools` コマンドは [ 設定ファイルの ](../application/hooks-property.md)hooks プロパティ `.magento.app.yaml` にあります。

## Docker 設定ジェネレーター

`ece-tools` パッケージには、[magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker) パッケージの依存関係が含まれています。このパッケージは、クラウドインフラストラクチャ上でAdobe Commerce用の Docker 開発環境を起動するための Docker イメージの機能と設定ファイルを提供します。 また、Cloud Docker for Commerceをスタンドアロンパッケージとして実行することもできます。 [Docker 開発 ](../dev-tools/cloud-docker.md) を参照してください。

## サービス、ルート、変数

`ece-tools` パッケージを使用すると、任意のクラウド環境で使用される Base64 でエンコードされた [ クラウド変数 ](../environment/variables-cloud.md) に関する詳細情報を表示できます。 次のコマンドは、すべてのサービス、ルート、変数を表示します。

```bash
php ./vendor/bin/ece-tools env:config:show
```

特定の情報セットを表示するには、次の形式を使用します。

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - `MAGENTO_CLOUD_RELATIONSHIPS` ファイルで定義された `services.yaml` 環境変数の関係データを表示します。
- `routes` - `MAGENTO_CLOUD_ROUTES` 環境変数を使用して、プロジェクトに設定されているルートを表示します。
- `variables` - `MAGENTO_CLOUD_VARIABLES` 環境変数を使用して、プロジェクトの設定済み変数を表示します。

`services` オプションの出力例：

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## 環境設定の確認

プロジェクトの設定を評価するのに役立つ一連の検証コマンドを使用できます。 各ウィザードのコマンドの詳細については、[ 展開の最適化 ](../deploy/smart-wizards.md) のセクションの _スマート ウィザード_ を参照してください。 `wizard:ideal-state` コマンドは、ビルドフェーズで自動的に実行されます。 プロジェクトの理想的な状態を確認するには：

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>`wizard:ideal-state` コマンドは、リモート クラウド環境で実行する必要があります。 ローカル開発環境で実行すると、常にコマンドが `The configured state is not ideal` エラーを返します。

サンプル出力：

```
Ideal state is configured
```

[ece-tools のリリースノート ](../release-notes/cloud-tools-suite.md) を参照してください。

## Adobeのパッチとカスタムパッチ

`ece-tools` パッケージには、[magento/magento-cloud-patches](https://github.com/magento/magento-cloud-patches) パッケージの依存関係が含まれています。このパッケージは、Adobeのパッチとホットフィックスを提供し、Adobe Commerceのすべてのバージョンとクラウド環境の統合を強化し、重要な修正の迅速な配信をサポートします。 「」には、クラウドインフラストラクチャプロジェクト上のAdobe Commerceに追加するカスタムパッチも提供されます。 [ パッチの適用 ](../development/apply-patches.md) を参照してください。

