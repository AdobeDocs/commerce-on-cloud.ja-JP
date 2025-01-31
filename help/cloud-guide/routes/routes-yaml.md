---
title: ルートの設定
description: クラウドインフラストラクチャ環境でAdobe Commerceの受信 HTTPS リクエストのルートを定義する方法について説明します。
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# ルートの設定

`.magento/routes.yaml` ディレクトリの `routes.yaml` ファイルは、クラウドインフラストラクチャ統合、ステージングおよび実稼動環境でのAdobe Commerceのルートを定義します。 ルートは、受信する HTTP リクエストと HTTPS リクエストをアプリケーションがどのように処理するかを決定します。

デフォルトの `routes.yaml` ファイルは、単一のデフォルトドメインを持つプロジェクトと、複数のドメインに対して設定されたプロジェクトで、HTTP リクエストを HTTPS として処理するためのルートテンプレートを指定します。

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

`magento-cloud` CLI を使用して、設定済みルートのリストを表示します。

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Pro 環境の設定アップデート

{{pro-self-service-warning}}

## ルートテンプレート

`routes.yaml` ファイルは、テンプレート化されたルートとその設定のリストです。 ルートテンプレートでは、以下のプレースホルダーを使用できます。

- `{default}` プレースホルダーは、プロジェクトのデフォルトとして設定された修飾ドメイン名を表します。

  例えば、デフォルトのドメイン `example.com` と次のルートテンプレートを使用するプロジェクトの場合は、

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  これらのテンプレートは、実稼動環境では次の URL に解決されます。

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- `{all}` プレースホルダーには、プロジェクトに設定されたすべてのドメイン名が表示されます。

  例えば、次のルートテンプレートを持つ `example.com` ドメインと `example1.com` ドメインを持つプロジェクトでは、

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  これらのテンプレートは、プロジェクト内のすべてのドメインの次のルートに解決されます。

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  `{all}` プレースホルダーは、複数のドメイン用に設定されたプロジェクトに役立ちます。 実稼動以外のブランチでは、`{all}` は各ドメインのプロジェクト ID と環境 ID に置き換えられます。

  プロジェクトにドメインが設定されていない場合（開発時に一般的）、`{all}` プレースホルダーは `{default}` プレースホルダーと同じ方法で動作します。

Adobe Commerceは、アクティブな統合環境ごとにルートも生成します。 統合環境の場合、`{default}` のプレースホルダーは次のドメイン名に置き換えられます。

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

例えば、`us` クラスターでホストされる `mswy7hzcuhcjw` プロジェクトの `refactorcss` ブランチには、次のドメインがあります。

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>クラウドプロジェクトが複数のストアをサポートしている場合は、[ 複数の web サイトまたはストア ](../store/multiple-sites.md) のルート設定手順に従います。

### 末尾のスラッシュ

ルート定義には、フォルダーまたはディレクトリを示す末尾のスラッシュが含まれています。ただし、末尾のスラッシュの有無にかかわらず、同じコンテンツを提供できます。 次の URL は同じように解決されますが、_2 つの異なる_ URL として解釈される可能性があります。

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>ディレクトリには末尾のスラッシュを使用することをお勧めしますが、どちらの方法を選択する場合でも、2 つの場所を生成しないようにするために、**一貫性を維持** することが重要です。

## ルート プロトコル

すべての環境で、HTTP と HTTPS の両方を自動的にサポートしています。

- 設定で HTTP ルートのみが指定されている場合、HTTPS ルートが自動的に作成され、リダイレクトを必要とせずにサイトが HTTP と HTTPS の両方から提供されます。

  例えば、デフォルトのドメイン `example.com` と次のルートテンプレートを持つプロジェクトの場合：

  ```text
  http://{default}/
  ```

  このテンプレートは次の URL に解決されます。

  ```text
  http://example.com/
  
  https://example.com/
  ```

