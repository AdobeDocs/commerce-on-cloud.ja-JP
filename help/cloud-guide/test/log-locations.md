---
title: ログの表示と管理
description: クラウドインフラストラクチャで使用できるログファイルのタイプと、それらのログファイルの場所について説明します。
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: 731cc36816afdb5374269e871d337e056a71c050
workflow-type: tm+mt
source-wordcount: '1205'
ht-degree: 0%

---

# ログの表示と管理

クラウドインフラストラクチャプロジェクトのAdobe Commerceのログは、[ ビルドとデプロイ ](../application/hooks-property.md)、クラウドサービス、Adobe Commerce アプリケーションに関連する問題のトラブルシューティングに役立ちます。

ログは、ファイル・システム、[!DNL Cloud Console]、`magento-cloud` CLI から表示できます。

- **ファイルシステム** - `/var/log` システムディレクトリには、すべての環境のログが含まれます。 `var/log/` ディレクトリには、特定の環境に固有のアプリ固有のログが含まれています。 これらのディレクトリは、クラスター内のノード間で共有されません。 実稼動環境およびステージング環境では、各ノードのログを確認する必要があります。

- **[!DNL Cloud Console]** – 環境 _メッセージ_ リストで、ビルド、デプロイおよびデプロイ後のログ情報を確認できます。

- **Cloud CLI** - `magento-cloud log` コマンドを使用してローカル環境ログを、または `magento-cloud ssh` コマンドを使用してリモート環境ログを表示できます。

## ログの場所

システムログは、次の場所に保存されています。

- 統合：`/var/log/<log-name>.log`
- Pro ステージング：`/var/log/platform/<project-ID>_stg/<log-name>.log`
- 試作品：`/var/log/platform/<project-ID>/<log-name>.log`

`<project-ID>` の値は、プロジェクトと、環境がステージングか実稼動かによって異なります。 例えば、プロジェクト ID が `yw1unoukjcawe` の場合、ステージング環境のユーザーは `yw1unoukjcawe_stg`、実稼動環境のユーザーは `yw1unoukjcawe` となります。

この例を使用する場合、デプロイログは `/var/log/platform/yw1unoukjcawe_stg/deploy.log` です。

### リモート環境ログの表示

ほとんどのログには、リモート環境で発生するイベントが含まれます。 Pro の場合、複数のノードがあり、各ノードには一意のログがあります。 すべてのホストのリストを表示するには、以下を使用します。

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

応答の例：

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**リモート環境ログのリストを表示するには**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Pro の例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**リモートログを表示するには**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Pro の例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Pro ステージング環境および Pro 実稼動環境では、固定ファイル名のログファイルに対して、自動ログローテーション、圧縮、削除が有効になります。 各ログ ファイル タイプには、回転パターンと有効期間があります。
>&#x200B;>環境のログのローテーションと圧縮ログの存続期間について詳しくは、`/etc/logrotate.conf` と `/etc/logrotate.d/<various>` を参照してください。
>&#x200B;>ステージング環境および実稼動環境が Pro の場合、ログローテーション設定の変更を依頼するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) する必要があります。

>[!TIP]
>
>ログのローテーションは、Pro 統合環境では設定できません。
>&#x200B;>Pro 統合の場合、カスタムソリューション/スクリプトを実装し、必要に応じてスクリプトを実行するように [cron を設定 ](../application/crons-property.md) する必要があります。

>[!NOTE]
>
>スタータープロジェクト環境にはログローテーションがありません。

## ログの作成とデプロイ

環境に変更をプッシュした後、`var/log/cloud.log` ファイルの各フックからログを確認できます。 ログには、各フックの開始メッセージと停止メッセージが含まれます。 次の例では、メッセージは「`Starting post-deploy.`」と「`Post-deploy is complete.`」です

特定のデプロイメントのログエントリのタイムスタンプを確認し、ログを確認および特定します。 次に、トラブルシューティングに使用できるログ出力の圧縮例を示します。

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
>クラウドを設定する際に、ビルドおよびデプロイアクション用の [ ログベースのSlackおよびメール通知 ](../environment/set-up-notifications.md) を設定できます。

次のログは、すべてのクラウドプロジェクトで共通の場所を持ちます。

- **配置ログ**: `var/log/cloud.log`
- **前回のデプロイメントエラーログ**: `var/log/cloud.error.log`
- **デバッグログ**: `var/log/debug.log`
- **例外ログ**: `var/log/exception.log`
- **システム ログ**: `var/log/system.log`
- **サポートログ**: `var/log/support_report.log`
- **報告書**: `var/report/`

`cloud.log` ファイルには、デプロイメントプロセスの各ステージからのフィードバックが含まれていますが、デプロイメントフックによって作成されるログは、環境ごとに一意です。 環境固有のデプロイログは、次のディレクトリにあります。

- **Starter と Pro の統合**: `/var/log/deploy.log`
- **Pro ステージング**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro 実稼働**: `/var/log/platform/<project-ID>/deploy.log`

### ログをデプロイ

各デプロイメントのログは、特定の `deploy.log` ファイルに連結されます。 次の例では、ターミナル内の現在の環境のデプロイログを出力します。

```bash
magento-cloud log -e <environment-ID> deploy
```

応答の例：

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### エラーログ

デプロイメントプロセス中に生成されたエラーメッセージと警告メッセージは、`var/log/cloud.log` ファイルと `var/log/cloud.error.log` ファイルの両方に書き込まれます。 Cloud エラーログファイルには、最新のデプロイメントのエラーと警告のみが含まれます。 空のファイルは、エラーのないデプロイメントが成功したことを示します。

