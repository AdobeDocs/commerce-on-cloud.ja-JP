---
title: 静的ファイルのキャッシュの設定
description: '[!DNL Commerce] アプリケーション設定ファイルでキャッシュ ストレージ オプションを設定する方法を説明します。'
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: 'https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%
---
# 静的ファイルのキャッシュの設定

メディアと静的ファイルのキャッシュ TTL （有効期間）は、`expires` キーを使用して`.magento.app.yaml`設定ファイルで設定されます。

>[!NOTE]
>
>実稼動環境を更新する前に、ステージング環境の変更をテストすることが重要です。 これらの環境の設定を更新するヘルプについては、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)を送信してください。

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

