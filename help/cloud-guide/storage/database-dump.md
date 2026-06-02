---
title: データベースのバックアップ
description: ECE ツールを使用して、Adobe Commerce on cloud インフラストラクチャプロジェクトのデータベースのバックアップを作成する方法を説明します。
feature: Cloud, Iaas, Storage
exl-id: 351f7691-3153-4b8a-83af-8b8895b93d98
TQID: https://experienceleague.adobe.com/bT80HnUguAzsYdVx-kNcxJUyggLzzyfomXHdZvUAcrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 364
ht-degree: 0%

---

# データベースのバックアップ

サービスとマウントからすべての環境データを取得しなくても、`ece-tools db-dump` コマンドを使用してデータベースのコピーを作成できます。 デフォルトでは、このコマンドは、環境設定で指定されているすべてのデータベース接続の`app/var/` ディレクトリにバックアップを作成します。 DB ダンプ操作は、アプリケーションをメンテナンスモードに切り替え、コンシューマーキュープロセスを停止し、ダンプが開始される前にcron ジョブを無効にします。

DB ダンプに関する次のガイドラインを検討します。

- 実稼動環境の場合、Adobeでは、サイトがメンテナンスモードの場合に発生するサービスの中断を最小限に抑えるために、オフピーク時にデータベースダンプ操作を実行することをお勧めします。
- ダンプ操作中にエラーが発生した場合、コマンドはダンプ ファイルを削除してディスク容量を節約します。 詳細については、ログを確認してください（`var/log/cloud.log`）。
- Pro実稼動環境の場合、このコマンドは3つの高可用性ノードのうち&#x200B;_one_&#x200B;からのみダンプされるため、ダンプ中に別のノードに書き込まれた実稼動データがコピーされない可能性があります。 コマンドは、コマンドが複数のノードで実行されないように`var/dbdump.lock` ファイルを生成します。
- すべての環境サービスのバックアップを作成する場合、Adobeでは[&#x200B; バックアップ &#x200B;](snapshots.md)を作成することをお勧めします。

コマンドにデータベース名を追加することで、複数のデータベースをバックアップできます。 次の例では、`main`と`sales`の2つのデータベースをバックアップしています。

```bash
php vendor/bin/ece-tools db-dump main sales
```

その他のオプションについては、`php vendor/bin/ece-tools db-dump --help` コマンドを使用してください。

- `--dump-directory=<dir>` - データベース ダンプのターゲット ディレクトリを選択します。 **`pub/media`や`pub/static`**&#x200B;などのパブリック web ディレクトリは選択しないでください。
- `--remove-definers` - データベースダンプからDEFINER ステートメントを削除します。

**ステージング環境または実稼動環境でデータベースダンプを作成するには**:

1. [SSHを使用してログインするか、トンネルを作成して、コピーするデータベースを含むリモート環境](../development/secure-connections.md)に接続します。

1. 環境の関係を一覧表示し、データベースのログイン情報をメモします。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   または

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. データベースのバックアップを作成します。 DB ダンプのターゲットディレクトリを選択するには、`--dump-directory` オプションを使用します。

   >[!WARNING]
   >
   >ターゲットディレクトリを指定する場合は、`pub/media`や`pub/static`などのパブリック web ディレクトリを選択しないでください。

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   回答サンプル：

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

1. `db-dump` コマンドは、リモート プロジェクト ディレクトリに`dump-<timestamp>.sql.gz` アーカイブ ファイルを作成します。

>[!TIP]
>
>このデータを特定の環境にプッシュする場合は、[&#x200B; データと静的ファイルの移行](../deploy/staging-production.md#migrate-static-files)を参照してください。
