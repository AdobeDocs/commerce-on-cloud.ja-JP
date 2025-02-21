---
title: データベースのバックアップ
description: ECE-tools を使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのデータベースのバックアップを作成する方法を説明します。
feature: Cloud, Iaas, Storage
exl-id: 351f7691-3153-4b8a-83af-8b8895b93d98
source-git-commit: 3a3b0cd6e28f3e6ed3521a86f7c7c8868be0cf83
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# データベースのバックアップ

サービスおよびマウントからすべての環境データを取得することなく、`ece-tools db-dump` コマンドを使用してデータベースのコピーを作成できます。 デフォルトでは、このコマンドは、環境設定で指定されたすべてのデータベース接続のバックアップを `app/var/` ディレクトリに作成します。 DB ダンプ操作は、アプリケーションをメンテナンスモードに切り替え、コンシューマーキュープロセスを停止し、ダンプを開始する前に cron ジョブを無効にします。

DB ダンプに関する次のガイドラインを考慮してください。

- 実稼動環境の場合、Adobeでは、サイトがメンテナンスモードの場合に発生するサービスの中断を最小限に抑えるために、ピーク外の時間にデータベースダンプ操作を完了することをお勧めします。
- ダンプ処理中にエラーが発生した場合は、ディスク容量を節約するために、ダンプ・ファイルが削除されます。 詳細については、ログを確認してください（`var/log/cloud.log`）。
- Pro 実稼動環境の場合、このコマンドは 3 つの高可用性ノードのうち _1 つ_ からのみダンプするので、ダンプ中に別のノードに書き込まれた実稼動データはコピーされません。 このコマンドは、コマンドが複数のノードで実行されないようにする `var/dbdump.lock` ファイルを生成します。
- すべての環境サービスのバックアップの場合、Adobeでは [ バックアップ ](snapshots.md) を作成することをお勧めします。

コマンドにデータベース名を追加することで、複数のデータベースをバックアップするように選択できます。 次の例では、2 つのデータベース（`main` および `sales`）がバックアップされます。

```bash
php vendor/bin/ece-tools db-dump main sales
```

その他のオプションを表示するには、`php vendor/bin/ece-tools db-dump --help` のコマンドを使用します。

- `--dump-directory=<dir>` - データベース・ダンプのターゲット・ディレクトリを選択します。 **`pub/media` や`pub/static`** などのパブリック web ディレクトリは選択しないでください。
- `--remove-definers` - データベース・ダンプから DEFINER 文を削除します。

**ステージング環境または実稼動環境でデータベースダンプを作成するには**:

1. [SSH を使用してログインするか、リモート環境に接続するトンネルを作成します ](../development/secure-connections.md)。このトンネルには、コピーするデータベースが含まれています。

1. 環境の関係を一覧表示し、データベースのログイン情報をメモします。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. データベースのバックアップを作成します。 DB ダンプのターゲット・ディレクトリを選択するには、`--dump-directory` オプションを使用します。

   >[!WARNING]
   >
   >ターゲットディレクトリを指定する場合は、`pub/media` や `pub/static` などのパブリック web ディレクトリを選択しないでください。

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   応答の例：

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /app/qxmtlseakof6y/var/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. `db-dump` コマンドは、リモート プロジェクト ディレクトリに `dump-<timestamp>.sql.gz` アーカイブ ファイルを作成します。

>[!TIP]
>
>このデータを特定の環境にプッシュする場合は、[ データと静的ファイルを移行する ](../deploy/staging-production.md#migrate-static-files) を参照してください。
