---
title: サービスの設定
description: MySQL、Redis、Elasticsearchなど、Adobe Commerceのクラウドインフラストラクチャで使用されるサービスを設定する方法について説明します。
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
TQID: https://experienceleague.adobe.com/qvCjqNc8E9QGme-zM42vMg-kb1WjwTlWUqjbm-NI2bg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
last-update: 2026-09-01
source-git-commit: 0f88ef7d75bc2a02eb7988dc815071c5894a4662
workflow-type: tm+mt
source-wordcount: 1176
ht-degree: 0%

---

# サービスの設定

`services.yaml` ファイルは、MySQL、RedisまたはValkey、ElasticsearchまたはOpenSearchなど、Adobe Commerceがクラウドインフラストラクチャ上でサポートおよび使用するサービスを定義します。 外部サービスプロバイダーに加入する必要はありません。

>[!NOTE]
>
>`.magento/services.yaml` ファイルは、プロジェクトの`.magento` ディレクトリ内でローカルに管理されます。 デプロイメント時に、Adobe Commerce オンクラウドインフラストラクチャは、この設定を使用して、ターゲット環境でサポートされているサービスをプロビジョニングします。 `.magento` ディレクトリはデプロイ後にリモート サーバーから削除されるので、`services.yaml`はデプロイされた環境には存在しません。

