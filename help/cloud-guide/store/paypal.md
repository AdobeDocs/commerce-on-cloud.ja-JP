---
title: PayPal決済方法を設定する
description: Adobe CommerceクラウドインフラストラクチャでPayPal支払い方法を設定する。
feature: Cloud, Checkout, Payments
exl-id: 577639f8-74a1-4bb2-96fc-72135252cbd1
source-git-commit: e3a2c8580ad1f27ddd3dc8fc40207bce68ee1c7f
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# PayPal決済方法を設定する

Adobe Commerce オンクラウドインフラストラクチャには、PayPal Express チェックアウトアカウントを管理画面から直接設定するためのオンボーディングツールが用意されています。 このツールは、ECE 2.1.8以降で使用できます。 PayPal支払い方法の本番稼働中およびテストをより適切にサポートするには、サンドボックスまたは実稼動アカウント用にPayPal Express Checkout アカウントを有効にして設定します。

サンドボックスまたは実稼動アカウントは、すべての環境で設定できます。

* 統合環境とステージング環境の場合は、サンドボックス資格情報を設定します。
* 実稼動環境の場合は、初期テスト用にサンドボックス認証情報を設定し、起動したストアのライブ実稼動認証情報に置き換えます。

## PayPal アカウント

準備して設定したPayPal加盟店アカウントを使用することをお勧めしますが、アカウントを作成するか、管理者から個人アカウントをアップグレードできます。

[!DNL PayPal onboarding]は、次のアカウントとの接続をサポートしています：

* PayPal法人用アカウント
* 法人アカウントに変換するPayPal個人アカウント。 既存の個人PayPal アカウントをお持ちの場合は、それらの資格情報でログインし、同期を完了するとこのアカウントをビジネスアカウントにアップグレードできます。

既存のPayPal アカウントがない場合は、アカウントを作成します。 新しいアカウントのメールアドレスを入力してください。 一致するPayPal アカウントが見つからない場合は、プロンプトに従ってPayPal Business アカウントを作成します。 または、[PayPal](https://www.paypal.com/us/webapps/mpp/account-selection)から直接アカウントを作成できます。

### PayPalの制限

PayPalは、次の制限事項を除き、世界中の国のPayPal Express Checkoutの接続をサポートしています。

* インドと日本（今後のPayPalのアップデートはこれらのアカウントをサポートする可能性があります）
* イスラエル

ブラジルの場合は、接続するには既存のPayPal ビジネスアカウントが必要です。 このプロセス中に、ブラジルの既存の個人PayPal アカウントを変換することはできません。 アカウントが必要な場合は、[法人PayPal アカウントを作成します](https://www.paypal.com/us/webapps/mpp/account-selection)。

## PayPal Express チェックアウトの設定

PayPal Express チェックアウトを設定するには：

1. 環境の管理者にアクセスします。
1. 左側のナビゲーションで、**ストア** > **設定**&#x200B;を選択し、**販売** > **支払い方法**&#x200B;を選択します。
1. PayPalの場合は、**Configure**&#x200B;を選択します。 設定フィールドは、Express Checkout、Advertising PayPal Credit、BasicおよびAdvanced設定の拡張可能なセクションに表示されます。
1. PayPal アカウントに接続します。 アカウントが接続されるまで、有効にするオプションは無効になります。 接続と制限に使用できる利用可能なアカウントとサポートされているアカウントについて詳しくは、[PayPal アカウント &#x200B;](#paypal-account)を参照してください。

   * PayPal ライブアカウントを接続するには、「PayPalとつながる」をクリックし、プロンプトに従います。 ライブペイパルを使用して購入した顧客は、すべて完了し、ライブストアで顧客に積極的に請求されます。
   * テスト用にサンドボックスアカウントを接続するには、「サンドボックス認証情報」をクリックし、プロンプトに従います。 顧客に積極的に請求することなく、サンドボックス PayPal コンプリートを使用して購入する顧客。

1. PayPal APIを認証して使用するには、Express Checkout設定を設定します。

   * **PayPal加盟店アカウントに関連付けられた電子メール** （オプション） PayPal加盟店アカウントに関連付けられた電子メールアドレスを入力します。 このメールでは大文字と小文字が区別されます。
   * API署名またはAPI証明書としての&#x200B;**API認証方法**。
   * PayPal アカウントから取得したAPI ユーザー名、パスワード、および署名。
   * **サンドボックスモード**&#x200B;入力した資格情報がサンドボックス用であるかどうかを示すには、「はい」または「いいえ」を選択します。 実稼動資格情報を入力した場合は、「いいえ」を選択します。
   * **APIはプロキシを使用**&#x200B;は、「はい」または「いいえ」を選択して、システムがプロキシサーバーを使用してAdobe CommerceとPayPal支払いシステム間の接続を確立するかどうかを設定します。 「はい」の場合は、プロキシホストとポートを入力します。

1. アカウントを設定するための詳細な情報と手順については、「[PayPal Express Checkout](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout) （手順2）で始まる）必要な設定を完了」を参照してください。

アカウントが設定され、認証されている場合は、必須のPayPal設定でPayPal支払いオプションを有効または無効にできます。

* **このソリューションを有効にする**&#x200B;は、Web サイトを通じてお客様にPayPal支払い方法を表示します。
* **インコンテクスト チェックアウト エクスペリエンスを有効にする**
* **PayPal クレジットを有効にする**&#x200B;を使用すると、追加費用なしでPayPal クレジットによる融資を行うことができます。 PayPalは注文を前払いし、クレジットのすべての返済を顧客と直接処理します。

## PayPal変数

Adobe Commerce on cloud infrastructureでPayPal オンボーディングツールを使用する場合は、`magento.app.yaml` ファイルの`variables:env` セクションに次の変数を追加します。

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

2.1.8以降から2.2にアップグレードする場合でも、この変数を追加する必要があります。
