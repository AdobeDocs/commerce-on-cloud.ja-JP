---
title: ログの表示と管理
description: クラウドインフラストラクチャで使用可能なログファイルの種類と、ログファイルを検索する場所について説明します。
last-substantial-update: 2023-05-23T00:00:00.000Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
TQID: https://experienceleague.adobe.com/VAsmOv6sBa37A2IAubUnWd4UAMRIuKTNt8JGKNJlrCI
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1287
ht-degree: 0%

---

# ログの表示と管理

クラウドインフラストラクチャプロジェクト上のAdobe Commerceのログは、[ フックのビルドとデプロイ ](../application/hooks-property.md)、クラウドサービス、Adobe Commerce アプリケーションに関する問題のトラブルシューティングに役立ちます。

ファイルシステム、[!DNL Cloud Console]および`magento-cloud` CLIからログを表示できます。

- **ファイルシステム** - `/var/log` システムディレクトリには、すべての環境のログが含まれています。 `var/log/` ディレクトリには、特定の環境に固有のアプリ固有のログが含まれています。 これらのディレクトリは、クラスター内のノード間で共有されません。 Pro実稼動環境とステージング環境では、各ノードのログを確認する必要があります。

- **[!DNL Cloud Console]** – 環境&#x200B;_メッセージ_ リストで、ビルド、デプロイ、デプロイ後のログ情報を確認できます。

- **Cloud CLI**- `magento-cloud log` コマンドを使用してローカル環境ログを表示するか、`magento-cloud ssh` コマンドを使用してリモート環境ログを表示できます。

## ログの場所

システムログは、次の場所に保存されます。

- 統合：`/var/log/<log-name>.log`
- プロステージング：`/var/log/platform/<project-ID>_stg/<log-name>.log`
- プロプロダクション：`/var/log/platform/<project-ID>/<log-name>.log`

`<project-ID>`の値は、プロジェクトと、環境がステージング環境か実稼動環境かによって異なります。 例えば、プロジェクト IDが`yw1unoukjcawe`の場合、ステージング環境ユーザーは`yw1unoukjcawe_stg`、実稼動環境ユーザーは`yw1unoukjcawe`です。

この例を使用すると、デプロイログは次のとおりです。`/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### 特定のエラーログレコードの検索

特定のログレコード番号（`475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9`など）でエラーが発生した場合は、次の方法を使用してCommerce application server remote environment logsをクエリすることで、レコードを見つけることができます。

>[!NOTE]
>
>Secure Shell （SSH）を使用してCommerce アプリケーションのリモート環境ログにアクセスする手順については、[ リモート環境へのセキュアな接続](../development/secure-connections.md)を参照してください。

#### 方法1:grepを使用して検索する

```bash
# Search for the specific error record in all log files
magento-cloud ssh -e <environment-ID> "grep -r '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' /var/log/"

