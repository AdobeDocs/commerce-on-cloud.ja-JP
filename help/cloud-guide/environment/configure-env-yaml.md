---
title: 環境の設定
description: 環境変数を使用して、Pro ステージングと実稼動環境を含む、すべてのCommerce on cloud infrastructure環境でビルドアクションとデプロイアクションを設定する方法について説明します。
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: f39c73fc-351a-41ed-9e74-2c3f14871246
TQID: https://experienceleague.adobe.com/Ub0FWkUN9uOVzLhVbNbPhUV5kj808ODlbjVrRDDA-4E
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 706
ht-degree: 0%

---

# デプロイメント用の環境変数の設定

`.magento.env.yaml` ファイルでは、環境変数を使用して、Pro ステージングと実稼動環境を含むすべての環境でビルドとデプロイのアクションを一元管理します。 各環境で一意のアクションを設定するには、各環境でこのファイルを変更する必要があります。

>[!TIP]
>
>YAML ファイルでは大文字と小文字が区別され、タブは使用できません。 `.magento.env.yaml` ファイル全体で一貫したインデントを使用するように注意してください。そうしないと、設定が期待どおりに動作しない可能性があります。 ドキュメントとサンプルファイルの例では、_2つのスペース_&#x200B;のインデントが使用されています。 [ece-tools validate コマンド &#x200B;](#validate-configuration-file)を使用して、設定を確認します。

## ファイル構造

`.magento.env.yaml` ファイルには、`stage`と`log`の2つのセクションがあります。 `stage` セクションは、[&#x200B; クラウド展開プロセス &#x200B;](../deploy/process.md)のフェーズで発生するアクションを制御します。

- `stage` - 「ステージ」セクションを使用して、デプロイメントの次のステージの特定のアクションを定義します。
   - `global` - ビルド、デプロイ、デプロイ後の両方のフェーズでアクションを制御します。 ビルド、デプロイ、デプロイ後のセクションでこれらの設定を上書きできます。
   - `build` – 作成段階のアクションのみを制御します。 このセクションで設定を指定しない場合、ビルドフェーズではグローバルセクションの設定が使用されます。
   - `deploy` - デプロイ フェーズでのみアクションを制御します。 このセクションで設定を指定しない場合、デプロイフェーズでは、グローバルセクションの設定が使用されます。
   - `post-deploy` - アプリケーションのデプロイ後&#x200B;_後_、コンテナが接続の受け入れを開始した後&#x200B;_後_&#x200B;のアクションを制御します。
- `log` - ログセクションを使用して、通知タイプと詳細レベルを含む[通知](set-up-notifications.md)を設定します。
   - `slack` - Slack ボットに送信するメッセージを設定します。
   - `email` - 1人以上のメール受信者に送信するメールを設定します。
   - [&#x200B; ログハンドラー](log-handlers.md) - リモートのログサーバーに送信されるハードウェアおよびソフトウェアのアプリケーションメッセージを設定します。

### 環境変数

`ece-tools` パッケージは、[&#x200B; クラウド変数](variables-cloud.md)、[!DNL Cloud Console]で設定された変数、`.magento.env.yaml`設定ファイルの値に基づいて、`env.php` ファイルの値を設定します。 `.magento.env.yaml` ファイルの環境変数は、既存のCommerce設定を上書きしてCloud環境をカスタマイズします。 デフォルト値が`Not Set`の場合、`ece-tools` パッケージは&#x200B;**NO** アクションを実行し、[!DNL Commerce]のデフォルトまたはMAGENTO_CLOUD_RELATIONSHIPS コンフィギュレーションの値を使用します。 デフォルト値が設定されている場合、`ece-tools` パッケージはそのデフォルト値を設定するように動作します。

次のトピックには、`.magento.env.yaml` ファイルで使用できるすべての変数のデフォルト値が設定されているかどうかなどの詳細な定義が含まれています。

- [&#x200B; グローバル &#x200B;](variables-global.md) – 変数は、ビルド、デプロイ、デプロイ後の各フェーズのアクションを制御します
- [&#x200B; ビルド &#x200B;](variables-build.md) – 変数はビルドアクションを制御します
- [&#x200B; デプロイ &#x200B;](variables-deploy.md) – 変数がデプロイアクションを制御します
- [&#x200B; デプロイ後](variables-post-deploy.md) – 変数はデプロイ後のアクションを制御します

### CLIからの設定ファイルの作成

次の`ece-tools` コマンドを使用して、クラウド環境用の`.magento.env.yaml`設定ファイルを生成できます。

>設定ファイルを作成します

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>設定ファイルの値を更新する

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

どちらのコマンドも、1つの引数（少なくとも1つのビルド、デプロイ、デプロイ後の変数の値を指定するJSON形式の配列）が必要です。 例えば、次のコマンドは`SCD_THREADS`変数と`CLEAN_STATIC_FILES`変数の値を設定します。

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

次の設定で`.magento.env.yaml` ファイルを作成します。

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

`cloud:config:update` コマンドを使用して、新しいファイルを更新できます。 例えば、次のコマンドは`SCD_THREADS`値を変更し、`SCD_COMPRESSION_TIMEOUT`設定を追加します。

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

更新されたファイルには、次の設定が含まれています。

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### 設定ファイルの検証

リモート クラウド環境に変更をプッシュする前に、次の`ece-tools` コマンドを使用して`.magento.env.yaml`設定ファイルを検証します。

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

次の応答の例では、修正する項目のリストを示します。

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP定数

ハードコーディング値の代わりに、`.magento.env.yaml`個のファイル定義でPHP定数を使用できます。 次の例では、PHP定数を使用して`driver_options`を定義します。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>3.2より前のバージョンの`symfony/yaml` パッケージを使用する場合、定数解析は機能しません。

## エラー処理

`.magento.env.yaml`設定ファイルの予期しない値が原因でエラーが発生すると、エラーメッセージが表示されます。 例えば、次のエラーメッセージでは、各項目に対する変更の候補のリストが予期しない値で表示されます。有効なオプションが提供されることもあります。

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

修正を加え、コミットし、変更をプッシュします。 エラーメッセージが表示されない場合は、設定ファイルの変更が検証に渡されます。

## 設定管理の最適化

設定をダンプした後に構成管理を有効にした場合は、SCD_*変数をデプロイからビルドステージに移動する必要があります。 [静的コンテンツ展開戦略](../deploy/static-content.md)を参照してください。

>構成管理の前：

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>構成管理を有効にした後、SCD_*変数をビルドステージに移動します。

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```

