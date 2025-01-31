---
title: 正常性通知
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceでディスク容量の使用状況に関するSlack、メール、PagerDuty 通知を設定する方法について説明します。
feature: Cloud, Observability, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# 正常性通知

クラウドインフラストラクチャー上のAdobe Commerceは、Starter 環境または Pro 統合環境のすべてのアプリケーションおよびサービスのディスクスペースの使用状況を監視します。 データベース ディスクの領域が不足すると、データが破損する可能性があります。 ヘルスステータスチェックは 5 分ごとに行われ、メールまたはその他の外部サービスで通知できます。 正常性通知には、ディスクの少ない次の 3 つの警告があります。

- **警告**：使用可能なディスク容量が 20% を下回る
- **重大**：使用可能なディスク容量が 10% を下回る
- **すべてクリア** - ディスク不足イベントの後、使用可能なディスク領域が 20% を超えて戻ります

>[!NOTE]
>
>Pro 実稼動環境では、New Relicの Managed alerts for Adobe Commerce アラートポリシーを使用して、ディスク容量を監視できます。 [ 管理アラートによるパフォーマンスの監視 ](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) を参照してください。

## メール通知

ヘルスメール統合には、送信元アドレスと少なくとも 1 つの受信者アドレスが必要です。 `from-address` と `recipients` アドレスに同じメールアドレスを使用できます。 次の例では、2 人の受信者と 1 つのヘルスメール統合を登録します。

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slackチャネルの通知

Slackは、ボットと呼ばれるインタラクティブアプリを使用してチャットルームでメッセージを投稿する外部サービスです。 Slackでヘルス通知を受け取るには、Slackグループのカスタム [ ボットユーザー ](https://api.slack.com/bot-users) を作成する必要があります。 チャネルまたはチャネルのボットユーザーを設定したら、Integration から提供される [ ボットトークン ](https://api.slack.com/docs/token-types#bot) を保存して、Slackを登録します。 次の例では、ヘルス通知をSlackチャネルに登録します。

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuty 通知

PagerDuty は、オンコールチームメンバーに重要な問題を通知できる外部サービスです。 PagerDuty で正常性通知を受信するには、Events API バージョン 2 を使用する [PagerDuty 統合 ](https://developer.pagerduty.com/v2/docs/integrating) を作成する必要があります。 統合キーまたは _ルーティングキー_ を使用して、統合を登録します。 次の例では、ルーティング キーを使用して PagerDuty の通知を登録します。

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
