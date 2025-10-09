---
title: クラウドインフラストラクチャー上のCommerce
description: クラウドインフラストラクチャー上で Commerce を構築、デプロイ、管理する方法を学びます。
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
source-git-commit: 11d0e86ad4633d9c4232474b9921ed491f285969
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 3%

---


# クラウドインフラストラクチャー上のCommerce

クラウドインフラストラクチャー上のAdobe Commerceは、クラウドネイティブ環境で **アプリケーションを構築、デプロイ、管理するための** セルフサービス [!DNL Commerce] アプローチを備えた自動ホスティングプラットフォームを提供します。 Adobe Commerce on cloud infrastructure には、オンプレミスのAdobe CommerceおよびMagento Open Source プラットフォームとは異なる次の追加機能が付属しています。

- PHP、MySQL （MariaDB）、Redis、[!DNL RabbitMQ]、およびサポートされている検索エンジンテクノロジーを含む、プロビジョニング済みのインフラストラクチャ。
- 自動ビルドおよび自動デプロイを使用した Git ベースのワークフロー Platform as a Service （PaaS）環境でコードの変更をプッシュするたびに、効率的な迅速な開発と継続的なデプロイメントを実現します。
- 高度にカスタマイズ可能な環境設定ファイルとコマンドラインインターフェイス（CLI）により、ツールを管理および導入できます。
- Amazon Web Services（AWS）ホスティングは、オンラインセールスおよび小売業にスケーラブルで安全な環境を提供します。

![&#x200B; クラウドのメリット &#x200B;](../assets/CloudBenefits.svg)

>[!NOTE]
>
>セキュリティについて詳しくは、[&#x200B; セキュリティのローンチチェックリスト &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration) を参照してください。

[Commerceのクラウドアーキテクチャ &#x200B;](architecture/tech-stack.md) で、[&#x200B; テクノロジースタック &#x200B;](architecture/cloud-architecture.md) を詳しく表示するか、特定の機能とサポートされる製品について詳しく説明します。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## クラウド地域

以下の節では、クラウドインフラストラクチャー上のAdobe Commerceで使用できる様々なAWSおよび Azure リージョンについて詳しく説明します。

## AWS地域

![AWS地域を示す図 &#x200B;](../assets/aws-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。

## Azure 地域

![Azure の地域を示す図 &#x200B;](../assets/azure-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。 統合環境を必要とするすべてのマーチャントは、米国の地域を使用する必要があります。

## Adobe Commerce ドキュメント

このクラウドインフラストラクチャー上のCommerce ガイドは、Adobe Commerce アプリケーションに関する十分な知識と理解があることを前提としています。 以下の [!DNL Commerce] 開発者およびユーザーガイドを参照してください。

- [Adobe Commerce開発者向けドキュメント &#x200B;](https://developer.adobe.com/commerce/docs/) （Adobe Developer サイト） – 高度な機能を開発、カスタマイズ、統合、拡張、使用します

- [Adobe Commerce ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/commerce.html) （Adobe Experience League） – [!DNL Commerce] プロジェクトの計画、実装、運用、アップグレード、保守

{{$include /help/_includes/templated/whats-new.md}}

<!-- Last updated from includes: 2025-10-08 16:18:12 -->
