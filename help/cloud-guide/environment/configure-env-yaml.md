---
title: 環境の設定
description: 環境変数を使用して、ステージング環境および実稼動環境を含む、クラウドインフラストラクチャ環境のすべてのCommerceでビルドおよびデプロイ操作を設定する方法を説明します。
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# デプロイメント用の環境変数の設定

`.magento.env.yaml` ファイルでは、環境変数を使用して、ステージング環境や実稼動環境を含むすべての環境で、ビルドおよびデプロイアクションの管理を一元化します。 各環境で一意のアクションを設定するには、各環境でこのファイルを変更する必要があります。

>[!TIP]
>
>YAML ファイルでは大文字と小文字が区別され、タブは使用できません。 `.magento.env.yaml` ファイル全体で一貫性のあるインデントを使用するように注意してください。使用しないと、設定が期待どおりに動作しない場合があります。 ドキュメントとサンプルファイルの例では、_2 スペース_ インデントを使用しています。 [ece-tools validate コマンド ](#validate-configuration-file) を使用して、設定を確認します。

## ファイル構造

`.magento.env.yaml` ファイルには、`stage` と `log` の 2 つのセクションがあります。 `stage` の節では、[ クラウドデプロイメントプロセス ](../deploy/process.md) の各段階で行われるアクションを制御します。

- `stage` - 「ステージ」セクションを使用して、次のデプロイメントのステージに対する特定のアクションを定義します。
   - `global` - ビルド、デプロイ、およびデプロイ後の両方のフェーズでアクションを制御します。 これらの設定は、ビルド、デプロイ、デプロイ後のセクションで上書きできます。
   - `build` – 構築フェーズのアクションのみを制御します。 このセクションで設定を指定しない場合、ビルドフェーズではグローバルセクションの設定が使用されます。
   - `deploy` – 配置フェーズのアクションのみを制御します。 このセクションで設定を指定しない場合、デプロイフェーズではグローバルセクションの設定が使用されます。
   - `post-deploy` - アプリケーションのデプロイ _後_ アクションを制御し、コンテナは接続の受け入れを開始 _後_ します。
- `log` - 「ログ」セクションを使用して、通知のタイプや詳細レベルを含む [ 通知 ](set-up-notifications.md) を構成します。
   - `slack` - Slackボットに送信するメッセージを設定します。
   - `email` - 1 人または複数の電子メール受信者に送信する電子メールを設定します。
   - [ ログハンドラー ](log-handlers.md) - リモートログサーバーに送信されるハードウェアおよびソフトウェアアプリケーションメッセージを設定します。

### 環境変数

`ece-tools` パッケージは、[ クラウド変数 ](variables-cloud.md)、[!DNL Cloud Console] で設定された変数、`.magento.env.yaml` 設定ファイルの値に基づいて `env.php` ファイルの値を設定します。 `.magento.env.yaml` ファイルの環境変数は、既存の Cloud Configuration を上書きしてCommerce環境をカスタマイズします。 デフォルト値が `Not Set` の場合、`ece-tools` パッケージは **NO** アクションを実行し、[!DNL Commerce] のデフォルトまたはMAGENTOCLOUD_RELATIONSHIPS 設定の値を使用します。 デフォルト値が設定されている場合、`ece-tools` パッケージはそのデフォルト値を設定します。

次のトピックには、`.magento.env.yaml` ファイルで使用できるすべての変数に関する詳細な定義（デフォルト値が設定されているかどうかなど）が含まれています。

- [ グローバル ](variables-global.md) – 変数は各フェーズのアクション（構築、デプロイ、デプロイ後）を制御します
- [ ビルド ](variables-build.md) – 変数はビルドアクションを制御します
- [Deploy](variables-deploy.md) – 変数はデプロイアクションを制御します。
- [ デプロイ後 ](variables-post-deploy.md) – 変数は、デプロイ後のアクションを制御します

### CLI からの構成ファイルの作成

次の `ece-tools` コマンドを使用して、クラウド環境用の `.magento.env.yaml` 設定ファイルを生成できます。

>設定ファイルを作成します

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>設定ファイルの値を更新

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

両方のコマンドには単一の引数が必要です。少なくとも 1 つの build、deploy または post-deploy 変数の値を指定する JSON 形式の配列です。 例えば、次のコマンドは、`SCD_THREADS` 変数と `CLEAN_STATIC_FILES` 変数の値を設定します。

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

次の設定で `.magento.env.yaml` ファイルを作成します。

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

`cloud:config:update` コマンドを使用して、新しいファイルを更新できます。 例えば、次のコマンドは `SCD_THREADS` の値を変更し、`SCD_COMPRESSION_TIMEOUT` の設定を追加します。

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

### 設定ファイルを検証

次の `ece-tools` コマンドを使用して、変更をリモートクラウド環境にプッシュする前に `.magento.env.yaml` 設定ファイルを検証します。

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

次の応答例では、修正する項目のリストを示します。

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP 定数

`.magento.env.yaml` ファイル定義では、値をハードコーディングする代わりに PHP 定数を使用できます。 次の例では、PHP 定数を使用して `driver_options` を定義します。

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
>3.2 より前の `symfony/yaml` パッケージバージョンを使用している場合、定数の解析は機能しません。

## エラー処理

`.magento.env.yaml` 設定ファイル内の予期しない値が原因でエラーが発生した場合は、エラーメッセージが表示されます。 例えば、次のエラーメッセージは、各項目に対して提案された変更のリストを予期しない値で表示し、場合によっては有効なオプションを提供します。

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

修正を加え、コミットし、変更をプッシュします。 エラーメッセージが表示されない場合は、設定ファイルに対する変更が検証をパスします。

## 構成管理の最適化

設定のダンプ後に Configuration Management を有効にした場合は、SCD_*変数をデプロイからビルドステージに移動する必要があります。 [ 静的コンテンツデプロイメント戦略 ](../deploy/static-content.md) を参照してください。

>構成管理前：

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

>設定管理を有効にした後、SCD_*変数をビルドステージに移動します。

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
