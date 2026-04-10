---
title: Web プロパティ
description: Web プロパティの設定方法については、 [!DNL Commerce]  アプリケーション設定ファイルの例を参照してください。
feature: Cloud, Configuration
exl-id: 6ecf6fb5-57a8-435c-8de3-f66dc56837fe
source-git-commit: 94a7748348ba590bb4fed740df658c5bac4c31e9
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# Web プロパティ

`web` プロパティは、アプリケーションをWeb （HTTP）に公開する方法を定義し、web アプリケーションがコンテンツを提供する方法を決定し、各場所&#x200B;_ブロック_&#x200B;にルールを設定することで、アプリケーションコンテナが受信リクエストにどのように応答するかを制御します。 ブロックは、スラッシュ （`/`）で始まる絶対パスを表します。

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

`locations` ブロックごとに次のキー値を使用して、`locations`設定を微調整できます。

| 属性 | 説明 |
| :--- | :--- |
| `allow` | 「ルール」に一致しないファイルを提供します。 デフォルト値= `true` |
| `expires` | ブラウザーにコンテンツをキャッシュする秒数を設定します。 このキーは、静的コンテンツの`cache-control`および`expires` ヘッダーを有効にします。 この値が設定されていない場合、静的コンテンツファイルを提供する際に`expires` ディレクティブと結果ヘッダーが含まれません。 負の1 （`-1`）値を指定すると、キャッシュが行われず、デフォルト値になります。 時間値は、次の単位で表現できます。`ms` （ミリ秒）、`s` （秒）、`m` （分）、`h` （時間）、`d` （日）、`w` （週）、`M` （月、30日）、または`y` （年、365d） |
| `headers` | この場所から配信される静的コンテンツに対して、`X-Frame-Options`などのカスタムヘッダーを設定します。 |
| `index` | `index.html` ファイルなど、アプリケーションを提供する静的ファイルを一覧表示します。 このキーはコレクションを必要としています。 これは、この場所の`allow`または`rules` キーによってファイルまたはファイルへのアクセスが「許可」されている場合にのみ機能します。 |
| `rules` | 場所の上書きを指定します。 リクエストを一致させるには、正規表現を使用します。 受信リクエストがルールに一致する場合、リクエストの通常の処理は、ルールで使用されるキーによって上書きされます。 |
| `passthru` | 静的ファイルまたはPHP ファイルが見つからない場合に使用するURLを設定します。 通常、このURLは、`/index.php`や`/app.php`などのアプリケーションのフロントコントローラーです。 |
| `root` | Web上で公開されるアプリケーションのルートに対する相対パスを設定します。 Cloud プロジェクトのパブリックディレクトリ（場所「/」）は、デフォルトで「pub」に設定されています。 |
| `scripts` | この場所でスクリプトの読み込みを許可します。 スクリプトを許可するには、値を`true`に設定します。 `pub/media`および`pub/static` ディレクトリの場合、アップロードされたファイルの実行を防ぐため、デフォルト設定は`scripts: false`に設定されます。 |

>[!IMPORTANT]
>
>**セキュリティに関するメモ：** Cloud上のAdobe Commerceのデフォルトの`web` プロパティ設定では、アップロードされたファイルの実行を防ぐために、メディアの場所に`scripts: false`が設定されます。 実装のセキュリティへの影響を完全に理解しない限り、この設定を上書きしないでください。

デフォルトの設定では、次のことが可能です。

- ルート （`/`） パスからアクセスできるのは、webとメディアのみです
- `~/pub/media`および`~/pub/static`のパスから、すべてのファイルにアクセスできます

次の例は、[`mounts` プロパティ &#x200B;](properties.md#mounts)のエントリに関連付けられたweb アクセス可能な場所のセットに対する`.magento.app.yaml` ファイルのデフォルト設定を示しています。

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
>次の例は、単一のドメインをサポートするように設定されたCloud プロジェクトのデフォルトのweb設定を示しています。 複数のweb サイトまたはストアのサポートが必要なプロジェクトの場合は、共有ドメインをサポートするように`web`設定を設定する必要があります。 共有ドメインの場所の設定[を参照してください](../store/multiple-sites.md#configure-locations-for-shared-domains)。
