---
title: リクエストを許可するためのカスタム VCL
description: Fastly Edge ACL リストとカスタム VCL スニペットを使用して、Adobe Commerceサイトの受信リクエストをフィルタリングし、IP アドレスによるアクセスを許可します。
feature: Cloud, Configuration, Security
exl-id: 836779b5-5029-4a21-ad77-0c82ebbbcdd5
TQID: https://experienceleague.adobe.com/szgjjm841ttfcCwULGf3lBNSRhixIhMPfmoYILbNGKY
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
source-wordcount: 873
ht-degree: 0%

---

# リクエストを許可するためのカスタム VCL

カスタム VCL コードスニペットを使用してFastly EdgeのACL リストを使用し、受信リクエストをフィルタリングしたり、IP アドレスによるアクセスを許可したりできます。 ACL リストは、許可するIP アドレスを指定します。

ステージング環境へのアクセスを制限する許可リストを作成して、内部開発者および承認済み外部サービスに対する指定されたIP アドレスからのリクエストのみが許可されるようにします。 また、ステージング環境と実稼動環境の管理者へのアクセスを保護するための許可リストを作成することもできます。

次の例は、[Fastly Access Control List （ACL） &#x200B;](https://docs.fastly.com/guides/access-control-lists/about-acls)を含むカスタム VCL スニペットを使用して、Adobe Commerce on cloud infrastructure プロジェクト環境の管理者へのアクセスを保護する方法を示しています。 カスタム VCL スニペットをCloud環境に追加すると、Fastlyでは、ACLに含まれるIP アドレスからのリクエストのみが許可されます。

>[!TIP]
>
>公開アクセスできないステージング環境と統合環境の場合は、[[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface)で使用可能なHTTP アクセス制御オプションを使用して、サイト全体へのアクセスをIP アドレスで管理します。

**前提条件：**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- クライアント IP アドレスの一覧を表示して許可リストに含める

## クライアント IP アドレスを許可するためのEdge ACLの作成

Edge ACLは、サイトへのアクセスを管理するためのIP アドレスリストを作成します。 この例では、Edge ACLを作成し、プロジェクト環境の管理者にアクセスできるクライアント IP アドレスのリストを追加します。

{{admin-login-step}}

1. **ストア** / 設定/**構成** / **詳細** / **システム**&#x200B;をクリックします。

1. **フルページキャッシュ** > **Fastly Configuration** > **Edge ACL**&#x200B;を展開します。

1. ACL コンテナを作成します。

   - 「**ACL**&#x200B;を追加」をクリックします。

   - *ACL コンテナ* ページで、**ACL名**—`allowlist`を入力します。

   - 変更の後に&#x200B;**アクティブ化**&#x200B;を選択して、編集中のFastly サービス設定のバージョンに変更をデプロイします。

   - 「**アップロード**」をクリックして、ACLをFastly サービス設定に添付します。

1. 管理者へのアクセスが許可されているIP アドレスのリストを追加します。

   - `allowlist` ACLの設定アイコンをクリックします。

   - クライアント IP アドレスごとに&#x200B;*IP値*&#x200B;を追加して保存します。

   - 「**キャンセル**」をクリックして、システム設定ページに戻ります。

1. 「**設定を保存**」をクリックします。

1. ページ上部の通知に従ってキャッシュを更新します。

## 管理者アクセスを保護するためのカスタム VCL スニペットの作成

次のカスタム VCL スニペットコード（JSON形式）は、管理者へのリクエストをフィルタリングし、クライアント IP アドレスが`allowlist` ACLのアドレスと一致する場合にアクセスを許可するロジックを示しています。

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

この例から[&#x200B; カスタムスニペット &#x200B;](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet)を作成する前に、値を確認して、変更が必要かどうかを判断してください。 次に、それぞれのフィールドに各値を入力します。例えば、「`type`」を「タイプ」フィールドに、「`content`」をコンテンツフィールドに入力します。

- `name` — VCL スニペットの名前。 この例では、`allowlist`です。

- `priority` — VCL スニペットが実行されるタイミングを決定します。 管理者要求が許可されたIP アドレスから送信されているかどうかを即座に実行して確認する優先度は`5`です。 スニペットは、デフォルトのMagento VCL スニペット（`magentomodule_*`）のいずれかが優先度50に割り当てられる前に実行されます。 スニペットを実行するタイミングに応じて、各カスタムスニペットの優先度を50より高くまたは低く設定します。 優先度の低いスニペットが最初に実行されます。

- `type` - バージョン管理されたVCL コードにスニペットを挿入する場所を指定します。 このVCLは`recv` スニペット型で、デフォルトのFastly VCL コードの下、および任意のオブジェクトの上の`vcl_recv` サブルーチンにスニペット コードを追加します。

- `content` – 実行するVCL コードのスニペット。 この例では、コードは管理者へのリクエストをフィルタリングし、クライアント IP アドレスが`allowlist` ACLのアドレスと一致する場合にアクセスを許可します。 アドレスが一致しない場合、リクエストは`403 Forbidden` エラーでブロックされます。

  管理者のURLが変更された場合は、サンプル値`/admin`を環境のURLに置き換えます。 例：`/company-admin`。

コードサンプルでは、[&#x200B; オリジンシールド &#x200B;](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)を使用する場合、条件`!req.http.Fastly-FF`が重要です。 このコードを削除または編集しないでください。

環境のコードを確認して更新したら、次のいずれかの方法を使用して、Fastly サービス設定にカスタム VCL スニペットを追加します。

- [管理者](#add-the-custom-vcl-snippet)からカスタム VCL スニペットを追加します。 管理者にアクセスできる場合は、この方法をお勧めします。 （Magento 2 バージョン 1.2.58[&#128279;](fastly-configuration.md#upgrade)以降ではFastly CDN モジュールが必要です）。

- JSON コードの例をファイル （例：`allowlist.json`）に保存し、Fastly API[&#128279;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)を使用して アップロードします。 管理者にアクセスできない場合は、この方法を使用します。

## カスタム VCL スニペットの追加

{{admin-login-step}}

1. **ストア** / 設定/**構成** / **詳細** / **システム**&#x200B;をクリックします。

1. **フルページキャッシュ** > **Fastly設定** > **カスタム VCL スニペット**&#x200B;を展開します。

1. 「**カスタムスニペットを作成**」をクリックします。

1. VCL スニペット値を追加します。

   - **名前** — `allowlist`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツを追加します。

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. **作成**&#x200B;をクリックして、名前パターン `type_priority_name.vcl`のVCL スニペットファイルを生成します（例：`recv_5_allowlist.vcl`）

1. ページがリロードされたら、「*Fastly設定*」セクションの「**VCLをFastly**&#x200B;にアップロード」をクリックして、ファイルをFastly サービス設定に追加します。

1. アップロードが完了したら、ページ上部の通知に従ってキャッシュを更新します。

アップロードプロセス中に、VCL コードの更新バージョンを迅速に検証します。 検証が失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCLをもう一度アップロードします。

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
