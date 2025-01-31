---
title: グローバル変数
description: クラウドインフラストラクチャデプロイメントプロセス上のAdobe Commerceのアクションを制御する環境変数のリストを参照してください。
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# グローバル変数

グローバル変数は、[!DNL Commerce] デプロイメントプロセスの各フェーズ（ビルド、デプロイ、デプロイ後）でのアクションを制御します。 グローバル変数は各フェーズに影響を与えるので、`.magento.env.yaml` ファイルの `global` の段階で設定する必要があります。

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズに関する詳細情報：

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `ENABLE_EVENTING`

- **デフォルト**-_設定なし_
- **バージョン** - Adobe Commerce 2.4.5 以降

`true` に設定すると、cron でメッセージキューコンシューマーを実行できます。 Adobe I/O Events for Adobe Commerceでは、メッセージキューを使用して重要なイベントの配信を迅速に行います。

Adobeでは、`true` に設定した [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) 変数を `.magento.env.yaml` ファイルの `deploy` のステージに追加す `cron_run` こともお勧めします。

次の例は、完全に設定された `ENABLE_EVENTING` 変数を示しています。

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOK

- **デフォルト**-_設定なし_
- **バージョン** - Adobe Commerce 2.4.4 以降

`true` に設定すると、Commerce Webhook が有効になります。 Webhook は、App Builder ランタイムアクションやサードパーティの在庫管理システムなどの外部エンドポイントで実行されます。 [_Webhook ガイド_](https://developer.adobe.com/commerce/extensibility/webhooks) では、この機能について詳しく説明します。

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

コードを変更せずに、すべての出力ストリームの最小ログレベルを上書きします。これは、デプロイメントに関する問題のトラブルシューティングに役立ちます。 例えば、デプロイメントが失敗した場合、この変数を使用して、ログの精度をグローバルに高めることができます。 [ ログレベル ](log-handlers.md#log-levels) を参照してください。 ログハンドラーの `min_level` の値は、この設定を上書きします。

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>`MIN_LOGGING_LEVEL` 変数の設定は、デフォルトで `debug` に設定されているファイルハンドラーのログレベル設定を変更しません。

## `SCD_ON_DEMAND`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

ユーザー（SCD）から要求されたときに静的コンテンツを生成できるようにします。 静的コンテンツは、デプロイメント時間が短縮されるので、開発およびテストワークフローに最適です。

[`post_deploy` フックを使用してキャッシュをプリロードすると ](../application/hooks-property.md) サイトのダウンタイムが削減されます。 キャッシュウォーミングは、[!DNL Cloud Console] にステージング環境と実稼動環境が含まれている Pro プロジェクトと、スタータープロジェクトでのみ使用できます。 `SCD_ON_DEMAND` 環境変数を `.magento.env.yaml` ファイルの `global` ステージに追加します。

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

`SCD_ON_DEMAND` 変数は、両方のフェーズ（build と deploy）で SCD をスキップし、`pub/static` フォルダーと `var/view_preprocessed` フォルダーをクリアして、以下を `app/etc/env.php` ファイルに書き込みます。

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0 以降

静的コンテンツのデプロイメントの予想最大実行時間を増やすことができます。

デフォルトでは、Adobe Commerceは想定される最大実行時間を 900 秒に設定しますが、場合によっては、Cloud プロジェクトの静的コンテンツのデプロイメントを完了するためにより多くの時間が必要になることがあります。

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.4.2 以降

ビルドおよびデプロイメントフェーズで親テーマの静的コンテンツが生成されないようにするには、`true` に設定します。 このオプションを `true` に設定すると、生成される静的コンテンツが少なくなり、ビルドとデプロイメントの全体的な時間が短縮されます。

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.3.0 以降

[Baler](https://github.com/magento/baler) は、生成されたJavaScript コードをスキャンし、最適化されたJavaScript バンドルを作成するモジュールです。 最適化されたバンドルをサイトにデプロイすると、サイトを読み込む際のネットワークリクエストの数を減らし、ページの読み込み時間を短縮できます。

静的コンテンツのデプロイメントの実行後に Baler を実行する場合は、`true` に設定します。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>この機能を使用する前に、Baler モジュールをインストールして設定します。 Baler はアルファリリースなので、ステージング環境でのみ、このオプションを有効にします。

## `SKIP_HTML_MINIFICATION`

- **デフォルト**:
   - `true` - `ece-tools` 2002.0.13 以降
   - `false` - `ece-tools` の以前のバージョン用
- **バージョン** - Adobe Commerce 2.1.4 以降

ビルド段階の最後の `<magento_root>/init/` ディレクトリへの静的ビューファイルのコピーを有効または無効にします。 `true` に設定した場合、ファイルはコピーされず、リクエストに応じてHTMLの縮小が可能になります。 この値を `true` に設定すると、ステージング環境と実稼動環境にデプロイする際のダウンタイムを短縮できます。

- **`false`** - `view_preprocessed` ディレクトリを構築フェーズの最後の `<magento_root>/init/` ディレクトリにコピーし、デプロイフェーズの最初の `<magento_root>/var` ディレクトリにディレクトリを復元します。
- **`true`** - オンデマンドのHTML縮小が可能です。構築フェーズの最後に `<magento_root>var/view_preprocessed` を `<magento_root>/init/` ディレクトリにコピーする _しない_ です。

`SKIP_HTML_MINIFICATION` 環境変数を `.magento.env.yaml` ファイルの `global` ステージに追加します。

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

`X_FRAME_CONFIGURATION` 変数を使用して、Adobe Commerce サイトの [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html) ヘッダー設定を変更します。 この設定は、`<frame>`、`<iframe>` または `<object>` でブラウザーがページをレンダリングする方法を制御します。 次のいずれかのオプションを使用します。

- `DENY` - ページをフレーム内に表示できません。
- `SAMEORIGIN` – （デフォルトのAdobe Commerce設定。） ページは、ページ自体と同じ原点のフレーム内でのみ表示できます。

>[!WARNING]
>
>Adobe Commerceでサポートされているブラウザーがサポートしなくなったため、`ALLOW-FROM <uri>` オプションは非推奨（廃止予定）になりました。 [ ブラウザー互換性 ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility) を参照してください。

`X_FRAME_CONFIGURATION` 環境変数を `.magento.env.yaml` ファイルの `global` ステージに追加します。

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
