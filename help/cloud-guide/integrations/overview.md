---
title: 統合の概要
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceのサードパーティ統合オプションについて説明します。
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# 統合の概要

統合は、Git ホスティングやSlackボットなどの外部サービスを使用したり、GitHub のコードリビュープルリクエスト機能の使用など、現在の開発プロセスを維持管理したりするのに役立ちます。 クラウドインフラストラクチャー上のAdobe Commerce プロジェクトに次の統合を追加できます。

![&#x200B; 統合 &#x200B;](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Cloud CLI を使用して統合を追加するには**:

次のコマンドを実行すると、新しい統合のタイプとオプションを選択するための対話型プロンプトが表示されます。

```bash
magento-cloud integration:add
```

**プロジェクトに設定されている統合をリストするには**:

```bash
magento-cloud integration:list
```

応答の例：

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB  コンソール ]

**[!DNL Cloud Console]** を使用して統合を追加するには：

1. _プロジェクト設定_ で、「**[!UICONTROL Integrations]**」をクリックします。

1. 統合の種類をクリックするか、「**[!UICONTROL Add integration]**」をクリックします。

1. 統合タイプの選択と設定の手順について説明します。

1. 統合を追加すると、統合ビューのリストに表示されます。

>[!ENDTABS]

## Commerce Webhook

[ENABLE_WEBHOOK グローバル変数 &#x200B;](../environment/variables-global.md#enable_webhooks) を使用して、クラウドプロジェクト内でCommerce Webhook を設定できます。 Commerce Webhook は、Commerceが生成したイベントに応答して、外部サーバーにリクエストを送信します。 [_Webhook ガイド_](https://developer.adobe.com/commerce/extensibility/webhooks) では、この機能について詳しく説明します。

## 汎用 Webhook

カスタム Webhook 統合を使用して、クラウドインフラストラクチャーとリポジトリのイベントを取得し、レポートして、JSON メッセージを _Webhook_ URL に `POST` すことができます。

**Webhook URL を追加するには、次の構文を使用します**。

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` - `webhook` 統合タイプを指定します。
- `url` - JSON メッセージを受信できる Webhook URL を指定します。

応答のサンプルでは、統合をカスタマイズする機会を提供する一連のプロンプトを示しています。 デフォルト（空白）の応答を使用すると、プロジェクト内のすべての環境のすべてのイベントに関するメッセージが送信されます。

統合をカスタマイズして、分岐へのコードのプッシュなど、特定の [&#x200B; イベント &#x200B;](#events-to-report) をレポートすることができます。 例えば、ユーザーがブランチにコードをプッシュする際にメッセージを送信するように、`environment.push` イベントを指定できます。

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

イベントの状態は、`pending`、`in_progress`、`complete` から選択できます。

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

また、特定の環境に対してメッセージを _含める_ または _除外_ できます。

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

統合が完了すると、値の概要が表示されます。

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 既存の統合を更新

既存の統合は更新できます。 例えば、以下を使用して状態を `complete` から `pending` に変更します。

```bash
magento-cloud integration:update --states=pending <int-id>
```

応答の例：

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 報告するイベント

| イベント | 説明 |
| ----- | :-----------|
| `environment.access.add` | ユーザーに環境へのアクセス権が付与されました |
| `environment.access.remove` | ユーザーが環境から削除されました |
| `environment.activate` | 環境を含むブランチが「アクティブ化」されました |
| `environment.backup` | ユーザーがスナップショットをトリガーしました |
| `environment.branch` | 管理コンソールを使用してブランチが作成されました |
| `environment.deactivate` | ブランチが「非アクティブ化」されました。 コードはまだそこにありますが、環境は破壊されました |
| `environment.delete` | 分岐が削除されました |
| `environment.initialize` | プロジェクトの `master` ブランチが最初のコミットで初期化されました |
| `environment.merge` | アクティブなブランチが、管理コンソールまたは API を使用して統合されました |
| `environment.push` | ユーザーがブランチにコードをプッシュした |
| `environment.restore` | ユーザーがスナップショットを復元しました |
| `environment.route.create` | 管理コンソールを使用してルートが作成されました |
| `environment.route.delete` | 管理コンソールを使用してルートが削除されました |
| `environment.route.update` | 管理コンソールを使用してルートが変更されました |
| `environment.subscription.update` | サブスクリプションが変更されたので、`master` 環境のサイズが変更されましたが、コンテンツの変更はありません |
| `environment.synchronize` | 環境が親環境からデータまたはコードをコピーした |
| `environment.update.http_access` | 環境の HTTP アクセスルールが変更されました |
| `environment.update.restrict_robots` | ブロック オールロボット機能が有効または無効になっています |
| `environment.update.smtp` | 環境に対するメールの送信が有効または無効になっています |
| `environment.variable.create` | 変数が作成されました |
| `environment.variable.delete` | 変数が削除されました |
| `environment.variable.update` | 変数が変更されました |
| `project.domain.create` | ドメインが作成され、プロジェクトに追加されました |
| `project.domain.delete` | プロジェクトに関連付けられているドメインが削除されました |
| `project.domain.update` | プロジェクトに関連付けられているドメインが更新されました |
