---
title: MySQL サービスの設定
description: クラウドインフラストラクチャー上のAdobe Commerceを使用して、永続的データストレージ用の MySQL サービスを管理する方法について説明します。
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# MySQL サービスの設定

`mysql` サービスは、[MariaDB](https://mariadb.com/) バージョン 10.2 から 10.4 に基づいて永続的なデータストレージを提供し、[XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) ストレージエンジンをサポートし、MySQL 5.6 と 5.7 の機能を再実装しました。

MariaDB 10.4 でのインデックス再作成は、他の MariaDB または MySQL バージョンに比べて時間がかかります。 『 _パフォーマンスのベストプラクティス [&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) ガイドの  インデクサー_ を参照してください。

>[!WARNING]
>
>MariaDB をバージョン 10.1 から 10.2 にアップグレードする場合は注意が必要です。MariaDB 10.1 は、ストレージエンジンとして _XtraDB_ をサポートする最後のバージョンです。 MariaDB 10.2 は、ストレージエンジンに _InnoDB_ を使用します。 10.1 から 10.2 にアップグレードした後は、変更をロールバックできません。 Adobe Commerceは両方のストレージエンジンをサポートしていますが、MariaDB 10.2 と互換性があることを確認するには、プロジェクトで使用されている拡張機能やその他のシステムを確認する必要があります。[10.1 ～ 10.2 の間で互換性のない変更 ](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102) を参照してください。

{{service-instruction}}

**MySQL を有効にするには**:

1. 必要な名前、タイプ、ディスク値（MB 単位）を `.magento/services.yaml` ファイルに追加します。

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >`PDO Exception: MySQL server has gone away` などの MySQL エラーは、ディスク領域が不十分な結果として発生する場合があります。 [`.magento/services.yaml`](services-yaml.md#disk) ファイルのサービスに十分なディスク領域が割り当てられていることを確認します。

1. `.magento.app.yaml` ファイルで関係を設定します。

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [ サービスの関係を確認します ](services-yaml.md#service-relationships)。

{{service-change-tip}}

## MySQL データベースの設定

MySQL データベースを設定する際には、次のオプションがあります。

- **`schemas`** - スキーマはデータベースを定義します。 デフォルトのスキーマは `main` データベースです。
- **`endpoints`** – 各エンドポイントは、特定の権限を持つ資格情報を表します。 デフォルトのエンドポイントは `mysql` で、`main` データベースへのアクセス `admin` 許可されています。
- **`properties`** - プロパティは、追加のデータベース構成を定義するために使用されます。

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

上記の例の `properties` は、（『 パフォーマンスのベストプラクティスガイドで推奨 [ に従って、デフォルトの `optimizer` 設定を変更し ](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) す。

**MariaDB 設定オプション**:

| オプション | 説明 | デフォルト値 |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | デフォルトの文字セット。 | utf8mb4 |
| `default_collation` | 既定の照合順序です。 | utf8mb4_unicode_ci |
| `max_allowed_packet` | パケットの最大サイズ （MB 単位）。 範囲 `1` ～ `100`。 | 16 |
| `optimizer_switch` | クエリオプティマイザーの値を設定します。 [MariaDB ドキュメント ](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch) を参照してください。 | |
| `optimizer_use_condition_selectivity` | オプティマイザが使用する統計を選択します。 範囲 `1` ～ `5`。 [MariaDB ドキュメント ](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity) を参照してください。 | 10.4 以降の場合は 4 |

### 複数のデータベースユーザーの設定

オプションとして、`main` データベースへのアクセス権が異なる複数のユーザーを設定できます。

デフォルトでは、データベースへの管理者アクセス権を持つ `mysql` という名前のエンドポイントが 1 つあります。 複数のデータベースユーザーを設定するには、`services.yaml` ファイルに複数のエンドポイントを定義し、`.magento.app.yaml` ファイルで関係を宣言する必要があります。 ステージング環境および実稼動環境がプロの場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、追加のユーザーをリクエストします。

ネストされた配列を使用して、特定のユーザーアクセスのエンドポイントを定義します。 各エンドポイントは、1 つ以上のスキーマ（データベース）へのアクセスと、それぞれに対する異なるレベルの権限を指定できます。

有効な権限レベルは次のとおりです。

- `ro`: SELECT クエリのみが許可されています。
- `rw`: SELECT クエリおよび INSERT クエリ、UPDATE クエリ、DELETEクエリが許可されています。
- `admin`: DDL クエリ （CREATE TABLE、DROP TABLE など）を含むすべてのクエリが許可されます。

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

前述の例では、`admin` エンドポイントは、管理者レベルでの `main` データベースへのアクセスを提供し、`reporter` エンドポイントは、読み取り専用アクセスを提供し、`importer` エンドポイントは、読み取り/書き込みアクセスを提供します。つまり、次のようになります。

- `admin` ユーザーはデータベースの完全な制御が可能です。
- `reporter` ユーザーは SELECT 権限のみ持っています。
- `importer` ユーザーは、SELECT、INSERT、UPDATE およびDELETE権限を持っています。

上記の例で定義したエンドポイントを、`.magento.app.yaml` ファイルの `relationships` プロパティに追加します。 例：

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>1 つの MySQL ユーザーを設定した場合、ストアドプロシージャおよびビューに [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) のアクセス制御メカニズムを使用することはできません。

## データベースへの接続

MariaDB データベースに直接アクセスするには、SSH を使用してリモート クラウド環境にログインし、データベースに接続する必要があります。

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. [$login_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) 変数の `database` プロパティおよび `type` プロパティから MySQL MAGENTO資格情報を取得します。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   応答で、MySQL 情報を見つけます。 例：

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

   - スターターの場合は、次のコマンドを使用します。

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Pro の場合は、次のコマンドを、`$MAGENTO_CLOUD_RELATIONSHIPS` 変数から取得したホスト名、ポート番号、ユーザー名、パスワードと共に使用します。

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>`magento-cloud db:sql` コマンドを使用すると、リモート・データベースに接続し、SQL コマンドを実行できます。

## セカンダリ データベースに接続する

>[!IMPORTANT]
>
>この機能は、実稼動環境およびステージングクラスターでのみ使用できます。

場合によっては、データベースのパフォーマンスを向上させたり、データベース ロックの問題を解決するために、セカンダリ データベースに接続する必要があります。 この設定が必要な場合は、`"port" : 3304` を使用して接続を確立します。 [ 実装のベストプラクティス ](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) ガイドの _MySQL スレーブ接続を設定するためのベストプラクティス_ トピックを参照してください。

## トラブルシューティング

MySQL の問題のトラブルシューティングについては、次のAdobe Commerce サポートの記事を参照してください。

- [ 低速のクエリの確認と MySQL の処理 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [ クラウドにデータベースダンプを作成する ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [ データ移行ツールのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Adobe Commerceのアップグレード：動的テーブル 2.2.x へのコンパクト、2.3.x から 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
