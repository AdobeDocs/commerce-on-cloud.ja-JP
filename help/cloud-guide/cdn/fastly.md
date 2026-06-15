---
title: Fastly サービスの概要
description: Adobe Commerce on cloud インフラストラクチャに含まれるFastly サービスが、Adobe Commerceサイトのコンテンツ配信処理を最適化し、保護する方法をご紹介します。
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: 429b6762-0b01-438b-a962-35376306895b
TQID: https://experienceleague.adobe.com/Lq2rzR14xlcj5y3ycfAWGHEKAIxboekZX8YtyOJPQXA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
subfeature_v2:
  - id: f2261633-201d-46c5-8a66-999e70527a83
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: e0e1d3994a6b9ceef9e45b55cc9946bc62203ddb
workflow-type: tm+mt
source-wordcount: 1667
ht-degree: 0%

---

# Fastly サービスの概要

>[!WARNING]
>
>Cloud PlatformにデプロイされたAdobe Commerce サイトのPCI認定を維持するには、Starter メインブランチ、Pro実稼動、およびPro ステージング環境でFastlyをセットアップします。 ヘッドレス実装でAdobe Commerceを使用する場合は、GraphQLの応答をキャッシュするためにFastlyを使用することを強くお勧めします。 *GraphQL デベロッパーガイド*&#x200B;の「[Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly)でのキャッシュ」を参照してください。

Fastlyは、Adobe Commerceのクラウドインフラプロジェクト向けに、コンテンツ配信処理を最適化および保護するために次のサービスを提供しています。 これらのサービスは、Adobe Commerce on cloud インフラストラクチャに含まれており、追加費用はかかりません。

- **コンテンツ配信ネットワーク （CDN）** – 設定したバックエンド データセンターにサイト ページ、アセット、CSSなどをキャッシュするVarnish ベースのサービス。 顧客がサイトやストアにアクセスすると、リクエストはFastlyに送信され、キャッシュされたページの読み込みが速くなります。 CDN サービスには、次の機能が用意されています。

