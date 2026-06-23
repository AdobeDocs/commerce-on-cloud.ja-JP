---
title: プロパティ
description: ビルドおよびクラウドインフラストラクチャへのデプロイ用に [!DNL Commerce]  アプリケーションを設定する際に、プロパティリストを参照として使用します。
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 32bd1f64-43d6-48a3-84b7-bea22f125bb0
TQID: https://experienceleague.adobe.com/5HoI8DnJqL6pyBZRt3u-jVlQvhP1UGqN70B9fq2c9-Y
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 855
ht-degree: 0%

---

# アプリケーション設定のプロパティ

`.magento.app.yaml` ファイルは、プロパティを使用して[!DNL Commerce] アプリケーションの環境サポートを管理します。

| 名前 | 説明 | Default | 必須 |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | ユーザーの役割のカスタマイズ | — | いいえ |
| [`crons`](crons-property.md) | スペックの更新とcron ジョブのスケジュール | — | いいえ |
| [`dependencies`](#dependencies) | 追加の依存関係を有効にする | `php:composer/composer: '2.2.4'` | いいえ |
| [`disk`](#disk) | 永続的なディスクサイズを定義 | `5120` | はい |
| [`firewall`](firewall-property.md) | （スターターのみ）アウトバウンドトラフィックの制御 | — | いいえ |
| [`hooks`](hooks-property.md) | ビルド、デプロイ、デプロイ後のフェーズのシェルコマンドをカスタマイズする | — | いいえ |
| [`mounts`](#mounts) | パスの設定 | パス：<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | いいえ |
| [`name`](#name) | アプリケーション名の定義 | `mymagento` | はい |
| [`relationships`](#relationships) | マップサービス | サービス：<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | いいえ |
| [`runtime`](#runtime) | ランタイムプロパティには、[!DNL Commerce] アプリケーションで必要な拡張機能が含まれています。 | 拡張機能：<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | はい |
| [`type`](#type-and-build) | ベースコンテナイメージの設定 | `php:8.3` | はい |
| [`variables`](variables-property.md) | 特定のCommerce バージョンに対する環境変数の適用 | — | いいえ |
| [`web`](web-property.md) | 外部要求の処理 | — | はい |
| [`workers`](workers-property.md) | 外部要求の処理 | — | はい、Web プロパティを使用しない場合 |

{style="table-layout:auto"}

## `name`

`name` プロパティは、HTTP アップストリームを定義するために[`routes.yaml`](../routes/routes-yaml.md) ファイルで使用されるアプリケーション名を提供します（デフォルトは`mymagento:http`）。 例えば、`name`の値が`app`の場合、アップストリームフィールドに`app:http`を使用する必要があります。

>[!WARNING]
>
>デプロイ後にアプリケーションの名前を変更しないでください。 そうすると、データが失われます。

## `type`と`build`

`type`および`build` プロパティは、プロジェクトをビルドおよび実行するためのベース コンテナー画像に関する情報を提供します。

サポートされている`type`言語はPHPです。 PHPのバージョンを次のように指定します。

```yaml
type: php:<version>
```

`build` プロパティは、プロジェクトの構築時に既定で実行される処理を決定します。 `flavor`は、実行するビルドタスクのデフォルトセットを指定します。 次の例は、`magento-cloud/.magento.app.yaml`からの`type`と`build`のデフォルト設定を示しています。

```yaml
# The toolstack used to build the application.
type: php:8.4
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Composer 2のインストールと使用

`build: flavor:` プロパティはComposer 2.xには使用されません。そのため、ビルド フェーズでComposerを手動でインストールする必要があります。 スタータープロジェクトおよびPro プロジェクトにComposer 2.xをインストールして使用するには、次の3つの変更を行う必要があります。`.magento.app.yaml`設定：

1. `composer`を`build: flavor:`として削除し、`none`を追加します。 この変更により、Cloudはデフォルトの1.x バージョンのComposerを使用してビルドタスクを実行できなくなります。
1. Composer 2.xをインストールするための`php`依存関係として`composer/composer: '^2.0'`を追加します。
1. `composer` ビルドタスクを`build` フックに追加して、Composer 2.xを使用してビルドタスクを実行します。

独自の`.magento.app.yaml`設定で次の設定フラグメントを使用します。

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

Composerについて詳しくは、[必要なパッケージ ](../development/overview.md#required-packages)を参照してください。

## `dependencies`

ビルドプロセス中にアプリケーションに必要な依存関係を指定します。

Adobe Commerceでは、次の言語に対する依存関係をサポートしています。

- PHP
- Ruby
- Node.js

これらの依存関係は、アプリケーションの最終的な依存関係とは独立しており、ビルド プロセス中およびアプリケーションのランタイム環境の`PATH`で使用できます。

これらの依存関係は次のように指定できます。

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

を使用して、拡張機能を有効にするなど、実行時にPHP設定を変更します。 次の拡張機能が必要です。

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

拡張機能の有効化について詳しくは、[PHP設定](php-settings.md)を参照してください。

## `disk`

アプリケーションの永続ディスクサイズをMB単位で定義します。

```yaml
disk: 5120
```

推奨される最小ディスクサイズは256 MBです。 エラー`UserError: Error building the project: Disk size may not be smaller than 128MB`が表示された場合は、サイズを256 MBに増やします。

>[!NOTE]
>
>Pro ステージング環境および実稼動環境の場合、アプリケーションの`mounts`および`disk`設定を更新するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。 チケットを送信するときは、必要な設定変更を示し、更新されたバージョンの`.magento.app.yaml` ファイルを含めてください。
>
>ステージングまたは実稼動環境のディスクストレージを一時的に増やすことはできません。このプロセスは元に戻すことができません。

## `relationships`

アプリケーション内のサービスマッピングを定義します。

関係`name`は、`MAGENTO_CLOUD_RELATIONSHIPS`環境変数のアプリケーションで利用できます。 `<service-name>:<endpoint-name>`関係は、`.magento/services.yaml` ファイルで定義された名前とタイプの値にマップされます。

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

次に、デフォルトの関係の例を示します。

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

現在サポートされているサービスの種類とエンドポイントの完全なリストについては、[ サービス ](../services/services-yaml.md)を参照してください。

## `mounts`

キーがアプリケーションのルートに対する相対パスであるオブジェクト。 マウントは、ファイル用にディスク上に書き込み可能な領域です。 次に、`volume_id[/subpath]`構文を使用して`magento.app.yaml` ファイルで設定されたマウントのデフォルトのリストを示します。

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

このリストにマウントを追加する形式は次のとおりです。

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared` – 環境内のアプリケーション間でボリュームを共有します。
- `disk` – 共有ボリュームで使用可能なサイズを定義します。

>[!NOTE]
>
>Pro ステージング環境および実稼動環境の場合、アプリケーションの`mounts`および`disk`設定を更新するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。 チケットを送信するときは、必要な設定変更を示し、更新されたバージョンの`.magento.app.yaml` ファイルを含めてください。

マウント webを場所の[`web`](web-property.md) ブロックに追加することで、マウント webにアクセスできるようにできます。

>[!WARNING]
>
>サイトにデータがある場合は、マウント名の`subpath`部分を変更しないでください。 この値は、`files`領域の一意の識別子です。 この名前を変更すると、以前の場所に保存されているすべてのサイトデータが失われます。

## `access`

`access` プロパティは、環境へのSSH アクセスが許可されているユーザーの役割レベルの最小値を示します。 使用可能なユーザーの役割は次のとおりです。

- `admin` – 環境で設定を変更し、アクションを実行できます。_コントリビューター_&#x200B;および&#x200B;_ビューアー_&#x200B;の権限があります。
- `contributor` – この環境にコードをプッシュし、環境からブランチできます。_閲覧者_&#x200B;の権限があります。
- `viewer` – 環境のみを表示できます。

デフォルトのユーザー役割は`contributor`で、ユーザーからのSSH アクセスは&#x200B;_閲覧者_&#x200B;の権限でのみ制限されます。 ユーザーの役割を`viewer`に変更して、_閲覧者_&#x200B;権限のみのユーザーに対するSSH アクセスを許可できます。

```yaml
access:
    ssh: viewer
```

