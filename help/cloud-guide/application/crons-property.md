---
title: Crons プロパティ
description: ' [!DNL Commerce] application 設定ファイルで「cron」プロパティを設定する方法の例を参照してください。'
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Crons プロパティ

Adobe Commerceは、`crons` プロパティを使用して、繰り返しアクティビティをスケジュールします。 1 日の特定の時間に実行する特定のタスクをスケジュールするのに最適です。 読み取り専用環境の性質上、クラウドインフラストラクチャプロジェクト上のAdobe Commerceの web インスタンスでは一度に 1 つの cron ジョブのみを実行できます。 長時間実行されるタスクをキューに入れられた小さなタスクに分類することをお勧めします。 または、[ ワーカーインスタンス ](workers-property.md) を作成することもできます。

Adobeでは、`crons` を [ ファイルシステムのオーナー ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) として実行することをお勧めします。 `crons` を `root` または Web サーバーユーザーとして実行 _ない_ でください。

この設定は、複数のデフォルト cron ジョブを持つAdobe Commerceのオンプレミスデプロイメントとは異なります。 _設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) の [cron ジョブの設定_ を参照してください。

## cron ジョブの設定

`crons` プロパティは、スケジュールに従ってトリガーされるプロセスを記述します。 各ジョブには、名前と次のオプションが必要です。

- `spec` - スケジュールに使用する Cron 式。
- `cmd` - `start` および `stop` で実行するコマンド。
- `shutdown_timeout` – （_オプション_） Cron ジョブがキャンセルされた場合、これはジョブまたはプロセスを停止するために SIGKILL シグナルが送信されるまでの秒数です。 デフォルト値は 10 秒です。
- `timeout` – （_オプション_） Cron ジョブがタイムアウトまでに実行できる最大時間。 デフォルトでは、許容される最大値の 86400 秒（24 時間）に設定されます。

デフォルトでは、すべてのCommerce クラウドプロジェクトの `.magento.app.yaml` ファイルに次のようなデフォルトの `crons` 設定があります。

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

プロジェクトにカスタム cron ジョブが必要な場合は、それらをデフォルトの `crons` 設定に追加できます。 [cron ジョブの作成 ](#build-a-cron-job) を参照してください。

### `crontab`

Adobe Commerceでは、ステージング環境と実稼動環境でのセルフサービスサー `crons` ス設定をサポートするために、Pro プロジェクトにのみ自動クローン設定オプションを追加しました。 このオプションが有効な場合は、`crontab` を使用して cron 設定を確認できます。 これは、スタータープロジェクトでは使用で _ません_。

`crontab` を使用して Pro プロジェクトの設定を確認できますが、Adobe Commerceでは、クラウドインフラストラクチャにデプロイされたサイトの cron ジョブを実行するために `crontab` を使用しません。

**Pro 環境での cron 設定を確認するには**:

1. [SSH](../development/secure-connections.md#use-an-ssh-command) を使用して、リモート環境にログインします。

1. スケジュールされた cron プロセスを一覧表示します。

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >`crontab -l` コマンドで `Command not found` エラーが返される場合（プロのステージング環境および実稼動環境のみ）は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、プロジェクトで自動クローンのセルフサービス設定オプションを有効にする必要があります。

次の例は、デフォルトの `crons` 設定のみを持つ環境の `crontab` の出力を示しています。

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Cron ジョブの作成

cron ジョブには、スケジュールとタイミングの仕様、およびスケジュールされた時刻に実行するコマンドが含まれます。 スターター環境および Pro `integration` 環境の場合、最小間隔は 5 分に 1 回です。 ステージング環境および実稼動環境の場合、最小間隔は 1 分あたり 1 回です。 クラウドインフラストラクチャー上のAdobe Commerceでは、`crons` セクションの `.magento.app.yaml` ファイルにカスタム cron ジョブを追加します。 一般的な形式は、実行するコマンドまたはカスタムスクリプトを指定するた `cmd` のスケジュールと `spec` です。

### 仕様

Adobe Commerceでは、`crons` 仕様（spec）に 5 つの値を持つ式を使用します：`* * * * *`

1. 分（0～59）すべての Starter 環境および Pro 環境で、cron ジョブでサポートされる最小頻度は 5 分です。 管理者による設定が必要になる場合があります。
2. 時間（0 ～ 23）
3. 日付（1 ～ 31）
4. 月（1 ～ 12）
5. 曜日（0～6）（日曜日～土曜日、一部のシステムでは 7 も日曜日）

次に例を示します。

- `00 */3 * * *` は 3 時間ごとに最初の分（午前 12 時、午前 3 時、午前 6 時）に実行されます
- `20 */8 * * *` は 8 時間ごとに分 20 に実行されます（午前 12 時 20 分、午前 8 時 20 分、午後 4 時 20 分）。
- `00 00 * * *` は 1 日 1 回深夜に実行されます
- `00 * * * 1` は週に 1 回、月曜日の深夜に実行されます。

>[!NOTE]
>
>`.magento.app.yaml` ファイルで指定される `crons` 時は、データベースのストア設定値で指定されたタイムゾーンではなく、サーバーのタイムゾーンに基づきます。

スケジュールを決定する際は、タスクの完了に要する時間を考慮します。 例えば、3 時間ごとにジョブを実行し、タスクが完了するまでに 40 分かかる場合は、スケジュールされたタイミングの変更を検討できます。

### コマンド

`cmd` は、実行するコマンドまたはカスタムスクリプトを指定します。 コマンドスクリプトの形式には、次のものが含まれます。

```text
<path-to-php-binary> <project-dir>/<script-command>
```

例：

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

この例では、`<path-to-php-binary>` は `/usr/bin/php` です。 インストールディレクトリ（プロジェクト ID を含む）は `/app/abc123edf890/bin/magento`、スクリプトアクションは `export:start catalog_category_product` です。

### プロジェクトへのカスタム cron ジョブの追加

クラウドインフラストラクチャプラットフォーム上のAdobe Commerceでは、[`.magento.app.yaml`](../application/configure-app-yaml.md) ファイルの `crons` セクションにカスタマイズを追加できます。

>[!NOTE]
>
>スターター環境および Pro `integration` 環境の場合、最小間隔は 5 分に 1 回です。 ステージング環境および実稼動環境の場合、最小間隔は 1 分あたり 1 回です。 デフォルトの最小間隔よりも頻繁な間隔を設定することはできません。

Adobe Commerce Pro プロジェクトでは、`.magento.app.yaml` ファイルを使用してステージング環境と実稼動環境にカスタム cron ジョブを追加する前に、プロジェクトで [ 自動クローン機能 ](#set-up-cron-jobs) を有効にする必要があります。 この機能が有効になっていない場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して自動クローンを有効にします。

**カスタム cron ジョブを追加するには**

1. ローカル開発環境で、Adobe Commerce `/app` ディレクトリの `.magento.app.yaml` ファイルを編集します。

1. `crons` セクションで、次の形式を使用してカスタマイズを追加します。

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   次の例では、`productcatalog` ジョブによって、製品カタログが 8 時間ごとに、1 時間後に 20 分後に書き出されています。

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### cron ジョブの更新

カスタマイズされたジョブを追加、削除、または更新するには、`.magento.app.yaml` ファイルの `crons` セクションで設定を変更します。 次に、変更をステージング環境と実稼動環境にプッシュする前に、リモート `integration` 環境で更新をテストします。

## Cron ジョブの無効化

パフォーマンスの問題を防ぐために、インデックスの再作成やキャッシュのクリーニングなどのメンテナンスタスクを完了する前に、cron ジョブを手動で無効にすることができます。 `ece-tools` CLI コマンド `cron:disable` を使用すると、すべての cron ジョブを無効化し、アクティブな cron プロセスを停止できます。

**cron ジョブを無効にするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. cron ジョブを無効にし、アクティブな cron プロセスを停止します。

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. 必要なメンテナンスタスクを完了したら、cron ジョブを再度有効にします。

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## cron ジョブのトラブルシューティング

Adobeは、クラウドインフラストラクチャプラットフォーム上のAdobe Commerce上で cron 処理を最適化し、cron 関連の問題を修正するために、Adobe Commerce on cloud infrastructure パッケージを更新しました。 Cron 処理で問題が発生した場合は、プロジェクトで最新バージョンの `ece-tools` パッケージが使用されていることを確認します。 [ECE ツールの更新 ](../dev-tools/update-package.md) を参照してください。

各環境のアプリケーションレベルのログファイルで、cron 処理情報を確認できます。 [ アプリケーションログ ](../test/log-locations.md#application-logs) を参照してください。

cron 関連の問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [Cron タスクは他のグループからタスクをロックする ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [ クラウドでスタックした cron ジョブを手動でリセットする ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
