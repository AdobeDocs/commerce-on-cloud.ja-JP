---
title: サイトマップと検索エンジンロボットを追加
description: クラウドインフラストラクチャー上でサイトマップと検索エンジンロボットをAdobe Commerceに追加する方法を説明します。
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1ecb820d55faa78e369d63996f11cd4d1d554e26
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# サイトマップと検索エンジンロボットを追加

`sitemap.xml` ファイルを生成してルートディレクトリに書き込もうとすると、次のエラーが発生します。

```
Please make sure that "/" is writable by the web-server.
```

クラウドインフラストラクチャー上のAdobe Commerceでは、`var`、`pub/media`、`pub/static`、`app/etc` など、特定のディレクトリにのみ書き込むことができます。 管理パネルを使用して `sitemap.xml` ファイルを生成する場合は、`/media/` パスを指定する必要があります。

`robots.txt` ファイルは、オンデマンドで `robots.txt` コンテンツを生成してデータベースに保存するので、生成する必要はありません。 `<domain.your.project>/robots.txt` または `<domain.your.project>/robots` のリンクを使用して、ブラウザーでコンテンツを表示できます。

これには、更新された `.magento.app.yaml` ファイルを含む ECE-Tools バージョン 2002.0.12 以降が必要です。 [magento-cloud リポジトリー ](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49) のこれらのルールの例を参照してください。

**バージョン 2.2 以降で `sitemap.xml` ファイルを生成するには**:

1. 管理者にアクセスします。
1. _マーケティング_ メニューで、「**SEO と検索** セクションの _サイトマップ_ をクリックします。
1. _サイトマップ_ ビューで、「**サイトマップを追加**」をクリックします。
1. _新しいサイトマップ_ ビューで、次の値を入力します。

   - **ファイル名**:`sitemap.xml`
   - **パス**:`/media/`

1. **保存して生成** をクリックします。 新しいサイトマップが _サイトマップ_ グリッドで使用できるようになります。
1. _Googleへのリンク_ 列のパスをクリックします。

**コンテンツを `robots.txt` ファイルに追加するには**:

1. 管理者にアクセスします。
1. _コンテンツ_ メニューで、「**デザイン** セクションの _設定_ をクリックします。
1. _デザイン設定_ ビューで、「**アクション** 列の web サイトの _編集_ をクリックします。
1. _メイン Web サイト_ ビューで、「**検索エンジンロボット**」をクリックします。
1. 「**robots.txt のカスタム命令を編集**」フィールドを更新します。
1. **設定を保存** をクリックします。
1. ブラウザーで `<domain.your.project>/robots.txt` ファイルまたは `<domain.your.project>/robots` URL を確認します。

>[!NOTE]
>
>`<domain.your.project>/robots.txt` ファイルで `404 error` が生成された場合は、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、`/robots.txt` から `/media/robots.txt` へのリダイレクトを削除します。

## Fastly VCL スニペットを使用した書き換え

異なるドメインがあり、個別のサイトマップが必要な場合は、適切なサイトマップにルーティングする VCL を作成できます。 上記のように、管理パネルで `sitemap.xml` ファイルを生成し、リダイレクトを管理するカスタム Fastly VCL スニペットを作成します。 [Custom Fastly VCL スニペット ](../cdn/fastly-vcl-custom-snippets.md) を参照してください。

>[!NOTE]
>
> 管理 UI から、または Fastly API を使用して、カスタム VCL スニペットをアップロードできます。 [ カスタム VCL スニペットの例とチュートリアル ](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code) を参照してください。

### リダイレクトに Fastly VCL スニペットを使用

カスタム VCL スニペットを作成し、`sitemap.xml` と `/media/sitemap.xml` のキーと値のペアを使用して、`type` のパスを書き換えて `content` きます。

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

次の例は、`robots.txt` および `sitemap.xml` のパスを `/media/robots.txt` および `/media/sitemap.xml` に書き換える方法を示しています

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**特定のドメインリダイレクトに Fastly VCL スニペットを使用するには**:

ドメインが `pub/media/domain_robots.txt` の `domain.com` ファイルを作成し、次の VCL スニペットを使用します。

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL スニペットは `http://domain.com/robots.txt` をルートし、`pub/media/domain_robots.txt` ファイルを表示します。

単一のスニペット内で `robots.txt` と `sitemap.xml` のリダイレクトを設定するには、`pub/media/domain_robots.txt` ファイルと `pub/media/domain_sitemap.xml` ファイルを作成します。ここで、ドメインは `domain.com` で、次の VCL スニペットを使用します。

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

`sitemap` 管理設定では、`pub/media/` ではなく `/` を使用してファイルの場所を指定する必要があります。

### 検索エンジンによるインデックス作成の設定

実稼動環境で `robots.txt` のカスタマイズを有効にするには、Cloud Console のプロジェクト設定で「`<environment-name>`**」オプションの検索エンジンによるインデックス作成を有効にします。

- 従来の Cloud Console - URL はパターン `https://<region-id>.magento.cloud/projects/<project_id>` に従います。
- Adobe Cloud Console - URL はパターン ``https://console.adobecommerce.com/<username>/<project_id>`` に従います。

1. 設定 [!UICONTROL Indexing by search engines] を **オン** に切り替えます。

   ![[!DNL Cloud Console] を使用した環境の管理 ](../../assets/robots-indexing-by-search-engine.png)

1. 設定 [!UICONTROL Hide from search engines] のチェックを外します。

また、magento-cloud CLI を使用してこの設定を更新することもできます。

```bash
magento-cloud environment:info -p <project_id> -e production restrict_robots false
```

>[!NOTE]
>
>- 検索エンジンによるインデックス作成は、実稼動環境でのみ有効にできますが、下位環境では有効にできません。
>
>- PWA Studio許可リストに加えるを使用していて、設定済みの `robots.txt` ファイルにアクセスできない場合は、`robots.txt`Front Name[](https://github.com/magento/magento2-upward-connector#front-name-allowlist)Stores **/Configuration/** General **/** Web **/UPWARD PWA Configuration に** を追加します。