- **キャッシュ管理**：帯域幅の負荷とコストを削減するために設定したバックエンド データセンターに、サイト ページ、アセット、CSSなどをキャッシュします

   - [Fastly カスタム VCL スニペット &#x200B;](fastly-vcl-custom-snippets.md) （Varnish 2.1準拠）を使用して、リクエストに対するキャッシュの応答方法を変更します

   - [GeoIP サービス サポートの設定](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [暗号化されていないリクエストをTLSに強制的に送信する](fastly-custom-cache-configuration.md#force-tls)

   - [Fastlyのタイムアウト &#x200B;](fastly-custom-cache-configuration.md#extend-fastly-timeout)設定をカスタマイズして、一括操作リクエストに対する503件の応答を防ぎます

   - [&#x200B; カスタムエラー応答ページを作成](fastly-custom-response.md)

- **セキュリティ** - Adobe Commerce サイトに対してFastly サービスを有効にした後、サイトとネットワークを保護するために追加のセキュリティ機能を利用できます。

   - [Web Application Firewall](fastly-waf-service.md) （WAF）：本番Adobe Commerceをクラウド インフラストラクチャ サイトおよびネットワーク上で損傷させる前に、悪意のあるトラフィックをブロックするためのPCI準拠の保護を提供するマネージド web アプリケーション ファイアウォール サービス。 WAF サービスは、ProおよびStarter Production環境でのみ使用できます。

   - [分散型サービス拒否（DDoS）保護](#ddos-protection) - Ping of Death、スマーフ攻撃、その他のICMP ベースのフラッド攻撃などの一般的なレイヤ 3および4攻撃に対する組み込みのDDoS保護機能。 組み込みの保護には、レイヤ 7攻撃に対する保護は含まれていません。 [DDoS保護対策](#ddos-protection)を参照してください。

   - [SSL/TLS証明書](fastly-configuration.md#provision-ssltls-certificates):Fastly サービスでは、HTTPS経由で安全なトラフィックを提供するためにSSL/TLS証明書が必要です。

     Adobe Commerceでは、ステージング環境と実稼動環境ごとにドメイン検証済みのLet&#39;s Encrypt SSL/TLS証明書を提供しています。 Adobe Commerceは、Fastlyの設定プロセスにおいて、ドメインの検証と証明書のプロビジョニングを完了します。

- **オリジンクローキング** – すべてのトラフィックがFastlyを経由することを保証し、オリジンサーバーへの直接アクセスをブロックするセキュリティ機能。 以下の「[発信元のクローキング &#x200B;](#origin-cloaking)」セクションを参照してください。

- **[画像の最適化](fastly-image-optimization.md)** - サーバーがより効率的に注文とコンバージョンを処理できるように、画像処理とサイズ変更の負荷をFastly サービスにオフロードします。

- **[Fastly CDNおよびWAF ログ](../monitor/new-relic-service.md#new-relic-log-management)**—Adobe Commerce on cloud infrastructure Pro プロジェクトの場合、New Relic Logs サービスを使用して、Fastly CDNおよびWAF ログデータをレビューおよび分析できます。

## オリジンのクローキング {#origin-cloaking}

オリジンのクローキングは、Fastly以外のトラフィックがAdobe Commerce オンクラウドインフラストラクチャのオリジンに到達するのを防ぐセキュリティ機能です。 すべてのリクエストは、この強制パスに従う必要があります。

**Fastly > Load Balancer > Adobe Commerce アプリケーションインスタンス**

このパスを使用すると、すべてのトラフィックがFastly Web アプリケーションファイアウォール（WAF）とロードバランサーの内部WAFによって検査されます。 オリジンクローキングは、ダイレクトアクセスの試みからサイトを保護し、DDoS攻撃のリスクを軽減します。

### イネーブルメントステータス

オリジンのクロージングは、2021年以降、すべてのAdobe Commerce on cloud インフラストラクチャプロジェクトで完全に有効になっています。\
2021年以降にプロビジョニングされたプロジェクトには、デフォルトでこの設定が含まれます。\
**発信元のクローキングの有効化をリクエストするために**&#x200B;操作は必要ありません。

#### 発信元のクローキングブロック

オリジンのクローキングは、次のようなオリジンのインフラストラクチャへの直接アクセスをブロックします。

```
mywebsite.com.c.abcdefghijkl.ent.magento.cloud
mcstaging2.mywebsite.com.c.abcdefghijkl.dev.ent.magento.cloud
mcstagingX.mywebsite.com.c.abcdefghijkl.X.dev.ent.magento.cloud
```

パブリックドメインを介したリクエストは、REST API トラフィックを含め、引き続き正常に動作します。 例：

```
mywebsite.com/rest/default/V1/integration/admin/token
mywebsite.com/rest/default/V1/orders/
mywebsite.com/rest/default/V1/products/
mywebsite.com/rest/default/V1/inventory/source-items
```

#### サービス行動への影響

- **送信IP アドレスは変更されません。**
- **REST APIは影響を受けません。** FastlyはAPI呼び出しをキャッシュしません。
- **デプロイメントとダウンタイムは影響を受けません。**
- プロジェクトに複数のステージング環境がある場合、**オリジンのクローキングは、それらすべてに適用されます**。

## Fastly CDN module for Magento 2

クラウドインフラストラクチャ上のAdobe Commerce用Fastly サービスでは、次の環境にインストールされているMagento 2&rbrack;用の&lbrack;Fastly CDN モジュールを使用します：Pro ステージングおよび実稼動、スタータープロダクション（`master` ブランチ）。

Adobe Commerce プロジェクトの最初のプロビジョニングまたはアップグレード時に、Adobeは最新バージョンのFastly CDN モジュールをステージング環境および実稼動環境にインストールします。 Fastlyがモジュールの更新をリリースすると、環境の管理者に通知が届きます。 Adobeでは、最新リリースを使用するように環境を更新することをお勧めします。 [Fastlyのアップグレード &#x200B;](fastly-configuration.md#upgrade-the-fastly-module)を参照してください。

## Fastlyのサービスアカウントと認証情報

Adobe Commerce on cloud インフラストラクチャプロジェクトには、専用のFastly アカウントは割り当てられません。 Fastly サービスは、Adobeに登録された一元的なアカウントで管理され、ダッシュボードへのアクセスはクラウドサポートチームに限定されます。 そのため、サポートでは、お客様のリクエストに応じてFastly ダッシュボードへのアクセスを提供することはできません。 サポートされているFastlyの設定タスクと管理タスクには、Adobe Commerce管理者と環境固有のFastly認証情報を使用します。

代わりに、各ステージングおよび実稼動環境には、Commerce管理者からFastly サービスを設定および管理するための一意のFastly認証情報（API トークンおよびサービス ID）があります。 Fastly APIは、Fastly サービスの高度な管理を実行するために使用できます。これには、資格情報が必要です。

プロジェクトのプロビジョニング中に、AdobeはAdobe Commerce クラウドインフラストラクチャ上のFastly サービスアカウントにプロジェクトを追加し、ステージング環境と実稼動環境の設定にFastly資格情報を追加します。 [Fastly資格情報の取得](fastly-configuration.md#get-fastly-credentials)を参照してください。

### Fastly API トークンの変更

Adobe Commerce サポートチケットを送信して、新しいFastly API トークン資格情報[が検証に失敗した場合、期限切れになった場合、または侵害されたと思われる場合に発行します](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials)。

新しいトークンを受け取ったら、新しいトークンを使用するようにステージング環境または実稼動環境を更新します。

**Fastly API トークン資格情報を変更するには**:

1. 新しいFastly API資格情報をリクエストする[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信します。

   Adobe Commerce on cloud インフラストラクチャプロジェクト IDと、新しい資格情報を必要とする環境を含めます。

1. 新しいAPI トークンを受け取った後、管理者または[[!DNL Cloud Console] 環境変数](../project/overview.md#configure-environment)の[Fastly資格情報設定](fastly-configuration.md#test-the-fastly-credentials)でAPI トークンの値を更新します。

1. [新しい資格情報をテストします](fastly-configuration.md#test-the-fastly-credentials)。

1. 資格情報を更新したら、Adobe Commerce サポートチケットを送信して、古いAPI トークンを削除します。

### 複数のFastly アカウントと割り当てられたドメイン

Fastlyでは、1つのFastly サービスとアカウントにapex ドメインと関連するサブドメインのみを割り当てることができます。 Adobe Commerce サイトに使用されているのと同じapexとサブドメインをリンクする既存のFastly アカウントがある場合は、次のオプションがあります。

- Adobe Commerce on cloud infrastructure プロジェクト環境のFastly サービス資格情報をリクエストする前に、既存のアカウントからapexとサブドメインを削除します。 Fastly ドキュメントの[ ドメインの操作]を参照してください。

  このオプションを使用して、apex ドメインとすべてのサブドメインをAdobe Commerce on cloud infrastructureのFastly サービスアカウントにリンクします。

- Adobe Commerce サポートチケットを送信してドメインデリゲーションをリクエストし、apexとサブドメインを別のアカウントにリンクできるようにします。

  Adobe CommerceおよびAdobe Commerce以外のサイト用の複数のサブドメインを持つapex ドメインがあり、これらのサブドメインを異なるFastly アカウントにリンクする場合は、このオプションを使用します。

#### ドメインのデリゲーションをリクエスト

*シナリオ 1:*

apex ドメイン （`testweb.com`および`www.testweb.com`）は、既存のFastly アカウントにリンクされています。 次のサブドメインで構成されたAdobe Commerce on cloud infrastructure プロジェクトがあります：`mcstaging.testweb.com`と`mcprod.testweb.com`。 apex ドメインをAdobe Commerce on cloud infrastructureのFastly サービスアカウントに移行しないでください。

サブドメインを既存のFastly アカウントからAdobe Commerce on cloud infrastructureのFastly アカウントにデリゲートすることを要求する[Fastly サポートチケット ]を送信します。 チケットにAdobe Commerce プロジェクト IDを含めます。

デリゲーションが完了したら、プロジェクトのサブドメインをAdobe Commerce on cloud infrastructureのFastly サービスアカウントに追加できます。 [Fastly資格情報の取得](fastly-configuration.md#get-fastly-credentials)を参照してください。

*シナリオ 2:*

apex ドメイン （`testweb.com`および`www.testweb.com`）は、Adobe Commerce on cloud infrastructure Fastly サービスアカウントにリンクされています。 異なるFastly アカウントの`service.testweb.com`と`product-updates.testweb.com`のサブドメインのFastly サービスを管理する場合。

サブドメインをAdobe Commerce on cloud infrastructure Fastly サービスアカウントからFastly アカウントにデリゲートすることをリクエストするAdobe Commerce サポートチケットを提出します。 Fastly アカウントのサービス IDをチケットに含めます。

## DDoS対策

DDOS保護機能は、Fastly CDN サービスに組み込まれています。 Adobe CommerceサイトでFastly サービスを有効にすると、Fastlyはすべてのweb トラフィックと管理者トラフィックをフィルタリングして、潜在的な攻撃を検出およびブロックします。

- レイヤ 3または4をターゲットとする攻撃の場合、Fastly サービスはポートとプロトコルに基づいてトラフィックをフィルタリングし、HTTPまたはHTTPS リクエストのみを検査します。 ICMP、UDP、その他のネットワークが開始した攻撃は、当社のネットワークエッジでドロップされます。 これには、SSDPやNTPなどのUDP サービスを使用するリフレクション攻撃や増幅攻撃が含まれます。 このレベルの保護を提供することで、Ping of Death、Smurf攻撃、その他のICMP ベースの洪水などの複数の一般的な攻撃を効果的にブロックします。

  Fastlyは、キャッシュ層でTCP レベルの攻撃を管理します。 この戦略は、SYN フラッド攻撃とその多くのバリエーション（TCP スタック、リソース攻撃、Fastly システム内のTLS攻撃など）に対処するためにクライアントごとに必要な規模とコンテキストを提供します。

>[!NOTE]
>
>レイヤ 7攻撃に対する保護は、Adobe Commerceと統合されたFastly CDN サービスではカバーされていません。 レイヤ 7攻撃に対する保護のヒントについては、*Adobe Commerce ナレッジベース*&#x200B;の[DDoS攻撃の確認](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli)および[悪意のある攻撃をブロックする方法](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level)を参照してください。

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Checking for DDoS attacks]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Fastly CDN module for Magento 2]: https://github.com/fastly/fastly-magento2

[Fastly サポートチケット]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[How to block malicious traffic]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[ドメインの操作]: https://docs.fastly.com/en/guides/working-with-domains
