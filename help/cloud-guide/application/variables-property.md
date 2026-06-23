---
title: Variables プロパティ
description: variables プロパティを使用して、 [!DNL Commerce]  アプリケーションのストア設定オプションをカスタマイズします。
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 85
ht-degree: 0%

---

# Variables プロパティ

アプリケーションベースの環境変数を使用して、ストア設定をカスタマイズできます。 これらの変数は特定の構文を使用します。 _構成ガイド_&#x200B;の「[構成設定を上書き](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html)」を参照してください。

[!DNL Commerce] アプリケーションの特定のバージョンには、`.magento.app.yaml` ファイルに含まれる次の環境変数が必要です。

Adobe Commerce 2.2.xから2.3.xに必要：

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Adobe Commerce 2.4.xの場合は、次の変数を設定します。

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

