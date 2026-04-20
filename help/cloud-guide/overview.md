---
title: Commerce on Cloud Infrastructure
description: クラウドインフラストラクチャー上で Commerce を構築、デプロイ、管理する方法を学びます。
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
source-git-commit: 7bbda734fbaccca9366c11b03e38b30513908cfe
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 3%

---


# Commerce on Cloud Infrastructure

Adobe Commerce on cloud infrastructureは、クラウドネイティブ環境で[!DNL Commerce] アプリケーションを構築、デプロイ、管理するための&#x200B;**セルフサービス** アプローチを備えた自動ホスティングプラットフォームです。 Adobe Commerce on cloud infrastructureには、オンプレミスのAdobe CommerceおよびMagento Open Source プラットフォームとは一線を画す追加機能が搭載されています。

- PHP、MySQL （MariaDB）、Redis、メッセージキューサービス（[!DNL RabbitMQ]または[!DNL ActiveMQ]）、サポートされる検索エンジンテクノロジーを含む、事前プロビジョニング済みのインフラストラクチャ。
- Platform as a Service （PaaS）環境でコード変更をプッシュするたびに、効率的な迅速な開発と継続的なデプロイメントを実現する自動ビルドとデプロイを備えたGit ベースのワークフロー。
- 高度にカスタマイズ可能な環境設定ファイルとコマンドラインインターフェイス（CLI）ツールの管理とデプロイ。
- Amazon Web Services（AWS）ホスティングは、オンライン販売と小売業のための拡張性と安全性の高い環境を提供します。

![ クラウドのメリット ](../assets/CloudBenefits.svg)

>[!NOTE]
>
>セキュリティについて詳しくは、[ セキュリティ起動チェックリスト ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration)を参照してください。

[ テクノロジースタック ](architecture/tech-stack.md)の詳細を確認するか、[Commerceのクラウドアーキテクチャ ](architecture/cloud-architecture.md)の特定の機能とサポート対象の製品について詳しく確認します。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## クラウド地域

以下の節では、Adobe Commerce on cloud infrastructureで使用できるさまざまなAWSおよびAzure リージョンについて詳しく説明します。

## AWS地域

![AWS地域を示す図](../assets/aws-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。

## Azure地域

![Azure地域を示す図](../assets/azure-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 中国とロシアのオンプレミスのみ。 統合環境を必要とするすべてのマーチャントは、米国の地域を使用する必要があります。

## Adobe Commerce ドキュメント

Commerce on cloud インフラストラクチャガイドでは、Adobe Commerce アプリケーションに関する実務的な知識と理解を有していることを前提としています。 以下の[!DNL Commerce]開発者およびユーザーガイドを参照してください。

- [Adobe Commerce開発者向けドキュメント ](https://developer.adobe.com/commerce/docs/) （Adobe Developer サイト）：高度な機能の開発、カスタマイズ、統合、拡張、使用

- [Adobe Commerce ドキュメント ](https://experienceleague.adobe.com/docs/commerce.html) （Adobe Experience League） - [!DNL Commerce] プロジェクトの計画、実装、操作、アップグレード、保守

{{$include /help/_includes/templated/whats-new.md}}


<!-- Last updated from includes: 2026-04-19 20:38:08 -->
