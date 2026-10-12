---
title: キャッシュ
description: Adobe Commerce on cloud infrastructure環境のキャッシュを有効にする方法について説明します。
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
TQID: 'https://experienceleague.adobe.com/dCr0px-0XWXIznsg1w8tUnBaAeXvanY1h-mwiu6GfzU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 1783ae9f-7157-5aeb-a915-91e260301e46
    internal-label: Routes
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%
---
# キャッシュ

クラウドインフラストラクチャプロジェクト環境でキャッシュを有効にできます。 キャッシュを無効にすると、Adobe Commerceはファイルを直接提供します。

{{route-placeholder}}

## キャッシュの設定

次のように`.magento/routes.yaml` ファイルでキャッシュルールを設定して、アプリケーションのキャッシュを有効にします。

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## ルートベースのキャッシュ

次の例に示すように、複数のルートに対して個別にキャッシングルールを設定して、きめ細かいキャッシュを有効にします。

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

上記の例では、次のルートをキャッシュします。

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

次のルートは&#x200B;**not** キャッシュです：

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>ルートの正規表現は&#x200B;**サポートされていません**。

## キャッシュ時間

キャッシュ時間は、`Cache-Control`応答ヘッダー値によって決まります。 応答に`Cache-Control` ヘッダーがない場合、`default_ttl` キーが使用されます。

## キャッシュキー

レスポンスをキャッシュする方法を決定するために、Adobe Commerceは、いくつかの要素に依存するキャッシュキーを作成し、このキーに関連付けられたレスポンスを保存します。 リクエストが同じキャッシュキーを持つ場合、応答は再利用されます。 その目的は、HTTP [`Vary` ヘッダー](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44)と似ています。

パラメーター`headers`および`cookies` キーを使用すると、このキャッシュキーを変更できます。

これらのキーのデフォルト値は次のとおりです。

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## キャッシュ属性

### `enabled`

`true`に設定した場合は、このルートのキャッシュを有効にします。 `false`に設定した場合は、このルートのキャッシュを無効にします。

### `headers`

キャッシュキーが依存する値を定義します。

例えば、`headers` キーが次の場合です。

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

次に、Adobe Commerceは、`Accept` HTTP ヘッダーの値ごとに異なるレスポンスをキャッシュします。

### `cookies`

`cookies` キーは、キャッシュキーがどの値に依存する必要があるかを定義します。

例：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

キャッシュキーは、リクエスト内の`value` Cookieの値によって異なります。

`cookies` キーの値が`["*"]`の場合、特殊ケースが存在します。 この値は、Cookieを使用したリクエストがキャッシュをバイパスすることを意味します。 これがデフォルト値です。

>[!NOTE]
>
>Cookie名にワイルドカードを使用することはできません。 正確なCookie名を使用するか、すべてのCookieにアスタリスク （`*`）を付けます。 例えば、`SESS*`または`~SESS`は現在&#x200B;**not**&#x200B;の有効な値です。

Cookieには次の制限があります。

- システムには最大&#x200B;**50 Cookie**&#x200B;が設定されています。 それ以外の場合は、`Unable to send the cookie. Maximum number of cookies would be exceeded`例外がスローされます。 Cookieの数を200に増やすには、[品質パッチツール &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-learn/tutorials/tools/quality-patch-tool)を使用して[MDVA-12304 パッチ &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/quality-patches-tool/release-notes)を適用します。
- 最大Cookie サイズは&#x200B;**4096 バイト**&#x200B;です。 それ以外の場合は、`Unable to send the cookie. Size of '%name' is %size bytes`例外がスローされます。

### `default_ttl`

応答に`Cache-Control` ヘッダーがない場合、`default_ttl` キーを使用してキャッシュ時間を秒単位で定義します。 デフォルト値は`0`です。つまり、何もキャッシュされません。
