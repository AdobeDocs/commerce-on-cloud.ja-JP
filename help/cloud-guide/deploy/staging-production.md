---
title: ステージングおよび実稼動へのデプロイ
description: Adobe Commerce on cloud インフラストラクチャコードをステージング環境および実稼動環境にデプロイして、さらにテストを行う方法について説明します。
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
TQID: https://experienceleague.adobe.com/SJZ2BuPEe6QsgkPyODiZx6118qd6vxh72r3nVuPrLnM
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: bd989d82-1e15-4534-88db-f1f51dd77ffaid: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 1374
ht-degree: 0%

---

# ステージングおよび実稼動へのデプロイ

デプロイと本番稼動のプロセスは、開発から始まり、ステージングに続き、本番稼動に移行して終了します。 Adobeは、構成の一貫性を確保するためのエンドツーエンドの環境ソリューションを提供します。 あらゆる環境で、ストアフロントへの直接URL アクセスと、CLI コマンドの管理者およびSSH アクセスがサポートされています。

ストアをデプロイする準備ができたら、実稼動にデプロイする前に、ステージング環境でのデプロイメントとテストを完了する必要があります。 この節では、ビルドとデプロイのプロセス、データとコンテンツの移行、テストについて詳しい手順と情報を提供します。

>[!TIP]
>
>Adobeでは、デプロイメントの前に環境の[ バックアップ ](../storage/snapshots.md)を作成することをお勧めします。

また、[New Relic](../monitor/track-deployments.md)でデプロイメントを追跡を有効にして、デプロイメントイベントを監視し、デプロイメント間のパフォーマンスを分析できます。

## スターターのデプロイメントフロー

Adobeでは、スタータープランの開発と展開を最大限にサポートするために、`master` ブランチから`staging` ブランチを作成することをお勧めします。 次に、4つのアクティブな環境のうち2つを準備します。実稼動用には`master`、ステージング用には`staging`。

プロセスの詳細については、[ スターターワークフローの開発とデプロイ ](../architecture/starter-develop-deploy-workflow.md)を参照してください。

## プロのデプロイメントフロー

Proには、アクティブな2つのブランチ、グローバル `master` ブランチ、ステージング、実稼動ブランチを備えた大規模な統合環境が付属しています。 プロジェクトを作成すると、サイトの構築とデプロイのためにコードをブランチ、開発、プッシュする準備が整います。 統合環境には多数のブランチを含めることができますが、ステージング環境と実稼動環境には、各環境に対して1つのブランチしか含まれていません。

プロセスの詳細については、[Pro ワークフローの開発とデプロイ ](../architecture/pro-develop-deploy-workflow.md)を参照してください。

## ステージングへのコードのデプロイ

ステージング環境では、データベース、web サーバー、およびFastlyやNew Relicを含むすべてのサービスを含む、ほぼ実稼動環境を提供します。 ターミナルアプリケーションを使用して、[[!DNL Cloud Console]](../project/overview.md)または[Cloud CLI コマンド ](../dev-tools/cloud-cli-overview.md)を通じて、完全にプッシュ、結合、デプロイできます。

### [!DNL Cloud Console]を使用したコードのデプロイ

[!DNL Cloud Console]には、スタータープランとPro プランの統合、ステージング、実稼動環境でコードを作成、管理、デプロイする機能が用意されています。

**Pro プロジェクトの場合、統合ブランチをステージング**&#x200B;にデプロイします。

