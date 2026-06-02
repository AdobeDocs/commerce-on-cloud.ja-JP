---
title: Commerce on Cloudのプロビジョニング
description: Adobe Commerce on Cloud Infrastructure プロジェクトをプロビジョニングするためのAdobe カスタマーテクニカルアドバイザーの準備方法について説明します。
recommendations: noDisplay, catalog
role: Admin
exl-id: 77e8c9fb-8c4a-4c98-adbc-e57871c5bdbc
TQID: https://experienceleague.adobe.com/GzCPqYxn0-ACS34UfvHypqv6BveTRpbLJNN2jNmCL-E
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: bd989d82-1e15-4534-88db-f1f51dd77ffaid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: cc72dcf1-72e1-48cc-b434-e7c27d62d67c
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 774
ht-degree: 0%

---

# Commerce on Cloud プロビジョニングの前提条件

クラウドインフラストラクチャでのCommerce プロジェクトの開始と初期化を行います。

AdobeでCommerceをクラウドプロジェクト環境にプロビジョニングする前に、次の手順を検討し、最初にAdobe アカウントチームと相談するための回答を準備することをお勧めします。 次のセクションをチェックリストとして使用すると、お客様のテクニカルアドバイザーとの会話に備えてクラウドプロジェクトをプロビジョニングできます。

## ドメイン定義

**質問1**: _サイトの起動に使用するドメインまたはドメインを選択してください。_

Pro ステージング環境と実稼動環境のトップレベルドメインとサブドメインを定義することで、Fastlyとnginx サービスの統合を準備します。 初期設定の後、Adobe Commerce サポートチケットを送信してドメインを追加できるため、ドメイン情報の準備を整えることをお勧めします。

実稼動ドメインとステージングドメインの例：

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

複数または一意のドメインに関する詳細なガイダンスについては、_Commerce on Cloud Infrastructure_ ガイドの[複数のweb サイトまたはストアの設定](../cloud-guide/store/multiple-sites.md)を参照してください。

Adobe Commerce サイトで使用されている同じapexとサブドメインをリンクする既存のFastly アカウントがある場合は、[複数のFastly アカウントと割り当て済みドメイン ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"}を参照してください。

## トランザクションメールドメイン

**質問2**: _トランザクションメールに使用するドメインまたはドメインはどれですか？_

Adobe Commerce on Cloudでは、送信メール認証およびレピュテーション監視サービスを提供するSendGrid Simple Mail Transfer Protocol （SMTP）プロキシサービスを使用しています。 SendGridはあなたの代わりにトランザクションメールを送信するので、ドメイン情報が必要です。

SendGrid ドメインの例：`example@your-store.com`

トランザクションメールとドメイン設定に関する詳細なガイダンスについては、_Commerce on Cloud Infrastructure_ ガイドの[SendGrid メールサービス ](../cloud-guide/project/sendgrid.md)を参照してください。

## ストレージ配分

**質問3**: _契約したストレージのうち、ファイルのアップロードとデータベースに割り当てる予定の容量は？_

Adobe Commerce on cloud infrastructureでは、ファイル構造、検索インデックス、データベースにストレージが使用されます。 必要に応じて、各パーティションに対して10 GBから始めてストレージを再分割できます。

クラウドプロジェクト用に契約したストレージの量は、各環境の合計ストレージを表します。 例えば、50 GBのストレージ容量を購入した場合、各環境に50 GBのストレージが割り当てられます。 実稼動環境、ステージング環境、および各統合環境には、それぞれ50 GBの個別のストレージがあります。

契約ストレージはいつでも増やすことができます。 Pro実稼動環境およびステージング環境の場合、ディスク領域の割り当てを変更するには、Adobe Commerce サポートチケットを送信する必要があります。 Proの実稼動環境とステージング環境のサイズの増加は、特定の間隔でのみ発生する可能性があります。 現在のディスク容量の使用状況に応じて、サポートチームはディスク容量の割り当てを少なくとも10 GB増やすことを推奨する場合があります。 割り当てが完了すると、Pro ステージングおよび実稼動環境のストレージの増加を&#x200B;**not**&#x200B;に戻すことができます。

_Commerce on Cloud Infrastructure_ ガイドの「[ ディスク領域の管理](../cloud-guide/storage/manage-disk-space.md)」を参照してください。

## クラウドサービスリージョン

**質問4**: _お近くにお住まいの方が最も便利なクラウドサービスの地域はどれですか？_

Amazon Web Services（AWS）またはMicrosoft Azureを、Adobe Commerce on cloud infrastructure Pro プロジェクトのInfrastructure as a Service （IaaS）基盤として選択します。 各サービスプロバイダーは、複数の地域で事業を展開し、複数の可用性ゾーンを提供しています。 現在地に便利な地域を選択し、待ち時間が発生する可能性を減らし、コストを増大させます。

Adobe Commerce クラウドリージョンのマップ [を参照](../cloud-guide/overview.md)。

## 接続サービス

**質問5**: _PrivateLink サービスが必要ですか？ その場合、PrivateLink接続はどの地域にありますか？_

Adobe Commerce クラウドインフラストラクチャでは、AWS PrivateLinkまたはAzure Private Link サービスとの統合をサポートしています。 このサービスはオプションですが、PrivateLinkは、外部システムでホストされているサービスとアプリケーションとのクラウドインフラストラクチャ環境間の安全でプライベートな通信を確立するために使用されます。

Adobe Commerce インスタンスに同じリージョン内からアクセスできるように、クラウドサービス戦略（AWSまたはAzure）を検討することが重要です。 地域のアクセシビリティについて詳しくは、_Commerce on Cloud Infrastructure_ ガイドの[PrivateLink サービス ](../cloud-guide/development/privatelink-service.md)を参照してください。

## ターゲットサイトの立ち上げ

**質問6**: _目標のローンチ予定日を教えてください。_

サイトを立ち上げるには、サイトの立ち上げを成功させるために、反復的な設定とテストが必要です。 目標日を設定することで、Adobeアカウントチームは、最終ステップを調整するための呼び出しを含む、ローンチ前の最終的なアクティビティに備えることができます。

完全なプロセスを確認し、Launch チェックリストのコピーをダウンロードするには、_Commerce on Cloud Infrastructure_ ガイドの[Launch サイトの概要](../cloud-guide/launch/overview.md)を参照してください。

>[!TIP]
>
> Cloud Portalを見て、新しいクラウドプロジェクトにアクセスします。
>
>**次の手順**: [Commerceへのオンボーディング ](onboarding.md)
