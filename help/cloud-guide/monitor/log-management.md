---
title: New Relic log management
description: New Relicのログ管理を使用して、Commerce クラウドステージング環境と実稼動環境のログデータを集計、検索、視覚化する方法について説明します。
feature: Cloud, Logs, Observability
exl-id: b7636075-56fd-4227-b7e8-67acbe1defc5
TQID: 'https://experienceleague.adobe.com/gh3OUHKvbN462Z4w-2qnTwVQHA0IbwWNvRBfdrQKkrE'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 4239b8a6-e74f-567d-a7a5-b98b9ead0ea4
    internal-label: Observability
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
subfeature_v2:
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%
---
# New Relic log management

すべてのクラウドインフラプロジェクトには、[New Relic ログ管理](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)が含まれます。 このサービスは、ステージング環境と実稼動環境のすべてのログデータを集約し、一元化されたログ管理ダッシュボードに表示するように事前設定されています。

集約されたデータには、次のログの情報が含まれます。

- `~/var/log` ディレクトリからのすべての`ece-tools`およびアプリケーションログ
- `var/log/platform/<project-ID>` ディレクトリからのクラウドサービスのログ
- Fastly CDNとWAF

プロジェクトがNew Relicに接続されている場合は、New Relic Logs サービスを使用して、次のようなタスクを実行できます。

- New Relic クエリを使用したログデータの検索
- New Relic Logs アプリケーションを使用したログデータの視覚化
- カスタムチャート、ダッシュボード、アラートの作成
- 単一のダッシュボードからのパフォーマンスの問題のトラブルシューティング

## ログデータの表示と分析

New Relic Logs アプリケーションを使用して、集約されたログデータを検索し、アプリケーション、インフラストラクチャ、CDN、WAFのエラーをトラブルシューティングします。 New Relic APMおよびインフラストラクチャサービスから収集したログデータを使用して、チャート、ダッシュボード、アラートを作成できます。

**New Relic Logs アプリケーションを使用するには**:

1. [New Relic アカウント &#x200B;](https://login.newrelic.com/login)にログインします。

1. エクスプローラーのナビゲーションメニューから「**ログ**」を選択します。

1. アカウントが&#x200B;_すべてのログ_ ビューの上部で選択されていることを確認します。

1. ログクエリの時間範囲を選択します。

1. クラウドサービスのインフラストラクチャログデータ（`~/var/log/`からのログ）を確認するには、_ログの検索_ フィールドにクエリ文字列`has: "filePath"`を入力します。 次に、**[!UICONTROL Query logs]**&#x200B;をクリックします。

   ログファイルの名前は、`filePath`列に保存され、ログファイルへの完全なパスが表示されます。

   ![Cloud プロジェクト New Relic サービスのログデータ &#x200B;](../../assets/new-relic/var-log-query.png)

1. Fastly ログデータを確認するには、_ログの検索_ フィールドにクエリ文字列`has: "client_ip"`を入力します。 次に、**[!UICONTROL Query logs]**&#x200B;をクリックします。

1. Fastlyのログ結果を国コードでフィルタリングするには、**[!UICONTROL Add column]**&#x200B;をクリックし、**[!UICONTROL geo_country_code]**&#x200B;を選択します。

   ![Cloud プロジェクト New Relic CDN ログ属性フィルター](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>クエリ ビューは、_保存ビュー_ ドロップダウンから保存できます。 **[!UICONTROL Create new]**&#x200B;をクリックし、名前を入力してオプションを選択し、**[!UICONTROL Save view]**&#x200B;をクリックします。
>
>_New Relic Docs_ サイトの[&#x200B; ログ管理の基本](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)および[New Relic クエリ言語の概要](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/)を参照してください。
