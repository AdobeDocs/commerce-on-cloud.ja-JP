---
title: キャッシュ
description: クラウドインフラストラクチャ環境でAdobe Commerceのキャッシュを有効にする方法を説明します。
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
source-git-commit: a1ed2818cbaf5adf8b673df0ee9b9218e6f700a2
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# キャッシング

クラウドインフラストラクチャープロジェクト環境キャッシュを有効にすることができます。 キャッシュを無効にすると、Adobe Systems Commerce がファイルを直接提供します。

{{route-placeholder}}

## 設定アップキャッシュ

次のように `.magento/routes.yaml` ファイルでキャッシュルールを設定して、アプリケーションのキャッシュを有効にします。

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

次の例に示すように、複数のルートに対して個別に キャッシュ ルールを設定することで、きめ細かなキャッシュを有効にします。

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

キャッシュ期間は、 `Cache-Control` 応答ヘッダー値によって決まります。 応答に `Cache-Control` ヘッダーが含まれていない場合は、 `default_ttl` キーが使用されます。

## キャッシュキー

応答をキャッシュする方法を決定するために、Adobe Systems Commerce は、いくつかの要因に依存するキャッシュキーを作成し、このキーに関連付けられた応答ストアます。 リクエストに同じキャッシュキーが付いている場合、応答が再利用されます。 その目的は、HTTP [`Vary` ヘッダー](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44)に似ています。

パラメーター `headers` キーと `cookies` キーを使用すると、このキャッシュ キーを変更できます。

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

次にAdobe Systemsコマースは、 `Accept` HTTP ヘッダーの値ごとに異なる応答をキャッシュします。

### `cookies`

`cookies` キーは、キャッシュ キーが依存する必要がある値を定義します。

例：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

キャッシュキーは、リクエスト内の `value` cookieの値によって異なります。

`cookies` キーに `["*"]` 値がある場合は、特別なケースがあります。この値は、cookieを持つすべてのリクエストキャッシュをバイパスすることを意味します。 これがデフォルト値です。

>[!NOTE]
>
>cookie名にワイルドカードは使用できません。 正確な cookie 名を使用するか、アスタリスク（`*`）ですべての cookie に一致します。 例えば、`SESS*` や `~SESS` は現在 **無効** 値です。

Cookie には次の制限があります。

- システムには最大 **50 個の cookie** が設定されています。 それ以外の場合、アプリケーションは `Unable to send the cookie. Maximum number of cookies would be exceeded` 例外をスローします。 Cookie の数を 200 に増やすには、[Quality Patches Tool](https://experienceleague.adobe.com/ja/docs/commerce-learn/tutorials/tools/quality-patch-tool) を使用して [MDVA-12304 パッチ ](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html?lang=ja) を適用します。
- 最大cookieサイズは **4096 バイト**&#x200B;です。 それ以外の場合、アプリケーションは `Unable to send the cookie. Size of '%name' is %size bytes` 例外をスローします。

### `default_ttl`

応答に `Cache-Control` ヘッダーがない場合は、 `default_ttl` キーを使用してキャッシュ期間を秒単位で定義します。 デフォルト値は `0` です。これは、何もキャッシュされないことを意味します。
