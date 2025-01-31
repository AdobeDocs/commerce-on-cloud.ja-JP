---
title: New Relicの監視
description: New Relic ダッシュボードにアクセスし、クラウドインフラストラクチャプロジェクト上のAdobe Commerceからデータを分析する方法について説明します。
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# New Relicの監視

New Relicは、PHP エージェントを使用して、インフラストラクチャと [!DNL Commerce] アプリケーションを接続およびモニタリングします。 クラウド環境がNew Relicに接続したら、New Relic アカウントにログインして、エージェントが収集したデータを確認できます。

_APM とサービス_ ページで、「**概要**」を選択して、アプリケーションに関するトランザクション情報を表示します。 この表示は、潜在的なエラーを特定し、アプリケーションとサービスの全体的な正常性を確認するのに役立ちます。

![ クラウドプロジェクトのNew Relicの概要ページ ](../../assets/new-relic/dashboard.png)

このビューを使用すると、応答の遅延やボトルネックが発生しているトランザクション、アプリケーションのスループット、web エラーなどを追跡できます。

追跡データを確認：

- **最も時間がかかる** - リクエストを並行して追跡することで、時間の消費を判断します。 例えば、製品ビューとカテゴリビューで費やしたトランザクション時間が最も長くなる場合があります。 顧客アカウントページが時間の消費で突然上位にランクされた場合、呼び出しやクエリによるドラッグのパフォーマンスがアプリケーションに影響を与える可能性があります。

- **最高スループット** – 送信されたバイトのサイズと頻度に基づいて、最もヒットしたページを特定します。

収集されたすべてのデータは、データ、クエリ、または _Redis_ データを送信するアクションに費やした時間の詳細です。 クエリによって問題が発生した場合、New Relicはそれらの問題を追跡して対応するための情報を提供します。

>[!TIP]
>
>このデータを使用してアプリケーションのパフォーマンスの問題をトラブルシューティングする方法について詳しくは、_Adobe Commerce ヘルプセンター [ 「New Relicを使用したパフォーマンスのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html)」を参照してください_

## 管理されたアラートによるパフォーマンスの監視

Adobeでは、パフォーマンス指標を追跡するための _Adobe Commerceの管理アラート_ アラートポリシーを提供しています。 このポリシーには、インフラストラクチャやアプリケーションの問題がサイトのパフォーマンスに影響を与えた場合に、閾値およびトリガーに関する警告と重要な通知を設定するアラートの集合が含まれます。 ポリシーは、実稼動環境の次の指標を追跡します。

| 指標 | データ収集 | 対象 |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] スコア | APM | Pro および Starter |
| CPUの使用 | NRI | プロ |
| ディスク容量 | NRI | プロ |
| エラー率 | APM | Pro および Starter |
| メモリ使用量 | NRI | プロ |
| MariaDB クエリの読み込み | NRI | プロ |
| Redis メモリ | NRI | プロ |

サイトインフラストラクチャまたはアプリケーションの状態がアラートしきい値をトリガーすると、New Relicからアラート通知が送信されるので、問題にプロアクティブに対処できます。 アラートしきい値の詳細と、アラートをトリガーした問題を解決するためのトラブルシューティング手順については、_Adobe Commerce ヘルプセンター_ にある [Adobe Commerceの管理アラート ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) を参照してください。

>[!TIP]
>
>Pro ステージング環境および統合環境とスターター環境の場合は、[ 正常性通知 ](../integrations/health-notifications.md) を使用して、ディスク容量を監視します。

