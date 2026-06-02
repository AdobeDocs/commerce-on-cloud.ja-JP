---
title: Commerce on Cloud Infrastructure
description: クラウドインフラストラクチャー上で Commerce を構築、デプロイ、管理する方法を学びます。
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
TQID: https://experienceleague.adobe.com/-sgz85xapPKNipyFVB4yMrLilEku3ff5IJg3OddymsA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: efe0f95c16d9aab9af6a9c263034b08dce4d7c93
workflow-type: tm+mt
source-wordcount: 323
ht-degree: 3%

---

# Commerce on Cloud Infrastructure

Adobe Commerce on cloud infrastructureは、クラウドネイティブ環境で[!DNL Commerce] アプリケーションを構築、デプロイ、管理するための&#x200B;**セルフサービス** アプローチを備えた自動ホスティングプラットフォームです。 Adobe Commerce on cloud infrastructureには、オンプレミスのAdobe CommerceおよびMagento Open Source プラットフォームとは一線を画す追加機能が搭載されています。

- PHP、MySQL （MariaDB）、Redis、メッセージキューサービス（[!DNL RabbitMQ]または[!DNL ActiveMQ]）、サポートされる検索エンジンテクノロジーを含む、事前プロビジョニング済みのインフラストラクチャ。
- Platform as a Service （PaaS）環境でコード変更をプッシュするたびに、効率的な迅速な開発と継続的なデプロイメントを実現する自動ビルドとデプロイを備えたGit ベースのワークフロー。
- 高度にカスタマイズ可能な環境設定ファイルとコマンドラインインターフェイス（CLI）ツールの管理とデプロイ。
- Amazon Web Services（AWS）ホスティングは、オンライン販売と小売業のための拡張性と安全性の高い環境を提供します。

![&#x200B; クラウドのメリット &#x200B;](../assets/CloudBenefits.svg)

>[!NOTE]
>
>セキュリティについて詳しくは、[&#x200B; セキュリティ起動チェックリスト &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration)を参照してください。

[&#x200B; テクノロジースタック &#x200B;](architecture/tech-stack.md)の詳細を確認するか、[Commerceのクラウドアーキテクチャ &#x200B;](architecture/cloud-architecture.md)の特定の機能とサポート対象の製品について詳しく確認します。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## クラウド地域

以下の節では、Adobe Commerce on cloud infrastructureで使用できるさまざまなAWSおよびAzure リージョンについて詳しく説明します。

## AWS地域

![AWS地域を示す図](../assets/aws-regions.png){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。

## Azure地域

![Azure地域を示す図](../assets/azure-regions.png){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。 統合環境を必要とするすべてのマーチャントは、米国の地域を使用する必要があります。

## Adobe Commerce ドキュメント

Commerce on cloud インフラストラクチャガイドでは、Adobe Commerce アプリケーションに関する実務的な知識と理解を有していることを前提としています。 以下の[!DNL Commerce]開発者およびユーザーガイドを参照してください。

- [Adobe Commerce開発者向けドキュメント &#x200B;](https://developer.adobe.com/commerce/docs/) （Adobe Developer サイト）：高度な機能の開発、カスタマイズ、統合、拡張、使用

- [Adobe Commerce ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/commerce.html?lang=ja) （Adobe Experience League） - [!DNL Commerce] プロジェクトの計画、実装、操作、アップグレード、保守

{{$include /help/_includes/templated/whats-new.md}}

<!-- Last updated from includes: 2026-05-13 23:38:24 -->
