---
title: 統合の概要
description: Adobe Commerce on cloud infrastructure プロジェクトのサードパーティ統合オプションについて説明します。
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 97c5f70d-1465-46c9-bb33-98897262c5ef
TQID: https://experienceleague.adobe.com/07ozUcb8SWNcyRzgxClhaG5S-cOHqywkYWfke1rJB1g
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 574
ht-degree: 0%

---

# 統合の概要

統合機能は、Git ホスティングやSlack ボットなどの外部サービスの使用や、GitHubでのコードレビュープルリクエスト関数の使用などの現在の開発プロセスの維持に役立ちます。 Adobe Commerce on cloud インフラストラクチャプロジェクトには、次の統合機能を追加できます。

![統合](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Cloud CLI**&#x200B;を使用して統合を追加するには：

次のコマンドは、新しい統合のタイプとオプションを選択するためのインタラクティブなプロンプトを開始します。

```bash
magento-cloud integration:add
```

**プロジェクトに設定された統合を一覧表示するには**:

```bash
magento-cloud integration:list
```

回答サンプル：

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

**[!DNL Cloud Console]**&#x200B;を使用して統合を追加するには：

1. _プロジェクト設定_&#x200B;で、**[!UICONTROL Integrations]**&#x200B;をクリックします。

1. 統合タイプをクリックするか、**[!UICONTROL Add integration]**&#x200B;をクリックします。

1. 統合タイプの選択と設定の手順を順を追って実行します。

1. 統合を追加すると、統合ビューのリストに表示されます。

>[!ENDTABS]

## Commerce webhook

Cloud プロジェクトでCommerce Webhookを設定するには、[ENABLE_WEBHOOKS グローバル変数](../environment/variables-global.md#enable_webhooks)を使用します。 Commerce webhookは、Commerceが生成したイベントに応じて、リクエストを外部サーバーに送信します。 [_Webhook ガイド_](https://developer.adobe.com/commerce/extensibility/webhooks)では、この機能について詳しく説明しています。

## 汎用webhook

カスタム Webhook統合を使用して、Cloud インフラストラクチャおよびリポジトリイベントをキャプチャし、レポートできます。`POST` JSON メッセージを&#x200B;_Webhook_ URLに統合します。

**Webhook URLを追加するには、次の構文を使用します**。

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` - `webhook`統合タイプを指定します。
- `url` - JSON メッセージを受信できるWebhook URLを指定します。

サンプル応答には、統合をカスタマイズする機会を提供する一連のプロンプトが表示されます。 デフォルトの（空白）応答を使用すると、プロジェクト内のすべての環境のすべてのイベントに関するメッセージが送信されます。

ブランチへのコードのプッシュなど、特定の[&#x200B; イベント &#x200B;](#events-to-report)をレポートするように統合をカスタマイズできます。 例えば、ユーザーがブランチにコードをプッシュしたときにメッセージを送信するように、`environment.push` イベントを指定できます。

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

`pending`、`in_progress`、または`complete`の状態でイベントをレポートするように選択できます。

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

また、特定の環境に対して&#x200B;_include_&#x200B;または&#x200B;_exclude_&#x200B;のメッセージを実行できます。

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

既存の統合を更新できます。 例えば、次を使用して状態を`complete`から`pending`に変更します。

```bash
magento-cloud integration:update --states=pending <int-id>
```

回答サンプル：

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

### レポートするイベント

| イベント | 説明 |
| ----- | :-----------|
| `environment.access.add` | ユーザーには環境へのアクセス権が付与されています |
| `environment.access.remove` | ユーザーが環境から削除されました |
| `environment.activate` | ブランチが環境で「アクティブ化」されました |
| `environment.backup` | ユーザーがスナップショットをトリガーしました |
| `environment.branch` | 管理コンソールを使用してブランチが作成されました |
| `environment.deactivate` | ブランチが「非アクティブ化」されました。 コードは残っていますが、環境は破壊されています |
| `environment.delete` | 分岐が削除されました |
| `environment.initialize` | プロジェクトの`master` ブランチが最初のコミットで初期化されました |
| `environment.merge` | 管理コンソールまたはAPIを使用して、アクティブなブランチがマージされました |
| `environment.push` | ユーザーがブランチにコードをプッシュしました |
| `environment.restore` | ユーザーがスナップショットを復元しました |
| `environment.route.create` | 管理コンソールを使用してルートが作成されました |
| `environment.route.delete` | 管理コンソールを使用してルートが削除されました |
| `environment.route.update` | 管理コンソールを使用してルートが変更されました |
| `environment.subscription.update` | サブスクリプションが変更されたため、`master`環境のサイズが変更されましたが、コンテンツの変更はありません |
| `environment.synchronize` | 環境の親環境からデータまたはコードがコピーされました |
| `environment.update.http_access` | 環境のHTTP アクセスルールが変更されました |
| `environment.update.restrict_robots` | すべてのロボットをブロック機能が有効または無効になりました |
| `environment.update.smtp` | 環境でメールの送信が有効または無効になりました |
| `environment.variable.create` | 変数が作成されました |
| `environment.variable.delete` | 変数が削除されました |
| `environment.variable.update` | 変数が変更されました |
| `project.domain.create` | ドメインが作成され、プロジェクトに追加されました |
| `project.domain.delete` | プロジェクトに関連付けられているドメインが削除されました |
| `project.domain.update` | プロジェクトに関連付けられているドメインが更新されました |
