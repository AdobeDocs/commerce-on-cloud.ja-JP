---
title: ダウンタイムのゼロ導入
description: クラウドインフラストラクチャプロジェクトにAdobe Commerceをデプロイする際に、全体的なダウンタイムを削減する方法について説明します。
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
TQID: 'https://experienceleague.adobe.com/wYFZNd42AoVZxdlWWG6Jr-K6FV2XhTdWp-9HFoof4rE'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
  - id: 8b440f30-6794-5ed6-981f-391de4e9b0cc
    internal-label: Themes
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%
---
# ダウンタイムのゼロ導入

Adobe Commerce on cloud infrastructureは、デプロイメントフェーズ中に&#x200B;[_maintenance_ モード &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/setup/application-modes#production-mode)でアプリケーションを実行します。デプロイメントが完了するまで、サイトはオフラインになります。 実稼動サイトがメンテナンスモードになる時間は、サイトのサイズ、デプロイメント中に適用される変更回数、静的コンテンツのデプロイメントの設定によって異なります。 プロジェクトは、**ゼロ**&#x200B;のダウンタイム効果でデプロイするように設定できます。

デプロイメントプロセス中、すべての接続は、アクティブなセッションと保留中のアクション（カートへの追加やチェックアウトなど）を保持するために、最大5分間キューに入れます。 デプロイメント後、キューは解放され、接続は中断なく続行されます。 この&#x200B;_接続保留_&#x200B;を利用してデプロイメントを&#x200B;_0_&#x200B;のダウンタイムに短縮するには、最も効率的なデプロイ戦略を使用するようにプロジェクトを設定する必要があります。

>[!NOTE]
>
>デプロイメントのダウンタイムを最小限に抑えるようにクラウドプロジェクトが最適に構成されているかどうかを確認するには、[&#x200B; スマートウィザード &#x200B;](smart-wizards.md)を使用します。 スマートウィザードは、現在の設定を確認し、ダウンタイムなしのデプロイメントのベストプラクティスを有効にするために推奨される設定調整をガイドします。

実稼動環境にアップデートをデプロイするのにかかる時間を短縮するには、次の手順を実行します。

1. [&#x200B; パッケージ `ece-tools`へのアップグレード &#x200B;](../dev-tools/install-package.md)または[&#x200B; バージョン `ece-tools`の更新](../dev-tools/update-package.md)
最適なデプロイメントを構成するために利用可能なツールを使用するには、Adobe Commerce on cloud infrastructure プロジェクトに最新の`ece-tools` パッケージが必要です。 最新の`ece-tools`がある場合は、次の手順に進みます。

   >[!NOTE]
   >
   >最新の`ece-tools` パッケージを使用することはベストプラクティスですが、ダウンタイムなしのデプロイメント方法は、`ece-tools` [&#x200B; バージョン 2002.0.13](../release-notes/cloud-release-archive.md#v2002013)以降で機能します。

1. [静的コンテンツ展開の設定](static-content.md)
デプロイメントフェーズで静的コンテンツのデプロイメントが失敗すると、サイトがメンテナンスモードで停止します。 ビルド フェーズでエラーが発生した場合、デプロイ フェーズは開始されないため、プロセスはダウンタイムを回避します。 [最小化されたHTML](static-content.md#setting-the-scd-on-build)を使用してビルド段階で静的コンテンツを生成します。これは理想的な状態とも呼ばれ、ダウンタイムなしのデプロイに最適な設定です。_エラーが発生した場合の_ ダウンタイムを防ぎます。

1. [&#x200B; デプロイ後のフックの設定](../application/hooks-property.md)
キャッシュのクリーニングとウォームを行うには、デプロイ後のフックを設定する必要があります。 デフォルトでは、サイトがダウンしている場合、デプロイメントフェーズ中にキャッシュクリーンが発生します。 キャッシュをデプロイ後のフェーズに移行すると、デプロイフェーズが完了するまでキャッシュが有効のままになり、キャッシュを安全にクリーニングできます。

   [WARM_UP_PAGES環境変数](../environment/variables-post-deploy.md#warmuppages)を使用して、キャッシュのプリロードに使用するページのリストをカスタマイズします。

1. [&#x200B; テーマファイルを減らす](../environment/variables-deploy.md#scdmatrix)
SCD\_MATRIX環境変数を設定することで、不要なテーマファイルの数を減らすことができます。

1. [静的コンテンツのデプロイメントを高速化](../environment/variables-deploy.md#scdthreads)
SCD\_THREADS環境変数を更新して、静的コンテンツのデプロイメント用のスレッド数を増やすことで、デプロイメントプロセスを高速化できます。

>[!NOTE]
>
>理想的な状態ウィザード [&#128279;](smart-wizards.md#verifying-an-ideal-configuration)を実行して、最適なデプロイメント用にプロジェクト設定を検証できます。

