---
title: テクノロジースタック
description: Commerce on Cloud インフラストラクチャを形成するテクノロジースタックをご覧ください。
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
TQID: https://experienceleague.adobe.com/2-uZdx1Oi-3LQUK-L7rC4kZWYcYobEhnVcUDNwOHpFs
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: c1256247-af4b-46d8-9dca-0c654ecfa157id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: df5e974b-6742-4873-a687-a6bedaafdaa2id: f2261633-201d-46c5-8a66-999e70527a83
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 405
ht-degree: 0%

---

# テクノロジースタック

Adobe Commerce on cloud インフラストラクチャは、次に示す5つの機能レイヤーと考えてください。

![ クラウドスタック ](../../assets/CloudStack.png)

1. [**Cloud Infrastructure**](pro-architecture.md): Adobe Commerce on Cloud Infrastructure Pro プロジェクトのInfrastructure as a Service （IaaS）基盤として、Amazon Web Services（AWS）またはMicrosoft Azureのいずれかを選択します。

   Adobeでは、バーチャルコンピューティングリソース（vCPU）の使用状況を定期的に分析し、長期的な使用状況を最適化し、vCPUの年間最大許容値を超えるリスクを軽減するために、リソースを自動的に割り当てます。 特定の期間にサイトトラフィックの増加が予想される場合は、引き続きサポートチケットを開いて[一時的なアップサイズをリクエスト ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html)する必要があります。

1. [**Platform as a Service**](cloud-architecture.md)：各Adobe Commerce クラウド インフラストラクチャ プロジェクトでは、サービスの開発、テスト、および統合用のPlatform as a Service （PaaS）統合環境が提供されます。
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce on cloud インフラストラクチャは、PHP、MySQL （MariaDB）、Redis、メッセージキューサービス（[!DNL RabbitMQ]または[!DNL ActiveMQ]）、サポートされている検索エンジンテクノロジーを含む、事前プロビジョニング済みのインフラストラクチャを提供します。
1. [**パフォーマンスツール**](../monitor/new-relic-service.md): New Relicのパフォーマンスツールを使用すると、クラウドインフラストラクチャプロジェクト上のAdobe Commerceからデータを収集、分析、表示することで、アプリケーションとインフラストラクチャをデバッグ、モニター、管理できます。
1. [**コンテンツ配信ネットワーク （CDN）、Web アプリケーションファイアウォール （[!DNL WAF]）、および画像最適化（IO）**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) - [!DNL Ping of Death]攻撃、[!DNL Smurf]攻撃、その他のInternet Control Message Protocol （ICMP）ベースのフラッド攻撃などの分散型サービス拒否（DDoS）攻撃からの組み込み保護を備えた安全なCDN サービスを提供します。
   * [Web Application Firewall （WAF） ](../cdn/fastly-waf-service.md)- WAF サービスは、Adobe CommerceのストアフロントのPCI コンプライアンスを本番環境およびWAF ポリシーで確保し、Adobe Commerce Web アプリケーションをインジェクション攻撃、悪意のある入力、クロスサイトスクリプティング、データ流出、HTTP プロトコル違反、その他[[!DNL OWASP]  セキュリティ上の脅威](https://owasp.org/www-project-top-ten/)から保護します。
   * [画像最適化（IO） ](../cdn/fastly-image-optimization.md) - リアルタイムの画像操作と最適化を提供して、画像配信を高速化し、レスポンシブ web アプリケーションの画像ソースセットのメンテナンスを簡素化します。 FastlyのIOは、画像処理やサイズ変更の負荷を軽減し、サーバーを解放して注文やコンバージョンを効率的に処理します。

モノリシックアプリケーションはリソース集約的で、迅速な拡張や提供が困難です。 クラウドのインフラストラクチャにより、Commerceのお客様は、比類のない、充実したパフォーマンスと、インテリジェントなSaaS ベースのマイクロサービスを利用できるようになります。 [ サポートされているソフトウェアとサービス ](cloud-architecture.md#supported-software-and-services)を参照してください。

[Commerce入門ガイド ](../../get-started/overview.md)を使用して、新しいCloud プログラムを設定し、クラウドネイティブ環境で[!DNL Commerce] アプリケーションの管理を開始します。

