---
title: Variables プロパティ
description: variables プロパティを使用して、アプリケーションのストア設定オプション  [!DNL Commerce]  カスタマイズします。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Variables プロパティ

アプリケーションベースの環境変数を使用して、ストア設定をカスタマイズできます。 これらの変数は、特定の構文を使用します。 [&#x200B; 設定ガイド &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=ja) の「_設定のオーバーライド_」を参照してください。

[!DNL Commerce] アプリケーションの特定のバージョンには、`.magento.app.yaml` ファイルに含まれる次の環境変数が必要です。

Adobe Commerce 2.2.x から 2.3.x へのアップグレードに必要：

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Adobe Commerce 2.4.x の場合は、次の変数を設定します。

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
