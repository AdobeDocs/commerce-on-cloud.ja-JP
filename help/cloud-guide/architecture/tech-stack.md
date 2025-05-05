---
title: 技術スタック
description: クラウドインフラストラクチャー上のCommerceを構成するテクノロジースタックを参照してください。
feature: Cloud, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# 技術スタック

次に示すように、クラウドインフラストラクチャー上のAdobe Commerceは 5 つの機能レイヤーと考えることができます。

![ クラウドスタック ](../../assets/CloudStack.svg)

1. [**クラウドインフラストラクチャー**](pro-architecture.md)：クラウドインフラストラクチャー上のAdobe Commerce Pro プロジェクトのインフラストラクチャ as a Service （IaaS）基盤として、Amazon Web Services（AWS）またはMicrosoft Azure を選択します。

   Adobeでは、仮想コンピューティングリソース（vCPU）の使用状況を定期的に分析し、リソースを自動的に割り当てて、長期使用を最適化し、vCPU の 1 日の最大許容量を超えるリスクを軽減します。 特定の期間にサイトトラフィックの増加が予想される場合は、引き続きサポートチケットを開いて [ 一時的なアップサイズをリクエスト ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=ja) する必要があります。

1. [**サービスとしてのプラットフォーム**](cloud-architecture.md)：クラウドインフラストラクチャプロジェクト上の各Adobe Commerceは、サービスを開発、テスト、統合するためのサービスとしてのプラットフォーム（PaaS）統合環境を提供します。
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce on cloud infrastructure は、PHP、MySQL （MariaDB）、Redis、[!DNL RabbitMQ]、およびサポートされている検索エンジン技術を含む、プロビジョニング済みのインフラストラクチャを提供します。
1. [**パフォーマンスツール**](../monitor/new-relic-service.md): New Relic パフォーマンスツールを使用すると、クラウドインフラストラクチャプロジェクト上でAdobe Commerceからデータを収集、分析、表示することにより、アプリケーションとインフラストラクチャをデバッグ、モニタリング、管理できます。
1. [**コンテンツ配信ネットワーク（CDN）、web アプリケーションファイアウォール（[!DNL WAF]）、画像最適化（IO）**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection):[!DNL Ping of Death]、[!DNL Smurf] 攻撃、その他の Internet Control Message Protocol （ICMP; インターネット制御メッセージ プロトコル）ベースのフラッド攻撃などの Distributed Denial of Service （DDoS；分散型サービス拒否）攻撃からの保護機能を組み込み、安全な CDN サービスを提供します。
   * [Web Application Firewall （WAF） ](../cdn/fastly-waf-service.md)—WAF サービスは、実稼動環境のAdobe Commerce ストアフロントおよびWAF ポリシーの PCI コンプライアンスを確保します。これにより、Adobe Commerce web アプリケーションをインジェクション攻撃、悪意のある入力、クロスサイトスクリプティング、データ抽出、HTTP プロトコル違反などの [[!DNL OWASP]  上位 10 件のセキュリティ上の脅威 ](https://owasp.org/www-project-top-ten/) から保護します。
   * [ 画像最適化（IO） ](../cdn/fastly-image-optimization.md) – 画像のリアルタイムな操作と最適化を実現し、迅速な画像配信と、レスポンシブ Web アプリケーション向けの画像ソースセットのメンテナンスを簡素化します。 Fastly IO は、画像処理やサイズ変更の負荷を軽減し、サーバーを解放して注文やコンバージョンを効率的に処理します。

モノリシックアプリケーションはリソースを大量に消費するので、拡張や迅速な提供が困難です。 クラウドインフラストラクチャを使用すると、Commerceのお客様は、豊富でインテリジェント、かつ高性能な SaaS ベースのマイクロサービスに比類のないアクセスができます。 [ サポートされるソフトウェアとサービス ](cloud-architecture.md#supported-software-and-services) を参照してください。

[Commerce入門ガイド ](../../get-started/overview.md) を使用して、新しいクラウドプログラムを設定し、クラウドネイティブな環境で [!DNL Commerce] アプリケーションを管理し始めます。
