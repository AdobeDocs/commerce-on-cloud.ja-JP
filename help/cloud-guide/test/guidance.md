---
title: テストのガイダンス
description: Adobe Commerce on cloud infrastructureをローンチするためのテストの種類とベストプラクティスについて説明します。
exl-id: 70fdfbbd-1763-4b1b-9ffd-9ffdc92f4f91
TQID: https://experienceleague.adobe.com/uCBGY5eP-R4zbUWeD-WE95F49k-U-nI058FR-rOJfsU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 371
ht-degree: 0%

---

# テストのガイダンス

Adobe Commerce on cloud インフラストラクチャプロジェクトを設定してカスタマイズした後、ストア web サイトを起動する前に、アプリケーションを徹底的にテストすることをお勧めします。 テストは、クラスターサイズの期待値を適切に管理し、将来のビジネスニーズに応じて適切に拡張するのに役立ちます。

## 機能テスト

開発中は、Adobe Commerce on cloud infrastructure プロジェクトでエンドツーエンドの機能テストを実行することが重要です。 Docker環境で機能テストを実行する場合は、次のガイダンスを参照してください。

- **アプリケーションテスト** - Cloud Docker環境でのアプリケーションテストには、[Magento Functional Testing Framework （MFTF） ](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing)を使用します。

- **コードテスト**- PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing)の[ コードセプションテストフレームワークを使用して、Cloud パッケージリポジトリへの貢献用コードを検証します。

## ローンチ前のベストプラクティス

サイトを立ち上げる前に実行するベストプラクティスとして、次のテストタイプを検討します。

- **負荷テスト** – 負荷テストを実行して、予想される負荷の下でのシステムの動作を把握します。 例えば、設定された期間内に各ユーザーが特定の数のトランザクションを実行する場合、アプリケーション上の同時アクティブユーザー数をテストします。 このテストでは、データベースやアプリケーションサーバーの動作など、ビジネスに不可欠な重要なトランザクションの応答時間を明らかにします。 負荷テストは、ボトルネックを特定するのに役立ちます。

- **負荷テスト** – 現在の負荷が予想される最大値を大幅に上回ったときに、システムが十分に機能するかどうかを判断するために、システム内の容量の上限に挑戦します。

- **セキュリティスキャン**:Adobeでは、無料の[ セキュリティスキャンツール ](../launch/overview.md#set-up-the-security-scan-tool)をサイトに提供しています。

- **侵入テスト** - システムのセキュリティを評価するために設計されたコンピューター・システムに対する許可されたシミュレートされたサイバー攻撃です。 侵入テストは、権限のない関係者がシステムの機能やデータにアクセスする可能性など、脆弱性や弱点を特定するのに役立ちます。

>[!WARNING]
>
>お客様は、AWS インフラストラクチャまたはAWS サービス自体のセキュリティ評価を実行することはできません。 セキュリティ評価で確認されたAWS サービス内でセキュリティの問題が発生した場合は、[AWS セキュリティ ](mailto:aws-security@amazon.com)に直ちに連絡してください。 侵入テストについては、[AWSのお客様のポリシー](https://aws.amazon.com/security/penetration-testing/)を参照してください。
