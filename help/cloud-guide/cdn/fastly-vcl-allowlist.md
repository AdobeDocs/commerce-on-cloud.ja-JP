---
title: リクエストを許可するカスタム VCL
description: Fastly Edgeの ACL リストとカスタム VCL スニペットを使用して、で受信リクエストをフィルタリングし、Adobe Commerce サイトの IP アドレスでアクセスを許可します。
feature: Cloud, Configuration, Security
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# リクエストを許可するカスタム VCL

カスタム VCL コードスニペットと共に Fastly Edgeの ACL リストを使用して、受信リクエストをフィルタリングし、IP アドレスによるアクセスを許可できます。 ACL リストは、許可する IP アドレスを指定します。

許可リストを作成して、ステージング環境へのアクセスを制限し、内部開発者および承認済みの外部サービス用に指定された IP アドレスからのリクエストのみが許可されるようにします。 また、ステージング環境と実稼動環境で管理者へのアクセスを保護するための許可リストを作成することもできます。

次の例は、[Fastly アクセス制御リスト（ACL） ](https://docs.fastly.com/guides/access-control-lists/about-acls) を含むカスタム VCL スニペットを使用して、クラウドインフラストラクチャプロジェクト環境でのAdobe Commerceの管理者へのアクセスを保護する方法を示しています。 カスタム VCL スニペットをクラウド環境に追加する場合、Fastly では、ACL に含まれる IP アドレスからのリクエストのみを許可します。

>[!TIP]
>
>公開アクセスを禁止するステージング環境および統合環境の場合は、[[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) で使用可能な HTTP アクセス制御オプションを使用して、IP アドレスでサイト全体へのアクセスを管理します。

**前提条件：**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 許可リストに含めるクライアント IP アドレスのリスト

## Edge ACL を作成して、クライアント IP アドレスを許可します

Edge ACL は、サイトへのアクセスを管理するための IP アドレスリストを作成します。 この例では、Edge ACL を作成し、プロジェクト環境の管理者にアクセスできるクライアント IP アドレスのリストを追加します。

{{admin-login-step}}

1. **ストア**/設定/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**4&rbrace;Edge ACL&rbrace; を展開します。**

1. ACL コンテナを作成します。

   - **ACL を追加** をクリックします。

   - *ACL コンテナ* ページで **ACL 名**—`allowlist` を入力します。

   - **変更後にアクティベート** を選択して、編集中の Fastly サービス設定のバージョンに変更をデプロイします。

   - **アップロード** をクリックして、ACL を Fastly サービス設定に添付します。

1. 管理者にアクセスできる IP アドレスのリストを追加します。

   - `allowlist` ACL の設定アイコンをクリックします。

   - クライアント IP アドレスごとに *IP 値* を追加して保存します。

   - 「**キャンセル**」をクリックして、システム設定ページに戻ります。

1. 「**設定を保存**」をクリックします。

1. ページ上部の通知に従ってキャッシュを更新します。

## 管理アクセスを保護するためのカスタム VCL スニペットの作成

次のカスタム VCL スニペットコード（JSON 形式）は、管理者へのリクエストをフィルタリングして、クライアントの IP アドレスが `allowlist` の ACL 内のアドレスと一致した場合にアクセスを許可するロジックを示しています。

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

この例では [ カスタムスニペットの作成 ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html?lang=ja#add-the-custom-vcl-snippet) の前に、値を確認して、変更が必要かどうかを判断してください。 次に、各値をそれぞれのフィールドに入力します（例えば、「タイプ」フィールドに `type` を入力 `content`、「コンテンツ」フィールドに入力します）。

- `name` — VCL スニペットの名前。 この例の場合は `allowlist` です。

- `priority` — VCL スニペットを実行するタイミングを指定します。 優先度は、直ちに実行し、管理者リクエストの送信元が許可された IP アドレスであるかどうかを確認するように `5` 定されています。 このスニペットは、デフォルトのMagento VCL スニペット（`magentomodule_*`）に優先度 50 が割り当てられる前に実行されます。 各カスタムスニペットの優先度を、スニペットを実行するタイミングに応じて 50 より高くまたは低く設定します。 優先度の低いスニペットが最初に実行されます。

- 「`type`」 – バージョン管理された VCL コードにスニペットを挿入する場所を指定します。 この VCL は、デフォルトの Fastly VCL コードの下およびオブジェクトの上の `vcl_recv` サブルーチンにスニペットコードを追加する `recv` スニペットタイプです。

- `content` – 実行する VCL コードのスニペット。 この例では、コードは管理者への要求をフィルタリングし、クライアント IP アドレスが `allowlist` ACL 内のアドレスと一致する場合はアクセスを許可します。 アドレスが一致しない場合、リクエストは `403 Forbidden` エラーでブロックされます。

  管理者の URL が変更された場合は、サンプル値 `/admin` を環境の URL に置き換えます。 例：`/company-admin`。

コードサンプルでは、[ 原点シールド ](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding) を使用する場合、条件 `!req.http.Fastly-FF` が重要です。 このコードを削除または編集しないでください。

環境のコードを確認して更新した後、次のいずれかの方法を使用して、カスタム VCL スニペットを Fastly サービス設定に追加します。

- [ カスタム VCL スニペットを管理者から追加します ](#add-the-custom-vcl-snippet)。 管理者にアクセスできる場合は、この方法をお勧めします。 （Magento 2 バージョン 1.2.58[&#128279;](fastly-configuration.md#upgrade) 以降には Fastly CDN モジュールが必要です）。

- JSON コードの例をファイル（例：`allowlist.json`）に保存して、[Fastly API を使用してアップロード ](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api) します。 管理者にアクセスできない場合は、この方法を使用します。

## カスタム VCL スニペットの追加

{{admin-login-step}}

1. **ストア**/設定/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**カスタム VCL スニペット** の順に展開します。

1. **カスタムスニペットを作成** をクリックします。

1. VCL スニペットの値を追加します。

   - **名前** — `allowlist`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツを追加します。

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. **作成** をクリックして、名前パターン `type_priority_name.vcl` （たとえば `recv_5_allowlist.vcl`）で VCL スニペット ファイルを生成します

1. ページのリロード後、「**Fastly 設定**」セクションの「*Fastly に VCL をアップロード*」をクリックして、ファイルを Fastly サービス設定に追加します。

1. アップロードが完了したら、ページ上部の通知に従ってキャッシュを更新します。

Fastly は、アップロードプロセス中に VCL コードの更新バージョンを検証します。 検証に失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCL を再度アップロードします。

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
