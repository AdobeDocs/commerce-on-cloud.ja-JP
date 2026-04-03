---
title: サービスの設定
description: Adobe Commerce クラウドインフラストラクチャで使用されるサービスを設定する方法について説明します。
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 4ea85b008e2ea9d1c9ab623c413ff9c3c3aaffd2
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 0%

---

# サービスの設定

`services.yaml` ファイルは、MySQL、Redis、ElasticsearchまたはOpenSearchなど、Adobe Commerceでクラウドインフラストラクチャ上でサポートおよび使用されるサービスを定義します。 外部サービスプロバイダーに加入する必要はありません。

>[!NOTE]
>
>`.magento/services.yaml` ファイルは、プロジェクトの`.magento` ディレクトリ内でローカルに管理されます。 この設定は、統合環境で必要なサービスバージョンを定義するためのビルドプロセス中にのみアクセスされ、デプロイメントが完了すると削除されるので、サーバー上で見つかりません。

デプロイ スクリプトは、`.magento` ディレクトリの設定ファイルを使用して、設定されたサービスを使用して環境をプロビジョニングします。 サービスは、`.magento.app.yaml` ファイルの[`relationships`](../application/properties.md#relationships) プロパティに含まれている場合、アプリケーションで利用できるようになります。 `services.yaml` ファイルには、_type_&#x200B;と&#x200B;_disk_&#x200B;の値が含まれています。 サービスの種類は、サービス _name_&#x200B;および&#x200B;_version_&#x200B;を定義します。

サービス設定を変更すると、更新されたサービスを使用して環境をプロビジョニングするデプロイメントが発生します。これは、次の環境に影響します。

- 実稼動環境`master`を含むすべてのスターター環境
- Pro統合環境

{{pro-update-service}}

## デフォルトサービスとサポート対象サービス

クラウドインフラストラクチャは、次のサービスをサポートおよびデプロイします。

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>例えば、使用可能なバージョン [&#128279;](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/configure/service/rabbitmq#upgrading-the-rabbitmq-service)間でRabbitMQを順次 アップグレードする必要があります。例えば、3.9から4.1に直接アップグレードすることはできません
>
>新しいバージョンのRabbitMQにアップグレードした後、完全なデプロイメントをトリガーして、カスタムメッセージキューがRabbitMQで再作成されるようにします。

現在の[&#x200B; デフォルト `services.yaml` ファイル &#x200B;](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml)では、デフォルトバージョンとディスク値を表示できます。 次のサンプルは、`services.yaml`設定ファイルで定義された`mysql`、`redis`、`opensearch`または`elasticsearch`、`rabbitmq`および`activemq-artemis` サービスを示しています。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## サービス値

サービス IDとサービス タイプ設定`type: <name>:<version>`を指定する必要があります。 サービスで永続ストレージを使用する場合は、ディスク値を指定する必要があります。

次の形式を使用します。

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

`service-id`値は、プロジェクト内のサービスを識別します。 小文字の英数字のみを使用できます：`a` ～ `z`、および`0` ～ `9` （例：`redis`）。

この&#x200B;_service-id_&#x200B;値は、`.magento.app.yaml`設定ファイルの[`relationships`](../application/properties.md#relationships) プロパティで使用されます。

```yaml
relationships:
    redis: "<name>:redis"
```

各サービスタイプの複数のインスタンスに名前を付けることができます。 例えば、複数のRedis インスタンス（セッション用に1つ、キャッシュ用に1つ）を使用できます。

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

`services.yaml` ファイル **のサービスの名前を変更すると、次の**&#x200B;が完全に削除されます。

- 指定した新しい名前のサービスを作成する前の既存のサービス。
- サービスの既存のデータはすべて削除されます。 Adobeでは、既存のサービスの名前を変更する前に、[Starter環境](../storage/snapshots.md)をバックアップすることを強くお勧めします。

### `type`

`type`値は、サービス名とバージョンを指定します。 例：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

`disk`値は、サービスに割り当てる永続的なディスク ストレージのサイズ （MB）を指定します。 MySQLなどの永続ストレージを使用するサービスでは、ディスク値を提供する必要があります。 Redisなどの永続ストレージの代わりにメモリを使用するサービスでは、ディスク値は必要ありません。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

プロジェクトごとの現在のデフォルトのストレージ容量は5 GB、つまり512 0MBです。 この金額は、アプリケーションとその各サービスの間で配布できます。

## サービスとの関係

クラウドインフラストラクチャプロジェクト上のAdobe Commerceで、`.magento.app.yaml` ファイルで設定されたサービス [relationships](../application/properties.md#relationships)によって、アプリケーションで使用できるサービスが決まります。

すべてのサービス関係の設定データは、[`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md)環境変数から取得できます。 設定データには、サービス名、タイプ、バージョンと、ポート番号やログイン資格情報などの必要な接続の詳細が含まれます。

**ローカル環境の関係を確認するには**:

1. ローカル環境で、アクティブな環境の関係を表示します。

   ```bash
   magento-cloud relationships
   ```

1. 応答から`service`と`type`を確認してください。 応答は、IP アドレスやポート番号などの接続情報を提供します。

   >簡略化されたサンプル応答

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**リモート環境での関係を確認するには**:

1. SSHを使用してリモート環境にログインします。

1. 環境内で設定されたすべてのサービスの関係設定データを一覧表示します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または、次の`ece-tools` コマンドを使用して関係を表示します。

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 応答から`service`と`type`を確認してください。 応答は、IP アドレスとポート番号、必要なユーザー名とパスワードの資格情報などの接続情報を提供します。

## サービスバージョン

Adobe Commerce on cloud infrastructureのサービスバージョンと互換性のサポートは、クラウドインフラストラクチャにデプロイおよびテストされたバージョンによって決まり、Adobe Commerce オンプレミスのデプロイメントでサポートされているバージョンとは異なる場合があります。 Adobeが特定のAdobe CommerceおよびMagento Open Source リリースでテストしたサードパーティ製ソフトウェアの依存関係の一覧については、_インストール_ ガイドの[必要システム構成](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=ja)を参照してください。

### ソフトウェアのEOL チェック

デプロイメントプロセス中、`ece-tools` パッケージは、各サービスの提供終了（EOL）日に対して、インストールされたサービスのバージョンを確認します。

- サービスのバージョンがEOL日から3か月以内の場合、デプロイログに通知が表示されます。
- EOL日が過去の場合は、警告通知が表示されます。

ストアのセキュリティを維持するには、インストール済みのソフトウェアのバージョンがEOLに達する前に更新する必要があります。 [ece-tools&#39; `eol.yaml` ファイル &#x200B;](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml)でEOLの日付を確認できます。

### OpenSearchに移行

{{elasticsearch-support}}

Adobe Commerce バージョン 2.4.4以降については、[OpenSearch サービスの設定](opensearch.md)を参照してください。

## サービスのバージョンを変更

インストール済みのサービスのバージョンは、Cloud環境にデプロイされているAdobe Commerceのバージョンと互換性を保つためにアップグレードできます。

インストール済みサービスのサービス バージョンを直接ダウンロードすることはできません。 ただし、必要なバージョンのサービスを作成できます。 [&#x200B; ダウングレードサービスバージョン &#x200B;](#downgrade-version)を参照してください。

### インストール済みサービスのバージョンのアップグレード

インストールされているサービスのバージョンは、`services.yaml` ファイルのサービス設定を更新することでアップグレードできます。

1. `.magento/services.yaml` ファイルのサービスの[`type`](#type)値を変更します。

   > 元のサービス定義

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > サービス定義を更新しました

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### ダウングレードバージョン

インストール済みのサービスを直接ダウングレードすることはできません。 選択肢は次の2つです。

1. 既存のサービスの名前を新しいバージョンに変更し、既存のサービスとデータを削除して新しいサービスを追加します。

1. サービスを作成し、既存のサービスからデータを保存します。

サービスのバージョンを変更する場合は、`services.yaml` ファイルのサービス設定を更新し、`.magento.app.yaml` ファイルの関係を更新する必要があります。

**既存のサービスの名前を変更してサービスのバージョンをダウングレードするには**:

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

**サービスを作成してサービスをダウングレードするには**:

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

1. 新しいサービスを使用するように、`.magento.app.yaml` ファイルの関係設定を変更します。

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
