---
title: カスタム VCL スニペットの基本を学ぶ
description: Varnish Control Language コードスニペットを使用して、Adobe Commerce用の Fastly サービス設定をカスタマイズする方法について説明します。
feature: Cloud, Configuration, Services
exl-id: 90f0bea6-4365-4657-94e9-92a0fd1145fd
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 0%

---

# カスタム VCL の概要

Fastly は、Varnish Configuration Language （VCL）のカスタマイズバージョンをサポートし、お客様の要件に合わせて Fastly サービス設定をカスタマイズします。

カスタム VCL スニペットは、Adobe Commerce サイトにアップロードされたアクティブな VCL バージョンに追加される VCL ロジックのブロックです。 カスタム VCL スニペットは、リクエストトラフィックに対する Fastly キャッシュサービスの応答方法を変更します。 例えば、カスタム VCL スニペットを追加して、指定されたクライアント IP アドレスからの要求トラフィックのみを許可することができます。 または、Adobe Commerce サイトに紹介スパムを送信することで知られている web サイトからのトラフィックをブロックするスニペットを作成します。

カスタム VCL スニペット（生成、コンパイル、すべての Fastly キャッシュに送信）が、サーバーを停止させることなく読み込みおよびアクティブ化されます。

>[!NOTE]
>
>カスタム VCL コード、エッジ辞書、ACL を Fastly モジュール設定に追加する前に、Fastly キャッシュサービスがデフォルト設定で機能することを確認してください。 詳しくは、[Fastly サービスの設定 &#x200B;](fastly-configuration.md) を参照してください。

Fastly では、次の 2 種類のカスタム VCL スニペットをサポートしています。

- [&#x200B; 通常のスニペット &#x200B;](https://docs.fastly.com/en/guides/about-vcl-snippets) - カスタムの通常の VCL スニペットは、特定の VCL バージョンに対してコード化されます。 管理者または Fastly API から、通常の VCL スニペットを作成、変更、およびデプロイできます。

- [&#x200B; 動的スニペット &#x200B;](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) - Fastly API を使用して作成された VCL スニペット。 サービスの Fastly VCL バージョンを更新しなくても、動的スニペットを変更およびデプロイできます。

Edgeの辞書とアクセス制御リスト（ACL）と共にカスタム VCL スニペットを使用して、カスタムコードで使用するデータを保存することをお勧めします。

- [**Edge ディクショナリ**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) - カスタム VCL スニペットから参照できるディクショナリ コンテナに、キーと値のペアとしてデータを格納します

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) - カスタム VCL スニペットを使用して実装されたブロックまたは許可ルールのアクセス制御リストを定義するクライアント IP アドレス データを格納します

ディクショナリと ACL データは、ネットワーク地域全体でアクセス可能な Fastly Edge ノードにデプロイされます。 また、データは、ステージング環境または実稼動環境用に VCL コードを再デプロイしなくても、ネットワーク全体で動的に更新できます。

>[!NOTE]
>
>ステージング環境または実稼動環境にカスタム VCL スニペットを追加できるのは、その環境に [Fastly サービスを設定 &#x200B;](fastly-configuration.md) している場合のみです。

## チュートリアル

このチュートリアルと例では、Edge ディクショナリとEdge ACL を含んだ通常のカスタム VCL スニペットを使用して、Adobe Commerce用の Fastly サービス設定をカスタマイズする方法について説明します。 詳しくは、Fastly のドキュメントを参照してください。

- [Fastly VCL のガイド &#x200B;](https://docs.fastly.com/guides/vcl/guide-to-vcl) - Fastly Varnish の実装、Fastly VCL の拡張機能、および Varnish と VCL の詳細を学習するためのリソースに関する情報です。
- [Fastly VCL リファレンス &#x200B;](https://docs.fastly.com/guides/vcl/) - Fastly カスタム VCL およびカスタム VCL スニペットを開発およびトラブルシューティングするための詳細なプログラミング リファレンスです。

カスタム VCL スニペットは、Adobe Commerce管理者から、または Fastly API を使用して作成および管理できます。

- [Adobe Commerce管理者 &#x200B;](#manage-custom-vcl-from-admin) - Adobe Commerce管理を使用してカスタム VCL スニペットを管理することをお勧めします。これは、VCL 変更の検証、アップロード、Fastly サービス設定への適用を自動化するからです。 また、Fastly サービス設定に追加されたカスタム VCL スニペットを、管理者で表示して編集することもできます。

- [Fastly API](#manage-vcl-using-the-api) – 管理者にアクセスできない場合は、Fastly API を使用してカスタム VCL スニペットを管理します。 例えば、API を使用して、サイトがダウンしたときに Fastly サービス設定をトラブルシューティングしたり、カスタム VCL スニペットを追加したりします。 また、API を使用してのみ完了できる操作もあります。 たとえば、API を使用して、古い VCL バージョンを再アクティブ化したり、指定した VCL バージョンに含まれるすべての VCL スニペットを表示する必要があります。 [VCL スニペットの API クイック リファレンス &#x200B;](#api-quick-reference-for-vcl-snippets) を参照してください。

### VCL スニペットコードの例

次の例は、クライアントの IP アドレスでトラフィックをフィルタリングするカスタム VCL スニペット（JSON 形式）を示しています。

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>この例では、VCL コードは JSON ペイロードとしてフォーマットされ、ファイルに保存して Fastly API リクエストで送信できます。 スニペットを API リクエストの JSON として送信する際に JSON 検証エラーが発生しないようにするには、バックスラッシュを使用してコード内の特殊文字をエスケープします。 Fastly VCL ドキュメントの [&#x200B; 動的 VCL スニペットの使用 &#x200B;](https://docs.fastly.com/vcl/vcl-snippets/) を参照してください。 管理者から VCL スニペットを送信する場合、特殊文字をエスケープする必要はありません。

`content` フィールドの VCL ロジックは、次のアクションを実行します。

- 各リクエストで `client.ip` 受信 IP アドレスを確認します

- *ACLNAME* エッジ ACL に含まれる IP アドレスを持つ要求をブロックし、`403 Forbidden` エラーを返します

次の表に、カスタム VCL スニペットのキーデータの詳細を示します。 詳しくは、Fastly のドキュメントの [VCL スニペット &#x200B;](https://docs.fastly.com/api/config#api-section-snippet) リファレンスを参照してください。

| 値 | 説明 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | Fastly アカウントにアクセスするための API キー。 [&#x200B; 資格情報の取得 &#x200B;](fastly-configuration.md) を参照してください。 |
| `active` | スニペットまたはバージョンのアクティブなステータス。 `true` または `false` を返します。 true の場合、スニペットまたはバージョンは使用中です。 バージョン番号を使用してアクティブなスニペットのクローンを作成します。 |
| `content` | 実行する VCL コードのスニペット。 Fastly では、すべての VCL 言語機能をサポートしているわけではありません。 また、Fastly は、カスタム機能を備えた拡張機能を提供します。 サポートされる機能の詳細については、[Fastly VCL プログラミングリファレンス &#x200B;](https://docs.fastly.com/vcl/reference/) を参照してください。 |
| `dynamic` | スニペットの動的ステータス。 Fastly サービス設定のバージョン管理された VCL に含まれる `false` 通常のスニペット [&#x200B; の &#x200B;](https://docs.fastly.com/en/guides/about-vcl-snippets) を返します。 新しい VCL バージョンを必要とせずに変更およびデプロイが可能な `true` 動的スニペット [&#x200B; の &#x200B;](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) を返します。 |
| `number` | スニペットが含まれている VCL バージョン番号。 Fastly では、サンプル値に *編集可能なバージョン #* を使用します。 API からカスタムスニペットを追加する場合は、API リクエストにバージョン番号を含めてください。 管理者からカスタム VCL を追加すると、のバージョンが提供されます。 |
| `priority` | カスタム VCL スニペット コードを実行するタイミングを指定する `1` ～ `100` の数値。 優先度の値が小さいスニペットが最初に実行されます。 指定しない場合、`priority` 値はデフォルトで `100` になります。<p>優先順位の値が `5` のカスタム VCL スニペットは直ちに実行されます。これは、リクエストルーティング（ブロックおよび許可リストとリダイレクト）を実装する VCL コードに最適です。 優先順位 `100` は、デフォルトの VCL スニペットコードを上書きする場合に最適です。<p>Magento-Fastly モジュールに含まれているすべての [&#x200B; デフォルト VCL スニペット &#x200B;](fastly-configuration.md#upload-vcl-snippets) には、`priority=50` が含まれています。<ul><li>`100` などの高い優先度を割り当てて、他のすべての VCL 関数の後にカスタム VCL コードを実行し、デフォルトの VCL コードをオーバーライドします。</li></ul> |
| `service_id` | 特定のステージング環境または実稼動環境の Fastly サービス ID。 この ID は、プロジェクトがクラウドインフラストラクチャ上のAdobe Commerce[Fastly サービスアカウント &#x200B;](fastly.md#fastly-service-account-and-credentials) に追加されたときに割り当てられます。 |
| `type` | 生成されたスニペットを挿入する場所を指定します。例えば、`init` （サブルーチンの上）や `recv` （サブルーチン内）などです。 詳しくは、Fastly [VCL スニペット &#x200B;](https://docs.fastly.com/api/config#api-section-snippet) リファレンスを参照してください。 |

## 管理者からのカスタム VCL の管理

[&#x200B; カスタム VCL スニペットの追加 &#x200B;](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) は、管理画面の *Fastly 設定*/*カスタム VCL スニペット* セクションから行えます。

![&#x200B; カスタム VCL スニペットの管理 &#x200B;](../../assets/cdn/fastly-edit-snippets.png)

*カスタム VCL スニペット* ビューには、管理者を通じて追加されたスニペットのみが表示されます。 Fastly API を使用してスニペットを追加する場合は、API を使用して [&#x200B; スニペットを管理 &#x200B;](#manage-vcl-using-the-api) します。

以下の例は、カスタム VCL スニペットを管理者から作成および管理する方法と、Fastly Edge モジュールおよびEdgeの辞書を使用する方法を示しています。

- [CMS バックエンドへのリクエストの再ルーティング](fastly-vcl-wordpress.md)
- [参照スパムをブロック](fastly-vcl-badreferer.md)
- [リファラルスパムをブロック](fastly-vcl-badreferer.md)
- [IP 用のカスタム VCL許可リスト](fastly-vcl-allowlist.md)
- [IP 用のカスタム VCLブロックリスト](fastly-vcl-blocking.md)
- [Fastly キャッシュのバイパス](fastly-vcl-bypass-to-origin.md)

## Commerce管理者で表示/変更できないスニペット

一部のスニペットは、Commerce Admin 内で直接表示または変更することはできません。 例えば、[&#x200B; 動的スニペット &#x200B;](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) です。 「カスタム VCL スニペット」セクションには、クラウドサポートチームによって [Fastly 管理ダッシュボード &#x200B;](fastly.md#fastly-service-account-and-credentials) に直接追加されたスニペットは表示されません。


**クラウドサポートチームによって追加されたスニペットを確認するには：**

1. 「**ツール**」セクションに移動します。

1. **バージョン履歴** の横にある _すべてのバージョンをリスト_ をクリックします。

1. 該当する VCL バージョンの横にある目のアイコンをクリックして、既存のスニペットを表示します。


## API を使用した VCL の管理

次のウォークスルーでは、通常の VCL スニペットファイルを作成し、Fastly API を使用して Fastly サービス設定に追加する方法を示します。 *ターミナル* アプリケーションからスニペットを作成および管理できます。 特定の環境への SSH 接続は必要ありません。

**前提条件：**

- Fastly サービスを使用するには、クラウドインフラストラクチャ環境でAdobe Commerceを設定します。 [Fastly の設定 &#x200B;](fastly-configuration.md) を参照してください。

- [Fastly API 資格情報を取得 &#x200B;](fastly-configuration.md) して、Fastly API へのリクエストを認証します。 正しい環境（ステージングまたは実稼動）の資格情報を取得していることを確認します。

- cURL コマンドで使用できる bash 環境変数として、Fastly サービスの資格情報を保存します。

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  書き出された環境変数は、現在の bash セッションでのみ使用でき、ターミナルを閉じると失われます。 新しい値を書き出して変数を再定義できます。 Fastly に関連する、書き出された変数のリストを表示するには：

  ```bash
  export | grep FASTLY
  ```

## VCL スニペットの追加

このチュートリアルでは、Fastly API を使用してカスタムスニペットを追加する基本的な手順を説明します。

>[!NOTE]
>
>Adobe Commerce Admin からカスタム VCL スニペットを管理する方法については、[Adobe Commerce Admin からの VCL の管理 &#x200B;](#manage-custom-vcl-from-admin) を参照してください。


**前提条件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### 手順 1：アクティブな VCL バージョンを探す

Fastly API [&#x200B; バージョンを取得 &#x200B;](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) 操作を使用して、アクティブな VCL バージョン番号を取得します。

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

JSON 応答では、`number` キーで返されたアクティブな VCL バージョン番号（例：`"number": 99`）をメモします。 VCL を編集用に複製する場合は、バージョン番号が必要です。

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

以降の API リクエストで使用するために、アクティブなバージョン番号を bash 環境変数に保存します。

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### 手順 2：アクティブな VCL バージョンとすべてのスニペットを複製する

カスタム VCL スニペットを追加または修正する前に、編集用にアクティブな VCL バージョンのコピーを作成する必要があります。 Fastly API [clone](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) 操作を使用します。

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

JSON 応答では、バージョン番号が増分され、*アクティブ* キー値は `false` になります。 新しい非アクティブな VCL バージョンは、ローカルで修正できます。

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

以降のコマンドで使用するために、新しいバージョン番号を bash 環境変数に保存します。

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### 手順 3：カスタム VCL スニペットの作成

カスタム VCL コードを作成し、次の内容と形式で JSON ファイルに保存します。

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

値には次が含まれます。

- `name` - VCL スニペットの名前。

- `dynamic` – これが [&#x200B; 通常のスニペット &#x200B;](https://docs.fastly.com/en/guides/about-vcl-snippets) か [&#x200B; 動的スニペット &#x200B;](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets) かを示します。

- `type` – 生成されたスニペットを挿入する場所を指定します。たとえば、`init` （サブルーチンの上）や `recv` （サブルーチン内）などです。 [Fastly VCL スニペットオブジェクトの値 &#x200B;](https://docs.fastly.com/api/config#snippet) を参照してください。

- `priority` - カスタム VCL スニペットコードを実行するタイミングを決定する `1` ～ `100` の値。 値が小さいカスタム VCL スニペットが最初に実行されます。

  Fastly VCL モジュールのデフォルトの VCL コードはすべて、`priority` が `50` です。 アクションを最後に実行する場合、またはデフォルトの VCL コードをオーバーライドする場合は、`100` のように大きい数字を使用します。 カスタム VCL スニペットコードをすぐに実行するには、優先度を `5` などの低い値に設定します。

- `content` – 改行なしで 1 行で実行する VCL コードのスニペット。 [&#x200B; カスタム VCL スニペットの例 &#x200B;](#example-vcl-snippet-code) を参照。

### 手順 4:Fastly 設定への VCL スニペットの追加

Fastly API [&#x200B; スニペットを作成 &#x200B;](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) 操作を使用して、カスタム VCL スニペットを VCL バージョンに追加します。

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

`<filename.json>` は、前の手順で準備したファイルの名前です。 各 VCL スニペットに対して、このコマンドを繰り返します。

Fastly サービスから `500 Internal Server Error` 応答を受け取った場合は、JSON ファイル構文を調べ、有効なファイルをアップロードしていることを確認します。

### 手順 5：カスタム VCL スニペットの検証とアクティブ化

カスタム VCL スニペットを追加すると、編集中の VCL バージョンにスニペットが挿入されます。 変更を適用するには、次の手順を実行して VCL スニペット コードを検証し、VCL バージョンをアクティブにします。

1. Fastly API [VCL バージョンの検証 &#x200B;](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) 操作を使用して、更新された VCL コードを検証します。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Fastly API がエラーを返した場合は、問題を修正し、更新された VCL バージョンを再度検証します。

1. Fastly API [&#x200B; アクティベート &#x200B;](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) 操作を使用して、新しい VCL バージョンをアクティベートします。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```


## VCL スニペットの API クイックリファレンス

これらの API リクエストの例では、書き出された環境変数を使用して、Fastly で認証するための資格情報を提供しています。 これらのコマンドについて詳しくは、[Fastly API リファレンス &#x200B;](https://docs.fastly.com/api/config#vcl) を参照してください。

>[!NOTE]
>
>次のコマンドを使用して、Fastly API を使用して追加したスニペットを管理します。 管理者からスニペットを追加した場合は、「[&#x200B; 管理者を使用して VCL スニペットを管理する &#x200B;](#manage-vcl-using-the-api)」を参照してください。

- **アクティブな VCL バージョン番号を取得**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **サービスに添付されているすべての通常の VCL スニペットを一覧表示する**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **個々のスニペットの確認**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  `<snippet_name>` はスニペットの名前（`my_regular_snippet` など）です。

- **スニペットの更新**

  [&#x200B; 準備された JSON ファイル &#x200B;](#step-3-create-a-custom-vcl-snippet) を変更し、次のリクエストを送信します。

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **個々の VCL スニペットを削除する**

  スニペットのリストを取得し、次の `curl` コマンドを削除する特定のスニペット名と共に使用します。

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **[&#x200B; デフォルトの Fastly VCL コードの値を上書き &#x200B;](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  更新された値でスニペットを作成し、`100` の優先度を割り当てます。

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
