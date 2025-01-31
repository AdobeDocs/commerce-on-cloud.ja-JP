---
title: ビルド変数
description: クラウドインフラストラクチャー上のAdobe Commerceのビルドフェーズでアクションを制御する環境変数のリストを参照してください。
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# ビルド変数

次の _ビルド_ 変数は、ビルドフェーズでのアクションを制御し、[ グローバル変数 ](variables-global.md) の値を継承および上書きできます。 `.magento.env.yaml` ファイルの `build` のステージに、次の変数を挿入します。

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズに関する詳細情報：

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

v2.2 では、次の変数が削除されました。

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **デフォルト**—`1`
- **バージョン** - Adobe Commerce 2.1.4 以降

エラーレポートファイルの保存に使用するディレクトリネストのレベルを設定して、レポートディレクトリが数万のファイルで埋もれてしまい、データの管理やレビューが困難になるのを回避します。 この設定のデフォルトは `1` です。 通常、`<magento_root>/var/report/` ディレクトリ内のエラーレポート ファイルを管理する際に問題が発生しない限り、既定値を変更する必要はありません。

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

デプロイ時に適用するAdobe Commerce品質パッチのリストを指定します。

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

次の例では、デプロイメント時に適用する 3 つのパッチを指定します。

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

[ パッチの適用 ](../development/apply-patches.md) を参照してください。

## `SCD_COMPRESSION_LEVEL`

- **デフォルト**—`6`
- **バージョン** - Adobe Commerce 2.1.4 以降

静的コンテンツを圧縮するときに使用する [gzip](https://www.gnu.org/software/gzip) 圧縮レベル （`0` ～ `9`）を指定します。`0` では圧縮を無効にします。

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **デフォルト**—`600`
- **バージョン** - Adobe Commerce 2.1.4 以降

静的アセットの圧縮に要する時間が圧縮タイムアウトの制限を超えると、デプロイメントプロセスが中断されます。 静的コンテンツ圧縮コマンドの最大実行時間を秒単位で設定します。

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **デフォルト**—`false`
- **バージョン** - Adobe Commerce 2.4.2 以降

ビルドフェーズで親テーマの静的コンテンツが生成されないようにするには、`true` に設定します。

親テーマの静的コンテンツを生成してもサイトのデプロイメントに影響を与えたり、不要なサイトのダウンタイムを引き起こしたりしないように、ビルドフェーズで `SCD_NO_PARENT: false` を設定します。 [ 静的コンテンツのデプロイメント ](../deploy/static-content.md) を参照してください。

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

テーマごとに複数のロケールを設定できます。 このカスタマイズにより、不要なテーマファイルの数を減らすことで、ビルドプロセスを迅速化できます。 例えば、英語の _magento/backend_ テーマと、他の言語のカスタムテーマを作成できます。

次の例では、3 つのロケールで `Magento/backend` テーマをビルドします。

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

次の例では、3 つのロケールで 3 つのテーマを構築します。

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

または、テーマをデプロイする _しない_ を選択できます。

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0 以降

静的コンテンツのデプロイメントの予想最大実行時間を増やすことができます。

デフォルトでは、クラウドインフラストラクチャー上のAdobe Commerceでは、予想される最大実行時間が 900 秒に設定されていますが、場合によっては、クラウドプロジェクトの静的コンテンツのデプロイメントを完了するためにより多くの時間が必要になることがあります。

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **デフォルト**—`quick`
- **バージョン** - Adobe Commerce 2.2.0 以降

静的コンテンツの [ デプロイメント戦略 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) をカスタマイズします。 [ 静的表示ファイルのデプロイ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) を参照してください。

複数のロケールがある場合は、次のオプション _のみ_ を使用します。

- `standard` – すべてのパッケージのすべての静的ビューファイルをデプロイします。
- `quick` - （_デフォルト_）展開時間を最小限に抑えます。
- `compact` - サーバー上のディスク領域を節約します。 Adobe Commerce バージョン 2.2.4 以前では、この設定によって `scd_threads` の値が `1` で上書きされます。

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default** – 自動
- **バージョン** - Adobe Commerce 2.1.4 以降

静的コンテンツのデプロイメントのスレッド数を設定します。 デフォルト値は、検出されたCPU スレッドの数に基づいて設定され、値 4 を超えることはありません。 スレッド数を増やすと、静的コンテンツのデプロイメントが高速化されます。スレッド数を減らすと、速度が低下します。 スレッドの値は、次のように設定できます。

```yaml
stage:
  build:
    SCD_THREADS: 2
```

デプロイメント時間をさらに短縮するには、`scd-dump` コマンドで [ 設定管理 ](../store/store-settings.md) を使用して、静的デプロイメントをビルドフェーズに移行します。

## `SCD_USE_BALER`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.3.0 以降

[Baler](https://github.com/magento/baler) は、生成されたJavaScript コードをスキャンし、最適化されたJavaScript バンドルを作成します。 最適化されたバンドルをサイトにデプロイすると、サイトを読み込む際のネットワークリクエストの数を減らし、ページの読み込み時間を短縮できます。

静的コンテンツのデプロイメントの実行後に Baler を実行する場合は、`true` に設定します。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Baler はアルファリリースなので、実稼動環境で使用することはお勧めしません。

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **デフォルト**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

Cloud Docker のインストール中に `composer dump-autoload` コマンドをスキップする場合は、`true` に設定します。 この変数は、書き込み可能なファイルシステムを持つ Cloud Docker コンテナにのみ関連します。 このような場合は、コマンドをスキップすると、削除された `generated` ディレクトリからコードにアクセスしようとする他のコマンドからのエラーを防ぐことができます。

Adobe Commerceを `composer dump-autoload` 実行すると、`generated` フォルダー内に、生成されたクラスへのリンクを含んだ自動読み込みファイルが作成されます。これは、読み取り専用ファイルシステムを使用した実稼動環境では問題になりません。 ただし、書き込み可能なファイルシステム（`./vendor/bin/ece-docker build:compose --with-test` を使用したテストと開発のためにのみ作成されたもの）を持つ Cloud Docker インストールの場合は、`--keep-generated` オプションを指定せずに `bin/magento -n setup:upgrade` コマンドを実行すると、`generated` ディレクトリが削除されます。 ディレクトリを削除すると、自動読み込みには削除されたディレクトリ内のファイルへのリンクが含まれるため、`composer dump-autoload` コマンドは失敗します。

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **デフォルト**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

ビルドフェーズで静的コンテンツのデプロイメントをスキップする場合は、`true` に設定します。

[Configuration Management](../store/store-settings.md) を使用してビルドフェーズで既に静的コンテンツをデプロイしている場合は、迅速なビルドテストのために静的コンテンツのデプロイメントをスキップできます。

ビルドフェーズでは、静的コンテンツのビルドがビルドフェーズ中に実行され、プロセスがサイトのデプロイメントに影響を与えず、不要なサイトのダウンタイムを引き起こさないように、`SKIP_SCD: false` を設定します。 [ 静的コンテンツのデプロイメント ](../deploy/static-content.md) を参照してください。

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

デプロイメントフェーズで実行される CLI コマンドの [Symfony](https://symfony.com/doc/current/console/verbosity.html) debug 冗長レベル `bin/magento` 有効または無効にします。

>[!NOTE]
>
>VERBOSE_COMMANDS を使用して、CLI コマンドの成功および失敗の両方のコマンド出力の詳細 `bin/magento` 制御するには、[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug` を設定する必要があります。

ログに表示される詳細レベルを選択します。

- `-v`=通常出力
- `-vv`=より詳細な出力
- `-vvv` =デバッグに最適な詳細出力

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
