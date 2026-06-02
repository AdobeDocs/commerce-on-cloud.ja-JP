---
title: サイトマップと検索エンジンロボットを追加する
description: Adobe Commerce on cloud infrastructureにサイトマップと検索エンジンロボットを追加する方法について説明します。
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
TQID: https://experienceleague.adobe.com/Nve-76Ow3rv0PrGEUVTSfr3eyJcw8IFj9bbpS10HnNY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 612
ht-degree: 0%

---

# サイトマップと検索エンジンロボットを追加する

`sitemap.xml` ファイルを生成してルートディレクトリに書き込もうとすると、次のエラーが発生します。

```
Please make sure that "/" is writable by the web-server.
```

Adobe Commerce クラウド基盤では、`var`、`pub/media`、`pub/static`、`app/etc`などの特定のディレクトリにのみ書き込むことができます。 管理パネルを使用して`sitemap.xml` ファイルを生成する場合は、`/media/` パスを指定する必要があります。

`robots.txt` ファイルは、オンデマンドで`robots.txt` コンテンツを生成し、データベースに保存するため、生成する必要はありません。 `<domain.your.project>/robots.txt`または`<domain.your.project>/robots` リンクを使用すると、ブラウザーでコンテンツを表示できます。

これには、ECE-Tools バージョン 2002.0.12以降と、更新された`.magento.app.yaml` ファイルが必要です。 これらのルールの例については、[magento-cloud リポジトリ &#x200B;](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49)を参照してください。

**バージョン 2.2以降**&#x200B;で`sitemap.xml` ファイルを生成するには：

1. 管理者にアクセスします。
1. _マーケティング_ メニューで、_SEOと検索_ セクションの&#x200B;**サイトマップ**&#x200B;をクリックします。
1. _サイトマップ_&#x200B;表示で、**サイトマップを追加**&#x200B;をクリックします。
1. _新しいサイトマップ_ ビューで、次の値を入力します。

   - **ファイル名**:`sitemap.xml`
   - **パス**:`/media/`

1. 「**保存して生成**」をクリックします。 新しいサイトマップは、_サイトマップ_ グリッドで利用できるようになります。
1. Google _の_ リンク列のパスをクリックします。

**コンテンツを`robots.txt` ファイル**&#x200B;に追加するには：

1. 管理者にアクセスします。
1. _コンテンツ_ メニューで、「_デザイン_」セクションの「**設定**」をクリックします。
1. _デザイン設定_ ビューで、_アクション_&#x200B;列のweb サイトの&#x200B;**編集**&#x200B;をクリックします。
1. _メイン Web サイト_ ビューで、**検索エンジン ロボット**&#x200B;をクリックします。
1. robots.txt **フィールドの** Edit カスタム命令を更新します。
1. 「**設定を保存**」をクリックします。
1. ブラウザーで`<domain.your.project>/robots.txt` ファイルまたは`<domain.your.project>/robots` URLを確認します。

>[!NOTE]
>
>`<domain.your.project>/robots.txt` ファイルで`404 error`が生成された場合、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信して、`/robots.txt`から`/media/robots.txt`へのリダイレクトを削除します。

## Fastly VCL スニペットを使用した書き換え

ドメインが異なり、個別のサイトマップが必要な場合は、VCLを作成して適切なサイトマップにルーティングできます。 前述のように、管理パネルで`sitemap.xml` ファイルを生成し、カスタム Fastly VCL スニペットを作成してリダイレクトを管理します。 [&#x200B; カスタム Fastly VCL スニペット &#x200B;](../cdn/fastly-vcl-custom-snippets.md)を参照してください。

>[!NOTE]
>
> カスタム VCL スニペットは、Admin UIまたはFastly APIを使用してアップロードできます。 [&#x200B; カスタム VCL スニペットの例とチュートリアル &#x200B;](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code)を参照してください。

### リダイレクトにFastly VCL スニペットを使用する

`type`と`content`のキーと値のペアを使用して`sitemap.xml`から`/media/sitemap.xml`までのパスを書き換えるカスタム VCL スニペットを作成します。

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

次の例は、`robots.txt`および`sitemap.xml`のパスを`/media/robots.txt`および`/media/sitemap.xml`に書き換える方法を示しています

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**特定のドメインリダイレクトにFastly VCL スニペットを使用するには**:

ドメインが`domain.com`の`pub/media/domain_robots.txt` ファイルを作成し、次のVCL スニペットを使用します。

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL スニペットは`http://domain.com/robots.txt`をルーティングし、`pub/media/domain_robots.txt` ファイルを表示します。

単一のスニペットで`robots.txt`と`sitemap.xml`のリダイレクトを設定するには、`pub/media/domain_robots.txt`と`pub/media/domain_sitemap.xml`のファイルを作成します。このファイルには、ドメインが`domain.com`あり、次のVCL スニペットを使用します。

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

`sitemap`管理者設定では、`/`ではなく`pub/media/`を使用してファイルの場所を指定する必要があります。

### 検索エンジンによるインデックス作成の設定

実稼動環境で`robots.txt` カスタマイズをアクティブ化するには、Cloud Consoleのプロジェクト設定の`<environment-name>` オプションに対する検索エンジンによるインデックス作成を有効にします。

- レガシークラウドコンソール - URLはパターン `https://<region-id>.magento.cloud/projects/<project_id>`に従います

  設定[!UICONTROL Indexing by search engines] （Legacy Console） [!UICONTROL Hide from search engines] （Adobe Console）を&#x200B;**On**&#x200B;に切り替えます。

  ![環境の管理に[!DNL Cloud Console]を使用](../../assets/robots-indexing-by-search-engine.png)

- Adobe Cloud Console - URLはパターン ``https://console.adobecommerce.com/<username>/<project_id>``に従います

  設定[!UICONTROL Hide from search engines]のチェックを外します。

- magento-cloud CLIを使用して、この設定を更新することもできます。

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- 検索エンジンによるインデックス作成は、実稼動環境でのみ有効にできますが、下位の環境では有効にできません。
>
>- PWA Studioを使用しており、設定した`robots.txt` ファイルにアクセスできない場合は、**Stores**/Configuration > **General** > **Web**/UPWARD PWA Configurationの[Front Name 許可リストに加える](https://github.com/magento/magento2-upward-connector#front-name-allowlist)に`robots.txt`を追加します。

