---
title: ビルド変数
description: Adobe Commerce on cloud infrastructureのビルドフェーズでアクションを制御する環境変数のリストを参照してください。
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
exl-id: 67bc77da-d7f2-4a92-bc11-aa4673d733c1
TQID: https://experienceleague.adobe.com/faQLVL7yA4G8uh8VNVpjouJdVPdwio8PdpOh-a-hHyE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 944
ht-degree: 0%

---

# ビルド変数

次の&#x200B;_ビルド_&#x200B;変数は、ビルド フェーズのアクションを制御し、[ グローバル変数](variables-global.md)から値を継承して上書きできます。 これらの変数を`.magento.env.yaml` ファイルの`build` ステージに挿入します。

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズについて詳しくは、次を参照してください。

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

v2.2では、次の変数が削除されました。

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Default**—`1`
- **バージョン** - Adobe Commerce 2.1.4以降

エラーレポートファイルを保存するためのディレクトリネストのレベルを設定して、レポートディレクトリに数万ファイルを入力しないようにします。これにより、データの管理とレビューが困難になる可能性があります。 この設定はデフォルトで`1`です。 通常、`<magento_root>/var/report/` ディレクトリでのエラーレポートファイルの管理に問題がある場合を除き、デフォルト値を変更する必要はありません。

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

デプロイメント時に適用するAdobe Commerce品質パッチのリストを指定します。

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

次の例では、デプロイメント時に適用する3つのパッチを指定します。

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

[ パッチの適用](../development/apply-patches.md)を参照してください。

## `SCD_COMPRESSION_LEVEL`

- **Default**—`6`
- **バージョン** - Adobe Commerce 2.1.4以降

