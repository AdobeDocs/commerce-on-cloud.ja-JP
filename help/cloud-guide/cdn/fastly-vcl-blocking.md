---
title: ブロック要求のカスタム VCL
description: カスタム VCL スニペットを使用したEdge Access Control List （ACL）を使用して、IP アドレスで受信リクエストをブロックします。
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
TQID: https://experienceleague.adobe.com/AhSqQYill1D5hYn06pkQXnUsIW-0pc6k51OZwHA8Qtg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1026
ht-degree: 0%

---

# ブロック要求のカスタム VCL

Magento 2用のFastly CDN モジュールを使用して、ブロックするIP アドレスのリストを含むEdge ACLを作成できます。 そのリストをVCL スニペットと共に使用して、着信リクエストをブロックできます。 このコードは、受信リクエストのIP アドレスを確認します。 ACL リストに含まれるIP アドレスと一致する場合、Fastlyはリクエストによるサイトへのアクセスをブロックし、`403 Forbidden error`を返します。 他のすべてのクライアント IP アドレスにアクセスが許可されます。

**前提条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- ブロックするクライアント IP アドレスのリスト

## クライアント IP アドレスをブロックするためのEdge ACLの作成

ブロックするIP アドレスのリストを定義するには、Edge ACLを作成します。 ACLを作成したら、カスタム VCL スニペットで使用して、ステージング サイトまたは実稼動サイトへのアクセスを管理できます。

両方の環境で同じ名前のEdge ACLを作成して、ステージングサイトと実稼動サイトの両方のアクセスを管理します。 VCL スニペットコードは、両方の環境に適用されます。

1. Adminにログインします。
1. **ストア** >設定> **設定** > **詳細** > **システム** > **フルページキャッシュ** > **Fastly設定**&#x200B;に移動します。
1. **Edge ACL** セクションを展開します。
1. 「**ACL**&#x200B;を追加」をクリックして、リストを作成します。 この例では、リストに「ブロックリスト」という名前を付けます。
1. リストにIP アドレスの値を入力します。 このリストに追加されたクライアント IP アドレスはブロックされ、サイトにアクセスできません。
1. 必要に応じて、「**否定**」チェックボックスを選択します。

VCL スニペットコードでは、名前でEdge ACLを参照します。

## ブロックリスト用のカスタム VCLの作成

