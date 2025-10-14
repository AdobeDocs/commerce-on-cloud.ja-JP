---
title: サーバーサイドインクルード
description: クラウドインフラストラクチャー上のAdobe Commerceでサーバーサイドインクルードを使用する方法について説明します。
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# サーバーサイドインクルード

[&#x200B; サーバーサイドインクルード &#x200B;](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) （SSI）は、ページのレンダリング中にサーバーで評価される、HTMLページ内のディレクティブです。 SSI を使用すると、ページ全体を提供することなく、動的に生成されたコンテンツを既存のHTMLページに追加できます。

SSI は、ルートごとに `.magento/routes.yaml` で有効または無効にすることができます。次に例を示します。

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI を使用すると、既存の [&#x200B; キャッシュ設定 &#x200B;](caching.md) に従ってHTMLがHTMLの一部を埋める原因となるサーバーレスポンスディレクティブをサーバーに含めることができます。

次の例では、ページの上部に動的日付コントロールを挿入し、下部に 600 秒ごとに更新される別の日付コントロールを挿入する方法を示します。

`/index.php` などの任意のページに次の内容を追加します。

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

`time.php` に次の内容を追加します。

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

コントロールを追加したページを参照します。 ページを数回更新すると、ページの上部の時間は変更されますが、下部の時間は 600 秒ごとにのみ変更されます。
