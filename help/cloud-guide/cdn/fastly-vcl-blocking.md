---
title: リクエストをブロックするカスタム VCL
description: カスタム VCL スニペットを使用したEdge アクセス制御リスト（ACL）を使用して、IP アドレスごとに受信リクエストをブロックします。
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# リクエストをブロックするカスタム VCL

Magento 2 用の Fastly CDN モジュールを使用して、ブロックする IP アドレスのリストを含むEdge ACL を作成できます。 次に、このリストを VCL スニペットと一緒に使用して、着信リクエストをブロックできます。 このコードは、受信リクエストの IP アドレスを確認します。 ACL リストに含まれる IP アドレスと一致する場合、Fastly は、リクエストがサイトにアクセスするのをブロックし、`403 Forbidden error` を返します。 その他すべてのクライアント IP アドレスへのアクセスが許可されます。

**前提条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- ブロックするクライアント IP アドレスのリスト

## クライアント IP アドレスをブロックするためのEdge ACL を作成する

Edge ACL を作成して、ブロックする IP アドレスのリストを定義します。 ACL を作成したら、それをカスタム VCL スニペットで使用して、ステージングまたは実稼動サイトへのアクセスを管理できます。

両方の環境で同じ名前のEdge ACL を作成して、ステージングサイトと実稼動サイトの両方のアクセスを管理します。 VCL スニペットコードは、両方の環境に適用されます。

1. 管理者にログインします。
1. **ストア**/設定/**設定**/**詳細**/**システム**/**フルページキャッシュ**/**Fastly 設定** に移動します。
1. 「**Edge ACL**」セクションを展開します。
1. **ACL を追加** をクリックして、リストを作成します。 この例では、リストに「ブロックリスト」という名前を付けます。
1. リストに IP アドレス値を入力します。 このリストに追加されたクライアントの IP アドレスはすべてブロックされ、サイトにアクセスできません。
1. 必要に応じて、「**否定**」チェックボックスを選択します。

VCL スニペットコードでは、Edge ACL を名前で参照します。

## ブロックリストのカスタム VCL の作成

>[!NOTE]
>
>この例では、上級ユーザーに対して、Fastly サービスにアップロードするカスタムブロッキングルールを設定する VCL コードスニペットの作成方法を示します。 Fastly CDN for Magento 2 モジュールで利用できる [Blocking](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) 機能を使用して、Adobe Commerce管理者から国に基づくブロックリスト許可リストに加えるまたはチャネルを設定できます。

Edge ACL を定義した後、それを使用して、ACL で指定された IP アドレスへのアクセスをブロックする VCL スニペットを作成できます。 ステージング環境と実稼動環境の両方で同じ VCL スニペットを使用できますが、スニペットは各環境に個別にアップロードする必要があります。

次のカスタム VCL スニペットコード（JSON 形式）は、ブロックリスト ACL 内のアドレスと一致するクライアント IP アドレスを持つ受信リクエストをブロックするロジックを示しています。

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

この例に基づいてスニペットを作成する前に、値を確認して、変更が必要かどうかを判断してください。

- `name`: VCL スニペットの名前。 この例では、`blocklist` という名前を使用しました。

- `priority`: VCL スニペットを実行するタイミングを指定します。 優先度は、直ちに実行し、許可された IP アドレスからの管理リクエストであるかどうかを確認するように `5` 定されています。 このスニペットは、デフォルトのMagento VCL スニペット（`magentomodule_*`）に優先度 50 が割り当てられる前に実行されます。 各カスタムスニペットの優先度を、スニペットを実行するタイミングに応じて 50 より高くまたは低く設定します。 優先度の低いスニペットが最初に実行されます。

- `type`：生成された VCL コード内のスニペットの場所を決定する VCL スニペットのタイプを指定します。 この例では、`recv` を使用して、VCL コードを `vcl_recv` サブルーチン内のボイラープレート VCL の下、およびオブジェクトの上に挿入します。 スニペットタイプのリストについては、[Fastly VCL スニペットリファレンス ](https://docs.fastly.com/api/config#api-section-snippet) を参照してください。

- `content`：実行する VCL コードのスニペット。クライアントの IP アドレスを確認します。 IP がEdgeの ACL に含まれている場合は、web サイト全体に対して `403 Forbidden` エラーが発生して IP のアクセスがブロックされます。 その他すべてのクライアント IP アドレスへのアクセスが許可されます。

環境のコードを確認して更新した後、次のいずれかの方法を使用して、カスタム VCL スニペットを Fastly サービス設定に追加します。

- [ カスタム VCL スニペットを管理者から追加します ](#add-the-custom-vcl-snippet)。 管理者にアクセスできる場合は、この方法をお勧めします。 （[Fastly バージョン 1.2.58](fastly-configuration.md#upgrade-fastly-module) 以降が必要です。）

- JSON コードの例をファイル（例：`blocklist.json`）に保存して、[Fastly API を使用してアップロード ](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api) します。 管理者にアクセスできない場合は、この方法を使用します。

## カスタム VCL スニペットの追加

{{admin-login-step}}

1. **ストア**/設定/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**カスタム VCL スニペット** の順に展開します。

1. **カスタムスニペットを作成** をクリックします。

1. VCL スニペットの値を追加します。

   - **名前** — `blocklist`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツを追加します。

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. **作成** をクリックして、名前パターン `type_priority_name.vcl` （たとえば `recv_5_blocklist.vcl`）で VCL スニペット ファイルを生成します

1. ページのリロード後、「**Fastly 設定**」セクションの「*Fastly に VCL をアップロード*」をクリックして、ファイルを Fastly サービス設定に追加します。

1. アップロード後、ページ上部の通知に従ってキャッシュを更新します。

Fastly は、アップロードプロセス中に VCL コードの更新バージョンを検証します。 検証に失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCL を再度アップロードします。

## リクエストをブロックする VCL の追加例

次の例は、ACL リストの代わりにインライン条件ステートメントを使用してリクエストをブロックする方法を示しています。

>[!WARNING]
>
>これらの例では、VCL コードは JSON ペイロードとしてフォーマットされ、ファイルに保存して Fastly API リクエストで送信できます。 [VCL スニペットは、管理者から ](#add-the-custom-vcl-snippet) または Fastly API を使用して JSON 文字列として送信できます。 Fastly API を JSON 文字列と共に使用する場合に検証エラーを防ぐには、バックスラッシュを使用して特殊文字をエスケープする必要があります。

>[!NOTE]
>VCL スニペットを管理者から送信する場合は、サンプル VCL コードから個々の値を抽出し、対応するフィールドに入力します。 例：
>- 名前：`<name of the VCL>`
>- 動的：`<0/1>`
>- 型：`<type>`
>- 優先度：`<priority>`
>- コンテンツ：`<content>`

Fastly VCL ドキュメントの [ 動的 VCL スニペットの使用 ](https://docs.fastly.com/vcl/vcl-snippets/) を参照してください。

### VCL コード サンプル：国コードによるブロック

この例では、IP アドレスに関連付けられた国に、2 文字の ISO 3166-1 国コードを使用しています。

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>カスタム VCL スニペットを使用する代わりに、クラウドインフラストラクチャー管理のAdobe Commerceにある Fastly [ ブロッキング ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) 機能を使用して、国コードまたは国コードのリスト別にブロッキングを設定できます。

### VCL コード例：HTTP ユーザーエージェントリクエストヘッダーによるブロック

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