[Cloud CLI SSH](#view-remote-environment-logs) を使用してログファイルを表示したり、ECE-Tools を使用してエラーと提案を表示したりできます。

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

応答の例：

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

ほとんどのエラーメッセージには、説明と推奨されるアクションが含まれています。 [ECE-Tools のエラーメッセージのリファレンス ](../dev-tools/error-reference.md) を使用して、エラーコードを検索し、詳しいガイダンスを得ます。 詳しいガイダンスについては、[Adobe Commerce デプロイメントのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=ja) を参照してください。

## アプリケーションログ

デプロイログと同様、アプリケーションログは環境ごとに一意です。

| ログファイル | スターターと Pro の統合 | 説明 |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **ログをデプロイ** | `/var/log/deploy.log` | [ デプロイフック ](../application/hooks-property.md) からのアクティビティ。 |
| **デプロイ後のログ** | `/var/log/post_deploy.log` | [ デプロイ後フック ](../application/hooks-property.md) からのアクティビティ。 |
| **Cron ログ** | `/var/log/cron.log` | Cron ジョブからの出力。 |
| **Nginx アクセス ログ** | `/var/log/access.log` | Nginx の起動時に、ディレクトリの欠落や除外されたファイル タイプの HTTP エラーが発生します。 |
| **Nginx エラーログ** | `/var/log/error.log` | Nginx に関連する構成エラーのデバッグに役立つスタートアップ メッセージ。 |
| **PHP アクセスログ** | `/var/log/php.access.log` | PHP サービスへのリクエスト。 |
| **PHP FPM ログ** | `/var/log/app.log` | |

ステージング環境および実稼動環境の場合、デプロイ、ポストデプロイ、Cron ログは、クラスターの最初のノードでのみ使用できます。

| ログファイル | Pro ステージング | 実稼動環境に対応 |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **ログをデプロイ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/deploy.log` |
| **デプロイ後のログ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Cron ログ** | 最初のノードのみ：<br>`/var/log/platform/<project-ID>_stg*/cron.log` | 最初のノードのみ：<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx アクセス ログ** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Nginx エラーログ** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP アクセスログ** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM ログ** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### アーカイブしたログファイル

アプリケーションログは 1 日に 1 回の頻度で圧縮およびアーカイブされ **デフォルトでは** 365 日間保持されます（Pro ステージングクラスターと実稼動クラスターの場合）。また、ログのローテーションは、すべての統合/スターター環境で使用できるわけではありません。 圧縮ログには、`Number of Days Ago + 1` に対応する一意の ID を使用して名前が付けられます。 例えば、Pro 実稼動環境では、過去 21 日間の PHP アクセスログが次のように保存され、名前が付けられます。

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

アーカイブされたログ・ファイルは、圧縮前に元のファイルがあったディレクトリに常に保存されます。

[ サポートチケットを送信 ](https://experienceleague.adobe.com/home?lang=ja&support-tab=home#support) して、ログ保持期間またはログ回転設定の変更をリクエストできます。 保存期間は最大 365 日まで延長できます。また、ストレージ・クォータを節約するために保存期間を短縮することも、ログ回転構成にログ・パスを追加することもできます。 これらの変更は、ステージング環境および実稼動環境のクラスターで使用できます。

例えば、ログを `var/log/mymodule` ディレクトリに保存するカスタムパスを作成した場合、このパスのログのローテーションをリクエストできます。 ただし、現在のインフラストラクチャでは、ログローテーションを正しく設定するために、Adobeの一貫したファイル名が必要です。 Adobeでは、設定の問題を回避するために、ログ名の一貫性を維持することをお勧めします。

>[!NOTE]
>
>**デプロイ** および **デプロイ後** ログファイルは、ローテーションされたりアーカイブされたりしません。 デプロイメント履歴全体がこれらのログファイルに書き込まれます。

## サービスログ

各サービスは個別のコンテナで実行されるので、サービスログは統合環境では使用できません。 クラウドインフラストラクチャー上のAdobe Commerceを使用すると、統合環境内の web サーバーコンテナにのみアクセスできます。 次のサービスログの場所は、実稼動環境およびステージング環境用です。

- **Redis ログ**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Elasticsearch ログ**: `/var/log/elasticsearch/elasticsearch.log`
- **Java ガベージコレクションログ**:`/var/log/elasticsearch/gc.log`
- **メール ログ**: `/var/log/mail.log`
- **MySQL エラーログ**: `/var/log/mysql/mysql-error.log`
- **MySQL 低速ログ**: `/var/log/mysql/mysql-slow.log`
- **RabbitMQ ログ**:`/var/log/rabbitmq/rabbit@host1.log`

サービス・ログは、ログ・タイプに応じて異なる期間にわたってアーカイブおよび保存されます。 例えば、MySQL ログの有効期間が最短で、7 日後に削除されます。

>[!TIP]
>
>スケールされたアーキテクチャにおけるログファイルの場所は、ノードタイプによって異なります。 [ スケールされたアーキテクチャのログの場所 ](../architecture/scaled-architecture.md#log-locations) のトピックを参照してください。

## 実稼動およびステージング用のログデータ

実稼動環境およびステージング環境では、プロジェクトと統合された [New Relic ログ管理 ](../monitor/log-management.md) を使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceに関連付けられたすべてのログからの集計ログデータを管理します。

New Relic ログアプリケーションは、クラウドインフラストラクチャの実稼動環境とステージング環境でAdobe Commerceをトラブルシューティングおよび監視するための一元的なログ管理ダッシュボードを提供します。 また、Fastly CDN、Image Optimization、Web Application Firewall （WAF）の各サービスのログデータにもアクセスできます。 [New Relic サービス ](../monitor/new-relic-service.md) を参照してください。
