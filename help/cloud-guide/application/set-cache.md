---
title: 静的ファイルのキャッシュの設定
description: ' [!DNL Commerce]  アプリケーション設定ファイルでキャッシュ ストレージ オプションを設定する方法を説明します。'
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 0%

---

# 静的ファイルのキャッシュの設定

メディアと静的ファイルのキャッシュ TTL （有効期間）は、`expires` キーを使用して`.magento.app.yaml`設定ファイルで設定されます。

>[!NOTE]
>
>実稼動環境を更新する前に、ステージング環境の変更をテストすることが重要です。 これらの環境の設定を更新するヘルプについては、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信してください。

1. `.magento.app.yaml` ファイルの[`web` プロパティ &#x200B;](web-property.md)でTTL時間（秒単位）を指定します。 `expires` キーは`locations`の下、または`"/media"`と`"/static"`の下に追加できます。

   キャッシュの有効期限が切れるのを防ぐには、`expires: -1` キーと値のペアを使用します。 次の例を参照してください。

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```

