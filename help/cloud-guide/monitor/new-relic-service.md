---
title: New Relic サービス
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceで使用できるNew Relic サービスについて説明します。
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# New Relic サービスの概要

クラウドインフラストラクチャ上のすべてのAdobe Commerce プロジェクトには、パフォーマンスを監視し、[!DNL Commerce] アプリケーションとクラウドインフラストラクチャのイベントを調査するのに役立つNew Relic サービスへのアクセスが含まれます。

実稼動環境とステージング環境では、次のNew Relic機能を使用できます。

- [New Relic APM](#new-relic-apm) （Pro および Starter）
- [New Relic インフラストラクチャ ](#new-relic-infrastructure) （Pro のみ）
- [New Relic Log Management](#new-relic-log-management) （Pro のみ）

>[!INFO]
>
>その他のNew Relic機能は、Adobe Commerce プロジェクトでは使用できません。

## NEW RELIC APM

[New Relic for Application Performance Management （APM） ](https://docs.newrelic.com/introduction-apm/) は、アプリケーションの操作を分析および向上させるのに役立つソフトウェア分析製品です。 New Relic APM は、クラウドインフラストラクチャプロジェクト上のすべてのAdobe Commerceで使用でき、次の機能が用意されています。

- **特定のトランザクションに焦点を当てる** - サイト内の主な顧客アクション（買い物かごへの追加、チェックアウト、支払いの処理など）をアクティブにマークおよび監視します。
- **データベース・クエリーの監視** - パフォーマンスに影響を与えるデータベース・クエリーを検索して監視します。
- **アプリマップ** - サイト、拡張機能、外部サービス内のすべてのアプリケーション依存関係を表示します。
- **[!DNL Apdex]スコア** - パフォーマンスを評価し、問題を特定して発生時に通知するアラートを作成します（フラッシュセールまたは web イベントの影響を受けたサイトのパフォーマンスなど）。 [Apdex スコア ](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/) を参照してください。
- **Adobe Commerceの管理アラート** – このNew Relic アラートポリシーを使用すると、業界のベストプラクティスに基づいてアプリケーションとインフラストラクチャのパフォーマンスを監視できます。 [Adobe Commerceの Managed アラートを使用したパフォーマンスの監視のアラートポリシー ](investigate-performance.md/#monitor-performance-with-managed-alerts) を参照してください。
- **導入の追跡**：導入イベントを監視し、導入の全体的なパフォーマンスへの影響を分析します。 [ デプロイメントの追跡 ](track-deployments.md) を参照してください。

Adobe Commerce on cloud infrastructure プロジェクトには、New Relic APM サービスのソフトウェアとライセンスキーが含まれています。 追加のソフトウェアを購入またはインストールする必要はありません。

## New Relic インフラストラクチャ

Pro プロジェクトには、[New Relic インフラストラクチャ （NRI） ](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) サービスが含まれます。このサービスは、アプリケーション データおよびパフォーマンス分析と自動的に接続し、動的なサーバーのモニタリングを提供します。 このサービスは、実稼動環境とステージング環境で利用できます。

## New Relic ログ管理

すべてのクラウドインフラストラクチャプロジェクトには、[New Relic ログ管理 ](log-management.md) が含まれます。 このサービスは、ステージング環境と実稼動環境のすべてのログデータを集計して、一元化されたログ管理ダッシュボードに表示するように事前に設定されています。
