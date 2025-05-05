---
title: リダイレクト
description: クラウドインフラストラクチャプロジェクトでAdobe Commerceのリダイレクトルールを管理する方法について説明します。
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# リダイレクト

リダイレクトルールの管理は、特に、時間の経過と共に変更または削除された受信リンクを失いたくない場合、web アプリケーションの一般的な要件です。

以下の例に、`routes.yaml` 設定ファイルを使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceでリダイレクトルールを管理する方法を示します。 このトピックで説明したリダイレクトメソッドが機能しない場合は、キャッシュヘッダーを使用して同じ操作を実行できます。

{{route-placeholder}}

## Pro 環境のアップデート

{{pro-self-service-warning}}

>[!WARNING]
>
>クラウドインフラストラクチャプロジェクトのAdobe Commerceの場合、`routes.yaml` ファイルに多数の非正規表現リダイレクトや書き換えを設定すると、パフォーマンスの問題が発生する可能性があります。 `routes.yaml` ファイルが 32 KB 以上の場合は、非正規表現のリダイレクトと書き換えを Fastly にオフロードします。 [2&rbrace;Adobe Commerce ヘルプセンター ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html?lang=ja) の Nginx （ルート）ではなく Fastly への非正規表現リダイレクトのオフロード _を参照してください。_

## ルート全体リダイレクト

ルート全体のリダイレクトを使用すると、`routes.yaml` ファイルを使用して単純なルートを定義できます。 例えば、次のように apex ドメインから `www` サブドメインにリダイレクトできます。

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## 部分的なルート リダイレクト

`.magento/routes.yaml` ファイルでは、パターン一致に基づいて、既存のルートに部分的なリダイレクトルールを追加できます。

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

部分リダイレクトは、アプリケーションから直接提供されるルートを含む、あらゆるタイプのルートで機能します。

`redirects` では、次の 2 つのキーを使用できます。

- **expires** - （オプション）ブラウザーでリダイレクトをキャッシュする時間を指定します。 有効な値の例としては、`3600s`、`1d`、`2w`、`3m` などがあります。

- **パス** – 部分ルートのリダイレクトルールの設定を指定する 1 つ以上のキーと値のペア。

  各リダイレクトルールでは、キーはリダイレクトのリクエストパスをフィルタリングするための式です。 値は、リダイレクトのターゲット先とリダイレクトを処理するためのオプションを指定するオブジェクトです。

  value オブジェクトには、次のプロパティがあります。

  | プロパティ | 説明 |
  | ---------- | ----------- |
  | `to` | 必須。リダイレクトルールのターゲット先を指定する、部分的な絶対パス、プロトコルとホストを含む URL、またはパターンです。 |
  | `regexp` | オプション。デフォルトは `false` です。 パス キーを PCRE 正規表現として解釈するかどうかを指定します。 |
  | `prefix` | リダイレクトをパスとその子の両方に適用するか、パス自体に適用するかを指定します。 デフォルトは `true` です。 `regexp` が `true` の場合、この値はサポートされません。 |
  | `append_suffix` | リダイレクトでサフィックスを引き継ぐかどうかを決定します。 デフォルトは `true` です。 `regexp` キーが `true` の場合、または `prefix` キーが `false` の場合、この値はサポートされません。 |
  | `code` | HTTP ステータスコードを指定します。 有効な状態コードは、[`301` （完全に移動） ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2)、[`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3)、[`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8)、および [`308`](https://www.rfc-editor.org/rfc/rfc7238) です。 デフォルトは `302` です。 |
  | `expires` | オプションで、ブラウザーでリダイレクトをキャッシュする時間を指定します。 デフォルトは、`redirects` キーの直下に定義された `expires` 値ですが、このレベルでは、個々の部分リダイレクトのキャッシュの有効期限を微調整できます。 |

## 部分的なルートリダイレクトの例

次の例は、様々な `paths` 設定オプションを使用して `routes.yaml` ファイルに部分ルートリダイレクトを指定する方法を示しています。

### 正規表現のパターンマッチング

正規表現に基づいてリダイレクトリクエストを設定するには、次の形式を使用します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

この設定では、正規表現に対してリクエストパスがフィルタリングされ、一致するリクエストが `https://example.com` にリダイレクトされます。 例えば、`https://example.com/regexp/a/b/c/match` へのリクエストは `https://example.com/a/b/c` にリダイレクトされます。

### プレフィックスのパターンマッチング

次の形式を使用して、指定した接頭辞パターンで始まるパスのリダイレクトリクエストを設定します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

この設定は次のように動作します。

- パターン `/from` に一致するリクエストをパス `http://{default}/to` にリダイレクトします。

- パターン `/from/another/path` に一致するリクエストを `https://{default}/to/another/path` にリダイレクトします。

- `prefix` プロパティを `false` に変更すると、`/from` パターンに一致するリクエストはリダイレクトをトリガーしますが、`/from/another/path` パターンに一致するリクエストはリダイレクトを拒否します。

### サフィックスパターンのマッチング

次の形式を使用して、リクエストのパスサフィックスをターゲット宛先に追加するリダイレクト要求を設定します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

この設定は次のように動作します。

- パターン `/from/path/suffix` に一致するリクエストをパス `https://{default}/to` にリダイレクトします。

- `append_suffix` プロパティを `true` に変更すると、に一致する要求はパス `https://{default}/to/path/suffix` にリダイレクト `/from/path/suffix` れます。

### パス固有のキャッシュ設定

次の形式を使用して、特定のパスからのリダイレクトをキャッシュする時間をカスタマイズします。

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

この設定は次のように動作します。

- 最初のパス（`/from`）からのリダイレクトは 1 日間キャッシュされます。

- 2 番目のパス（`/here`）からのリダイレクトは、2 週間キャッシュされます。