1. [ プロジェクトに](https://accounts.magento.cloud) ログインします。
1. `integration` ブランチを選択します。
1. 「**結合**」オプションを選択して、ステージングにデプロイします。

   ![結合](../../assets/button-merge.png){width="150"}

1. ステージング環境のすべての[ テスト ](../test/staging-and-production.md)を完了します。
1. ステージングの準備ができたら、**結合** オプションを選択して実稼動環境にデプロイします。

**Starterの場合、開発ブランチをステージング**&#x200B;にデプロイします。

1. [ プロジェクトに](https://accounts.magento.cloud) ログインします。
1. 準備されたコードブランチを選択します。
1. 「**結合**」オプションを選択して、ステージングにデプロイします。

   ![結合](../../assets/button-merge.png){width="150"}

1. ステージング環境のすべての[ テスト ](../test/staging-and-production.md)を完了します。
1. ステージングの準備ができたら、**結合** オプションを選択して、実稼動環境（`master`）にデプロイします。

### コマンドラインを使用したコードのデプロイ

Cloud CLIには、コードをデプロイするコマンドが用意されています。 プロジェクトにはSSHとGitのアクセス権が必要です。

#### 手順1：統合環境のデプロイとテスト

1. プロジェクトにログインした後、統合環境を確認します。

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

1. 環境に変更を追加、コミット、およびプッシュします。

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. 包括的なサイトテスト：

#### 手順2：変更をステージングに統合してデプロイする

1. ステージング環境を確認します。

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

1. 統合環境をステージングに統合してデプロイします。

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. 包括的なサイトテスト：

#### 手順3：実稼動環境へのデプロイ

1. ローカルの実稼動環境のスナップショットをチェックアウト、同期、作成します。

1. ステージング環境を実稼動環境にマージしてデプロイします。

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. 包括的なサイトテスト：

## 静的ファイルの移行

[静的ファイル ](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)は`mounts`に保存されています。 ローカル環境などのソースマウントの場所から目的のマウントの場所にファイルを移行する方法は2つあります。 どちらの方法も`rsync` ユーティリティを使用しますが、Adobeでは、`magento-cloud` CLIを使用してローカル環境とリモート環境の間でファイルを移動することをお勧めします。 また、Adobeでは、ファイルをリモート ソースから別のリモート場所に移動する場合は、`rsync` メソッドを使用することをお勧めします。

### CLIを使用したファイルの移行

`mount:upload`および`mount:download`のCLI コマンドを使用して、ローカル環境とリモート環境の間でファイルを移行できます。 どちらのコマンドも`rsync` ユーティリティを使用しますが、CLI コマンドは、Adobe Commerce on cloud infrastructure環境に合わせたオプションとプロンプトを提供します。 例えば、オプションなしでsimple コマンドを使用する場合、アップロードまたはダウンロードするマウントまたはマウントを選択するよう求めるプロンプトがCLIから表示されます。

```bash
magento-cloud mount:download
```

回答サンプル：

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

**ローカル `pub/media/` フォルダーから現在の環境**&#x200B;のリモート `pub/media/` フォルダーにファイルをアップロードするには：

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

回答サンプル：

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

`mount:upload`および`mount:download` コマンドの`--help` オプションを使用して、その他のオプションを表示します。 例えば、移行中に無関係なファイルを削除する`--delete` オプションがあります。

### rsyncを使用したファイルの移行

または、`rsync` ユーティリティを使用してファイルを移行することもできます。

```bash
rsync -azvP <source> <destination>
```

このコマンドでは、次のオプションが使用されます。

- `a` – アーカイブ
- 移行中に`z` – ファイルを圧縮
- `v` – 詳細
- `P`部分の進捗

[rsync](https://linux.die.net/man/1/rsync)のヘルプを参照してください。

>[!NOTE]
>
>リモートからリモート環境にメディアを直接転送するには、SSH エージェント転送を有効にする必要があります。[GitHub ガイダンス ](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)を参照してください。

**リモート環境からリモート環境に静的ファイルを直接移行するには（高速アプローチ）**:

1. SSHを使用してソース環境にログインします。 `magento-cloud` CLIは使用しないでください。 認証エージェント接続の転送を有効にするため、`-A` オプションの使用は重要です。

   >[!TIP]
   >
   >[!DNL Cloud Console]で&#x200B;**SSH アクセス** リンクを見つけるには、環境を選択して「**サイトにアクセス**」をクリックします。

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. `rsync` コマンドを使用して、`pub/media` ディレクトリをソース環境から別のリモート環境にコピーします。

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. 他のリモート環境にログインして、正常に移行されたファイルを確認します。

## データベースの移行

>[!WARNING]
>
>データベースの読み込みと書き出しの操作には時間がかかり、サイトのパフォーマンスと可用性に影響を与える可能性があります。 オフピーク時にインポートおよびエクスポート操作をスケジュールして、本番サイトのパフォーマンスの低下や停止を防ぎます。

>[!BEGINSHADEBOX]

**前提条件：** データベース ダンプ （手順3を参照）には、データベース トリガーを含める必要があります。 ダンプする場合は、[トリガー権限](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger)があることを確認してください。

>[!IMPORTANT]
>
>統合環境データベースは開発テスト用に厳密に作成されており、ステージングおよび実稼動に移行しないデータを含めることができます。

継続的インテグレーションのデプロイメントの場合、Adobe **はインテグレーションからステージングおよび実稼動へのデータの移行を**&#x200B;お勧めしません。 テストデータを渡したり、重要なデータを上書きしたりできます。 重要な設定はすべて、ビルドおよびデプロイ時に[設定ファイル ](../store/store-settings.md)および`setup:upgrade` コマンドを使用して渡されます。

>[!ENDSHADEBOX]

Adobe **では、すべてのサービスと設定を使用して、ほぼ実稼働環境でサイトとストアを完全にテストするために、実稼働環境からステージングにデータを移行することを**&#x200B;お勧めします。

>[!NOTE]
>
>リモートからリモート環境にメディアを直接転送するには、ssh エージェント転送を有効にする必要があります。[GitHub ガイダンス ](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)を参照してください。

### データベースのバックアップ

データベースのバックアップを作成することをお勧めします。 次の手順では、[ データベースをバックアップ ](../storage/database-dump.md)のガイダンスを使用します。

**データベースをダンプするには**:

1. [SSHを使用して、コピーするデータベースを含むリモート環境](../development/secure-connections.md#use-an-ssh-command)にログインします。

1. 環境の関係を一覧表示し、データベースのログイン情報をメモします。

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Pro ステージングおよび実稼動の場合、データベースの名前は`MAGENTO_CLOUD_RELATIONSHIPS`変数に含まれます（通常は、アプリケーション名とユーザー名と同じです）。

1. データベースのバックアップを作成します。 DB ダンプのターゲットディレクトリを選択するには、`--dump-directory` オプションを使用します。

   スターター環境とPro統合環境の場合は、データベースの名前として`main`を使用します。

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   ダンプオプション：
   - `--dump-directory=<dir>` - データベース ダンプのターゲット ディレクトリを選択します
   - `--remove-definers` - データベース ダンプからDEFINER ステートメントを削除します

1. ECE-Tools メソッドを使用することをお勧めしますが、別の方法として、GZIP形式でネイティブ MySQLを使用してデータベースダンプファイルを作成する方法があります。

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   ターゲット環境で2要素認証を設定している場合は、データベース移行後に再構成しないように、関連する2FA テーブルを除外することをお勧めします。

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. SSH接続を終了するには、`logout`と入力します。

### データベースをドロップして再作成します

データを読み込む場合は、データベースを削除して作成する必要があります。

**データベースを削除して再作成するには**:

1. リモート環境への[SSH トンネル ](../development/secure-connections.md#ssh-tunneling)を確立します。

1. データベースサービスに接続します。

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. `MariaDB [main]>` プロンプトで、データベースをドロップします。

   StarterとProの統合の場合：

   ```shell
   drop database main;
   ```

   実稼動環境とステージング環境の場合：

   ```shell
   drop database <database_name>;
   ```

1. データベースを再作成します。

   StarterとProの統合の場合：

   ```shell
   create database main;
   ```

1. データベースを読み込みます。

   実稼動用に読み込み：

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   ステージング用に読み込み：

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   これらのコマンドは、データベース ダンプ ファイルを解凍し、`DEFINER` ステートメントを削除し、指定された資格情報を使用してデータベースをインポートします。

