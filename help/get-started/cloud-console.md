---
title: ' [!DNL Cloud Console]にログイン'
description: Adobe Commerce on Cloud インフラストラクチャの [!DNL Cloud Console] について説明します。
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 4c68508a-f25a-457b-be6d-36dc8ac428f1
TQID: https://experienceleague.adobe.com/5j8ZQakF2ZDWiO0and7Pa4iHp-3CCXbEHgYgZBZ6avo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cc72dcf1-72e1-48cc-b434-e7c27d62d67c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 029e44cd82c07d692dbbfacb4573763dbb4b579a
workflow-type: tm+mt
source-wordcount: 393
ht-degree: 0%

---

# [!DNL Cloud Console]にログイン

[!DNL Cloud Console]には、Commerce コードをビルド、管理、デプロイするためのインタラクティブなメソッドが用意されています。 [!DNL Cloud Console]は、より現代的で使いやすいエクスペリエンスであり、将来のインターフェイス強化の基礎となります。

[&#x200B; [!DNL Cloud Console]](https://console.adobecommerce.com)にログインして、プロジェクト リストを表示します。

![&#x200B; プロジェクトリスト &#x200B;](../assets/ui-allprojects-list.png)

## 機能

新機能または改善された機能は次のとおりです。

- プロジェクトと環境の特徴を明確に把握
- 並べ替え可能な履歴を持つアクティビティストリーム
- スタータープロジェクトの手動バックアップ管理と履歴
- 強化されたログビュー
- 並べ替え可能リスト
- シンプルなフォームとガイダンスで統合を追加
- web コンテンツのアクセシビリティガイドライン（WCAG）への準拠

![[!DNL Cloud Console]](../assets/CloudConsole.png)

新機能または改善された機能は次のとおりです。

| 機能 | 改善点 |
| -------------- | ----------------------------------- |
| [&#x200B; アクティビティストリーム &#x200B;](../cloud-guide/project/activity-stream.md) | 実行中、保留中、または過去のアクションを並べ替え可能なリストで操作します。 アクティビティを選択してログを表示するか、実行中のビルドをキャンセルします。 |
| [&#x200B; プロジェクトと環境の概要](../cloud-guide/project/overview.md#project-overview) | プロジェクトを開き、プロジェクトの詳細と環境リストの概要を確認します。 環境の概要には、環境のステータス、アプリケーションアクセス、最近のアクティビティに関する詳細が表示されます。 |
| [統合フォーム &#x200B;](../cloud-guide/integrations/overview.md) | シンプルなフォームとガイダンスを使用して、BitbucketやSlack通知などの統合機能を追加します。 |
| [&#x200B; プロジェクトリスト &#x200B;](../cloud-guide/project/overview.md#cloud-console) | _すべてのプロジェクト_ ビューには、アクセス権限を持つすべてのプロジェクトが一覧表示されます。 **[!UICONTROL Show filters]**&#x200B;をクリックして、プロジェクト リストをタイプ、地域、またはプランでフィルタリングできます。 |
| [変数の表示オプション &#x200B;](../cloud-guide/environment/variable-levels.md) | ビルドまたはランタイム中に、プロジェクトレベルまたは環境レベルの変数の表示を制限します。 |

<!--
The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | 
-->

## コンソールに関する質問

**_スナップショット機能はどこにありますか_**?

[!DNL Starter] プロジェクトの場合、スナップショット機能は&#x200B;_バックアップ_&#x200B;と呼ばれるようになりました。 [!DNL Cloud Console]から[!DNL Starter]環境の手動バックアップを作成するか、クラウド CLIからスナップショットを作成できます。 環境の管理者ロールが必要です。

プロジェクトのナビゲーションバーから環境を選択します。 環境はアクティブである必要があります。 「**[!UICONTROL Backups]**」タブを選択します。 現在、このオプションはPro環境では使用できません。

**_環境の設定済みルートのリストはどこにありますか_**?

設定されたルートのリストは、環境の「_サービス_」タブにあります。

プロジェクトのナビゲーションバーから環境を選択します。 「**[!UICONTROL Services]**」タブを選択します。 **Router**&#x200B;の概要には、設定済みのルートが表示されます。 現在、新しい[!DNL Cloud Console]からルートを追加することはできません。

## アカウントメニュー

右上隅はアカウントメニューです。 メニューの下向き矢印をクリックし、**[!UICONTROL My Profile]**&#x200B;を選択します。 _マイプロファイル_ ビューでは、ユーザーの詳細と表示設定を制御し、[&#x200B; セキュリティ認証](../cloud-guide/project/user-access.md#user-authentication-requirements)、[API トークン &#x200B;](../cloud-guide/project/user-access.md#create-an-api-token)、[SSH キー](../cloud-guide/development/secure-connections.md)を管理できます。
