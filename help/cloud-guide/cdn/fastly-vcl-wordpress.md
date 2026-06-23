---
title: CMS バックエンドへのリクエストのルート変更
description: Fastly Edge モジュールを使用して、Adobe Commerce ストアから別のWordPress サイトにリクエストを再ルーティングする方法について説明します。
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
TQID: https://experienceleague.adobe.com/zRM-iTFGNPgSmT5xu1B9Lo3-onUtCHh-tVY-WPPiVC8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 322
ht-degree: 0%

---

# CMS バックエンドへのリクエストのルート変更

Fastly Edge Module _その他のAdobe Commerce/バックエンド統合_&#x200B;とEdge ディクショナリを使用して、CMS ストアからの着信リクエストを別のWordPress サイトにルートします。 同様のプロセスに従って、リクエストを他のCMS バックエンドに再ルーティングできます。

VCL コードを手動で記述してFastly APIを使用してアップロードする代わりに、Fastly Edge モジュールを使用して、管理者からカスタム VCL コードを作成してアップロードします。

>[!NOTE]
>
>実稼動環境でFastly サービス設定を更新する前にテストできるステージング環境にカスタム VCL設定を追加することをお勧めします。

**前提条件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Adobe CommerceからWordPress**&#x200B;にリクエストを再ルーティングするには：

1. ステージング環境または実稼動環境でFastly Edge モジュールを有効にします。

   - Adminにログインします。

   - **Stores** >設定> **Configuration** > **Advanced** > **System** > **フルページキャッシュ** > **Fastly Configuration** > **Advanced Configuration**&#x200B;に移動します。

   - **Fastly Edge Modules**&#x200B;の値を&#x200B;**Yes**&#x200B;に設定します。

   - 設定を保存します。

1. WordPress バックエンドに再ルーティングするURL パスを特定します。

1. 次のタスクを完了して、Fastly サービスを設定し、WordPress バックエンドにリクエストを再ルーティングするためのカスタム VCL コードを作成します。

   - Adobe Commerce ストアからバックエンドに再ルーティングするパスを指定するEdge ディクショナリを作成します。

   - WordPress バックエンドをFastly サービス設定に追加し、URL書き換え用のリクエスト条件を添付します。

   - _その他のCMS/バックエンド統合_ Edge Moduleを構成して、Adobe CommerceからWordPress バックエンドへのURL書き換えを処理します。

     詳しい手順については、「[Fastly Edge Modules – その他のCMS/バックエンド統合](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md)」を参照してください。Magento 2 _のドキュメント「_ Fastly CDN module」を参照してください。

1. Fastly サービス設定を更新した後、Adobe Commerceストアをテストして、WordPress用に指定したURL リクエストが正しくルーティングされていることを確認します。

<!-- Last updated from includes: 2025-01-27 17:16:28 -->

