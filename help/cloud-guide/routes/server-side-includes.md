---
title: サーバーサイドのインクルード
description: Adobe Commerce on cloud infrastructureでサーバーサイドインクルードを使用する方法について説明します。
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# サーバーサイドのインクルード

[&#x200B; サーバーサイドのインクルード &#x200B;](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) （SSI）は、HTML ページのディレクティブで、ページのレンダリング中にサーバー上で評価されます。 SSIを使用すると、ページ全体にサービスを提供することなく、動的に生成されたコンテンツを既存のHTML ページに追加できます。

`.magento/routes.yaml`では、ルートごとにSSIをアクティブ化または非アクティブ化できます。例：

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

SSIを使用すると、既存の[&#x200B; キャッシュ設定](caching.md)に従って、サーバーがHTMLの一部を入力する原因となるHTML応答ディレクティブを含めることができます。

次の例は、ページの上部に動的な日付コントロールを挿入し、下部に別の日付コントロールを挿入して600秒ごとに更新する方法を示しています。

`/index.php`などの任意のページに次を追加します。

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

以下を`time.php`に追加します。

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

コントロールを追加したページを参照します。 ページを何度か更新すると、ページの上部の時間は変化しますが、下部の時間は600秒ごとに変化します。
