---
title: ルートの設定
description: Adobe Commerce on cloud infrastructure環境に対する受信HTTPS リクエストのルートを定義する方法について説明します。
feature: Cloud, Configuration, Routes
exl-id: f0d6eefa-1122-4753-8a7c-1fa0c77590f0
TQID: https://experienceleague.adobe.com/4EUSHNE6YAfXk4e7ooGRjZiICgHueLKrEA-tDskIPl0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 910
ht-degree: 0%

---

# ルートの設定

`.magento/routes.yaml` ディレクトリの`routes.yaml` ファイルは、クラウドインフラストラクチャのIntegration、Staging、およびProduction環境でAdobe Commerceのルートを定義します。 ルートは、アプリケーションが受信するHTTP要求とHTTPS要求をどのように処理するかを決定します。

デフォルトの`routes.yaml` ファイルは、単一のデフォルトドメインを持つプロジェクトおよび複数のドメインに設定されたプロジェクトで、HTTP リクエストをHTTPSとして処理するためのルートテンプレートを指定します。

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

設定されたルートのリストを表示するには、`magento-cloud` CLIを使用します。

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Pro環境の設定の更新

{{pro-self-service-warning}}

## ルートテンプレート

`routes.yaml` ファイルは、テンプレート化されたルートとその設定のリストです。 ルートテンプレートでは、次のプレースホルダーを使用できます。

- `{default}` プレースホルダーは、プロジェクトのデフォルトとして設定された修飾ドメイン名を表します。

  例えば、デフォルトドメイン `example.com`と次のルートテンプレートを持つプロジェクトの場合です。

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  これらのテンプレートは、実稼動環境の次のURLに解決されます。

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- `{all}` プレースホルダーは、プロジェクトに設定されたすべてのドメイン名を表します。

  例えば、次のルートテンプレートを持つ`example.com`および`example1.com` ドメインを持つプロジェクトの場合：

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  これらのテンプレートは、プロジェクト内のすべてのドメインに対して次のルートに解決されます。

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  `{all}` プレースホルダーは、複数のドメイン用に設定されたプロジェクトに便利です。 実稼動以外のブランチでは、`{all}`は各ドメインのプロジェクト IDと環境IDに置き換えられます。

  プロジェクトにドメインが設定されていない場合（開発中に一般的なドメイン）、`{all}` プレースホルダーは、`{default}` プレースホルダーと同じように動作します。

また、Adobe Commerceは、アクティブな統合環境ごとにルートを生成します。 統合環境の場合、`{default}` プレースホルダーは次のドメイン名に置き換えられます。

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