デプロイ スクリプトは、`.magento` ディレクトリの設定ファイルを使用して、設定されたサービスを使用して環境をプロビジョニングします。 サービスは、`.magento.app.yaml` ファイルの[`relationships`](../application/properties.md#relationships) プロパティに含まれている場合、アプリケーションで利用できるようになります。 `services.yaml` ファイルには、_type_&#x200B;と&#x200B;_disk_&#x200B;の値が含まれています。 サービスの種類は、サービス _name_&#x200B;および&#x200B;_version_&#x200B;を定義します。

`.magento/services.yaml`のサービス設定は、`composer.json`で定義され、`composer.lock`でロックされているPHPおよびComposer パッケージの依存関係とは別になっています。

## サービスの変更が適用される場所

サービス設定を変更すると、更新されたサービスを使用して環境をプロビジョニングするデプロイメントが発生します。これは、次の環境に影響します。

- 実稼動環境`master`を含むすべてのスターター環境
- Pro統合環境

{{$include /help/_includes/pro-services-support.md}}

## デフォルトサービスとサポート対象サービス

Adobe Commerce on cloud infrastructureでは、プロジェクトに設定できる次のサービスがサポートされています。

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)または[Valkey](valkey.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>[使用可能なバージョン間でRabbitMQを順次アップグレード ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/rabbitmq#upgrading-the-rabbitmq-service)。 例えば、3.9から4.1に直接アップグレードしないでください。
>
>新しいバージョンにアップグレードした後、カスタムメッセージキューがRabbitMQで再作成されるようにするには、完全なデプロイメントをトリガーします。

## 設定済みのサービスとバージョンの表示

現在のテンプレート [`services.yaml` ファイル ](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml)のサービス定義とディスク値の例を表示できます。 実際のデフォルトバージョンとサポートされているサービスバージョンは、Adobe Commerceのバージョンと現在のクラウドテンプレートによって異なります。

次の例は、`services.yaml`設定ファイルのサービス定義を示しています。

```yaml
mysql:
    type: mysql:11.8
    disk: 5120

cache:
    type: valkey:9.0

opensearch:
    type: opensearch:3  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:4.3
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## サービス値

サービス IDとサービス タイプ設定`type: <name>:<version>`を指定してください。 サービスで永続ストレージを使用する場合は、ディスク値を指定する必要があります。

次の形式を使用します。

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

`service-id`値は、プロジェクト内のサービスを識別します。 小文字の英数字のみを使用できます：`a` ～ `z`、および`0` ～ `9` （例：`valkey`）。

この&#x200B;_service-id_&#x200B;値は、`.magento.app.yaml`設定ファイルの[`relationships`](../application/properties.md#relationships) プロパティで使用されます。

```yaml
relationships:
    valkey: "valkey:valkey"
```

各サービスタイプの複数のインスタンスに名前を付けることができます。 例えば、複数のValkey インスタンスを使用できます。1つはセッション用、1つはキャッシュ用です。

```yaml
valkey:
    type: valkey:<version>

valkey2:
    type: valkey:<version>
```

`services.yaml` ファイル内のサービスの名前を変更します。

- 指定した新しい名前のサービスを作成する前の既存のサービス。
- サービスの既存のデータはすべて削除されます。 Adobeでは、既存のサービスの名前を変更する前に、[Starter環境](../storage/snapshots.md)をバックアップすることをお勧めします。

### `type`

`type`値は、サービス名とバージョンを指定します。 例：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

`disk`値は、サービスに割り当てる永続的なディスク ストレージのサイズ （MB）を指定します。 MySQLなどの永続ストレージを使用するサービスでは、ディスク値を提供する必要があります。 Valkeyなどの永続ストレージの代わりにメモリを使用するサービスでは、ディスク値は必要ありません。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

プロジェクトごとの現在のデフォルトのストレージ容量は5 GB、つまり5120 MBです。 この金額は、アプリケーションとその各サービスの間で配布できます。

## サービスとの関係

クラウドインフラストラクチャプロジェクト上のAdobe Commerceで、`.magento.app.yaml` ファイルで設定されたサービス [relationships](../application/properties.md#relationships)によって、アプリケーションで使用できるサービスが決まります。

すべてのサービス関係の設定データは、[`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md)環境変数から取得できます。 設定データには、サービス名、タイプ、バージョンと、ポート番号やログイン資格情報などの必要な接続の詳細が含まれます。

### ローカル開発環境からの関係を確認する

1. ローカル開発環境から、アクティブな環境のリレーションシップを表示します。

   ```bash
   magento-cloud relationships
   ```

1. 応答から`service`と`type`を確認してください。 応答は、IP アドレスやポート番号などの接続情報を提供します。

   >簡略化されたサンプル応答

   ```yaml
   valkey:
       -
   ...
           type: 'valkey:8.0'
           port: 6379
   opensearch:
       -
   ...
           type: 'opensearch:3'
           port: 9200
   database:
       -
   ...
           type: 'mysql:11.8'
           port: 3306
   ```

### リモート環境での関係の確認

1. SSHを使用してリモート環境にログインします。

1. 環境内で設定されたすべてのサービスの関係設定データを一覧表示します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または、次の`ece-tools` コマンドを使用して関係を表示します。

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 応答から`service`と`type`を確認してください。 応答は、IP アドレス、ポート番号、必要なユーザー名およびパスワード資格情報などの接続情報を提供します。

## サービスバージョン

クラウドインフラストラクチャにデプロイおよびテストされたバージョンによって、Adobe Commerce オンプレミスのデプロイメントでサポートされているバージョンと異なる場合がある、クラウドインフラストラクチャ上のAdobe Commerceのサービスバージョンと互換性のサポートが判断されます。 Adobeが特定のAdobe CommerceおよびMagento Open Source リリースでテストしたサードパーティ製ソフトウェアの依存関係の一覧については、_インストール_ ガイドの[必要システム構成](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)を参照してください。

### ソフトウェアのEOL チェック

デプロイメントプロセス中、`ece-tools` パッケージは、各サービスの提供終了（EOL）日に対して、インストールされたサービスのバージョンを確認します。

- サービスのバージョンがEOL日から3か月以内の場合、デプロイログに通知が表示されます。
- EOL日が過去の場合は、警告通知が表示されます。

ストアのセキュリティを維持するには、インストール済みのソフトウェアのバージョンがEOLに達する前に更新する必要があります。 [ece-tools&#39; `eol.yaml` ファイル ](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml)でEOLの日付を確認できます。

### OpenSearchに移行

{{elasticsearch-support}}

Adobe Commerce バージョン 2.4.4以降については、[OpenSearch サービスの設定](opensearch.md)を参照してください。

## サービスのバージョンを変更

インストール済みのサービスのバージョンは、Cloud環境にデプロイされているAdobe Commerceのバージョンと互換性を保つためにアップグレードできます。

インストール済みサービスのサービス バージョンを直接ダウンロードすることはできません。 ただし、必要なバージョンのサービスを作成できます。 [ ダウングレードサービスバージョン ](#downgrade-version)を参照してください。

### インストール済みサービスのバージョンのアップグレード

インストールされているサービスのバージョンは、`services.yaml` ファイルのサービス設定を更新することでアップグレードできます。

1. `.magento/services.yaml` ファイルのサービスの[`type`](#type)値を変更します。

   > 元のサービス定義

   ```yaml
   mysql:
       type: mysql:11.8
       disk: 2048
   ```

   > サービス定義を更新しました

   ```yaml
   mysql:
       type: mysql:12.3
       disk: 5120
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 11.8 to 12.3."
   ```

   ```bash
   git push origin <branch-name>
   ```

### ダウングレードバージョン

インストール済みのサービスを直接ダウングレードすることはできません。 選択肢は次の2つです。

1. 既存のサービスの名前を新しいバージョンに変更し、既存のサービスとデータを削除して新しいサービスを追加します。

1. サービスを作成し、既存のサービスからデータを保存します。

サービスのバージョンを変更する場合は、`services.yaml` ファイルのサービス設定を更新し、`.magento.app.yaml` ファイルの関係を更新する必要があります。

#### 既存のサービスの名前を変更して、サービスのバージョンをダウングレード

1. `.magento/services.yaml` ファイル内の既存のサービスの名前を変更し、バージョンを変更します。

   >[!WARNING]
   >
   >既存のサービスの名前を変更すると、既存のサービスに置き換わり、すべてのデータが削除されます。 データを保持する必要がある場合は、既存の名前を変更する代わりにサービスを作成します。

   例えば、_mysql_ サービスのMariaDB バージョンをバージョン 10.4から10.3にダウングレードするには、既存の&#x200B;_service-id_&#x200B;および&#x200B;_type_&#x200B;設定を変更します。

   > 元の`services.yaml`定義

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 新しい`services.yaml`定義

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. `.magento.app.yaml` ファイルの関係を更新します。

   > 元の`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 更新された`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. コード変更を追加、コミット、プッシュします。

#### サービスの作成によるサービスのダウングレード

1. ダウングレードされたバージョン仕様を使用して、プロジェクトの`services.yaml` ファイルにサービス定義を追加します。 次の例の&#x200B;_mysql2_&#x200B;を参照してください。

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. 新しいサービスを使用するには、`.magento.app.yaml` ファイルの関係設定を変更します。

   > 元の`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 新しい`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. コード変更を追加、コミット、プッシュします。
