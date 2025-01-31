---
title: リファラルスパムをブロック
description: Fastly Edge辞書とカスタム VCL スニペットを使用して、サイトからリファラルスパムをブロックします。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# リファラルスパムをブロック

次の例は、カスタム VCL スニペットを使用して [Fastly Edge Dictionary](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) を設定し、クラウドインフラストラクチャサイト上のAdobe Commerceからの紹介スパムをブロックする方法を示しています。

>[!NOTE]
>
>カスタム VCL 設定をステージング環境に追加して、実稼動環境に対して実行する前にテストできるようにすることをお勧めします。

**前提条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- サイトのログで偽のリファラル URL を確認し、ブロックするドメインのリストを作成します。

## リファラーブロックリストの作成

Edge ディクショナリは、VCL スニペットの処理中に、VCL 関数からアクセス可能なキーと値のペアを作成します。 この例では、ブロックするリファラー web サイトのリストを提供するエッジ辞書を作成します。

{{admin-login-step}}

1. **ストア**/**設定**/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**Edgeの辞書** を展開します。

1. 辞書コンテナの作成：

   - **コンテナを追加** をクリックします。

   - *コンテナ* ページで **辞書名**—`referrer_blocklist` を入力します。

   - **変更後にアクティベート** を選択して、編集中の Fastly サービス設定のバージョンに変更をデプロイします。

   - **アップロード** をクリックして、Fastly サービス設定に辞書を添付します。

1. ブロックするドメイン名のリストを `referrer_blocklist` の辞書に追加します。

   - `referrer_blocklist` 辞書の「設定」アイコンをクリックします。

   - キーと値のペアを新しい辞書に追加して保存します。 この例では、各 **Key** はブロックするリファラー URL のドメイン名で、**Value** は `true` です。

     ![ 不正なリファラー辞書項目を追加 ](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - 「**キャンセル**」をクリックして、システム設定ページに戻ります。

1. 「**設定を保存**」をクリックします。

1. ページ上部の通知に従ってキャッシュを更新します。

Edge辞書について詳しくは、Fastly ドキュメントの [Edge辞書の作成と使用 ](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) および [ カスタム VCL スニペット ](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) を参照してください。

## リファラースパムをブロックするカスタム VCL スニペットの作成

次のカスタム VCL スニペットコード（JSON 形式）は、リクエストをチェックしてブロックするロジックを示しています。 VCL スニペットは、リファラー web サイトのホストをヘッダーに取り込み、ホスト名を `referrer_blocklist` ディクショナリ内の URL のリストと比較します。 ホスト名が一致する場合、リクエストは `403 Forbidden` エラーでブロックされます。

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

この例に基づいてスニペットを作成する前に、値を確認して、変更が必要かどうかを判断してください。

- `name` — VCL スニペットの名前。 この例では、`block_bad_referrer` を使用します。

- `dynamic` – 値 0 は、Fastly 設定用にバージョン管理された VCL にアップロードする [ 通常のスニペット ](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) を示します。

- `priority` — VCL スニペットを実行するタイミングを指定します。 優先度は、デフォルトのMagentoVCL スニペット（`magentomodule_*`）に優先度 50 が割り当てられる前に、このスニペットコードを実行する `5` 要があります。 各カスタムスニペットの優先度を、スニペットを実行するタイミングに応じて 50 より高くまたは低く設定します。 優先度の低いスニペットが最初に実行されます。

- 「`type`」 - VCL バージョンにスニペットを挿入する場所を指定します。 この例では、VCL スニペットが `recv` スニペットです。 スニペットを VCL バージョンに挿入すると、既定の Fastly VCL コードの下で、オブジェクトの上にある `vcl_recv` サブルーチンに追加されます。

- `content` – 改行なしで 1 行で実行する VCL コードのスニペット。

環境のコードを確認して更新した後、次のいずれかの方法を使用して、カスタム VCL スニペットを Fastly サービス設定に追加します。

- [ カスタム VCL スニペットを管理者から追加します ](#add-the-custom-vcl-snippet)。 管理者にアクセスできる場合は、この方法をお勧めします。 （[Fastly バージョン 1.2.58](fastly-configuration.md#upgrade) 以降が必要です。）

- JSON コードの例をファイル（例：`allowlist.json`）に保存して、[Fastly API を使用してアップロード ](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api) します。 管理者にアクセスできない場合は、この方法を使用します。

## カスタム VCL スニペットの追加

{{admin-login-step}}

1. **ストア**/設定/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**カスタム VCL スニペット** の順に展開します。

1. **カスタムスニペットを作成** をクリックします。

1. VCL スニペットの値を追加します。

   - **名前** — `block_bad_referrer`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツ —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. **作成** をクリックします。

   ![ カスタムリファラーブロック VCL スニペットの作成 ](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. ページのリロード後、「**Fastly 設定 *セクションの**Fastly に VCL をアップロード* をクリックします。

1. アップロードが完了したら、ページ上部の通知に従ってキャッシュを更新します。

Fastly は、アップロード処理中に更新された VCL バージョンを検証します。 検証に失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCL を再度アップロードします。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
