---
title: テクノロジースタック
description: Commerce on Cloud インフラストラクチャを形成するテクノロジースタックをご覧ください。
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
source-git-commit: 77d316fd53e477a2b45277db503ea8e5ede78930
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# テクノロジースタック

Adobe Commerce on cloud インフラストラクチャは、次に示す5つの機能レイヤーと考えてください。

![&#x200B; クラウドスタック &#x200B;](../../assets/CloudStack.png)

1. [**Cloud Infrastructure**](pro-architecture.md): Adobe Commerce on Cloud Infrastructure Pro プロジェクトのInfrastructure as a Service （IaaS）基盤として、Amazon Web Services（AWS）またはMicrosoft Azureのいずれかを選択します。

   Adobeでは、バーチャルコンピューティングリソース（vCPU）の使用状況を定期的に分析し、長期的な使用状況を最適化し、vCPUの年間最大許容値を超えるリスクを軽減するために、リソースを自動的に割り当てます。 特定の期間にサイトトラフィックの増加が予想される場合は、引き続きサポートチケットを開いて[一時的なアップサイズをリクエスト &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=ja)する必要があります。

1. [**Platform as a Service**](cloud-architecture.md)：各Adobe Commerce クラウド インフラストラクチャ プロジェクトでは、サービスの開発、テスト、および統合用のPlatform as a Service （PaaS）統合環境が提供されます。
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce on cloud インフラストラクチャは、PHP、MySQL （MariaDB）、Redis、メッセージキューサービス（[!DNL RabbitMQ]または[!DNL ActiveMQ]）、サポートされている検索エンジンテクノロジーを含む、事前プロビジョニング済みのインフラストラクチャを提供します。
1. [**パフォーマンスツール**](../monitor/new-relic-service.md): New Relicのパフォーマンスツールを使用すると、クラウドインフラストラクチャプロジェクト上のAdobe Commerceからデータを収集、分析、表示することで、アプリケーションとインフラストラクチャをデバッグ、モニター、管理できます。
1. [**コンテンツ配信ネットワーク （CDN）、Web アプリケーションファイアウォール （[!DNL WAF]）、および画像最適化（IO）**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) - [!DNL Ping of Death]攻撃、[!DNL Smurf]攻撃、その他のInternet Control Message Protocol （ICMP）ベースのフラッド攻撃などの分散型サービス拒否（DDoS）攻撃からの組み込み保護を備えた安全なCDN サービスを提供します。
   * [Web Application Firewall （WAF） &#x200B;](../cdn/fastly-waf-service.md)- WAF サービスは、Adobe CommerceのストアフロントのPCI コンプライアンスを本番環境およびWAF ポリシーで確保し、Adobe Commerce Web アプリケーションをインジェクション攻撃、悪意のある入力、クロスサイトスクリプティング、データ流出、HTTP プロトコル違反、その他[[!DNL OWASP]  セキュリティ上の脅威](https://owasp.org/www-project-top-ten/)から保護します。
   * [画像最適化（IO） &#x200B;](../cdn/fastly-image-optimization.md) - リアルタイムの画像操作と最適化を提供して、画像配信を高速化し、レスポンシブ web アプリケーションの画像ソースセットのメンテナンスを簡素化します。 FastlyのIOは、画像処理やサイズ変更の負荷を軽減し、サーバーを解放して注文やコンバージョンを効率的に処理します。

モノリシックアプリケーションはリソース集約的で、迅速な拡張や提供が困難です。 クラウドのインフラストラクチャにより、Commerceのお客様は、比類のない、充実したパフォーマンスと、インテリジェントなSaaS ベースのマイクロサービスを利用できるようになります。 [&#x200B; サポートされているソフトウェアとサービス &#x200B;](cloud-architecture.md#supported-software-and-services)を参照してください。

[Commerce入門ガイド &#x200B;](../../get-started/overview.md)を使用して、新しいCloud プログラムを設定し、クラウドネイティブ環境で[!DNL Commerce] アプリケーションの管理を開始します。
