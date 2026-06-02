---
title: '[!DNL ECE-Tools] パッケージ'
description: ' [!DNL ECE-Tools]  パッケージと、Adobe Commerceの管理とデプロイにどのように役立つかについて説明します。'
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
TQID: https://experienceleague.adobe.com/YMuy2Ta0Ylkewxb2EhQgpZG8WW8bG4kFzrCXm0A7rX0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 450
ht-degree: 0%

---

# ECE-Tools パッケージ

[!DNL ECE-Tools] パッケージは、[!DNL Commerce] アプリケーションを管理およびデプロイするために設計されたスクリプトとツールのセットです。 `ece-tools` パッケージは、cron ジョブの管理、プロジェクト設定の検証、Adobeのパッチとホットフィックスの適用など、多くのプロセスを簡素化します。 GitHub[&#128279;](https://github.com/magento/ece-tools)で オープンソース  [!DNL ECE-Tools]  コードリポジトリを表示して貢献できます。

{{ece-tools-package}}

`ece-tools` パッケージは、バージョン 2.1.4以降のAdobe Commerceと互換性があり、コードの管理とプロジェクトの自動ビルドおよびデプロイに役立つスクリプトとAdobe Commerce on cloud infrastructure コマンドが含まれています。

使用可能な`ece-tools` コマンドを次に示します。

```bash
php ./vendor/bin/ece-tools list
```

## ビルドとデプロイ

`ece-tools` パッケージには、Adobe Commerce on cloud infrastructure アプリケーションを起動するビルド、デプロイ、デプロイ後の各ステージの操作を実行するコマンドが含まれています。 例えば、`php ./vendor/bin/ece-tools build` コマンドは、アプリケーションのビルド プロセスを開始します。

デフォルトでは、これらの`ece-tools` コマンドは`.magento.app.yaml`設定ファイルの[hook プロパティ &#x200B;](../application/hooks-property.md)にあります。

## Docker設定ジェネレーター

`ece-tools` パッケージには、[magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker) パッケージの依存関係が含まれています。このパッケージは、Docker イメージでAdobe CommerceのDocker開発環境をクラウドインフラストラクチャ上で起動するための機能と設定ファイルを提供します。 また、Commerce用Cloud Dockerをスタンドアロンパッケージとして実行することもできます。 [Docker開発](../dev-tools/cloud-docker.md)を参照してください。

## サービス、ルート、変数

`ece-tools` パッケージを使用すると、任意のCloud環境で使用されているBase64 エンコードされた[Cloud variables](../environment/variables-cloud.md)に関する詳細情報を表示できます。 次のコマンドは、すべてのサービス、ルートおよび変数を示します。

```bash
php ./vendor/bin/ece-tools env:config:show
```

特定の情報セットを表示するには、次の形式を使用します。

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - `services.yaml` ファイルで定義されている`MAGENTO_CLOUD_RELATIONSHIPS`環境変数の関係データを表示します。
- `routes` - `MAGENTO_CLOUD_ROUTES`環境変数を使用してプロジェクトの設定済みルートを表示します。
- `variables` - `MAGENTO_CLOUD_VARIABLES`環境変数を使用して、プロジェクトに設定された変数を表示します。

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

プロジェクトの設定を評価するのに役立つ確認コマンドのセットがあります。 各ウィザードコマンドの詳細については、「_デプロイメントの最適化_」セクションの「[&#x200B; スマートウィザード &#x200B;](../deploy/smart-wizards.md)」を参照してください。 `wizard:ideal-state` コマンドは、ビルド フェーズ中に自動的に実行されます。 プロジェクトの理想的な状態を確認するには：

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>リモート クラウド環境で`wizard:ideal-state` コマンドを実行する必要があります。 このコマンドは、ローカル開発環境で実行すると、常に`The configured state is not ideal` エラーを返します。

出力サンプル：

```
Ideal state is configured
```

ece-tools[&#128279;](../release-notes/cloud-tools-suite.md)の リリースノートを参照してください。

## Adobeのパッチとカスタムパッチ

`ece-tools` パッケージには、[magento/magento-cloud-patches](https://github.com/magento/magento-cloud-patches) パッケージの依存関係が含まれています。このパッケージは、Adobeのパッチとホットフィックスを提供し、すべてのAdobe Commerce バージョンとCloud環境の統合を向上させ、重要な修正の迅速な提供をサポートします。 「」は、Adobe Commerce on cloud infrastructure プロジェクトに追加するカスタムパッチも提供します。 [&#x200B; パッチの適用](../development/apply-patches.md)を参照してください。

