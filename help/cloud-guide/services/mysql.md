---
title: MySQL サービスの設定
description: Adobe Commerce on cloud infrastructureを使用して、永続的なデータストレージ用のMySQL サービスを管理する方法について説明します。
feature: Cloud, Services, Storage
exl-id: 37b893ef-43cf-466b-9d18-ee3b80fdf2d8
TQID: https://experienceleague.adobe.com/xPikS7qhOEhhWDRuUYBJEqL7EUPObzPDxJEZ4xjKkuE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 921
ht-degree: 1%

---

# MySQL サービスの設定

`mysql` サービスは、[MariaDB](https://mariadb.com/) バージョン 10.2から10.4に基づいて永続的なデータストレージを提供し、[XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) ストレージエンジンをサポートし、MySQL 5.6および5.7の機能を再実装しました。

MariaDB 10.4でのインデックス再作成は、他のMariaDBまたはMySQL バージョンと比較してより多くの時間がかかります。 「_パフォーマンスのベストプラクティス_」ガイドの「[ インデクサー](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers)」を参照してください。

>[!WARNING]
>
>MariaDBをバージョン 10.1から10.2にアップグレードする場合は注意してください。 MariaDB 10.1は、_XtraDB_&#x200B;をストレージエンジンとしてサポートする最後のバージョンです。 MariaDB 10.2では、ストレージエンジンに&#x200B;_InnoDB_&#x200B;を使用しています。 10.1から10.2にアップグレードした後は、変更をロールバックできません。 Adobe Commerceは両方のストレージエンジンをサポートしていますが、MariaDB 10.2と互換性があることを確認するために、プロジェクトで使用されている拡張機能やその他のシステムを確認する必要があります。 [10.1から10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102)の間の互換性のない変更を参照してください。

{{service-instruction}}

**MySQL**&#x200B;を有効にするには：

1. 必要な名前、タイプ、およびディスク値（MB単位）を`.magento/services.yaml` ファイルに追加します。

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >ディスク領域が不足しているため、`PDO Exception: MySQL server has gone away`などのMySQL エラーが発生する可能性があります。 [`.magento/services.yaml`](services-yaml.md#disk) ファイルのサービスに十分なディスク領域を割り当てていることを確認してください。

1. `.magento.app.yaml` ファイルの関係を設定します。

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [ サービス関係を確認します](services-yaml.md#service-relationships)。

{{service-change-tip}}

## MySQL データベースの設定

MySQL データベースを設定する場合は、次のオプションがあります。

- **`schemas`** - スキーマはデータベースを定義します。 デフォルトのスキーマは`main` データベースです。
- **`endpoints`** – 各エンドポイントは、特定の権限を持つ資格情報を表します。 デフォルトのエンドポイントは`mysql`で、`main` データベースへの`admin` アクセス権があります。
- **`properties`** - プロパティを使用して、追加のデータベース設定を定義します。

次に、`.magento/services.yaml` ファイルの基本的な設定例を示します。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

上記の例の`properties`は、パフォーマンスのベストプラクティスガイド ](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers)で推奨されている[として、デフォルトの`optimizer`設定を変更します。

**MariaDB設定オプション**:

| オプション | 説明 | デフォルト値 |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | デフォルトの文字セット。 | utf8mb4 |
| `default_collation` | デフォルトの照合順序。 | utf8mb4_unicode_ci |
| `max_allowed_packet` | パケットの最大サイズ （MB単位）。 範囲`1` ～ `100`。 | 16 |
| `optimizer_switch` | クエリオプティマイザーの値を設定します。 [MariaDB ドキュメント ](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch)を参照してください。 | |
| `optimizer_use_condition_selectivity` | オプティマイザーが使用する統計情報を選択します。 範囲`1` ～ `5`。 [MariaDB ドキュメント ](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity)を参照してください。 | 4 （10.4以降） |

### 複数のデータベース・ユーザーの設定

必要に応じて、`main` データベースにアクセスするための異なる権限を持つ複数のユーザーを設定できます。

デフォルトでは、データベースへの管理者アクセス権を持つ`mysql`という名前のエンドポイントが1つあります。 複数のデータベースユーザーを設定するには、`services.yaml` ファイルで複数のエンドポイントを定義し、`.magento.app.yaml` ファイルで関係を宣言する必要があります。 Pro ステージング環境および実稼動環境の場合は、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信して、追加のユーザーをリクエストしてください。

ネストされた配列を使用して、特定のユーザーアクセスのエンドポイントを定義します。 各エンドポイントは、1つ以上のスキーマ（データベース）へのアクセスと、それぞれに対する異なるレベルの権限を指定できます。

有効な権限レベルは次のとおりです。

- `ro`: SELECT クエリのみが許可されます。
- `rw`: SELECT クエリ、INSERT クエリ、UPDATE クエリ、DELETE クエリは許可されています。
- `admin`: DDL クエリ （CREATE TABLE、DROP TABLEなど）を含むすべてのクエリが許可されます。

例：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

前述の例では、`admin` エンドポイントは`main` データベースへの管理者レベルのアクセス権を提供し、`reporter` エンドポイントは読み取り専用アクセス権を提供し、`importer` エンドポイントは読み取り/書き込みアクセス権を提供します。つまり、

- `admin` ユーザーはデータベースを完全に制御できます。
- `reporter` ユーザーはSELECT権限のみを持っています。
- `importer` ユーザーには、SELECT、INSERT、UPDATEおよびDELETE権限があります。

上記の例で定義されたエンドポイントを`.magento.app.yaml` ファイルの`relationships` プロパティに追加します。 例：

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>1人のMySQL ユーザーを設定する場合、ストアド プロシージャとビューに[`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) アクセス制御メカニズムを使用することはできません。

## データベースへの接続

MariaDB データベースに直接アクセスするには、SSHを使用してリモートクラウド環境にログインし、データベースに接続する必要があります。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)変数の`database`および`type` プロパティからMySQL ログイン資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、MySQL情報を見つけます。 例：

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. データベースに接続します。

   - Starterには、次のコマンドを使用します。

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Proの場合、`$MAGENTO_CLOUD_RELATIONSHIPS`変数から取得したホスト名、ポート番号、ユーザー名、パスワードで次のコマンドを使用します。

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>`magento-cloud db:sql` コマンドを使用して、リモート データベースに接続し、SQL コマンドを実行できます。

## セカンダリデータベースへの接続

>[!IMPORTANT]
>
>この機能は、Pro実稼動クラスターとステージングクラスターでのみ使用できます。

データベースのパフォーマンスを向上させたり、データベースのロック問題を解決したりするために、セカンダリデータベースに接続する必要がある場合があります。 この設定が必要な場合は、`"port" : 3304`を使用して接続を確立します。 MySQL スレーブ接続を設定するための[ ベストプラクティス ](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html)のトピックについては、_実装ベストプラクティス_ ガイドを参照してください。

## トラブルシューティング

MySQLの問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [MySQLの低速なクエリとプロセスを確認する](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [クラウドでのデータベースダンプの作成](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [データ移行ツールのトラブルシューティング](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Adobe Commerceのアップグレード：compactからdynamic tables 2.2.x、2.3.xから2.4.xへの移行](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
