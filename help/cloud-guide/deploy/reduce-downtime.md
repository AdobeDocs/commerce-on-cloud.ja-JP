---
title: ダウンタイムのゼロ導入
description: クラウドインフラストラクチャプロジェクトにAdobe Commerceをデプロイする際に、全体的なダウンタイムを削減する方法について説明します。
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
TQID: https://experienceleague.adobe.com/wYFZNd42AoVZxdlWWG6Jr-K6FV2XhTdWp-9HFoof4rE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 502
ht-degree: 0%

---

# ダウンタイムのゼロ導入

Adobe Commerce on cloud infrastructureは、デプロイメントフェーズ中に&#x200B;[_maintenance_ モード ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode)でアプリケーションを実行します。デプロイメントが完了するまで、サイトはオフラインになります。 実稼動サイトがメンテナンスモードになる時間は、サイトのサイズ、デプロイメント中に適用される変更回数、静的コンテンツのデプロイメントの設定によって異なります。 プロジェクトは、**ゼロ**&#x200B;のダウンタイム効果でデプロイするように設定できます。

デプロイメントプロセス中、すべての接続は、アクティブなセッションと保留中のアクション（カートへの追加やチェックアウトなど）を保持するために、最大5分間キューに入れます。 デプロイメント後、キューは解放され、接続は中断なく続行されます。 この&#x200B;_接続保留_&#x200B;を利用してデプロイメントを&#x200B;_0_&#x200B;のダウンタイムに短縮するには、最も効率的なデプロイ戦略を使用するようにプロジェクトを設定する必要があります。

>[!NOTE]
>
>デプロイメントのダウンタイムを最小限に抑えるようにクラウドプロジェクトが最適に構成されているかどうかを確認するには、[ スマートウィザード ](smart-wizards.md)を使用します。 スマートウィザードは、現在の設定を確認し、ダウンタイムなしのデプロイメントのベストプラクティスを有効にするために推奨される設定調整をガイドします。

実稼動環境にアップデートをデプロイするのにかかる時間を短縮するには、次の手順を実行します。

1. [ パッケージ `ece-tools`へのアップグレード ](../dev-tools/install-package.md)または[ バージョン `ece-tools`の更新](../dev-tools/update-package.md)
最適なデプロイメントを構成するために利用可能なツールを使用するには、Adobe Commerce on cloud infrastructure プロジェクトに最新の`ece-tools` パッケージが必要です。 最新の`ece-tools`がある場合は、次の手順に進みます。

   >[!NOTE]
   >
   >最新の`ece-tools` パッケージを使用することはベストプラクティスですが、ダウンタイムなしのデプロイメント方法は、`ece-tools` [ バージョン 2002.0.13](../release-notes/cloud-release-archive.md#v2002013)以降で機能します。

1. [静的コンテンツ展開の設定](static-content.md)
デプロイメントフェーズで静的コンテンツのデプロイメントが失敗すると、サイトがメンテナンスモードで停止します。 ビルド フェーズでエラーが発生した場合、デプロイ フェーズは開始されないため、プロセスはダウンタイムを回避します。 [最小化されたHTML](static-content.md#setting-the-scd-on-build)を使用してビルド段階で静的コンテンツを生成します。これは理想的な状態とも呼ばれ、ダウンタイムなしのデプロイに最適な設定です。_エラーが発生した場合の_ ダウンタイムを防ぎます。

1. [ デプロイ後のフックの設定](../application/hooks-property.md)
キャッシュのクリーニングとウォームを行うには、デプロイ後のフックを設定する必要があります。 デフォルトでは、サイトがダウンしている場合、デプロイメントフェーズ中にキャッシュクリーンが発生します。 キャッシュをデプロイ後のフェーズに移行すると、デプロイフェーズが完了するまでキャッシュが有効のままになり、キャッシュを安全にクリーニングできます。

   [WARM_UP_PAGES環境変数](../environment/variables-post-deploy.md#warmuppages)を使用して、キャッシュのプリロードに使用するページのリストをカスタマイズします。

1. [ テーマファイルを減らす](../environment/variables-deploy.md#scdmatrix)
SCD\_MATRIX環境変数を設定することで、不要なテーマファイルの数を減らすことができます。

1. [静的コンテンツのデプロイメントを高速化](../environment/variables-deploy.md#scdthreads)
SCD\_THREADS環境変数を更新して、静的コンテンツのデプロイメント用のスレッド数を増やすことで、デプロイメントプロセスを高速化できます。

>[!NOTE]
>
>理想的な状態ウィザード ](smart-wizards.md#verifying-an-ideal-configuration)を実行して、[最適なデプロイメント用にプロジェクト設定を検証できます。

