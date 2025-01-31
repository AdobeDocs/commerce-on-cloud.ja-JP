---
title: キャッシュ
description: クラウドインフラストラクチャ環境でAdobe Commerceのキャッシュを有効にする方法を説明します。
feature: Cloud, Cache, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# キャッシュ

クラウドインフラストラクチャプロジェクト環境でキャッシュを有効にすることができます。 キャッシュを無効にした場合、Adobe Commerceはファイルを直接提供します。

{{route-placeholder}}

## キャッシュの設定

`.magento/routes.yaml` ファイルにキャッシュルールを設定して、アプリケーションのキャッシュを次のように有効にします。

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

次の例に示すように、複数のルートのキャッシュルールを別々に設定して、詳細なキャッシュを有効にします。

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

また、次のルートは **キャッシュされません**。

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>ルートの正規表現はサポートされて **ません**。

## キャッシュ時間

キャッシュ時間は、`Cache-Control` 応答ヘッダー値によって決まります。 応答に `Cache-Control` ヘッダーがない場合は、`default_ttl` キーが使用されます。

## キャッシュキー

レスポンスをキャッシュする方法を決定するために、Adobe Commerceは、いくつかの要因に依存するキャッシュキーを作成し、そのキーに関連付けられたレスポンスを保存します。 リクエストに同じキャッシュキーが含まれる場合、応答は再利用されます。 その目的は HTTP [`Vary` ヘッダー ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44) に似ています。

パラメーター `headers` および `cookies` キーを使用すると、このキャッシュキーを変更できます。

これらのキーのデフォルト値は次のとおりです。

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## キャッシュ属性

### `enabled`

`true` に設定した場合は、このルートのキャッシュを有効にします。 `false` に設定した場合は、このルートのキャッシュを無効にします。

### `headers`

キャッシュキーが依存する必要がある値を定義します。

例えば、`headers` のキーが次のような場合：

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

次に、Adobe Commerceは、`Accept` HTTP ヘッダーの値ごとに異なる応答をキャッシュします。

### `cookies`

`cookies` キーは、キャッシュキーが依存する必要がある値を定義します。

例：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

キャッシュキーは、リクエストの `value` cookie の値によって異なります。

`cookies` キーの値が `["*"]` の場合は、特殊なケースが存在します。 この値は、cookie を持つリクエストによってキャッシュがバイパスされることを意味します。 これがデフォルト値です。

>[!NOTE]
>
>cookie 名にワイルドカードを使用することはできません。 正確な cookie 名を使用するか、アスタリスク（`*`）ですべての cookie に一致します。 例えば、`SESS*` や `~SESS` は現在 **無効** 値です。

Cookie には次の制限があります。

- システムでは、最大 **50 個の cookie** を設定できます。 それ以外の場合、アプリケーションは `Unable to send the cookie. Maximum number of cookies would be exceeded` 例外をスローします。
- Cookie の最大サイズは **4096 バイト** です。 それ以外の場合、アプリケーションは `Unable to send the cookie. Size of '%name' is %size bytes` 例外をスローします。

### `default_ttl`

応答に `Cache-Control` ヘッダーがない場合、`default_ttl` キーを使用してキャッシュ時間（秒単位）を定義します。 デフォルト値は `0` です。これは、何もキャッシュされないことを意味します。
