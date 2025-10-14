---
title: プロパティ
description: クラウドインフラストラクチャにビルドおよびデプロイする  [!DNL Commerce]  アプリケーションを設定する際には、プロパティリストを参照として使用します。
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 32bd1f64-43d6-48a3-84b7-bea22f125bb0
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# アプリケーション設定のプロパティ

`.magento.app.yaml` ファイルは、プロパティを使用して、[!DNL Commerce] アプリケーションの環境サポートを管理します。

| 名前 | 説明 | デフォルト | 必須 |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | ユーザーの役割のカスタマイズ | — | 不可 |
| [`crons`](crons-property.md) | 仕様の更新と cron ジョブのスケジュール | — | 不可 |
| [`dependencies`](#dependencies) | 追加依存関係を有効にする | `php:composer/composer: '2.2.4'` | 不可 |
| [`disk`](#disk) | 永続ディスクサイズの定義 | `5120` | はい |
| [`firewall`](firewall-property.md) | （スターターのみ）送信トラフィックの制御 | — | 不可 |
| [`hooks`](hooks-property.md) | ビルド、デプロイ、デプロイ後の各フェーズ用のシェルコマンドのカスタマイズ | — | 不可 |
| [`mounts`](#mounts) | パスを設定 | パス：<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | 不可 |
| [`name`](#name) | アプリケーション名を定義 | `mymagento` | はい |
| [`relationships`](#relationships) | マップサービス | サービス：<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | 不可 |
| [`runtime`](#runtime) | Runtime プロパティには、[!DNL Commerce] アプリケーションで必要な拡張機能が含まれています。 | 拡張機能：<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | はい |
| [`type`](#type-and-build) | ベース コンテナ イメージ設定 | `php:8.3` | はい |
| [`variables`](variables-property.md) | 特定のCommerce バージョンに環境変数を適用する | — | 不可 |
| [`web`](web-property.md) | 外部リクエストの処理 | — | はい |
| [`workers`](workers-property.md) | 外部リクエストの処理 | — | はい（web プロパティを使用していない場合） |

{style="table-layout:auto"}

## `name`

`name` プロパティは、HTTP アップストリームを定義するために [`routes.yaml`](../routes/routes-yaml.md) ファイルで使用されるアプリケーション名を提供します（デフォルトでは `mymagento:http`）。 例えば、`name` の値が `app` の場合、アップストリームフィールドで `app:http` を使用する必要があります。

>[!WARNING]
>
>デプロイ後にアプリケーションの名前を変更しないでください。 これにより、データが失われます。

## `type` と `build`

`type` プロパティと `build` プロパティは、プロジェクトをビルドして実行するためのベースコンテナイメージに関する情報を提供します。

サポートしている `type` 言語は PHP です。 PHP のバージョンを次のように指定します。

```yaml
type: php:<version>
```

`build` プロパティは、プロジェクトの構築時のデフォルトの動作を決定します。 `flavor` は、実行するビルドタスクのデフォルトセットを指定します。 次の例は、`magento-cloud/.magento.app.yaml` の `type` と `build` のデフォルト設定を示しています。

```yaml
# The toolstack used to build the application.
type: php:8.4
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Composer 2 のインストールと使用

`build: flavor:` プロパティは Composer 2.x では使用されないので、ビルド フェーズ中に Composer を手動でインストールする必要があります。 Composer 2.x を Starter プロジェクトおよび Pro プロジェクトにインストールして使用するには、`.magento.app.yaml` の設定に 3 つの変更を加える必要があります。

1. `composer` を `build: flavor:` として削除し、`none` を追加します。 この変更により、Cloud ではデフォルトの 1.x バージョンの Composer を使用してビルド タスクを実行できなくなります。
1. Composer 2.x をインストールする場合は、`php` の依存関係として `composer/composer: '^2.0'` を追加します。
1. Composer 2.x を使用してビルド タスクを実行するには、`composer` ビルド タスクを `build` フックに追加します。

独自の `.magento.app.yaml` 設定で次の設定フラグメントを使用します。

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Composer の詳細については、[&#x200B; 必須パッケージ &#x200B;](../development/overview.md#required-packages) を参照してください。

## `dependencies`

ビルドプロセス中にアプリケーションで必要になる可能性のある依存関係を指定します。

Adobe Commerceは、次の言語の依存関係をサポートしています。

- PHP
- Ruby
- Node.js

これらの依存関係はアプリケーションの最終的な依存関係には依存せず、`PATH`、ビルドプロセス中およびアプリケーションのランタイム環境で利用できます。

これらの依存関係は、次のように指定できます。

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

を使用して、拡張機能の有効化など、実行時に PHP 設定を変更します。 次の拡張機能が必要です。

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

拡張機能の有効化について詳しくは、[PHP 設定 &#x200B;](php-settings.md) を参照してください。

## `disk`

アプリケーションの永続ディスクサイズを MB 単位で定義します。

```yaml
disk: 5120
```

推奨される最小ディスクサイズは 256 MB です。 `UserError: Error building the project: Disk size may not be smaller than 128MB` というエラーが表示される場合は、サイズを 256 MB に増やします。

>[!NOTE]
>
>ステージング環境および実稼動環境をプロ環境にする場合は、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) して、アプリケーションの `mounts` 定と `disk` 定を更新する必要があります。 チケットを送信する際には、必要な設定変更を指定し、`.magento.app.yaml` ファイルの更新バージョンを含めます。
>
>ステージング環境または実稼動環境で一時的にディスクストレージを増やすことはできません。このプロセスを元に戻すことはできません。

## `relationships`

アプリケーションでサービスマッピングを定義します。

関係 `name` は、`MAGENTO_CLOUD_RELATIONSHIPS` 環境変数でアプリケーションが使用できます。 `<service-name>:<endpoint-name>` 関係は、`.magento/services.yaml` ファイルで定義された名前とタイプの値にマッピングされます。

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

デフォルトの関係の例を次に示します。

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

現在サポートされているサービスタイプとエンドポイントの完全なリストについては、[&#x200B; サービス &#x200B;](../services/services-yaml.md) を参照してください。

## `mounts`

キーがアプリケーションのルートに対する相対パスであるオブジェクト。 マウントは、ファイルのディスク上の書き込み可能な領域です。 `volume_id[/subpath]` の構文を使用して `magento.app.yaml` ファイルに設定されるマウントのデフォルトのリストを以下に示します。

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

このリストにマウントを追加する形式は、次のとおりです。

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared`：環境内のアプリケーション間でボリュームを共有します。
- `disk` – 共有ボリュームに使用できるサイズを定義します。

>[!NOTE]
>
>ステージング環境および実稼動環境をプロ環境にする場合は、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) して、アプリケーションの `mounts` 定と `disk` 定を更新する必要があります。 チケットを送信する際には、必要な設定変更を指定し、`.magento.app.yaml` ファイルの更新バージョンを含めます。

マウント web を [`web`](web-property.md) の場所のブロックに追加すると、そのマウント web にアクセスできるようになります。

>[!WARNING]
>
>サイトにデータが取り込まれたら、マウント名の `subpath` の部分は変更しないでください。 この値は、`files` 領域の一意の ID です。 この名前を変更すると、古い場所に保存されているすべてのサイト データが失われます。

## `access`

`access` プロパティは、環境への SSH アクセスが許可されている最小ユーザー役割レベルを示します。 使用できるユーザーの役割は次のとおりです。

- `admin` – 設定を変更し、環境内でアクションを実行できます。_contributor_ および _viewer_ 権限があります。
- `contributor` - コードをこの環境にプッシュし、環境から分岐できます。_ビューア_ 権限があります。
- `viewer`- 環境のみを表示できます。

デフォルトのユーザー役割は `contributor` で、 _閲覧者_ 権限のみを持つユーザーからの SSH アクセスが制限されます。 ユーザーの役割を `viewer` に変更して、_viewer_ 権限のみを持つユーザーに対して SSH アクセスを許可できます。

```yaml
access:
    ssh: viewer
```
