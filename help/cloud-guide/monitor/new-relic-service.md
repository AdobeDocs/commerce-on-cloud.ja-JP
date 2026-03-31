---
title: New Relic サービス
description: Adobe Commerce on cloud infrastructure プロジェクトで利用可能なNew Relic サービスについて説明します。
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 10966241-311d-4b68-804d-4c9569bf933d
source-git-commit: 3784e7b2ddc8f6ae20fd2c6fd557f2408d870cf2
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# New Relic サービスの概要

クラウド インフラストラクチャ上のすべてのAdobe Commerce プロジェクトには、New Relic サービスへのアクセスが含まれており、[!DNL Commerce] アプリケーションとクラウド インフラストラクチャのパフォーマンスの監視とイベントの調査に役立ちます。

実稼動環境とステージング環境では、次のNew Relic機能を使用できます。

- [New Relic APM](#new-relic-apm) （ProおよびStarter）
- [New Relic Infrastructure](#new-relic-infrastructure) （Proのみ）
- [New Relic Log Management](#new-relic-log-management) （Proのみ）

>[!INFO]
>
>その他のNew Relic機能は、Adobe Commerce プロジェクトでは使用できません。
>
>Adobe Commerce on Cloudのお客様は、プロビジョニングされたNew Relic アカウントに外部サーバーから直接データを統合または送信することはできません。 New Relic サービスは、Commerce Cloud アプリケーション環境のモニタリングに限定されます。 New Relic内で追跡および監視できるのは、アプリケーション自体によって呼び出されるサードパーティサービス（実行時に呼び出される外部APIやサービスなど）のみです。

## NEW RELIC APM

[New Relic for application performance management （APM） ](https://docs.newrelic.com/introduction-apm/)は、アプリケーションのインタラクションを分析および改善するのに役立つソフトウェア分析製品です。 New Relic APMは、すべてのAdobe Commerceのクラウドインフラストラクチャプロジェクトで利用でき、次の機能を提供します。

- **特定のトランザクションに集中** - カートへの追加、チェックアウト、支払いの処理など、サイト内の主要な顧客アクションに積極的にマークを付けて監視します。
- **データベースクエリ監視**：パフォーマンスに影響を与えるデータベースクエリを検出して監視します。
- **アプリ マップ** - サイト、拡張機能、外部サービス内のすべてのアプリケーション依存関係を表示します。
- **[!DNL Apdex]スコア** - パフォーマンスを評価し、問題を特定するアラートを作成し、Flash セールまたはweb イベントの影響を受けるサイトパフォーマンスなど、問題が発生したときに通知します。 [Apdex score](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/)を参照してください。
- **Adobe Commerceのアラートを管理** – このNew Relicのアラートポリシーを使用して、業界のベストプラクティスに基づいてアプリケーションとインフラストラクチャのパフォーマンスを監視します。 Adobe Commerceのアラート管理ポリシー](investigate-performance.md/#monitor-performance-with-managed-alerts)でパフォーマンスを監視するを参照してください。[
- **デプロイメントを追跡** – デプロイメントイベントを監視し、デプロイメントが全体的なパフォーマンスに与える影響を分析します。 [展開の追跡](track-deployments.md)を参照してください。

Adobe Commerce on cloud インフラストラクチャプロジェクトには、New Relic APM サービス用のソフトウェアとライセンスキーが含まれます。 追加のソフトウェアを購入したりインストールしたりする必要はありません。

## New Relic Infrastructure

プロプロジェクトには、[New Relic Infrastructure （NRI） ](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) サービスが含まれます。このサービスは、アプリケーションデータとパフォーマンス分析に自動的に接続して、動的なサーバーモニタリングを提供します。 このサービスは、Pro実稼動環境とステージング環境で利用できます。

## New Relic Log Management

すべてのクラウドインフラプロジェクトには、[New Relic ログ管理](log-management.md)が含まれます。 このサービスは、ステージング環境と実稼動環境のすべてのログデータを集約し、一元化されたログ管理ダッシュボードに表示するように事前設定されています。
