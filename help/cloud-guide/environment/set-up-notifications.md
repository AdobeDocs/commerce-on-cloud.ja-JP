---
title: 通知の設定
description: Adobe Commerce on cloud infrastructure環境の通知を設定する方法を説明します。
feature: Cloud, Configuration, Logs
exl-id: dfbe1084-ad30-4489-af2d-d6f6b5eae1c4
source-git-commit: e3a2c8580ad1f27ddd3dc8fc40207bce68ee1c7f
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 通知の設定

デフォルトでは、Adobe Commerce オンクラウドインフラストラクチャは、Adobe Commerce ルートアプリケーションディレクトリ内の`app/var/log/cloud.log` ファイルにビルドアクションとデプロイアクションを書き込みます。 オプションで、Slackや電子メールなどのメッセージングシステムにログを送信して、リアルタイム通知を受け取ることができます。

例えば、デプロイメントが失敗した場合にSlack メッセージを送信し、何が問題だったのかを調査するように促すことができます。

## プラン通知

通知を設定する前に、次の点を考慮してください。

- どのような通知を受け取るか（Slack メッセージ、電子メール、両方）?
- ログにどの程度の詳細を表示しますか？
- 通知（統合、ステージング、実稼動）をどこで設定しますか？

例えば、初期開発時には、ステージング環境にデプロイする前に問題をデバッグするために、統合環境に関する詳細なログを表示するメール通知を好むかもしれません。 ステージング環境または実稼動環境にデプロイする準備ができたら、詳細情報が少ないSlack メッセージを使用することをお勧めします。

>[!NOTE]
>
>通知の設定に使用する設定ファイルは、プロジェクトディレクトリのルートにあるため、変更を任意の環境にプッシュするときに適用されます。 環境ごとに通知をカスタマイズする場合は、その環境にプッシュする前に設定ファイルを変更する必要があります。

## 通知の設定

通知を設定するには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。
1. プロジェクト ルートの`.magento.env.yaml` ファイルで、メッセージ システムの設定（優先通知[&#x200B; ログレベル &#x200B;](log-handlers.md#log-levels)を含む）を追加します。

   例えば、Slack _と_&#x200B;の両方のメール設定を設定するには、次の手順を使用します。

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce on cloud infrastructureは、デプロイメントフェーズでのみメールを送信します。

1. 変更をコミットしてリモートサーバーにプッシュします。

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Slack設定の例

次の例は、Slackのみの設定を示しています。

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` – お使いのSlack [&#x200B; ユーザートークン &#x200B;](https://api.slack.com/docs/token-types#user)。 ユーザートークンは、クラウドインフラストラクチャ上のAdobe Commerceにメッセージの送信を許可します。
- `channel` - Slack チャネルの名前Adobe Commerce on cloud infrastructureは通知を送信します。
- `username` - クラウドインフラストラクチャ上のAdobe Commerceで、Slackで通知メッセージを送信する際に使用するユーザー名。
- `min_level` – 通知メッセージの最小ログレベル。 `info`を使用することをお勧めします。

### メール設定の例

次の例は、メールのみの設定を示しています。

>[!NOTE]
>
>Adobe Commerce on cloud infrastructureは、デプロイメントフェーズでのみメールを送信します。

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` - クラウドインフラストラクチャ上のメールアドレス Adobe Commerceが通知メッセージを送信します。
- `from` – 受信者に通知メッセージを送信するためのメールアドレス。
- `subject` – 電子メールの説明。
- `min_level` – 通知メッセージの最小ログレベル。 `notice`または`warning`を使用することをお勧めします。
