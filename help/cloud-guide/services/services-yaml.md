---
title: サービスの設定
description: クラウドインフラストラクチャー上でAdobe Commerceが使用するサービスを設定する方法について説明します。
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 322f7af2c79dd4eeeabafa2ba7e5a32cbd8b1925
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---

# サービスの設定

`services.yaml` ファイルは、MySQL、Redis、Elasticsearchまたは OpenSearch などのクラウドインフラストラクチャー上のAdobe Commerceでサポートされ、使用されるサービスを定義します。 外部サービスプロバイダーに登録する必要はありません。

>[!NOTE]
>
>`.magento/services.yaml` ファイルは、プロジェクトの `.magento` ディレクトリでローカルに管理されます。 この設定は、必要なサービスバージョンを定義するビルドプロセス中にのみ統合環境でアクセスされ、デプロイメントが完了すると削除されるので、サーバー上では見つかりません。

デプロイ スクリプトは、`.magento` ディレクトリの設定ファイルを使用して、設定済みのサービスを環境にプロビジョニングします。 サービスは、[`relationships`](../application/properties.md#relationships) ファイルの `.magento.app.yaml` プロパティに含まれていると、アプリケーションで使用できるようになります。 `services.yaml` ファイルには _type_ と _disk_ の値が含まれています。 サービスタイプは、サービス _name_ と _version_ を定義します。

サービス設定を変更すると、デプロイメントによって更新されたサービスを環境にプロビジョニングされます。これにより、次の環境が影響を受けます。

- 実稼動環境を含むすべてのスタータ `master` 環境
- Pro 統合環境

{{pro-update-service}}

## デフォルトおよびサポートされているサービス

クラウドインフラストラクチャは、次のサービスをサポートおよびデプロイします。

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>
>新しいバージョンの RabbitMQ へのアップグレード後、完全なデプロイメントをトリガーして、カスタムメッセージキューが RabbitMQ で再作成されるようにします。

現在の（デフォルトの [&#x200B; ファイル `services.yaml` のデフォルトのバージョンとディスク値を表示 &#x200B;](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml) きます。 次の例は、`mysql` 設定ファイルで定義されている `redis`、`opensearch`、`elasticsearch` または `rabbitmq`、`activemq-artemis`、`services.yaml` の各サービスを示しています。

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

サービス ID とサービスタイプ設定 `type: <name>:<version>` を指定する必要があります。 サービスが永続的なストレージを使用する場合は、ディスク値を指定する必要があります。

次の形式を使用します。

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

`service-id` 値は、プロジェクト内のサービスを識別します。 `a` のように、小文字の英数字（`z`～`0`、`9`～`redis`）のみを使用できます。

この _service-id_ 値は、[`relationships`](../application/properties.md#relationships) 設定ファイルの `.magento.app.yaml` プロパティで使用されます。

```yaml
relationships:
    redis: "<name>:redis"
```

各サービスタイプの複数のインスタンスに名前を付けることができます。 例えば、複数の Redis インスタンスを使用できます。1 つはセッション用、もう 1 つはキャッシュ用です。

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

`services.yaml` ファイル内のサービスの名前を変更すると **完全に削除されます**、次の問題が発生します。

- 指定した新しい名前でサービスを作成する前の既存のサービス。
- サービスの既存のデータがすべて削除されます。 Adobeでは、既存のサービスの名前を変更する前に [&#x200B; スターター環境をバックアップ &#x200B;](../storage/snapshots.md) することを強くお勧めします。

### `type`

`type` 値は、サービス名とバージョンを指定します。 例：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

`disk` 値は、サービスに割り当てる永続的なディスク記憶域のサイズ （MB 単位）を指定します。 MySQL などの永続ストレージを使用するサービスでは、ディスク値を指定する必要があります。 Redis などの永続的なストレージの代わりにメモリを使用するサービスでは、ディスク値は必要ありません。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

プロジェクトごとの現在のデフォルトのストレージ量は 5 GB （512 0 MB）です。 この金額は、アプリケーションと各サービスの間で配分できます。

## サービスの関係

クラウドインフラストラクチャプロジェクトのAdobe Commerceでは、[&#x200B; ファイルで設定されたサービス &#x200B;](../application/properties.md#relationships) 関係 `.magento.app.yaml` によって、アプリケーションで使用可能なサービスが決まります。

[`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md) 環境変数から、すべてのサービス関係の設定データを取得できます。 設定データには、サービス名、タイプ、バージョンのほか、ポート番号やログイン資格情報など、必要な接続の詳細が含まれます。

**ローカル環境で関係を検証するには**:

1. ローカル環境で、アクティブな環境の関係を表示します。

   ```bash
   magento-cloud relationships
   ```

1. `service` を確認し、応答から `type` を返します。 応答では、IP アドレスやポート番号などの接続情報が提供されます。

   >短縮されたサンプル応答

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

**リモート環境で関係を検証するには**:

1. SSH を使用してリモート環境にログインします。

1. 環境で設定されたすべてのサービスの関係設定データをリストします。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または、次の `ece-tools` コマンドを使用して関係を表示します。

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. `service` を確認し、応答から `type` を返します。 応答では、IP アドレスやポート番号、必要なユーザー名やパスワードの認証情報などの接続情報が提供されます。

## サービスのバージョン

クラウドインフラストラクチャにおけるAdobe Commerceのサービスバージョンと互換性のサポートは、クラウドインフラストラクチャにデプロイされテストされたバージョンによって決まり、Adobe Commerceのオンプレミスデプロイメントでサポートされているバージョンとは異なる場合があります。 Adobeが特定のAdobe CommerceおよびMagento Open Source リリースでテストしたサードパーティ製ソフトウェアの依存関係のリストについては、[&#x200B; インストール &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=ja) ガイドの _必要システム_ を参照してください。

### ソフトウェアの EOL チェック

デプロイメントプロセス中に、`ece-tools` パッケージはインストールされたサービスバージョンを各サービスの提供終了（EOL）日と照合して確認します。

- サービスのバージョンが提供終了（EOL）日から 3 か月以内の場合、デプロイログに通知が表示されます。
- EOL 日が過去の場合は、警告通知が表示されます。

ストアのセキュリティを維持するには、インストールされているソフトウェアのバージョンを EOL になる前に更新してください。 [ece-tools&#39;の `eol.yaml` ファイル &#x200B;](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml) で EOL 日付を確認できます。

### OpenSearch への移行

{{elasticsearch-support}}

Adobe Commerce バージョン 2.4.4 以降については、[OpenSearch サービスの設定 &#x200B;](opensearch.md) を参照してください。

## サービスバージョンの変更

インストールしたサービスバージョンは、クラウド環境にデプロイされたAdobe Commerceのバージョンと互換性を持つようにアップグレードできます。

インストールされているサービスのバージョンを直接ダウングレードすることはできません。 ただし、必要なバージョンを持つサービスを作成できます。 [&#x200B; サービスのバージョンのダウングレード &#x200B;](#downgrade-version) を参照してください。

### インストールされているサービスのバージョンのアップグレード

インストールされたサービスバージョンは、`services.yaml` ファイルのサービス設定を更新することでアップグレードできます。

1. [`type`](#type) ファイルのサービスの `.magento/services.yaml` の値を変更します。

   > 元のサービス定義

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > 更新されたサービス定義

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### ダウングレード版

インストールされているサービスを直接ダウングレードすることはできません。 次の 2 つのオプションがあります。

1. 既存のサービスの名前を新しいバージョンに変更します。これにより、既存のサービスとデータが削除され、新しいサービスが追加されます。

1. サービスを作成し、既存のサービスからデータを保存します。

サービスのバージョンを変更する場合は、`services.yaml` ファイルのサービス構成を更新し、`.magento.app.yaml` ファイルの関係を更新する必要があります。

**既存のサービスの名前を変更してサービスバージョンをダウングレードするには**:

1. `.magento/services.yaml` ファイルの既存のサービスの名前を変更し、バージョンを変更します。

   >[!WARNING]
   >
   >既存のサービスの名前を変更すると、そのサービスが置き換えられ、すべてのデータが削除されます。 データを保持する必要がある場合は、既存のサービスの名前を変更する代わりに、サービスを作成します。

   例えば、_mysql_ サービスの MariaDB バージョンをバージョン 10.4 から 10.3 にダウングレードするには、既存の _service-id_ と _type_ の設定を変更します。

   > 元の `services.yaml` 定義

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 新しい `services.yaml` 定義

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. `.magento.app.yaml` ファイルの関係を更新します。

   > 元の `.magento.app.yaml` 設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > `.magento.app.yaml` 設定を更新しました

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. コードの変更を追加、コミット、プッシュします。

**サービスを作成してサービスをダウングレードするには**:

1. ダウングレードされたバージョン仕様を使用して、プロジェクトの `services.yaml` ファイルにサービス定義を追加します。 以下の _mysql2_ を参照してください。

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

   > 元の `.magento.app.yaml` 設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 新しい `.magento.app.yaml` 設定

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. コードの変更を追加、コミット、プッシュします。