例えば、`us` クラスターでホストされている`mswy7hzcuhcjw` プロジェクトの`refactorcss` ブランチには、次のドメインがあります。

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Cloud プロジェクトで複数のストアをサポートしている場合は、[複数のweb サイトまたはストア &#x200B;](../store/multiple-sites.md)のルート設定手順に従います。

### 末尾のスラッシュ

ルート定義には、フォルダーまたはディレクトリを示す末尾のスラッシュが含まれていますが、同じコンテンツを末尾のスラッシュの有無で提供できます。 次のURLは同じように解決されますが、_2つの異なる_ URLとして解釈できます。

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>ディレクトリに末尾のスラッシュを使用することをお勧めしますが、どの方法を選択しても、2つの場所を生成しないようにするには、**一貫性を維持**&#x200B;することが重要です。

## プロトコルのルーティング

すべての環境で、HTTPとHTTPSの両方が自動的にサポートされます。

- 設定がHTTP ルートのみを指定する場合、HTTPS ルートは自動的に作成され、リダイレクトを必要とせずにサイトをHTTPとHTTPSの両方から提供できます。

  例えば、デフォルトドメイン `example.com`と次のルートテンプレートを持つプロジェクトの場合です。

  ```text
  http://{default}/
  ```

  このテンプレートは、次のURLに解決されます。

  ```text
  http://example.com/
  
  https://example.com/
  ```

- 設定でHTTPS ルートのみを指定する場合、すべてのHTTP リクエストはHTTPSにリダイレクトされます。

  例えば、次のルートテンプレートを持つデフォルトドメイン `example.com`を持つプロジェクトなどです。

  ```text
  https://{default}/
  ```

  このテンプレートは次のURLに解決されます。

  ```text
  https://example.com/
  ```

  また、次のリダイレクトも処理します。

  `http://example.com/` ==> `https://example.com/`

TLS経由ですべてのページを提供します。 この設定では、暗号化されていないすべてのリクエストのリダイレクトを、次のいずれかの方法を使用してTLS同等のリクエストに設定する必要があります。

- プロトコルを`routes.yaml` ファイルのHTTPSに変更します。

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- ステージング環境と実稼動環境の場合は、管理UIから「[FastlyにTLSを強制](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html)」オプションを有効にします。 このオプションを使用すると、FastlyはHTTPSへのリダイレクトを処理するので、`routes.yaml`設定を更新する必要はありません。

## ルートオプション

次のプロパティを使用して、各ルートを個別に設定します。

| プロパティ | 説明 |
| ---------------- | ----------- |
| `type: upstream` | アプリケーションを提供します。 また、アプリケーションの名前（`.magento.app.yaml`で定義されているように）の後に`:http` エンドポイントを指定する`upstream` プロパティもあります。 |
| `type: redirect` | 別のルートにリダイレクトします。 その後に`to` プロパティが続きます。このプロパティは、テンプレートによって識別される別のルートへのHTTP リダイレクトです。 |
| `cache:` | ルート [&#128279;](caching.md)の キャッシュを制御します。 |
| `redirects:` | [&#x200B; リダイレクトルール &#x200B;](redirects.md)を制御します。 |
| `ssi:` | [&#x200B; サーバーサイド インクルード &#x200B;](server-side-includes.md)の有効化を制御します。 |

## シンプルなルート

次の例では、ルート設定によってapex ドメインと`www` サブドメインが`mymagento` アプリケーションにルーティングされます。 このルートは、HTTPS リクエストをリダイレクトしません。

**例1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

この例では、リクエストルーティングは次のルールに従っています。

- サーバーは、次のURL パターンでリクエストに直接応答します。

  ```text
  http://example.com/path
  ```

- サーバーは、次のURL パターンを持つリクエストに対して&#x200B;_301 リダイレクト_&#x200B;を発行します。

  ```text
  http://www.example.com/mypath
  ```

  これらのリクエストは、例えば、apex ドメインにリダイレクトされます。

  ```text
  http://example.com/mypath
  ```

次の例では、ルート設定はwww ドメインからapex ドメインにURLをリダイレクトしません。 代わりに、リクエストはwww ドメインとapex ドメインの両方から提供されます。

**例2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## ワイルドカードルート

Adobe Commerce on cloud infrastructureはワイルドカードルートをサポートしているので、複数のサブドメインを同じアプリケーションにマッピングできます。 これは、リダイレクト ルートとアップストリーム ルートに対して機能します。 ルートの先頭にはアスタリスク（\*）を付けます。 例えば、同じアプリケーションに対して次のルートを作成します。

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

これは、ライブ環境でキャッチオールドメインとして機能します。

### マッピングされていないドメインのルーティング

ドット （`.`）を使用してサブドメインを分離し、ドメインにマッピングされていないシステムにルーティングできます。

**例：**

`add-theme` ブランチを持つプロジェクトは、次のURLにルーティングします。

```text
http://add-theme-projectID.us.magento.com/
```

次のルートテンプレートを定義する場合：

```text
http://www.{default}/
```

ルートは次のURLに解決されます。

```text
http://www.add-theme-projectID.us.magento.com/
```

ドットとルートが解決する前に、任意のサブドメインを挿入できます。

**例：**

次のルートテンプレートを定義します。

```text
http://*.{default}/
```

このテンプレートは、次の両方のURLを解決します。

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

マッピングされていないドメインのルートパターンを表示するには、環境へのSSH接続を確立し、`magento-cloud` CLIを使用してルートを一覧表示します。

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

詳細については、[&#x200B; リダイレクト &#x200B;](redirects.md)で説明しているように、_部分リダイレクト_&#x200B;などの複雑なリダイレクトルールを管理し、ルートベースの[&#x200B; キャッシュ &#x200B;](caching.md)のルールを指定できます。

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
