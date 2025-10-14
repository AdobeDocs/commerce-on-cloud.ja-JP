---
title: ログイン先  [!DNL Cloud Console]
description: クラウドインフラストラクチ  [!DNL Cloud Console]  上のAdobe Commerce用について説明します。
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# [!DNL Cloud Console] にログインする

[!DNL Cloud Console] には、Commerce コードを作成、管理およびデプロイするためのインタラクティブなメソッドが用意されています。 この [!DNL Cloud Console] は、より現代的で使いやすいエクスペリエンスであり、将来のインターフェイス機能強化の基盤となります。

[&#x200B; にログイン  [!DNL Cloud Console]](https://console.adobecommerce.com) して、プロジェクトのリストを表示します。

![&#x200B; プロジェクトリスト &#x200B;](../assets/ui-allprojects-list.png)

## 機能

新機能または強化された機能は次のとおりです。

- プロジェクトと環境の特性の明確な概要
- 並べ替え可能な履歴を含むアクティビティストリーム
- スタータープロジェクトの手動バックアップ管理と履歴
- 強化されたログビュー
- 並べ替え可能リスト
- 統合を追加するためのシンプルなフォームとガイダンス
- Web コンテンツアクセシビリティガイドライン（WCAG）への準拠

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

新機能または改善された機能は次のとおりです。

| 機能 | 改善点 |
| -------------- | ----------------------------------- |
| [&#x200B; アクティビティストリーム &#x200B;](../cloud-guide/project/activity-stream.md) | 実行中、保留中または履歴のアクションの並べ替え可能なリストを操作します。 アクティビティを選択してログを表示するか、実行中のビルドをキャンセルします。 |
| [&#x200B; プロジェクトと環境の概要 &#x200B;](../cloud-guide/project/overview.md#project-overview) | プロジェクトを開き、プロジェクトの詳細と環境リストの概要を確認します。 環境の概要には、環境のステータス、アプリケーションへのアクセス、最近のアクティビティに関する詳細が表示されます。 |
| [&#x200B; 統合フォーム &#x200B;](../cloud-guide/integrations/overview.md) | 簡単なフォームとガイダンスを使用して、Bitbucket やSlack通知などの統合機能を追加します。 |
| [&#x200B; プロジェクトリスト &#x200B;](../cloud-guide/project/overview.md#cloud-console) | _すべてのプロジェクト_ ビューには、アクセス権限を持つすべてのプロジェクトが一覧表示されます。 「**[!UICONTROL Show filters]**」をクリックし、タイプ、地域またはプランでプロジェクトリストをフィルタリングできます。 |
| [&#x200B; 変数の表示オプション &#x200B;](../cloud-guide/environment/variable-levels.md) | ビルドまたは実行中に、プロジェクトレベルまたは環境レベルの変数の表示を制限します。 |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## コンソールに関する質問

**_スナップショット機能はどこで入手できま_** か？

[!DNL Starter] プロジェクトのスナップショット機能は、_バックアップ_ と呼ばれるようになりました。 [!DNL Cloud Console] から [!DNL Starter] 環境の手動バックアップを作成するか、Cloud CLI からスナップショットを作成できます。 環境の管理者の役割が必要です。

プロジェクトナビゲーションバーから環境を選択します。 環境がアクティブである必要があります。 「**[!UICONTROL Backups]**」タブを選択します。 現在、このオプションは Pro 環境では使用できません。

**_環境に設定されたルートのリストはどこにありま_** か？

設定済みのルートのリストは、環境の「_サービス_」タブに表示されます。

プロジェクトナビゲーションバーから環境を選択します。 「**[!UICONTROL Services]**」タブを選択します。 **Router** の概要には、設定済みのルートが表示されます。 現在、新しい [!DNL Cloud Console] からルートを追加することはできません。

## アカウントメニュー

右上隅にアカウントメニューがあります。 メニューの下矢印をクリックし、「**[!UICONTROL My Profile]**」を選択します。 _マイプロファイル_ ビューでは、ユーザーの詳細と表示設定を制御したり、[&#x200B; セキュリティ認証 &#x200B;](../cloud-guide/project/user-access.md#user-authentication-requirements)、[API トークン &#x200B;](../cloud-guide/project/user-access.md#create-an-api-token)、[SSH キー &#x200B;](../cloud-guide/development/secure-connections.md) を管理したりできます。
