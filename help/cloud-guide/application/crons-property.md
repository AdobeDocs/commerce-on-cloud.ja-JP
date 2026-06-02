---
title: Crons プロパティ
description: ' [!DNL Commerce]  アプリケーション設定ファイルで「crons」プロパティを設定する方法の例を参照してください。'
feature: Cloud, Configuration
exl-id: ff176cb1-5b6c-48a0-ad3c-56cc1d606c97
TQID: https://experienceleague.adobe.com/E7qXe1VmZezG9AqJ2rchTUmbTibU0pNaGdqb00MkcXo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1172
ht-degree: 0%

---

# Crons プロパティ

Adobe Commerceは`crons` プロパティを使用して、繰り返し作業をスケジュールします。 特定のタスクを特定の時間帯に実行するようにスケジュールを設定するのに最適です。 読み取り専用の環境の性質により、Adobe Commerce on cloud infrastructure プロジェクトのweb インスタンスで一度に1つのcron ジョブのみを実行できます。 長時間実行しているタスクを、キューに登録されている小さなタスクに分割することをお勧めします。 または、[&#x200B; ワーカーインスタンス &#x200B;](workers-property.md)を構築することもできます。

Adobeでは、`crons`を[&#x200B; ファイルシステム所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)として実行することをお勧めします。 _not_&#x200B;は、`crons`を`root`として、またはweb サーバーユーザーとして実行します。

この設定は、複数のデフォルトのcron ジョブを持つAdobe Commerceのオンプレミスのデプロイメントとは異なります。 _設定ガイド_&#x200B;の「[cron ジョブの設定](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html)」を参照してください。

## cron ジョブの設定

`crons` プロパティは、スケジュールでトリガーされるプロセスを説明します。 各ジョブには名前と次のオプションが必要です。

- `spec` - スケジュールに使用されるcron式。
- `cmd` - `start`および`stop`で実行するコマンド。
- `shutdown_timeout` – （_オプション_） cron ジョブがキャンセルされた場合、これは、ジョブまたはプロセスを停止するためにSIGKILL シグナルが送信されるまでの秒数です。 デフォルトは10秒です。
- `timeout` – （_オプション_） cron ジョブがタイムアウトまでに実行できる最大時間です。 デフォルトでは、許可される最大値は86400秒（24時間）です。

デフォルトでは、すべてのCommerce クラウドプロジェクトには、`.magento.app.yaml` ファイルに次のデフォルト `crons`設定があります。

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

プロジェクトでカスタム cron ジョブが必要な場合は、それらをデフォルトの`crons`設定に追加できます。 [Cron ジョブの作成](#build-a-cron-job)を参照してください。

### `crontab`

Adobe Commerceでは、ステージング環境と実稼動環境でセルフサービス `crons`設定をサポートするために、Pro プロジェクトにのみ自動クローン設定オプションが追加されました。 このオプションが有効になっている場合は、`crontab`を使用してcron設定を確認できます。 これは、スタータープロジェクトで利用できる&#x200B;_not_&#x200B;です。

`crontab`を使用してPro プロジェクトの設定を確認できますが、Adobe Commerceでは、クラウド インフラストラクチャにデプロイされたサイトのcron ジョブの実行に`crontab`を使用しません。

**Pro環境でのcron設定を確認するには**:

1. [SSH](../development/secure-connections.md#use-an-ssh-command)を使用してリモート環境にログインします。

1. スケジュールされたcron プロセスのリストを表示します。

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >`crontab -l` コマンドで`Command not found` エラーが返された場合（Pro ステージング環境および実稼動環境のみ）、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信して、プロジェクトで自動crons セルフサービス設定オプションを有効にする必要があります。

次の例は、デフォルトの`crons`設定のみを持つ環境の`crontab`出力を示しています。

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## cron ジョブの作成

cron ジョブには、スケジュールとタイミングの指定と、スケジュールされた時間に実行するコマンドが含まれます。 スターター環境とPro `integration`環境の場合、最小間隔は5分に1回です。 Pro ステージング環境および実稼動環境の場合、最小間隔は1分あたり1回です。 Adobe Commerce クラウド インフラストラクチャでは、`crons` セクションの`.magento.app.yaml` ファイルにカスタム cron ジョブを追加します。 スケジュール設定の一般的な形式は`spec`で、実行するコマンドまたはカスタムスクリプトを指定する形式は`cmd`です。

### 仕様

Adobe Commerceは、`crons`仕様（仕様）に5値式を使用しています：`* * * * *`

1. Minute （0 ～ 59）すべてのStarterおよびPro環境で、cron ジョブでサポートされる最小頻度は5分です。 管理者の設定が必要な場合があります。
2. 時間（0 ～ 23）
3. 月の日（1 ～ 31）
4. 月（1 ～ 12）
5. 曜日（0～6）（日曜日～土曜日、7は一部のシステムでは日曜日）

一部の例：

- `00 */3 * * *`は、最初の1分（午前12:00、午前3:00、午前6:00）に3時間ごとに実行されます
- `20 */8 * * *`は20分（午前12:20、午前8:20、午後4:20）に8時間ごとに実行されます
- `00 00 * * *`が1日1回、午前0時に実行されます
- `00 * * * 1`は週1回、月曜日の午前0時に実行されます。

>[!NOTE]
>
>`.magento.app.yaml` ファイルで指定された`crons`時間は、データベースのストア設定値で指定されたタイムゾーンではなく、サーバーのタイムゾーンに基づいています。

スケジュールを決定する際には、タスクを完了するのにかかる時間を考慮します。 例えば、3時間ごとにジョブを実行し、タスクの完了に40分かかる場合、スケジュールされたタイミングを変更することを検討します。

### コマンド

`cmd`は、実行するコマンドまたはカスタムスクリプトを指定します。 コマンドスクリプト形式には、次のものが含まれます。

```text
<path-to-php-binary> <project-dir>/<script-command>
```

例：

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

この例では、`<path-to-php-binary>`は`/usr/bin/php`です。 プロジェクト IDを含むインストール ディレクトリは`/app/abc123edf890/bin/magento`、スクリプト アクションは`export:start catalog_category_product`です。

### プロジェクトにカスタム cron ジョブを追加する

Adobe Commerce on cloud infrastructure platformでは、[`.magento.app.yaml`](../application/configure-app-yaml.md) ファイルの`crons` セクションにカスタマイズを追加できます。

>[!NOTE]
>
>スターター環境とPro `integration`環境の場合、最小間隔は5分に1回です。 Pro ステージング環境および実稼動環境の場合、最小間隔は1分あたり1回です。 デフォルトの最小値より多くの頻度を設定することはできません。

Adobe Commerce Pro プロジェクトでは、`.magento.app.yaml` ファイルを使用してステージング環境と実稼動環境にカスタム cron ジョブを追加する前に、[自動cron機能](#set-up-cron-jobs)をプロジェクトで有効にする必要があります。 この機能が有効になっていない場合は、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信して自動実行を有効にします。

**カスタム cron ジョブを追加するには**:

1. ローカル開発環境で、Adobe Commerce `/app` ディレクトリの`.magento.app.yaml` ファイルを編集します。

1. 「`crons`」セクションで、次の形式を使用してカスタマイズを追加します。

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   次の例では、`productcatalog` ジョブは、8時間ごとに、1時間後20分ごとに製品カタログを書き出します。

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### cron ジョブの更新

カスタマイズされたジョブを追加、削除、または更新するには、`.magento.app.yaml` ファイルの`crons` セクションで設定を変更します。 次に、ステージング環境と実稼動環境に変更をプッシュする前に、リモート `integration`環境で更新をテストします。

## cron ジョブを無効にする

パフォーマンスの問題を防ぐために、インデックスの再作成やキャッシュのクリーニングなどのメンテナンスタスクを完了する前に、cron ジョブを手動で無効にすることができます。 `ece-tools` CLI コマンド `cron:disable`を使用して、すべてのcron ジョブを無効にし、アクティブなcron プロセスを停止できます。

**cron ジョブを無効にするには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. cron ジョブを無効にし、アクティブなcron プロセスを停止します。

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. 必要なメンテナンスタスクを完了したら、cron ジョブを再度有効にします。

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## cron ジョブのトラブルシューティング

Adobeは、Adobe Commerce on cloud infrastructure パッケージを更新し、Adobe Commerce on cloud infrastructure プラットフォームでのcron処理を最適化し、cron関連の問題を修正しました。 cron処理で問題が発生した場合は、プロジェクトで最新バージョンの`ece-tools` パッケージが使用されていることを確認してください。 [ECE-Toolsの更新](../dev-tools/update-package.md)を参照してください。

cron処理情報は、各環境のアプリケーションレベルのログファイルで確認できます。 [&#x200B; アプリケーションログ &#x200B;](../test/log-locations.md#application-logs)を参照してください。

cron関連の問題のトラブルシューティングについては、次のAdobe Commerce サポート記事を参照してください。

- [Cron タスクは、他のグループからタスクをロックします](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [クラウド上でスタックしたcron ジョブを手動でリセットする](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
