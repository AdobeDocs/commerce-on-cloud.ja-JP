---
title: リダイレクト
description: Adobe Commerce on cloud infrastructure プロジェクトのリダイレクションルールを管理する方法について説明します。
feature: Cloud, Routes
exl-id: f70a9035-bbae-4d23-bb7c-c0de6a7ccf6c
TQID: https://experienceleague.adobe.com/53acuGMa93oysIKX-agqJCttbxCdIFgyRpmeZh-G9gI
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 705
ht-degree: 0%

---

# リダイレクト

リダイレクトルールの管理は、web アプリケーションの一般的な要件です。特に、時間の経過とともに変更または削除された受信リンクを失いたくない場合に必要です。

次に、`routes.yaml`設定ファイルを使用して、Adobe Commerce on cloud infrastructure プロジェクトのリダイレクトルールを管理する方法を示します。 このトピックで説明したリダイレクト方法が機能しない場合は、キャッシュヘッダーを使用して同じことを行うことができます。

{{route-placeholder}}

## Pro環境へのアップデート

{{pro-self-service-warning}}

>[!WARNING]
>
>Adobe Commerce on cloud infrastructure プロジェクトの場合、`routes.yaml` ファイルで多数の非regex リダイレクトと書き換えを設定すると、パフォーマンスの問題が発生する可能性があります。 `routes.yaml` ファイルが32 KB以上の場合は、正規表現でないリダイレクトをオフロードし、Fastlyに書き換えます。 _Adobe Commerce ヘルプセンター_&#x200B;の「[正規表現でないリダイレクトをNginx （ルート） &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html?lang=ja)ではなくFastlyにオフロードする」を参照してください。

## ルート全体のリダイレクト

ルート全体のリダイレクトを使用すると、`routes.yaml` ファイルを使用して簡単なルートを定義できます。 例えば、次のようにapex ドメインから`www` サブドメインにリダイレクトできます。

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## 部分ルートリダイレクト

`.magento/routes.yaml` ファイルでは、パターン一致に基づいて、既存のルートに部分的なリダイレクト ルールを追加できます。

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

部分リダイレクトは、アプリケーションが直接提供するルートを含む、あらゆるタイプのルートで機能します。

次の2つのキーを`redirects`で使用できます。

- **有効期限** – オプション。ブラウザーでリダイレクトをキャッシュする時間を指定します。 有効な値の例には、`3600s`、`1d`、`2w`、`3m`などがあります。

- **paths** – 部分ルート リダイレクト ルールの設定を指定する1つ以上のキーと値のペア。

  各リダイレクトルールのキーは、リダイレクト用のリクエストパスをフィルタリングするための式です。 値は、リダイレクトのターゲット先とリダイレクトを処理するためのオプションを指定するオブジェクトです。

  value オブジェクトには、次のプロパティがあります。

  | プロパティ | 説明 |
  | ---------- | ----------- |
  | `to` | 必須、部分的な絶対パス、プロトコルとホストを含むURL、またはリダイレクトルールのターゲット先を指定するパターン。 |
  | `regexp` | オプションで、デフォルトは`false`です。 パス キーをPCRE正規表現として解釈するかどうかを指定します。 |
  | `prefix` | リダイレクトがパスとそのすべての子の両方に適用されるか、パス自体のみに適用されるかを指定します。 デフォルトは`true`です。 この値は、`regexp`が`true`の場合はサポートされていません。 |
  | `append_suffix` | 接尾辞がリダイレクトに引き継がれるかどうかを指定します。 デフォルトは`true`です。 この値は、`regexp` キーが`true`の場合はサポートされません。また、`prefix` キーが`false`の場合は*サポートされません。 |
  | `code` | HTTP ステータスコードを指定します。 有効な状態コードは[`301` （永続的に移動） &#x200B;](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2)、[`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3)、[`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8)、および[`308`](https://www.rfc-editor.org/rfc/rfc7238)です。 デフォルトは`302`です。 |
  | `expires` | オプションで、ブラウザーでリダイレクトをキャッシュする時間を指定します。 デフォルトは`redirects` キーの直下に定義された`expires`値ですが、このレベルでは、個々の部分リダイレクトのキャッシュ有効期限を微調整できます。 |

## 一部ルートリダイレクトの例

次の例は、様々な`paths`設定オプションを使用して、`routes.yaml` ファイルで部分ルートリダイレクトを指定する方法を示しています。

### 正規表現パターンマッチング

次の形式を使用して、正規表現に基づいてリダイレクトリクエストを設定します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

この設定では、正規表現に対してリクエストパスをフィルタリングし、一致するリクエストを`https://example.com`にリダイレクトします。 例えば、`https://example.com/regexp/a/b/c/match`へのリクエストは`https://example.com/a/b/c`にリダイレクトされます。

### 接頭辞パターンマッチング

指定された接頭辞パターンで始まるパスのリダイレクトリクエストを設定するには、次の形式を使用します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

この設定は次のように機能します。

- パターン `/from`に一致するリクエストをパス `http://{default}/to`にリダイレクトします。

- パターン `/from/another/path`に一致するリクエストを`https://{default}/to/another/path`にリダイレクトします。

- `prefix` プロパティを`false`に変更すると、`/from` パターントリガーに一致するリクエストはリダイレクトとなりますが、`/from/another/path` パターンに一致するリクエストはリダイレクトされません。

### 接尾辞パターンマッチング

次の形式を使用して、リクエストからターゲット宛先にパスのサフィックスを追加するリダイレクトリクエストを設定します。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

この設定は次のように機能します。

- パターン `/from/path/suffix`に一致するリクエストをパス `https://{default}/to`にリダイレクトします。

- `append_suffix` プロパティを`true`に変更すると、`/from/path/suffix`に一致するリクエストはパス `https://{default}/to/path/suffix`にリダイレクトされます。

### パス固有のキャッシュ設定

特定のパスからリダイレクトをキャッシュする時間をカスタマイズするには、次の形式を使用します。

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

この設定は次のように機能します。

- 最初のパス （`/from`）からのリダイレクトは1日キャッシュされます。

- 2番目のパス （`/here`）からのリダイレクトは2週間キャッシュされます。