静的コンテンツを圧縮する際に使用する[gzip](https://www.gnu.org/software/gzip)圧縮レベル （`0` ～ `9`）を指定します。`0`は圧縮を無効にします。

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Default**—`600`
- **バージョン** - Adobe Commerce 2.1.4以降

静的アセットの圧縮にかかる時間が圧縮タイムアウトの制限を超えると、デプロイメントプロセスが中断されます。 静的コンテンツ圧縮コマンドの最大実行時間を秒単位で設定します。

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Default**—`false`
- **バージョン** - Adobe Commerce 2.4.2以降

ビルド フェーズで親テーマの静的コンテンツを生成しないようにするには、`true`に設定します。

親テーマの静的コンテンツを生成しても、サイトの展開に影響を与えたり、不要なサイトのダウンタイムが発生したりしないように、ビルド段階で`SCD_NO_PARENT: false`を設定します。 [静的コンテンツ展開](../deploy/static-content.md)を参照してください。

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

テーマごとに複数のロケールを設定できます。 このカスタマイズは、不要なテーマファイルの数を減らすことによって、ビルドプロセスを高速化するのに役立ちます。 例えば、英語で&#x200B;_magento/backend_ テーマを作成し、他の言語でカスタムテーマを作成できます。

次の例では、3つのロケールを持つ`Magento/backend` テーマを構築します。

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

次の例では、3つのロケールを持つ3つのテーマを構築します。

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

または、テーマを&#x200B;_not_ デプロイするように選択できます。

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0以降

静的コンテンツのデプロイメントで想定される最大実行時間を増やすことができます。

デフォルトでは、Adobe Commerce on cloud infrastructureは、想定される最大実行時間を900秒に設定しますが、一部のシナリオでは、Cloud プロジェクトの静的コンテンツのデプロイメントを完了するのに多くの時間が必要になる場合があります。

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Default**—`quick`
- **バージョン** - Adobe Commerce 2.2.0以降

静的コンテンツの[ デプロイメント戦略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)をカスタマイズします。 [静的ビューファイルのデプロイ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)を参照してください。

複数のロケールがある場合は、次のオプション _のみ_&#x200B;を使用します。

- `standard` – すべてのパッケージのすべての静的ビューファイルをデプロイします。
- `quick` – （_default_）は、デプロイメント時間を最小限に抑えます。
- `compact` - サーバー上のディスク領域を節約します。 Adobe Commerce バージョン 2.2.4以前では、この設定は`scd_threads`の値を`1`の値で上書きします。

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default** – 自動
- **バージョン** - Adobe Commerce 2.1.4以降

静的コンテンツのデプロイメント用のスレッド数を設定します。 デフォルト値は、検出されたCPU スレッド数に基づいて設定され、値4を超えることはありません。 スレッド数を増やすと、静的コンテンツのデプロイメントが高速化されます。スレッド数を減らすと、速度が低下します。 スレッドの値を設定できます。例：

```yaml
stage:
  build:
    SCD_THREADS: 2
```

デプロイメントの時間をさらに短縮するには、`scd-dump` コマンドで[構成管理](../store/store-settings.md)を使用して、静的デプロイメントをビルド フェーズに移動します。

## `SCD_USE_BALER`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.3.0以降

[Baler](https://github.com/magento/baler)は、生成されたJavaScript コードをスキャンし、最適化されたJavaScript バンドルを作成します。 最適化されたバンドルをサイトにデプロイすると、サイトの読み込み時のネットワークリクエストの数を減らし、ページの読み込み時間を短縮できます。

静的コンテンツのデプロイメントを実行した後にBalerを実行するには、`true`に設定します。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Balerはアルファリリースであるため、実稼動環境で使用することはお勧めできません。

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Default**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

Cloud Dockerのインストール中に`composer dump-autoload` コマンドをスキップするには、`true`に設定します。 この変数は、書き込み可能なファイルシステムを持つCloud Docker コンテナにのみ関連します。 このような場合、コマンドをスキップすると、削除された`generated` ディレクトリからコードにアクセスしようとする他のコマンドのエラーが回避されます。

Adobe Commerceが`composer dump-autoload`を実行すると、生成されたクラスへのリンクを含む自動ロード ファイルが`generated` フォルダーに作成されます。これは、読み取り専用のファイル システムを備えた実稼動環境では問題ありません。 ただし、書き込み可能なファイルシステムを使用したCloud Dockerのインストール（`./vendor/bin/ece-docker build:compose --with-test`を使用したテストおよび開発用にのみ作成）の場合、`--keep-generated` オプションを使用せずに`bin/magento -n setup:upgrade` コマンドを実行すると、`generated` ディレクトリが削除されます。 ディレクトリが削除されると、オートロードに削除されたディレクトリ内のファイルへのリンクが含まれているため、`composer dump-autoload` コマンドは失敗します。

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Default**— _設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

ビルド フェーズ中に静的コンテンツのデプロイメントをスキップするには、`true`に設定します。

[構成管理](../store/store-settings.md)のビルド フェーズで既に静的コンテンツをデプロイしている場合、クイック ビルド テスト用に静的コンテンツのデプロイをスキップできます。

ビルド フェーズで、`SKIP_SCD: false`を設定して、静的コンテンツのビルドがビルド フェーズ中に発生し、プロセスがサイトのデプロイメントに影響を与えたり、不要なサイト ダウンタイムを引き起こしたりしないようにします。 [静的コンテンツ展開](../deploy/static-content.md)を参照してください。

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

デプロイメントフェーズ中に実行される`bin/magento` CLI コマンドの[Symfony](https://symfony.com/doc/current/console/verbosity.html) デバッグの冗長性レベルを有効または無効にします。

>[!NOTE]
>
>VERBOSE_COMMANDSを使用して、成功したCLI コマンドと失敗した`bin/magento` CLI コマンドの両方のコマンド出力の詳細を制御するには、[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`を設定する必要があります。

ログに記載されている詳細レベルを選択します。

- `-v`=通常の出力
- `-vv`=より詳細な出力
- `-vvv` = デバッグに最適な詳細な出力

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
