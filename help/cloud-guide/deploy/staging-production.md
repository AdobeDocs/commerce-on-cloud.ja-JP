---
title: ステージング環境および実稼動環境にデプロイ
description: さらなるテストのために、クラウドインフラストラクチャコード上でAdobe Commerceをステージング環境および実稼動環境にデプロイする方法について説明します。
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
source-git-commit: fe634412c6de8325faa36c07e9769cde0eb76c48
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%

---

# ステージング環境および実稼動環境にデプロイ

デプロイと運用開始のプロセスは、開発から始まり、ステージングに続き、実稼動での運用開始で終わります。 Adobeは、一貫性のある設定を確実に行うためのエンドツーエンドの環境ソリューションを提供します。 すべての環境で、ストアフロントへの直接 URL アクセス、および CLI コマンドへの管理者および SSH アクセスがサポートされています。

ストアをデプロイする準備が整ったら、実稼動環境にデプロイする前に、ステージング環境でのデプロイメントとテストを完了する必要があります。 この節では、ビルドおよびデプロイプロセス、データとコンテンツの移行およびテストに関する詳細な手順と情報を示します。

>[!TIP]
>
>Adobeでは、デプロイメントの前に環境の [&#x200B; バックアップ &#x200B;](../storage/snapshots.md) を作成することをお勧めします。

また、[New Relicでデプロイメントを追跡 &#x200B;](../monitor/track-deployments.md) を有効にしてデプロイメントイベントを監視し、デプロイメント間のパフォーマンスを分析することもできます。

## スターターデプロイメントフロー

Adobeでは、スタータープランの開発とデプロイメントを最適にサポートするために、`staging` ブランチから `master` ブランチを作成することをお勧めします。 次に、4 つのアクティブな環境（実稼動用 `master` とステージング用 `staging`）の 2 つが準備されています。

このプロセスについて詳しくは、[&#x200B; スターターの開発とデプロイのワークフロー &#x200B;](../architecture/starter-develop-deploy-workflow.md) を参照してください。

## プロデプロイメントフロー

Pro には、2 つのアクティブなブランチ（グローバルブランチ、ステージングおよび実稼動ブランチ）を持つ大規模な統合環境が `master` 意されています。 プロジェクトを作成すると、サイトの構築とデプロイのために、コードを分岐、開発、プッシュする準備が整います。 統合環境には多数のブランチを含めることができますが、ステージング環境と実稼動環境のブランチは各環境に 1 つだけです。

このプロセスについて詳しくは、「[Pro 開発およびデプロイワークフロー &#x200B;](../architecture/pro-develop-deploy-workflow.md)」を参照してください。

## コードをステージングにデプロイ

このステージング環境は、データベース、web サーバー、Fastly やNew Relicなどのすべてのサービスを含む、実稼動環境に近い環境を提供します。 [[!DNL Cloud Console]](../project/overview.md) または [Cloud CLI コマンド &#x200B;](../dev-tools/cloud-cli-overview.md) を使用して、ターミナルアプリケーションから完全にプッシュ、結合およびデプロイできます。

### [!DNL Cloud Console] を使用したコードのデプロイ

[!DNL Cloud Console] は、スタータープランと Pro プランの統合、ステージングおよび実稼動環境で、コードを作成、管理およびデプロイする機能を提供します。

**Pro プロジェクトの場合は、統合ブランチをステージングにデプロイします**。

1. プロジェクトに [&#x200B; ログイン &#x200B;](https://accounts.magento.cloud) します。
1. `integration` ブランチを選択します。
1. **結合** オプションを選択して、ステージング環境にデプロイします。

   ![&#x200B; 結合 &#x200B;](../../assets/button-merge.png){width="150"}

1. ステージング環境ですべての [&#x200B; テスト &#x200B;](../test/staging-and-production.md) を完了します。
1. ステージングの準備が整ったら、「**結合**」オプションを選択して、実稼動環境にデプロイします。

**スターターの場合、開発ブランチをステージングにデプロイします**。

1. プロジェクトに [&#x200B; ログイン &#x200B;](https://accounts.magento.cloud) します。
1. 準備済みコードブランチを選択します。
1. **結合** オプションを選択して、ステージング環境にデプロイします。

   ![&#x200B; 結合 &#x200B;](../../assets/button-merge.png){width="150"}

1. ステージング環境ですべての [&#x200B; テスト &#x200B;](../test/staging-and-production.md) を完了します。
1. ステージングの準備が整ったら、「**結合**」オプションを選択して、実稼動環境（`master`）にデプロイします。

### コマンドラインでコードをデプロイします。

Cloud CLI には、コードをデプロイするコマンドが用意されています。 プロジェクトに SSH および Git アクセス権が必要です。

#### 手順 1：統合環境のデプロイとテスト

1. プロジェクトにログインしたら、統合環境をチェックアウトします。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. ローカル統合環境をリモート環境と同期します。

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. 環境のスナップショットをバックアップとして作成します。

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. 必要に応じて、ローカルブランチのコードを更新します。

1. 環境に対する変更を追加、コミット、プッシュします。

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. 完全なサイトテスト。

#### 手順 2：変更をステージングとデプロイに結合する

1. ステージング環境をチェックアウトします。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. ローカルのステージング環境をリモート環境と同期します。

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. 環境のスナップショットをバックアップとして作成します。

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. デプロイする統合環境をステージング環境に結合します。

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. 完全なサイトテスト。

#### 手順 3：実稼動環境へのデプロイ

1. ローカルの実稼動環境のスナップショットをチェックアウト、同期、作成します。

1. ステージング環境を実稼動環境に結合してデプロイします。

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. 完全なサイトテスト。

## 静的ファイルの移行

[&#x200B; 静的ファイル &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/glossary) は `mounts` に保存されます。 ローカル環境などのソース・マウントの場所からターゲット・マウントの場所にファイルを移行する方法は 2 つあります。 どちらの方法でも `rsync` ユーティリティが使用されますが、Adobeでは、ローカル環境とリモート環境の間でファイルを移動するには `magento-cloud` CLI を使用することをお勧めします。 また、Adobeでは、リモートソースから別のリモートの場所にファイルを移動する場合に、`rsync` の方法を使用することをお勧めします。

### CLI を使用したファイルの移行

`mount:upload` および `mount:download` CLI コマンドを使用して、ローカル環境とリモート環境の間でファイルを移行できます。 どちらのコマンドも `rsync` ユーティリティを使用しますが、CLI コマンドは Cloud Infrastructure 環境のAdobe Commerceに合わせたオプションとプロンプトを提供します。 たとえば、オプションを指定せずに単純なコマンドを使用した場合、アップロードまたはダウンロードするマウントを選択するよう CLI によって求められます。

```bash
magento-cloud mount:download
```

応答の例：

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**現在の環境のローカル `pub/media/` フォルダーからリモート `pub/media/` フォルダーにファイルをアップロードするには**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

応答の例：

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

`--help` コマンドと `mount:upload` コマンドの `mount:download` オプションを使用して、その他のオプションを表示します。 例えば、移行中に不要なファイルを削除する `--delete` のオプションがあります。

### rsync を使用したファイルの移行

または、`rsync` ユーティリティを使用してファイルをマイグレーションすることもできます。

```bash
rsync -azvP <source> <destination>
```

このコマンドは、次のオプションを使用します。

- `a`-archive
- 移行中にファイルを `z` 圧縮
- `v`-verbose
- `P` 部的な進歩

[rsync](https://linux.die.net/man/1/rsync) ヘルプを参照してください。

>[!NOTE]
>
>リモートからリモート環境に直接メディアを転送するには、SSH エージェント転送を有効にする必要があります。[GitHub ガイダンス &#x200B;](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding) を参照してください。

**静的ファイルをリモート環境からリモート環境に直接移行する（高速アプローチ）**

1. SSH を使用してソース環境にログインします。 `magento-cloud` CLI は使用しないでください。 `-A` オプションを使用すると、認証エージェント接続の転送が有効になるので、このオプションは重要です。

   >[!TIP]
   >
   >**で** SSH アクセス [!DNL Cloud Console] リンクを見つけるには、環境を選択して **サイトにアクセス** をクリックします。

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. `rsync` コマンドを使用して、ソース環境から別のリモート環境に `pub/media` ディレクトリをコピーします。

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. 他のリモート環境にログインして、ファイルが正常に移行されたことを確認します。

## データベースの移行

>[!WARNING]
>
>データベースのインポートおよびエクスポート操作には時間がかかることがあり、サイトのパフォーマンスと可用性に影響を与える可能性があります。 本番サイトでのパフォーマンスの低下や停止を防ぐため、ピーク以外の時間帯にインポートおよびエクスポート操作をスケジュールします。

>[!BEGINSHADEBOX]

**前提条件：** データベース・ダンプ（手順 3 を参照）にデータベース・トリガーが含まれている必要があります。 ダンプする場合は、[トリガー権限 &#x200B;](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger) があることを確認します。

>[!IMPORTANT]
>
>統合環境データベースは開発テスト専用であり、ステージング環境や実稼動環境に移行しないデータを含めることができます。

継続的な統合デプロイメントの場合、Adobeは、統合からステージング環境および実稼動環境にデータを移行する **お勧めしません**。 テストデータに合格したり、重要なデータを上書きしたりできます。 重要な設定は、ビルドおよびデプロイ時に [&#x200B; 設定ファイル &#x200B;](../store/store-settings.md) とコマンド `setup:upgrade` 使用して渡されます。

>[!ENDSHADEBOX]

Adobe **お勧めします** データを実稼動環境からステージング環境に移行して、サイトを完全にテストし、すべてのサービスと設定を使用してほぼ実稼動環境に保存します。

>[!NOTE]
>
>リモート環境からリモート環境に直接メディアを転送するには、ssh エージェント転送を有効にする必要があります。[GitHub ガイダンス &#x200B;](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding) を参照してください。

### データベースのバックアップ

データベースのバックアップを作成することをお勧めします。 次の手順では、[&#x200B; データベースのバックアップ &#x200B;](../storage/database-dump.md) のガイダンスを使用しています。

**データベースをダンプするには**:

1. [SSH を使用して、コピーするデータベースが含まれているリモート環境にログインします &#x200B;](../development/secure-connections.md#use-an-ssh-command)。

1. 環境の関係を一覧表示し、データベースのログイン情報をメモします。

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   ステージング環境および実稼動環境の場合、データベースの名前は `MAGENTO_CLOUD_RELATIONSHIPS` 変数に入力します（通常、アプリケーション名およびユーザー名と同じ）。

1. データベースのバックアップを作成します。 DB ダンプのターゲット・ディレクトリを選択するには、`--dump-directory` オプションを使用します。

   スターター環境および Pro 統合環境の場合は、データベースの名前として `main` を使用します。

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   ダンプオプション：
   - `--dump-directory=<dir>` - データベース ダンプのターゲット ディレクトリを選択します
   - `--remove-definers` - データベース・ダンプから DEFINER 文を削除します。

1. ECE ツール方式をお勧めしますが、別の方法として、ネイティブの MySQL を使用して GZIP 形式でデータベースダンプファイルを作成する方法もあります。

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   ターゲット環境で二要素認証を設定した場合は、関連する 2FA テーブルを除外して、データベース移行後に再設定しないようにすることをお勧めします。

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. `logout` と入力して、SSH 接続を終了します。

### データベースを削除して再作成します

データをインポートする場合、データベースをドロップして作成する必要があります。

**データベースを削除して再作成するには、次の手順に従います**。

1. リモート環境への [SSH トンネル &#x200B;](../development/secure-connections.md#ssh-tunneling) を確立します。

1. データベースサービスに接続します。

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. `MariaDB [main]>` プロンプトで、データベースをドロップします。

   スターターと Pro の統合の場合：

   ```shell
   drop database main;
   ```

   実稼動環境とステージング環境の場合：

   ```shell
   drop database <database_name>;
   ```

1. データベースを再作成します。

   スターターと Pro の統合の場合：

   ```shell
   create database main;
   ```

1. データベースを読み込みます。

   実稼動用に読み込み：

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   ステージング用にインポート :

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   これらのコマンドは、データベース・ダンプ・ファイルを解凍し、`DEFINER` 文を削除し、指定された資格証明を使用してデータベースをインポートします。