# Search in specific log files
magento-cloud ssh -e <environment-ID> "grep '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' /var/log/exception.log"
```

#### 方法2：アーカイブされたログ内の検索

過去にエラーが発生した場合は、アーカイブされたログファイルを確認します。

```bash
# Search in compressed log files
magento-cloud ssh -e <environment-ID> "find /var/log -name '*.gz' -exec zgrep '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' {} \;"
```

#### 方法3:New Relicを使用する（Pro環境）

Pro実稼動環境およびステージング環境では、New Relic ログを使用して特定のエラーレコードを検索します。 詳しくは、[New Relic ログ管理](../monitor/log-management.md)を参照してください。

### リモート環境のログを表示する

ほとんどのログには、リモート環境で発生するイベントが含まれています。 Proの場合、複数のノードがあり、各ノードには一意のログがあります。 すべてのホストのリストを表示するには、次を使用します。

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

回答サンプル：

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**リモート環境ログのリストを表示するには**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Proの例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**リモートログを表示するには**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Proの例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Pro ステージング環境およびPro実稼動環境では、固定ファイル名のログファイルに対して、自動ログのローテーション、圧縮、削除が有効になります。各ログファイルタイプには、回転パターンとライフタイムがあります。
>環境のログのローテーションと圧縮されたログの有効期間に関する詳細は、`/etc/logrotate.conf`と`/etc/logrotate.d/<various>`で確認できます。
>Pro ステージング環境およびPro実稼動環境の場合、ログローテーション設定の変更を求めるには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。

>[!TIP]
>
>ログのローテーションは、Pro統合環境では設定できません。
>Pro統合の場合は、カスタムソリューション/スクリプトを実装し、必要に応じてスクリプトを実行するように[cron](../application/crons-property.md)を設定する必要があります。

>[!NOTE]
>
>スタータープロジェクト環境には、ログのローテーションがありません。

## ログのビルドとデプロイ

環境に変更をプッシュした後、`var/log/cloud.log` ファイル内の各フックからのログ記録を確認できます。 ログには、各フックの開始メッセージと停止メッセージが含まれます。 次の例では、メッセージは「`Starting post-deploy.`」および「`Post-deploy is complete.`」です

ログエントリのタイムスタンプを確認し、特定のデプロイメントのログを確認して見つけます。 次に、トラブルシューティングに使用できるログ出力の例を示します。

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Cloud環境を設定する際に、ビルドとデプロイのアクション用に[ ログベースのSlackとメール通知](../environment/set-up-notifications.md)を設定できます。

次のログは、すべてのCloud プロジェクトに共通の場所を持ちます。

- **デプロイメントログ**: `var/log/cloud.log`
- **前回のデプロイメントエラーログ**: `var/log/cloud.error.log`
- **デバッグログ**: `var/log/debug.log`
- **例外ログ**: `var/log/exception.log`
- **システム ログ**: `var/log/system.log`
- **サポートログ**: `var/log/support_report.log`
- **レポート**: `var/report/`

`cloud.log` ファイルには、デプロイメントプロセスの各段階からのフィードバックが含まれていますが、デプロイメントフックによって作成されたログは、各環境に固有です。 環境固有のデプロイログは、次のディレクトリにあります。

- **スターターとプロの統合**: `/var/log/deploy.log`
- **Pro ステージング**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro実稼動**: `/var/log/platform/<project-ID>/deploy.log`

### デプロイ ログ

各デプロイメントのログは、特定の`deploy.log` ファイルに連結されます。 次の例では、現在の環境のデプロイログをターミナルに出力します。

```bash
magento-cloud log -e <environment-ID> deploy
```

回答サンプル：

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### エラーログ

デプロイメントプロセス中に生成されたエラーと警告メッセージは、`var/log/cloud.log`と`var/log/cloud.error.log` ファイルの両方に書き込まれます。 クラウドエラーログファイルには、最新のデプロイメントからのエラーと警告のみが含まれます。 空のファイルは、エラーのないデプロイメントが成功したことを示します。

[Cloud CLI SSH](#view-remote-environment-logs)を使用してログファイルを表示するか、ECE-Toolsを使用してエラーを表示して候補を表示できます。

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

回答サンプル：

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

ほとんどのエラーメッセージには、説明と提案されたアクションが含まれています。 ECE-Tools](../dev-tools/error-reference.md)の[ エラーメッセージ参照を使用して、エラーコードを調べて詳細なガイダンスを得ることができます。 詳しいガイダンスについては、[Adobe Commerce デプロイメントのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)を参照してください。

## アプリケーションログ

デプロイログと同様に、アプリケーションログは各環境に対して一意です。

| ログファイル | スターターとプロの統合 | 説明 |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **ログのデプロイ** | `/var/log/deploy.log` | [ デプロイ フック ](../application/hooks-property.md)のアクティビティ。 |
| **デプロイ後のログ** | `/var/log/post_deploy.log` | [ デプロイ後のフック ](../application/hooks-property.md)のアクティビティ。 |
| **Cron ログ** | `/var/log/cron.log` | cron ジョブからの出力。 |
| **Nginx アクセス ログ** | `/var/log/access.log` | Nginxの開始時に、見つからないディレクトリと除外されたファイルタイプに対するHTTP エラーが発生します。 |
| **Nginx エラーログ** | `/var/log/error.log` | Nginxに関連する設定エラーのデバッグに役立つ起動メッセージ。 |
| **PHP アクセス ログ** | `/var/log/php.access.log` | PHP サービスへのリクエスト。 |
| **PHP FPM ログ** | `/var/log/app.log` | |

Pro ステージング環境および実稼動環境の場合、デプロイ、ポストデプロイ、およびCron ログは、クラスター内の最初のノードでのみ使用できます。

| ログファイル | プロステージング | プロ制作 |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **ログのデプロイ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/deploy.log` |
| **デプロイ後のログ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Cron ログ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/cron.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx アクセス ログ** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Nginx エラーログ** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP アクセス ログ** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM ログ** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### アーカイブされたログファイル