>[!NOTE]
>
>この例では、高度なユーザーがVCL コードスニペットを作成して、カスタムブロッキングルールを設定し、Fastly サービスにアップロードする方法を示します。 Magento向けFastly CDN 2 モジュールで利用可能な[&#x200B; ブロック &#x200B;](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)機能を使用すると、Adobe Commerce管理者の国に基づいてブロックリストまたは許可リストを設定できます。

Edge ACLを定義したら、これを使用してVCL スニペットを作成し、ACLで指定されたIP アドレスへのアクセスをブロックできます。 ステージング環境と実稼動環境の両方で同じVCL スニペットを使用できますが、スニペットを各環境に個別にアップロードする必要があります。

次のカスタム VCL スニペットコード（JSON形式）は、クライアント IP アドレスが指定された受信リクエストをブロックするロジックを示しています。このロジックは、受信リクエスト ACLのアドレスと一致します。

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

この例に基づいてスニペットを作成する前に、値を確認して、変更を加える必要があるかどうかを判断します。

- `name`: VCL スニペットの名前。 この例では、名前`blocklist`を使用しました。

- `priority`: VCL スニペットが実行されるタイミングを決定します。 管理者要求が許可されたIP アドレスから送信されているかどうかを即座に実行して確認する優先度は`5`です。 スニペットは、デフォルトのMagento VCL スニペット（`magentomodule_*`）のいずれかが優先度50に割り当てられる前に実行されます。 スニペットを実行するタイミングに応じて、各カスタムスニペットの優先度を50より高くまたは低く設定します。 優先度の低いスニペットが最初に実行されます。

- `type`：生成されたVCL コード内のスニペットの場所を決定するVCL スニペットのタイプを指定します。 この例では、`vcl_recv` サブルーチンにVCL コードを挿入する`recv`を、ボイラープレート VCLの下および任意のオブジェクトの上に使用します。 スニペットの種類のリストについては、[Fastly VCL スニペットのリファレンス &#x200B;](https://docs.fastly.com/api/config#api-section-snippet)を参照してください。

- `content`：実行するVCL コードのスニペット。クライアント IP アドレスを確認します。 IPがEdge ACL内にある場合、web サイト全体に`403 Forbidden` エラーが発生してアクセスがブロックされます。 他のすべてのクライアント IP アドレスにアクセスが許可されます。

環境のコードを確認して更新したら、次のいずれかの方法を使用して、Fastly サービス設定にカスタム VCL スニペットを追加します。

- [管理者](#add-the-custom-vcl-snippet)からカスタム VCL スニペットを追加します。 管理者にアクセスできる場合は、この方法をお勧めします。 （[Fastly バージョン 1.2.58](fastly-configuration.md#upgrade-fastly-module)以降が必要です）

- JSON コードの例をファイル （例：`blocklist.json`）に保存し、Fastly API[&#128279;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)を使用して アップロードします。 管理者にアクセスできない場合は、この方法を使用します。

## カスタム VCL スニペットの追加

{{admin-login-step}}

1. **ストア** / 設定/**構成** / **詳細** / **システム**&#x200B;をクリックします。

1. **フルページキャッシュ** > **Fastly設定** > **カスタム VCL スニペット**&#x200B;を展開します。

1. 「**カスタムスニペットを作成**」をクリックします。

1. VCL スニペット値を追加します。

   - **名前** — `blocklist`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツを追加します。

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. **作成**&#x200B;をクリックして、名前パターン `type_priority_name.vcl`のVCL スニペットファイルを生成します（例：`recv_5_blocklist.vcl`）

1. ページがリロードされたら、「*Fastly設定*」セクションの「**VCLをFastly**&#x200B;にアップロード」をクリックして、ファイルをFastly サービス設定に追加します。

1. アップロード後、ページ上部の通知に従ってキャッシュを更新します。

アップロードプロセス中に、VCL コードの更新バージョンを迅速に検証します。 検証が失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCLをもう一度アップロードします。

## ブロッキング要求の追加VCL例

次の例は、ACL リストの代わりにインライン条件ステートメントを使用してリクエストをブロックする方法を示しています。

>[!WARNING]
>
>これらの例では、VCL コードはJSON ペイロードとしてフォーマットされており、ファイルに保存し、Fastly API リクエストで送信できます。 Admin[&#128279;](#add-the-custom-vcl-snippet)からVCL スニペットを送信するか、Fastly APIを使用してJSON文字列として送信できます。 JSON文字列でFastly APIを使用する際の検証エラーを防ぐには、特殊文字をエスケープするためにバックスラッシュを使用する必要があります。

>[!NOTE]
>管理者からVCL スニペットを送信する場合は、サンプル VCL コードから個々の値を抽出し、対応するフィールドにに入力します。 例：
>- 名前：`<name of the VCL>`
>- 動的：`<0/1>`
>- 種類：`<type>`
>- 優先度：`<priority>`
>- コンテンツ：`<content>`

Fastly VCL ドキュメントの[動的VCL スニペットの使用](https://docs.fastly.com/vcl/vcl-snippets/)を参照してください。

### VCL コードサンプル：国コードでブロック

この例では、IP アドレスに関連付けられた国に2文字のISO 3166-1国コードを使用します。

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
>カスタム VCL スニペットを使用する代わりに、Adobe Commerce on cloud infrastructure AdminのFastly [&#x200B; ブロッキング &#x200B;](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)機能を使用して、国コードまたは国コードのリストによるブロッキングを設定できます。

### VCL コードサンプル：HTTP User-Agent リクエストヘッダーによるブロック

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
