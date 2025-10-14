---
title: Fastly キャッシュをバイパスするカスタム VCL
description: Fastly キャッシュをバイパスするカスタム VCL スニペットを作成して、オリジンサーバーへのリクエストトラフィックをトラブルシューティングします。
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Fastly キャッシュをバイパスするカスタム VCL

カスタム VCL スニペットを作成して、Fastly キャッシュをバイパスし、オリジンサーバーへのリクエストトラフィックのトラブルシューティングを行うことができます。 例えば、スニペットを作成して、サイトの問題がキャッシュに起因するものかどうかを判断したり、ヘッダーのトラブルシューティングを行ったりできます。

特定の IP アドレスまたは URL からのリクエストに対して Fastly のキャッシュをバイパスするように、スニペットを設定できます。

>[!NOTE]
>
>カスタム VCL 設定を実稼動環境に結合する前に、必ずステージング環境でコードをテストしてください。

**前提条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**IP アドレスまたは URL に基づいて Fastly キャッシュをバイパスするには**:

{{admin-login-step}}

1. **ストア**/設定/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**カスタム VCL スニペット** の順に展開します。

1. **カスタムスニペットを作成** をクリックします。

1. VCL スニペットの値を追加します。

   - **名前** — `bypass_fastly`

   - **種類** — `recv`

   - **優先度** — `5`

   - **VCL** スニペットコンテンツ —

     次の例では、特定の IP アドレスの Fastly をバイパスしています。

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     次の例では、特定の URL パターンに対して Fastly をバイパスしています。

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     URL が完全に一致する場合は、`==` 演算子の代わりに `~` 演算子を使用します。 詳しくは、[Fastly VCL リファレンス ] を参照してください。

1. **作成** をクリックします。

   ![Fastly バイパス VCL スニペットの作成 &#x200B;](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. ページのリロード後、「**Fastly 設定** セクションの *Fastly に VCL をアップロード* をクリックします。

1. アップロードが完了したら、ページ上部の通知に従ってキャッシュを更新します。

   Fastly は、アップロード処理中に更新された VCL バージョンを検証します。 検証に失敗した場合は、カスタム VCL スニペットを編集して問題を修正します。 次に、VCL を再度アップロードします。

VCL スニペットを追加した後、cURL コマンドを使用して、次の例に示すように、指定した IP アドレスまたは URL からオリジンサーバーにリクエストを送信できます。

```bash
curl -svo /dev/null www.example.com/index.html
```

次に、応答を調べて、キャッシュされていないコンテンツに関する問題のトラブルシューティングを行います。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL 参照]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