アプリケーションログは1日に1回圧縮およびアーカイブされ、デフォルトで&#x200B;**30日間**&#x200B;保持されます（Pro ステージングおよび実稼動クラスターの場合）。ログのローテーションは、すべての統合/スターター環境で利用できません。 圧縮されたログには、`Number of Days Ago + 1`に対応する一意のIDを使用して名前が付けられます。 例えば、Proの本番環境では、過去21日間のPHP アクセスログが保存され、次のように名前が付けられます。

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

アーカイブされたログファイルは、圧縮前に元のファイルが配置されていたディレクトリに常に保存されます。

[ サポートチケット ](https://experienceleague.adobe.com/home?support-tab=home#support)を送信して、ログの保持期間またはログローテーション設定の変更を要求できます。 保持期間を最大365日まで延長したり、ストレージ クォータを節約するために期間を短縮したり、logrotate設定に追加のログパスを追加したりできます。 これらの変更は、Pro ステージングおよび実稼動クラスターで使用できます。

例えば、`var/log/mymodule` ディレクトリにログを保存するためのカスタムパスを作成する場合、このパスのログローテーションをリクエストできます。 ただし、現在のインフラストラクチャでは、ログのローテーションを適切に設定するために、Adobeに一貫したファイル名が必要です。 Adobeでは、設定の問題を回避するために、ログ名の一貫性を維持することをお勧めします。

>[!NOTE]
>
>**デプロイ**&#x200B;および&#x200B;**デプロイ後**&#x200B;のログファイルはローテーションされず、アーカイブされません。 デプロイメント履歴はすべて、ログファイル内に書き込まれます。

## サービスログ

各サービスは個別のコンテナで実行されるため、サービスログは統合環境では使用できません。 Adobe Commerce クラウドインフラストラクチャでは、統合環境でのみweb サーバーコンテナにアクセスできます。 次のサービスログの場所は、Pro実稼動環境とステージング環境用です。

- **Redis ログ**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Elasticsearch ログ**: `/var/log/elasticsearch/elasticsearch.log`
- **Java ガベージコレクションログ**: `/var/log/elasticsearch/gc.log`
- **メールログ**: `/var/log/mail.log`
- **MySQL エラーログ**: `/var/log/mysql/mysql-error.log`
- **MySQL スローログ**: `/var/log/mysql/mysql-slow.log`
- **RabbitMQ ログ**: `/var/log/rabbitmq/rabbit@host1.log`

サービスログは、ログの種類に応じて、異なる期間アーカイブおよび保存されます。 例えば、MySQLのログの有効期間は最も短く、7日後に削除されます。

>[!TIP]
>
>スケーリングされたアーキテクチャ内のログファイルの場所は、ノードタイプによって異なります。 「[拡張アーキテクチャの場所をログに記録する](../architecture/scaled-architecture.md#log-locations)」トピックを参照してください。

## プロの実稼動とステージング用のログデータ

Pro実稼動環境およびステージング環境では、プロジェクトに統合された[New Relic ログ管理](../monitor/log-management.md)を使用して、Adobe Commerce on cloud infrastructure プロジェクトに関連付けられたすべてのログから集約されたログデータを管理します。

New Relic Logs アプリケーションは、クラウドインフラストラクチャの実稼動環境とステージング環境でAdobe Commerceをトラブルシューティングおよび監視するための一元化されたログ管理ダッシュボードを提供します。 また、ダッシュボードでは、Fastly CDN、Image Optimization、web アプリケーションファイアウォール（WAF）サービスのログデータへのアクセスも提供されます。 [New Relic サービス ](../monitor/new-relic-service.md)を参照してください。
