---
title: エラーページとメンテナンスページのカスタマイズ
description: Fastly オリジンサーバーへのリクエストが失敗した場合に表示されるデフォルトのエラーページをカスタマイズする方法を説明します。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# エラーページとメンテナンスページのカスタマイズ

Fastly オリジンへのリクエストが失敗した場合、Fastly は、ユーザーにとって混乱を招く可能性のある、基本的な形式と一般的なメッセージングを含むデフォルトの応答ページを返します。 例えば、Fastly オリジンへのリクエストが 503 エラーが原因で失敗した場合、Fastly は次のデフォルトのエラーページを返します。

![Fastly デフォルトエラーページ ](../../assets/cdn/fastly-503-example.png)

次の例に示すように、Adobe Commerce ストアの設定を更新して、デフォルトの応答ページの一部を、より使いやすいメッセージ機能と改善されたHTMLスタイルを持つページに置き換えることができます。

![Fastly カスタムエラーページ ](../../assets/cdn/fastly-new-error-page.png)

現在、クラウドインフラストラクチャプロジェクト上のAdobe Commerceについて、次の Fastly 応答ページをカスタマイズできます。

- [サーバーエラー – 内部サーバーエラー、タイムアウト、またはサイトメンテナンスの停止（エラーコード 500 以上）](#customize-the-503-error-page)
- [WAFが疑わしいリクエストトラフィックを検出した場合に発生するWAF ブロッキングイベント（403 Forbidden）](#customize-the-waf-error-page)

**HTMLのコーディング要件：**

カスタムページのHTMLコードは、次の要件を満たす必要があります。

- コンテンツには、最大 65,535 文字を含めることができます。
- HTMLソース内のすべての CSS をインラインで指定します。
- Fastly がオフラインでも表示されるように、base64 を使用してHTMLページに画像をバンドルします。 [css-tricks サイトのデータ URI](https://css-tricks.com/data-uris/) を参照してください。

## 503 エラーページのカスタマイズ

次の場合、デフォルトの 503 エラーページが表示されます。

- Fastly オリジンへのリクエストが 500 より大きい応答ステータスを返す場合
- タイムアウト、メンテナンスアクティビティ、正常性の問題など、Fastly オリジンがダウンしている場合

デフォルトページをカスタマイズするには、以下のHTMLコードを適応させて、Adobe Commerce Store のテーマに合わせたスタイル設定を行い、必要に応じてタイトルとメッセージングを変更します。

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
         <title>503</title>
   </head>
   <body>
      <p>Service unavailable</p>
   </body></html>
```

変更したソースがブラウザーに正しく表示されていることを確認します。 次に、カスタマイズしたHTMLコードを Fastly 設定に追加します。

Fastly 設定にカスタム応答ページを追加するには：

{{admin-login-step}}

1. **ストア**/**設定**/**設定**/**詳細**/**システム** を選択します。

1. 右側のパネルで、**フルページキャッシュ** / **Fastly 設定** / **カスタム合成ページ** を展開します。

   ![503 エラーページを編集 ](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. **HTMLを設定** を選択します。

1. カスタムHTMLページのソースコードをコピーして、「レスポンス」フィールドに貼り付けます。

   ![ 更新 503 エラーページ ](../../assets/cdn/fastly-customize-503-response.png)

1. ページの上部にある「**アップロード**」を選択して、カスタマイズしたHTMLソースを Fastly サーバーにアップロードします。

1. ページ上部の「**設定を保存**」を選択して、更新した設定ファイルを保存します。

1. キャッシュを更新します。

   - ページ上部の通知で、「*キャッシュ管理*」リンクを選択します。

   - キャッシュ管理ページで、「**Magentoキャッシュをフラッシュ**」を選択します。

## WAF エラーページのカスタマイズ

[WAF](fastly-waf-service.md) のブロッキングイベントが原因で `403 Forbidden` エラーが発生して Fastly オリジンへのリクエストが失敗すると、デフォルトのWAF エラーページが表示されます。

![WAF エラーページ ](../../assets/cdn/fastly-waf-403-error.png)

次のコードサンプルに、デフォルトページのHTMLソースを示します。

```html
<html>
  <head>
    <title>Magento 403 Forbidden</title>
  </head>
  <body>
    <p>The requested URL was rejected.</p>
    <p>For additional information, please contact support and provide this reference ID:</p>
    <p>"} req.http.x-request-id {"</p>
    <p><button onclick='history.back();'>Go Back</button></p>
  </body>
</html>
```

Fastly 設定メニューの **カスタム合成ページ**/**WAFページを編集** オプションを使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのデフォルトコードをカスタマイズすることができます。 このコードを編集する際は、WAF ブロッキングイベントの参照 ID を提供する次の行を保持します。

```html
<p>"} req.http.x-request-id {"</p>
```

>[!NOTE]
>
>「WAFを編集」オプションを使用できるのは、クラウドインフラストラクチャプロジェクト上のAdobe Commerceで Managed Cloud WAF サービスが有効になっている場合のみです。

**WAF エラーページを編集するには**:

1. [ 管理者にログインします ](../../get-started/onboarding.md#access-your-admin-panel)。

1. **ストア**/**設定**/**設定**/**詳細**/**システム** を選択します。

1. 右側のパネルで、**フルページキャッシュ** / **Fastly 設定** / **カスタム合成ページ** を展開します。

   ![ 「WAF エラーページを編集」オプション ](../../assets/cdn/fastly-custom-synthetic-pages-edit-waf.png)

1. **WAFページを編集** を選択します。

1. フィールドに入力して、HTMLを更新します。

   ![WAFの更新のエラーページ ](../../assets/cdn/fastly-edit-waf-html.png)

   - **ステータス**:`403 Forbidden` のステータスを選択します。
   - **MIME タイプ** - タイプ `text/html`。
   - **コンテンツ** - デフォルトのHTML応答を編集してカスタム CSS を追加し、必要に応じてタイトルとメッセージを更新します。

1. ページの上部にある「**アップロード**」を選択して、カスタマイズしたHTMLソースを Fastly サーバーにアップロードします。

1. ページ上部の「**設定を保存**」を選択して、更新した設定ファイルを保存します。

1. キャッシュを更新します。

   - ページ上部の通知で、「**キャッシュ管理**」リンクを選択します。

   - キャッシュ管理ページで、「**Magentoキャッシュをフラッシュ**」を選択します。

## エラーレポート番号を表示

デフォルトでは、Fastly は、*503 Service Unavailable* エラーの背後にあるすべてのAdobe Commerce エラーを非表示にします。 エラーログレポートの番号を表示して、ログでエラーの詳細を確認できるようにするには、次の手順に従って、Fastly を省略して web サイトを開きます。

1. ストアの IP アドレスを取得します。

   - ステージング環境および実稼動環境の場合：

     ```bash
     nslookup {your_project_id}.ent.magento.cloud
     ```

   - Pro 統合環境およびスターター環境の場合：

     ```bash
     nslookup gw.{your_region}.magentosite.cloud
     ```

1. ローカルワークステーションの hosts ファイルにアプリケーションドメインと IP アドレスを追加します。

   ```text
   {server_IP} {store_domain}
   ```

1. ブラウザーのキャッシュと Cookie をクリアします（または匿名モードに切り替えます）。

1. ストアの web サイトを再度開いて、エラーコードを表示します。

1. エラーコードを使用して、エラーレポートファイルの詳細を見つけます。

   - [SSH を使用して、影響を受ける環境に接続する](../development/secure-connections.md#connect-to-a-remote-environment)

   - `./var/report/{error_number}` ファイルを見つけます。
