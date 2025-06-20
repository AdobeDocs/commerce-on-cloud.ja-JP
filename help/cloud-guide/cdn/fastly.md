---
title: Fastly サービスの概要
description: クラウドインフラストラクチャー上のAdobe Commerceに含まれる Fastly サービスが、Adobe Commerce サイトのコンテンツ配信操作を最適化し、安全を確保する上でどのように役立つかを説明します。
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: 429b6762-0b01-438b-a962-35376306895b
source-git-commit: 3cef442321120d8ca813c760d2fd0435f4961235
workflow-type: tm+mt
source-wordcount: '1443'
ht-degree: 0%

---

# Fastly サービスの概要

>[!WARNING]
>
>クラウドプラットフォームにデプロイされたAdobe Commerce サイトの PCI コンプライアンスを維持するには、スターターメインブランチ、実稼動、ステージング環境に Fastly を設定します。 ヘッドレスデプロイメントでAdobe Commerceを使用する場合は、Fastly を使用してGraphQLの応答をキャッシュすることを強くお勧めします。 [2&rbrace;GraphQL開発者ガイド ](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) の「Fastly でのキャッシュ *」を参照してください。*

Fastly は、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのコンテンツ配信操作を最適化および保護するために、次のサービスを提供しています。 これらのサービスは、クラウドインフラストラクチャー上のAdobe Commerceに追加費用なしで含まれています。

- **コンテンツ配信ネットワーク（CDN）** - サイトページ、アセット、CSS などを、設定したバックエンドデータセンターにキャッシュするワニスベースのサービス。 顧客がサイトにアクセスして保存すると、リクエストは Fastly にヒットし、キャッシュされたページをより迅速に読み込みます。 CDN サービスは次の機能を提供します。

