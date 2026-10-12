---
title: クラウド導入の最適化
description: ダウンタイムの削減、静的コンテンツのデプロイメント、シナリオベースのデプロイメント、スマートウィザードなど、Adobe Commerce on cloud infrastructure プロジェクトのデプロイメントプロセスを最適化する方法について説明します。
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: 'https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew'
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
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
    internal-label: Optimization
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%
---
# デプロイメントの最適化

デプロイメントプロセス中にサイトのパフォーマンスが低下する可能性があります。 実稼動サイトにデプロイする際にサイトがメンテナンスモードになる時間は、環境設定やサイトに含まれるコンテンツの量など、多くの要因によって異なります。 クラウドのデプロイメントを最適化するための最初のベストプラクティスは、[&#x200B; アップグレードで`ece-tools`](../dev-tools/install-package.md)を使用して、データベースのバックアップを作成し、環境設定を検証するコマンドなどのパッケージ機能を利用することです。

次のトピックは、デプロイメントプロセスを最適化する方法をより深く理解するのに役立ちます。

- [&#x200B; クラウド展開プロセス](process.md)
クラウドのデプロイメントプロセスには3つのフェーズがあり、各フェーズの長所と短所を活用して活用できます。

- [&#x200B; ダウンタイムのデプロイメントをゼロにする](reduce-downtime.md)
デプロイメント中に何が起こるか、実稼動環境の更新中にストア体験のダウンタイムを削減する方法について説明します。

- [静的コンテンツの展開](static-content.md)
Cloud デプロイメントを最適化する最良の方法は、静的コンテンツを生成する方法とタイミングを制御することです。

- [&#x200B; スマートウィザード](smart-wizards.md)
`ece-tools` パッケージには、プロジェクト設定をすばやく評価するためのスマートウィザードコマンドが用意されています。

- [New Relicでデプロイメントを追跡する](../monitor/track-deployments.md)
New Relicサービスを使用して、デプロイメントイベントを監視し、デプロイメントが全体的なパフォーマンスに与える影響を分析します。

