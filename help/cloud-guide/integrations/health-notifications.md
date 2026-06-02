---
title: ヘルス通知
description: Adobe Commerce on cloud インフラストラクチャプロジェクトでディスク領域を使用するように、Slack、電子メール、およびPagerDuty通知を設定する方法を説明します。
feature: Cloud, Observability, Integration
exl-id: 5a7f37e9-e8f9-4b6b-b628-60dcaa60cc64
TQID: https://experienceleague.adobe.com/Tt4zWTqehO4SLsimUv3IE5VvjJ1OwAA27tT9Mot24FU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 386
ht-degree: 0%

---

# ヘルス通知

Adobe Commerce on cloud infrastructureは、Starter環境またはPro統合環境のすべてのアプリケーションとサービスのディスク容量の使用状況を監視します。 空き容量が不足しているデータベースディスクは、データ破損の原因となる可能性があります。 ヘルスステータスチェックは5分ごとに行われ、電子メールまたはその他の外部サービスで通知できます。 ヘルス通知に関する低ディスク警告は3つあります。

- **警告** – 使用可能なディスク容量が20%未満に減少します
- **クリティカル** – 使用可能なディスク容量が10%未満に減少
- **All-clear** – 利用可能なディスク領域が、ディスク不足イベントの後に20%を超えて返される

>[!NOTE]
>
>Pro実稼動環境では、New RelicのManaged Alerts for Adobe Commerceアラートポリシーを使用して、ディスク容量を監視できます。 「[管理済みアラートを使用したパフォーマンスの監視](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)」を参照してください。

## メール通知

正常性メール統合には、送信元アドレスと少なくとも1つの受信者アドレスが必要です。 `from-address`と`recipients` アドレスに同じ電子メールアドレスを使用できます。 次の例では、ヘルスメール統合を2人の受信者に登録します。

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slack チャネル通知

Slackは、チャットルームにメッセージを投稿するために、ボットと呼ばれるインタラクティブなアプリを使用する外部サービスです。 Slackでヘルス通知を受け取る前に、Slack グループのカスタム [&#x200B; ボットユーザー](https://api.slack.com/bot-users)を作成する必要があります。 チャネルまたはチャネルのボットユーザーを設定したら、Slackから提供された[&#x200B; ボットトークン &#x200B;](https://api.slack.com/docs/token-types#bot)を保存して統合を登録します。 次の例では、Slack チャネルにヘルス通知を登録します。

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuty通知

PagerDutyは、重要な問題をオンコールのチームメンバーに通知できる外部サービスです。 PagerDutyでヘルス通知を受け取る前に、Events API バージョン 2を使用する[PagerDuty統合](https://developer.pagerduty.com/v2/docs/integrating)を作成する必要があります。 統合キーまたは&#x200B;_ルーティングキー_&#x200B;を使用して、統合を登録します。 次の例では、ルーティングキーを使用してPagerDutyの通知を登録します。

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```

## ログ管理

使用可能なディスク容量を増やすには、環境からログファイルを切り捨てるか削除します。 logrotateが有効になっている場合は、最初にログのバックアップコピーをダウンロードしてから、それらを削除します。

```bash
rm -rf some-log-file.log.gz
```

または、個々のログファイルを切り捨てて、サイズを小さくすることもできます。 ログファイルの切り捨ての詳細な例については、ビデオチュートリアル「ログファイルを切り捨てる{target="_blank"}」を参照してください。
