---
title: Web プロパティ
description: Web プロパティを設定する方法の例については、アプリケーション設定ファイルを参照  [!DNL Commerce]  てください。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Web プロパティ

`web` プロパティは、アプリケーションが Web に公開される方法（HTTP 単位）を定義し、Web アプリケーションがコンテンツを提供する方法を決定し、各場所 _ブロック_ にルールを設定することによって受信リクエストに対するアプリケーション コンテナの応答方法を制御します。 ブロックは、スラッシュ（`/`）で始まる絶対パスを表します。

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

各 `locations` ブロックの次のキー値を使用して、`locations` 設定を微調整できます。

| 属性 | 説明 |
| :--- | :--- |
| `allow` | 「ルール」に一致しないファイルを提供する。 デフォルト値= `true` |
| `expires` | ブラウザーでコンテンツをキャッシュする秒数を設定します。 このキーにより、静的コンテンツの `cache-control` ヘッダーと `expires` ヘッダーが有効になります。 この値が設定されていない場合、静的コンテンツファイルを提供するときに `expires` ディレクティブと結果のヘッダーは含まれません。 負の値を 1 （`-1`）にするとキャッシュは行われず、がデフォルト値になります。 時刻の値は、`ms` （ミリ秒）、`s` （秒）、`m` （分）、`h` （時間）、`d` （日）、`w` （週）、`M` （月、30d）、`y` （年、365d）の単位で表すことができます |
| `headers` | この場所から提供される静的コンテンツのカスタムヘッダー（`X-Frame-Options` など）を設定します。 |
| `index` | `index.html` ファイルなど、アプリケーションに提供する静的ファイルを一覧表示します。 このキーにはコレクションが必要です。 これは、ファイルへのアクセスがこの場所の `allow` または `rules` キーによって「許可」されている場合にのみ機能します。 |
| `rules` | 場所の上書きを指定します。 リクエストに一致する正規表現を使用します。 受信リクエストがルールに一致する場合、リクエストの通常の処理はルールで使用されたキーによって上書きされます。 |
| `passthru` | 静的ファイルまたは PHP ファイルが見つからない場合に使用する URL を設定します。 通常、この URL はアプリケーションのフロントコントローラー（`/index.php` や `/app.php` など）です。 |
| `root` | Web に公開するアプリケーションのルートを基準とした相対パスを設定します。 クラウドプロジェクトのパブリックディレクトリ（場所「/」）は、デフォルトで「pub」に設定されています。 |
| `scripts` | この場所にスクリプトを読み込むことができます。 値を `true` に設定して、スクリプトを許可します。 |

デフォルトの設定では、次のことが可能です。

- ルート（`/`）パスからは、web とメディアにのみアクセスできます
- `~/pub/static` パスと `~/pub/media` パスから、任意のファイルにアクセスできます

次の例は、[`mounts` プロパティのエントリに関連付けられた一連の web アクセス可能な場所の `.magento.app.yaml` ファイルのデフォルト設定を示してい ](properties.md#mounts) す。

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>この例は、単一のドメインをサポートするように設定されたクラウドプロジェクトのデフォルトの web 設定を示しています。 複数の web サイトやストアのサポートが必要なプロジェクトの場合は、共有ドメインをサポートするように `web` 設定を設定する必要があります。 [ 共有ドメインの場所の設定 ](../store/multiple-sites.md#configure-locations-for-shared-domains) を参照してください。
