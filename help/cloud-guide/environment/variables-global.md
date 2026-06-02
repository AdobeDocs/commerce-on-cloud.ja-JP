---
title: グローバル変数
description: Adobe Commerce on cloud infrastructure デプロイメントプロセスのアクションを制御する環境変数の一覧を参照してください。
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
exl-id: 1f1ef6db-6836-4f71-b1e4-3629352d7e74
TQID: https://experienceleague.adobe.com/2aBPh7We4-KqoUVDfd4B-ZNWoaUVO-3mWVbqErdgyoQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 774
ht-degree: 0%

---

# グローバル変数

グローバル変数は、デプロイメントプロセス [!DNL Commerce]の各フェーズ（ビルド、デプロイ、デプロイ後）でのアクションを制御します。 グローバル変数は各フェーズに影響を与えるため、`.magento.env.yaml` ファイルの`global` ステージで設定する必要があります。

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズについて詳しくは、次を参照してください。

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `ENABLE_EVENTING`

- **既定**-_設定なし_
- **バージョン** - Adobe Commerce 2.4.5以降

`true`に設定すると、cronでメッセージキューコンシューマーを実行できるようになります。 Adobe I/O Events for Adobe Commerceでは、メッセージキューを使用して、重要なイベントの配信を迅速化します。

Adobeでは、`cron_run`が`true`に設定された`.magento.env.yaml` ファイルの`deploy` ステージに[`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner)変数を追加することをお勧めします。

次の例は、完全に設定された`ENABLE_EVENTING`変数を示しています。

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

## ENABLE_WEBHOOKS

- **既定**-_設定なし_
- **バージョン** - Adobe Commerce 2.4.4以降

`true`に設定すると、CommerceのWebhookを有効にします。 Webhookは、App Builder ランタイムアクションやサードパーティの在庫管理システムなどの外部エンドポイントで実行されます。 [_Webhook ガイド_](https://developer.adobe.com/commerce/extensibility/webhooks)では、この機能について詳しく説明しています。

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

コードを変更することなく、すべての出力ストリームの最小ログレベルを上書きします。これは、デプロイメントに関する問題のトラブルシューティングに役立ちます。 例えば、デプロイメントに失敗した場合、この変数を使用して、ログの精度をグローバルに増やすことができます。 [&#x200B; ログレベル &#x200B;](log-handlers.md#log-levels)を参照してください。 Logging ハンドラーの`min_level`値は、この設定を上書きします。

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>`MIN_LOGGING_LEVEL`変数の設定では、デフォルトで`debug`に設定されているファイルハンドラーのログレベル設定は変更されません。

## `SCD_ON_DEMAND`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

ユーザー（SCD）から要求されたときに、静的コンテンツの生成を有効にします。 静的なオンデマンド型コンテンツは、デプロイメント時間を短縮するため、開発やテストのワークフローに最適です。

[`post_deploy` フック &#x200B;](../application/hooks-property.md)を使用してキャッシュを事前に読み込むと、サイトのダウンタイムが短縮されます。 キャッシュ ウォーミングは、[!DNL Cloud Console]のステージング環境と実稼動環境を含むPro プロジェクトとスタータープロジェクトでのみ使用できます。 `SCD_ON_DEMAND`環境変数を`.magento.env.yaml` ファイルの`global` ステージに追加します。

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

`SCD_ON_DEMAND`変数は、SCDを両方のフェーズ（ビルドとデプロイ）でスキップし、`pub/static`および`var/view_preprocessed` フォルダーをクリアし、次の内容を`app/etc/env.php` ファイルに書き込みます。

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.2.0以降

静的コンテンツのデプロイメントで想定される最大実行時間を増やすことができます。

デフォルトでは、Adobe Commerceは想定される最大実行時間を900秒に設定しますが、一部のシナリオでは、Cloud プロジェクトの静的コンテンツのデプロイメントを完了するのに多くの時間が必要になる場合があります。

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.4.2以降

ビルドおよびデプロイメントフェーズで親テーマの静的コンテンツを生成しないようにするには、`true`に設定します。 このオプションを`true`に設定すると、より少ない静的コンテンツが生成されるので、全体的なビルドとデプロイメントの時間が短縮されます。

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.3.0以降

[Baler](https://github.com/magento/baler)は、生成されたJavaScript コードをスキャンし、最適化されたJavaScript バンドルを作成するモジュールです。 最適化されたバンドルをサイトにデプロイすると、サイトの読み込み時のネットワークリクエストの数を減らし、ページの読み込み時間を短縮できます。

静的コンテンツのデプロイメントを実行した後にBalerを実行するには、`true`に設定します。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>この機能を使用する前に、Baler モジュールをインストールして設定します。 Balerはアルファリリースであるため、このオプションはステージング環境でのみ有効にします。

## `SKIP_HTML_MINIFICATION`

- **既定**:
   - `true` - `ece-tools` 2002.0.13以降
   - `false` – 以前のバージョン `ece-tools`の場合
- **バージョン** - Adobe Commerce 2.1.4以降

ビルド ステージの最後にある`<magento_root>/init/` ディレクトリへの静的ビューファイルのコピーを有効または無効にします。 `true`に設定されている場合、ファイルはコピーされず、HTMLの縮小はリクエストに応じて利用できます。 この値を`true`に設定すると、ステージング環境と実稼動環境にデプロイする際のダウンタイムが短縮されます。

- **`false`** - ビルドフェーズの終了時に`view_preprocessed` ディレクトリを`<magento_root>/init/` ディレクトリにコピーし、デプロイフェーズの開始時に`<magento_root>/var` ディレクトリにディレクトリを復元します。
- **`true`** - オンデマンド HTMLの縮小を有効にします。_not_&#x200B;は、ビルド フェーズの最後に`<magento_root>var/view_preprocessed`を`<magento_root>/init/` ディレクトリにコピーします。

`SKIP_HTML_MINIFICATION`環境変数を`.magento.env.yaml` ファイルの`global` ステージに追加します。

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

Adobe Commerce サイトの[`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html) ヘッダー設定を変更するには、`X_FRAME_CONFIGURATION`変数を使用します。 この設定は、ブラウザーが`<frame>`、`<iframe>`、または`<object>`でページをレンダリングする方法を制御します。 次のいずれかのオプションを使用します。

- `DENY` - ページはフレーム内に表示できません。
- `SAMEORIGIN` – （デフォルトのAdobe Commerce設定） ページは、ページ自体と同じオリジンのフレームでのみ表示できます。

>[!WARNING]
>
>Adobe Commerceでサポートされているブラウザーではサポートされなくなったため、`ALLOW-FROM <uri>` オプションは廃止されました。 [&#x200B; ブラウザーの互換性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)を参照してください。

`X_FRAME_CONFIGURATION`環境変数を`.magento.env.yaml` ファイルの`global` ステージに追加します。

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