- 設定で HTTPS ルートのみが指定されている場合、すべての HTTP リクエストは HTTPS にリダイレクトされます。

  例えば、デフォルトのドメインを持つプロジェクト `example.com`、次のルートテンプレートを持つプロジェクトです。

  ```text
  https://{default}/
  ```

  このテンプレートは次の URL に解決されます。

  ```text
  https://example.com/
  ```

  また、次のリダイレクトも処理します。

  `http://example.com/` ==> `https://example.com/`

TLS 経由ですべてのページを提供します。 この設定では、次のいずれかの方法を使用して、暗号化されていないすべてのリクエストを TLS と同等の値にリダイレクトするように設定する必要があります。

- `routes.yaml` ファイルのプロトコルを HTTPS に変更します。

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- ステージング環境と実稼動環境の場合は、管理 UI から「[Fastly で TLS を強制 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html)」オプションを有効にします。 このオプションを使用すると、Fastly は HTTPS へのリダイレクトを処理するので、`routes.yaml` 設定を更新する必要はありません。

## ルートオプション

次のプロパティを使用して、各ルートを個別に設定します。

| プロパティ | 説明 |
| ---------------- | ----------- |
| `type: upstream` | アプリケーションを提供します。 また、（`.magento.app.yaml` で定義された）アプリケーションの名前の後に `:http` エンドポイントを指定する `upstream` プロパティもあります。 |
| `type: redirect` | 別のルートにリダイレクトします。 その後に `to` プロパティが続きます。これは、テンプレートで識別される別のルートへの HTTP リダイレクトです。 |
| `cache:` | [ ルートのキャッシュ ](caching.md) を制御します。 |
| `redirects:` | コントロール [ リダイレクトルール ](redirects.md)。 |
| `ssi:` | [ サーバーサイドインクルード ](server-side-includes.md) の有効化を制御します。 |

## シンプルなルート

次の例では、ルート設定は apex ドメインと `www` サブドメインを `mymagento` アプリケーションにルーティングします。 このルートでは、HTTPS リクエストはリダイレクトされません。

**例 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

この例では、リクエストルーティングは次のルールに従います。

- サーバーは、次の URL パターンを使用してリクエストに直接応答します。

  ```text
  http://example.com/path
  ```

- サーバーは、次の URL パターンのリクエストに対して _301 リダイレクト_ を発行します。

  ```text
  http://www.example.com/mypath
  ```

  これらのリクエストは、apex ドメインにリダイレクトされます。例：

  ```text
  http://example.com/mypath
  ```

次の例では、ルート設定で www ドメインから apex ドメインに URL がリダイレクトされません。 代わりに、リクエストは www ドメインと apex ドメインの両方から提供されます。

**例 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## ワイルドカードルート

クラウドインフラストラクチャー上のAdobe Commerceでは、ワイルドカードルートをサポートしているので、複数のサブドメインを同じアプリケーションにマッピングできます。 これは、リダイレクト ルートとアップストリーム ルートで機能します。 ルートの前にはアスタリスク（\*）を付けます。 例えば、同じアプリケーションに対して次のようなルートがあります。

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

これは、ライブ環境ではキャッチオール ドメインとして機能します。

### マッピングされていないドメインのルーティング

ドット（`.`）を使用して、ドメインにマッピングされていないシステムにルーティングすることで、サブドメインを分離できます。

**例：**

`add-theme` ブランチを持つプロジェクトは、次の URL にルーティングされます。

```text
http://add-theme-projectID.us.magento.com/
```

次のルートテンプレートを定義する場合：

```text
http://www.{default}/
```

ルートは次の URL に解決されます。

```text
http://www.add-theme-projectID.us.magento.com/
```

ドットとルートが解決される前に任意のサブドメインを挿入できます。

**例：**

次のルートテンプレートを定義します。

```text
http://*.{default}/
```

このテンプレートは次の両方の URL を解決します。

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

マッピングされていないドメインのルートパターンを確認するには、環境への SSH 接続を確立し、`magento-cloud` CLI を使用してルートをリストします。

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## リダイレクトとキャッシュ

[ リダイレクト ](redirects.md) で詳しく説明しているように、_部分リダイレクト_ などの複雑なリダイレクトルールを管理したり、ルートベースの [ キャッシュ ](caching.md) のルールを指定したりできます。

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
