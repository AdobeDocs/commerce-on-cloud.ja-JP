---
title: Web Application Firewall （WAF）
description: Fastly WAFサービスが、Adobe Commerceのネットワークやサイトに損害を与える前に、悪意のあるリクエストトラフィックを検出、ログに記録、ブロックする方法をご確認ください。
feature: Cloud, Configuration, Security
exl-id: f00e35f2-9800-4e24-a4d0-d36fde59a003
TQID: https://experienceleague.adobe.com/GhpLOxZbJMYhBTmj8W4a90wfmFYq-8h2bvrKQWj6ZWk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: bd989d82-1e15-4534-88db-f1f51dd77ffaid: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7
subfeature_v2: id: f2261633-201d-46c5-8a66-999e70527a83
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 987
ht-degree: 0%

---

# Web Application Firewall （WAF）

Fastlyを活用したweb アプリケーションファイアウォール（WAF）は、Adobe Commerceクラウドインフラストラクチャ向けのサービスで、サイトやネットワークに損害を与える前に、悪意のあるリクエストトラフィックを検出、記録、ブロックします。 WAF サービスは、実稼動環境でのみ使用できます。

WAF サービスには、次の利点があります。

- **PCI コンプライアンス**:WAF イネーブルメントにより、本番環境のAdobe Commerce ストアフロントがPCI DSS 6.6のセキュリティ要件を満たすことができます。
- **デフォルトのWAF ポリシー** - Fastlyが設定および管理するデフォルトのWAF ポリシーは、インジェクション攻撃、悪意のある入力、クロスサイトスクリプティング、データ流出、HTTP プロトコル違反、その他[OWASP トップ 10](https://owasp.org/www-project-top-ten/)のセキュリティ脅威など、幅広い攻撃からAdobe Commerce web アプリケーションを保護するためにカスタマイズされたセキュリティルールのコレクションを提供します。
- **WAFのオンボーディングとイネーブルメント**:Adobeは、プロビジョニングが完了してから2～3週間以内に実稼動環境にデフォルトのWAF ポリシーをデプロイして有効にします。
- **運用および保守サポート**—
   - AdobeとFastlyは、WAFサービスのログ、ルール、アラートを設定および管理します。
   - Adobeは、正当なトラフィックをブロックするWAF サービスの問題に関連するカスタマーサポートチケットを優先度1の問題としてトリガーします。
   - WAFのサービス版に自動アップグレードを適用することで、新たな脆弱性や進化する脆弱性に迅速に対応できます。 [WAFのメンテナンスとアップグレード ](#waf-maintenance-and-updates)を参照してください。

>[!TIP]
>
>Adobe Commerce on cloud infrastructure ストアのPCI コンプライアンスの維持について詳しくは、[PCI コンプライアンス ](https://business.adobe.com/products/magento/pci-compliance.html)を参照してください。

## WAFの有効化

Adobeを使用すると、プロビジョニングが完了してから2～3週間以内に、新しいアカウントでWAF サービスを有効にできます。 WAFは、Fastly CDN サービスを通じて実装されます。 ハードウェアやソフトウェアをインストールしたり保守したりする必要はありません。

>[!NOTE]
>
>WAF サービスを使用する前に、Adobe Commerce on cloud infrastructure プロジェクトへのすべての外部トラフィックをFastly サービス経由でルーティングする必要があります。 [Fastlyの設定](fastly-configuration.md)を参照してください。

## 仕組み

WAF サービスはFastlyと統合し、Fastly CDN サービス内のキャッシュロジックを使用して、Fastly グローバルノードでトラフィックをフィルタリングします。 実稼動環境でWAF サービスを有効にするには、Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity)の[ModSecurity RulesとOWASPの上位10個のセキュリティ脅威に基づくデフォルトのWAF ポリシーを使用します。

WAF サービスは、WAF ルールセットに対してHTTPおよびHTTPS トラフィック（GET リクエストおよびPOST リクエスト）を検査し、悪意のあるトラフィックや特定のルールに準拠しないトラフィックをブロックします。 このサービスは、キャッシュを更新しようとするオリジンバウンドのトラフィックのみを検査します。 その結果、Fastly キャッシュでほとんどの攻撃トラフィックを停止し、オリジントラフィックを悪意のある攻撃から保護します。 オリジンのトラフィックのみを処理することで、WAF サービスはキャッシュのパフォーマンスを保持し、キャッシュされていないリクエストごとに推定1.5 ミリ秒から20 ミリ秒の遅延が発生するだけです。

## ブロックされたリクエストのトラブルシューティング

WAF サービスが有効になっている場合、すべてのweb トラフィックと管理者トラフィックがWAF ルールに照らし合わせて調べられ、ルールをトリガーするすべてのweb リクエストがブロックされます。 リクエストがブロックされると、ブロック イベントの参照IDを含むデフォルトの`403 Forbidden` エラーページが依頼者に表示されます。

![WAF エラーページ ](../../assets/cdn/fastly-waf-403-error.png)

このエラー応答ページは、管理者からカスタマイズできます。 [WAFの回答ページのカスタマイズ ](fastly-custom-response.md#customize-the-waf-error-page)を参照してください。

Adobe Commerce管理ページまたはストアフロントで、正当なURL リクエストに応じて`403 Forbidden` エラーページが返された場合は、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)を送信します。 エラー応答ページから参照IDをコピーし、チケットの説明に貼り付けます。

New Relicを使用した特定のリクエストに対するWAFのレスポンスを特定するには、次を参照してください。

- `Agent_response` - WAF応答コードを示します（`200`は良好を意味し、`406`はブロックを意味します）
- `sigsci` タグ – リクエストの性質に基づいて、リクエストを特定のsignal sciences タグにタグ付けします

## WAFのメンテナンスとアップデート

Fastlyは、商用のサードパーティ、Fastlyの調査、オープンソースからのルール更新に基づいて、新しいCVEやテンプレート化されたルールに対するパッチを更新し、展開します。 公開されたルールは、必要に応じてポリシーに更新されるか、ルールの変更がそれぞれのソースから利用可能になったときに更新されます。 また、Fastlyは、WAF サービスが有効になった後、公開されたルールクラスに一致するルールを、任意のサービスのWAF インスタンスに追加できます。 これらのアップデートにより、新しいエクスプロイトや進化するエクスプロイトを即座にカバーできます。

AdobeとFastlyで更新プロセスを管理することで、更新がブロッキングモードでデプロイされる前に、新しいルールまたは変更されたWAFルールが本番環境で効果的に機能することを確認できます。

## 問題

WAFが正当なリクエストをブロックしていることに気づいた場合、これらは多くの場合、誤検出となり、WAF サービスで回避するか、回避策を実装する必要があります。 サポートチケットを送信し、影響を受けるURL、エラーを再現するための正確な手順、および文字起こしエラーを回避するための（スクリーンショットではなく）テキスト形式でのエラー参照を含めます。

## 制限

Fastlyを搭載した標準のWAF サービスは、次の機能をサポートしていません。

- マルウェアまたはボットの緩和に対する保護 – [ アクセス制御リスト ](./fastly-vcl-allowlist.md)またはサードパーティのサービスの使用を検討してください。
- レート制限 – Fastly ドキュメントの[ レート制限](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)を参照するか、_Commerce Web API_ セキュリティセクションの[ レート制限](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/)を参照してください。
- お客様のロギングエンドポイントの設定 – 代替手段として[PrivateLink サービス ](../development/privatelink-service.md)を参照してください。

WAF サービスでは、IP アドレスに基づいてトラフィックをブロックまたは許可できます。 Fastly サービスにアクセス制御リスト（ACL）とカスタム VCL スニペットを追加して、トラフィックをブロックまたは許可するためのIP アドレスとVCL ロジックを指定できます。 [ カスタム Fastly VCL スニペット ](fastly-vcl-custom-snippets.md)を参照してください。

TCP、UDP、またはICMP リクエストのフィルタリングは、WAF サービスではサポートされていません。 ただし、この機能は、Fastly CDN サービスに含まれている組み込みのDDoS保護機能によって提供されます。 [DDoS保護対策](fastly.md#ddos-protection)を参照してください。

