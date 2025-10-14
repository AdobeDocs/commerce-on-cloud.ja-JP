---
title: '[!DNL ECE-Tools] パッケージ'
description: パッケージの概要と、パッケ  [!DNL ECE-Tools]  ジを使用してAdobe Commerceを管理およびデプロイする方法について説明します。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# ECE-Tools パッケージ

[!DNL ECE-Tools] パッケージは、[!DNL Commerce] アプリケーションを管理およびデプロイするために設計された一連のスクリプトとツールです。 `ece-tools` パッケージを使用すると、cron ジョブの管理、プロジェクト設定の検証、Adobeパッチやホットフィックスの適用など、多くのプロセスを簡単に実行できます。 [GitHub のオープンソース  [!DNL ECE-Tools]  コードリポジトリ ][ece-repo] を表示し、投稿できます。

{{ece-tools-package}}

`ece-tools` パッケージは、バージョン 2.1.4 以降のAdobe Commerceと互換性があり、コードの管理とプロジェクトの自動ビルドおよびデプロイに役立つ、クラウドインフラストラクチャコマンド上のスクリプトおよびAdobe Commerceが含まれています。

使用可能な `ece-tools` コマンドを次に示します。

```bash
php ./vendor/bin/ece-tools list
```

## ビルドとデプロイ

`ece-tools` パッケージには、クラウドインフラストラクチャアプリケーション上でAdobe Commerceを起動する際の、ビルド、デプロイ、およびデプロイ後の段階で操作を実行するコマンドが含まれています。 例えば、`php ./vendor/bin/ece-tools build` コマンドは、アプリケーションビルドプロセスを開始します。

デフォルトでは、これらの `ece-tools` コマンドは `.magento.app.yaml` 設定ファイルの [hooks プロパティ &#x200B;](../application/hooks-property.md) にあります。

## Docker 設定ジェネレーター

`ece-tools` パッケージには、[magento/magento-cloud-docker] パッケージの依存関係が含まれています。このパッケージは、クラウドインフラストラクチャ上でAdobe Commerce用の Docker 開発環境を起動するための Docker イメージの機能と設定ファイルを提供します。 また、Cloud Docker for Commerceをスタンドアロンパッケージとして実行することもできます。 [Docker 開発 &#x200B;](../dev-tools/cloud-docker.md) を参照してください。

## サービス、ルート、変数

`ece-tools` パッケージを使用すると、任意のクラウド環境で使用される Base64 でエンコードされた [&#x200B; クラウド変数 &#x200B;](../environment/variables-cloud.md) に関する詳細情報を表示できます。 次のコマンドは、すべてのサービス、ルート、変数を表示します。

```bash
php ./vendor/bin/ece-tools env:config:show
```

特定の情報セットを表示するには、次の形式を使用します。

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - `services.yaml` ファイルで定義された `MAGENTO_CLOUD_RELATIONSHIPS` 環境変数の関係データを表示します。
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

プロジェクトの設定を評価するのに役立つ一連の検証コマンドを使用できます。 各ウィザードのコマンドの詳細については、_展開の最適化_ のセクションの [&#x200B; スマート ウィザード &#x200B;](../deploy/smart-wizards.md) を参照してください。 `wizard:ideal-state` コマンドは、ビルドフェーズで自動的に実行されます。 プロジェクトの理想的な状態を確認するには：

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

[ece-tools のリリースノート &#x200B;](../release-notes/cloud-tools-suite.md) を参照してください。

## Adobeパッチとカスタムパッチ

`ece-tools` パッケージには、[magento/magento-cloud-patches] パッケージの依存関係が含まれています。このパッケージは、クラウド環境とのすべてのAdobe Commerce バージョンのAdobeを向上させる統合パッチおよびホットフィックスを提供し、重要な修正の迅速な配信をサポートします。 「」には、クラウドインフラストラクチャプロジェクト上のAdobe Commerceに追加するカスタムパッチも提供されます。 [&#x200B; パッチの適用 &#x200B;](../development/apply-patches.md) を参照してください。

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