- **キャッシュ管理** - サイトページ、アセット、CSS などを、帯域幅の負荷とコストを削減するために設定したバックエンドデータセンターにキャッシュします

   - [Fastly カスタム VCL スニペット ](fastly-vcl-custom-snippets.md) （Varnish 2.1 準拠）を使用して、キャッシュが要求にどのように応答するかを変更します

   - [GeoIP サービスサポート ](fastly-custom-cache-configuration.md#configure-geoip-handling) を設定します。

   - [暗号化されていない要求を TLS に強制的に送信する](fastly-custom-cache-configuration.md#force-tls)

   - [Fastly タイムアウトをカスタマイズ ](fastly-custom-cache-configuration.md#extend-fastly-timeout) 設定を使用して、一括操作要求に対する 503 応答を防ぐ

   - [ カスタムエラー応答ページ ](fastly-custom-response.md) の作成

- **セキュリティ** - Adobe Commerce サイトに対して Fastly サービスを有効にすると、サイトとネットワークを保護するためのその他のセキュリティ機能を使用できるようになります。

   - [Web Application Firewall](fastly-waf-service.md) （WAF） – PCI に準拠した保護機能を提供する Managed Web Application Firewall サービスで、クラウドインフラストラクチャサイトおよびネットワーク上の実稼動のAdobe Commerceに損害を与える前に、悪意のあるトラフィックをブロックします。 WAF サービスは、Pro および Starter 実稼動環境でのみ使用できます。

   - [Distributed Denial of Service （DDoS）保護 ](#ddos-protection):Ping of Death、Smurf 攻撃、その他の ICMP ベースのフラッド攻撃などの一般的な攻撃に対する組み込みの DDoS 保護。

   - [SSL/TLS 証明書 ](fastly-configuration.md#provision-ssltls-certificates) - Fastly サービスは、HTTPS 経由で安全なトラフィックを提供するために SSL/TLS 証明書を必要とします。

     Adobe Commerceは、ステージング環境と実稼動環境ごとに、ドメインで検証された Let&#39;s Encrypt SSL/TLS 証明書を提供します。 Adobe Commerceは、Fastly のセットアッププロセス中に、ドメインの検証と証明書のプロビジョニングを完了します。

- **オリジンクローキング** - トラフィックが Fastly WAFをバイパスするのを防ぎ、オリジンサーバーの IP アドレスを非表示にして、ダイレクトアクセスや DDoS 攻撃から保護します。

  Cloud infrastructure Pro 実稼働プロジェクトのAdobe Commerceでは、オリジンクロークがデフォルトで有効になっています。 クラウドインフラストラクチャーのスターター実稼動プロジェクトでAdobe Commerceのオリジンクロークを有効にするには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) を送信します。 キャッシュを必要としないトラフィックがある場合は、リクエストが [Fastly キャッシュをバイパス ](fastly-vcl-bypass-to-origin.md) できるように Fastly サービス設定をカスタマイズできます。

- **[画像の最適化](fastly-image-optimization.md)** – 画像処理やサイズ変更の負荷を Fastly サービスにオフロードし、サーバーが注文やコンバージョンをより効率的に処理できるようにします。

- **[Fastly CDN とWAF ログ](../monitor/new-relic-service.md#new-relic-log-management)** - Cloud Infrastructure Pro プロジェクトのAdobe Commerceの場合、New Relic ログサービスを使用して、Fastly CDN とWAFのログデータを確認および分析できます。

## Magento 2 用 Fastly CDN モジュール

クラウドインフラストラクチャー上のAdobe Commerce向け Fastly サービスでは、[Fastly CDN module for Magento 2] が Pro Staging and Production、Starter Production （`master` ブランチ）の環境にインストールされます。

Adobe Commerce プロジェクトの初期プロビジョニングまたはアップグレード時に、Adobeは最新バージョンの Fastly CDN モジュールをステージング環境および実稼動環境にインストールします。 Fastly がモジュールのアップデートをリリースすると、お使いの環境の管理者で通知が届きます。 Adobeでは、最新のリリースを使用するように環境を更新することをお勧めします。 [Fastly へのアップグレード ](fastly-configuration.md#upgrade-the-fastly-module) を参照してください。

## Fastly サービスアカウントと資格情報

クラウドインフラストラクチャプロジェクト上のAdobe Commerceには、専用の Fastly アカウントが付与されません。 Fastly サービスは、Cloud に登録された一元的なアカウントで管理され、管理ダッシュボードにはAdobe サポートチームからのみアクセスできます。

代わりに、各ステージング環境と実稼動環境には、Commerce Admin から Fastly サービスを設定および管理するための一意の Fastly 資格情報（API トークンとサービス ID）があります。 Fastly API は、Fastly サービスのアドバンス管理を実行するために使用できます。この場合、リクエストを送信するために資格情報が必要になります。

プロジェクトのプロビジョニング時に、Adobeはプロジェクトをクラウドインフラストラクチャ上のAdobe Commerce用の Fastly サービスアカウントに追加し、Fastly 資格情報をステージング環境と実稼動環境の設定に追加します。 [Fastly 資格情報の取得 ](fastly-configuration.md#get-fastly-credentials) を参照してください。

### Fastly API トークンの変更

Adobe Commerce サポートチケットを送信して、新しい Fastly API トークン資格情報 [ 検証に失敗した場合、有効期限が切れた場合 ](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials) または侵害されたと思われる場合）を発行します。

新しいトークンを受け取ったら、新しいトークンを使用するようにステージング環境または実稼動環境を更新します。

**Fastly API トークン資格情報を変更するには**:

1. [Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket)、新しい Fastly API 資格情報をリクエストします。

   クラウドインフラストラクチャプロジェクト ID 上のAdobe Commerceと、新しい資格情報を必要とする環境を含めます。

1. 新しい API トークンを受け取ったら、管理者の [Fastly 資格情報設定 ](fastly-configuration.md#test-the-fastly-credentials) または [[!DNL Cloud Console]  環境変数 ](../project/overview.md#configure-environment) から API トークンの値を更新します。

1. [ 新しい資格情報をテストします ](fastly-configuration.md#test-the-fastly-credentials)。

1. 資格情報を更新したら、古い API トークンを削除するためのAdobe Commerce サポートチケットを送信します。

### 複数の Fastly アカウントと割り当てられたドメイン

Fastly では、apex ドメインと関連するサブドメインを 1 つの Fastly サービスおよびアカウントに割り当てることができます。 Adobe Commerce サイトで使用されるのと同じ apex およびサブドメインをリンクする既存の Fastly アカウントがある場合は、次のオプションがあります。

- クラウドインフラストラクチャプロジェクト環境でAdobe Commerceの Fastly サービス資格情報をリクエストする前に、apex とサブドメインを既存のアカウントから削除します。 Fastly ドキュメントの [ ドメインの操作 ] を参照してください。

  apex ドメインとすべてのサブドメインを、クラウドインフラストラクチャー上のAdobe Commerceの Fastly サービスアカウントにリンクするには、このオプションを使用します。

- Adobe Commerce サポートチケットを送信して、apex とサブドメインを異なるアカウントにリンクできるように、ドメインのデリゲーションをリクエストします。

  Adobe Commerce サイトとAdobe Commerce以外のサイトに対して複数のサブドメインを持つ apex ドメインがあり、これらのサブドメインを異なる Fastly アカウントにリンクする場合は、このオプションを使用します。

#### ドメインのデリゲーションをリクエスト

*シナリオ 1:*

apex ドメイン（`testweb.com` と `www.testweb.com`）は、既存の Fastly アカウントにリンクされています。 Adobe Commerce on cloud infrastructure プロジェクトがサブドメイン `mcstaging.testweb.com` および `mcprod.testweb.com` で設定されている。 Apex ドメインを、クラウドインフラストラクチャ上のAdobe Commerceの Fastly サービスアカウントに移動することは望ましくありません。

[Fastly サポートチケット ] を送信し、サブドメインを既存の Fastly アカウントから、クラウドインフラストラクチャ上のAdobe Commerceの Fastly アカウントにデリゲートするようにリクエストします。 チケットにAdobe Commerce プロジェクト ID を含めます。

デリゲーションが完了したら、クラウドインフラストラクチャー上のAdobe Commerce用 Fastly サービスアカウントにプロジェクトのサブドメインを追加できます。 [Fastly 資格情報の取得 ](fastly-configuration.md#get-fastly-credentials) を参照してください。

*シナリオ 2:*

apex ドメイン（`testweb.com` と `www.testweb.com`）は、クラウドインフラストラクチャ上のAdobe Commerce Fastly サービスアカウントにリンクされています。 `service.testweb.com` および `product-updates.testweb.com` サブドメインの Fastly サービスを別の Fastly アカウントから管理したい場合。

サブドメインをAdobe Commerce on cloud infrastructure Fastly サービスアカウントから Fastly アカウントにデリゲートするように求めるAdobe Commerce サポートチケットを送信します。 Fastly アカウントのサービス ID をチケットに含めます。

## DDoS 保護

DDOS 保護は、Fastly CDN サービスに組み込まれています。 Adobe Commerce サイトに対して Fastly サービスを有効にすると、Fastly はすべての web および管理者トラフィックをフィルタリングし、潜在的な攻撃を検出してブロックします。

- レイヤー 3 または 4 をターゲットとする攻撃の場合、Fastly サービスは、ポートとプロトコルに基づいてトラフィックをフィルターで除外し、HTTP または HTTPS リクエストのみを調べます。 ICMP、UDP、およびその他のネットワークによる攻撃は、ネットワーク エッジで廃棄されます。 これには、SSDP や NTP などの UDP サービスを使用する反射および増幅攻撃が含まれます。 このレベルの保護を提供することにより、Ping of Death、Smurf 攻撃、その他の ICMP ベースのフラッドなど、複数の一般的な攻撃を効果的にブロックします。

  Fastly は、キャッシュレイヤーで TCP レベルの攻撃を管理します。 この戦略は、SYN フラッド攻撃とその多くのバリアント（TCP スタック、リソース攻撃、Fastly システム内の TLS 攻撃など）に対処するために、クライアントごとに必要な規模とコンテキストを提供します。

- また、Fastly はレイヤー 7 攻撃に対する保護も提供します。 ストアでパフォーマンスの問題が発生しており、レイヤー 7 DDoS 攻撃が疑われる場合は、Adobe Commerce サポートチケットを送信します。 Adobeでは、カスタムルールを作成して Fastly サービスに適用し、攻撃トラフィックを特定するヘッダー、ペイロード、または属性の組み合わせに基づいて、悪意のあるリクエストを調べて除外できます。 [4&rbrace;Adobe Commerce ヘルプセンター ] の [DDoS 攻撃の確認 ] および *悪意のあるトラフィックをブロックする方法 &rbrace; を参照してください。*

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[DDoS 攻撃の確認]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html?lang=ja

[Magento 2 用 Fastly CDN モジュール]: https://github.com/fastly/fastly-magento2

[Fastly サポートチケット]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[悪意のあるトラフィックをブロックする方法]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html?lang=ja

[ドメインの操作]: https://docs.fastly.com/en/guides/working-with-domains
