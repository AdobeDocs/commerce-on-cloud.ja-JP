---
title: Fastly キャッシュをバイパスするカスタム VCL
description: Fastly キャッシュをバイパスするカスタム VCL スニペットを作成することで、オリジンサーバーへのリクエストトラフィックをトラブルシューティングします。
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
TQID: https://experienceleague.adobe.com/67LdlbG62T-cBEgNTwQ5p5MvvYQwNuHUYVQhrVBSn1A
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
source-wordcount: 303
ht-degree: 0%

---

# Fastly キャッシュをバイパスするカスタム VCL

Fastly キャッシュをバイパスするようにカスタム VCL スニペットを作成することで、オリジンサーバーへのリクエストトラフィックをトラブルシューティングできます。 例えば、サイトの問題がキャッシュによって発生しているかどうかを判断したり、ヘッダーのトラブルシューティングを行うためにスニペットを作成できます。

特定のIP アドレスまたはURLからのリクエストに対して、Fastlyのキャッシュをバイパスするようにスニペットを設定できます。

>[!NOTE]
>
>カスタム VCL設定を実稼動環境にマージする前に、必ずステージング環境でコードをテストしてください。

**前提条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**IP アドレスまたはURL**&#x200B;に基づいてFastly キャッシュをバイパスするには：

{{admin-login-step}}

1. **ストア** / 設定/**構成** / **詳細** / **システム**&#x200B;をクリックします。

1. **フルページキャッシュ** > **Fastly設定** > **カスタム VCL スニペット**&#x200B;を展開します。

1. 「**カスタムスニペットを作成**」をクリックします。

1. VCL スニペット値を追加します。

   - **名前** — `bypass_fastly`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツ —

     次の例では、特定のIP アドレスに対してFastlyをバイパスします。

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     次の例では、特定のURL パターンに対してFastlyをバイパスします。

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     正確なURL一致を得るには、`~`演算子の代わりに`==`演算子を使用します。 詳しくは、[Fastly VCL リファレンス ]を参照してください。

1. 「**作成**」をクリックします。

   ![FastlyでVCL スニペットをバイパスする](/help/assets/cdn/fastly-create-bypass-snippet.png)を作成

1. ページが再読み込みされたら、「*Fastly設定*」セクションの「**VCLをFastly**&#x200B;にアップロード」をクリックします。

1. アップロードが完了したら、ページ上部の通知に従ってキャッシュを更新します。

   アップロードプロセス中に、更新されたVCL バージョンをFastlyが検証します。 検証が失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCLをもう一度アップロードします。

VCL スニペットを追加した後、次の例に示すように、cURL コマンドを使用して、指定したIP アドレスまたはURLからオリジンサーバーにリクエストを送信できます。

```bash
curl -svo /dev/null www.example.com/index.html
```

次に、応答を調べて、キャッシュされていないコンテンツに関する問題をトラブルシューティングします。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL リファレンス]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