>[!PREREQUISITES]
>
>- **New Relic資格情報** - クラウドプロジェクトのNew Relic アカウントにログインするための資格情報
>- **アクティブなNew Relic統合** - クラウド環境がNew Relicに接続されていることを確認します
>- **ワークフロー通知** - アラート通知を受信するには、少なくとも 1 つの [ ワークフロー ](#set-up-a-workflow-for-notifications) を設定します

**Adobe Commerceの管理アラートのポリシーを確認するには**:

1. [New Relic アカウント ](https://login.newrelic.com/login) にログインします。

1. _Managed Alerts for Adobe Commerce_ ポリシーを見つけます。

   - エクスプローラーナビゲーションメニューで、「**[!UICONTROL Alerts & AI]**」をクリックします。

   - _検出_ の下の [ 検 **[!UICONTROL Alert Conditions & Policies]**] をクリックします。

   - _アラート条件およびポリシー_ ビューの上部でアカウントが選択されていることを確認します。

   - _ポリシー_ リストで、「**Adobe Commerceの管理されたアラート** ポリシーを選択します。

     ![ 生成されたアラートポリシー ](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >_Adobe Commerceの管理アラート_ ポリシーを使用できない場合は、_Adobe Commerce ヘルプセンター [ の ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html)Adobe Commerceの管理アラート_ を参照してください。

1. 「**[!UICONTROL Alert conditions]**」タブをクリックして、ポリシーで定義されたアラート条件を確認します。

## アラートポリシーの作成

Adobe Commerceの管理アラート ポリシーに含まれるアラートを変更しないでください。 Adobeは、このポリシーのアラート条件を経時的に更新および改善します。これにより、ポリシーに追加したカスタマイズが上書きされます。

既存のアラートを変更する代わりに、アラートポリシーを作成できます。 次に、アラート条件を新しいポリシーにコピーします。

>[!TIP]
>
>アラート、アラートポリシーおよびワークフローについて詳しくは、_New Relic_ ドキュメントの [ アラートの概要 ](https://docs.newrelic.com/docs/alerts/overview/) を参照してください。

## 通知用のワークフローの設定

以前は通知チャネルと呼ばれていた _ワークフロー_ を設定して、アラートポリシーなどのフィルタリングされたデータに基づいてサイトのパフォーマンスに関する通知を受信できるようになりました。 アプリケーションまたはインフラストラクチャの状態がアラートをトリガーする場合、パフォーマンスの問題に関する通知は、アラート・ポリシーに関連づけられたすべてのワークフローに送信されます。 また、イシューが承認されてクローズされたときに通知を受け取ることもできます。

New Relicには、メール、Slack、PagerDuty、Webhook など、様々なタイプのワークフロー通知を設定するためのテンプレートが用意されています。

**ワークフローを設定するには**:

1. [New Relic アカウント ](https://login.newrelic.com/login) にログインします。

1. ワークフローを作成します。

   - エクスプローラーナビゲーションメニューで、「**[!UICONTROL Alerts & AI]**」をクリックします。

   - 左側のナビゲーションの _エンリッチメントして通知_ の下で、「**[!UICONTROL Workflows]**」をクリックします。

   - 右側の「**[!UICONTROL Add a workflow]**」をクリックします。

     ![New Relic ワークフローを追加 ](../../assets/new-relic/add-a-workflow.png)

   - _ワークフローの設定_ ページで、ワークフローの名前を入力します。

   - 「_データをフィルター_」セクションで、「**[!UICONTROL Policy]**」ドロップダウンリストから「**[!UICONTROL Managed Alerts for Adobe Commerce]**」を選択します。

   - 「_通知_」セクションで、チャネルを選択し、指示に従います。

   - 「**[!UICONTROL Test workflow]**」をクリックして設定を確認します。

1. 「**[!UICONTROL Activate workflow]**」をクリックします。

[ ワークフロー ](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/) については、New Relicのドキュメントを参照してください。

>[!WARNING]
>
>Adobe Commerceの管理アラート ポリシーのアラートには、Adobe Commerce on cloud infrastructure のお客様をサポートするAdobeチームに通知するように、デフォルトのワークフローが設定されています。 これらのデフォルトチャネルの設定を変更しないでください。また、割り当てられたアラートポリシーも削除しないでください。
