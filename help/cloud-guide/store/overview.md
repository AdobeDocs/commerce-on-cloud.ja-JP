---
title: ストアオプションと設定管理の概要
description: クラウドインフラストラクチャー上のAdobe Commerce ストアをカスタマイズします。
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# ストアオプションと設定管理の概要

カスタムテーマの追加、拡張機能のインストール、クラウドインフラストラクチャ環境全体での特定の設定の適用など、ストアをカスタマイズする方法は多数あります。 ステージング環境と実稼動環境で、特定のサービスの設定を直接構成できます。 複数の web サイトやストアを設定できます。 ストア設定は、ローカルワークステーションでこれらのオプションを設定し、環境全体に特定の設定をデプロイするのに役立ちます。

ストアフロントにアクセスするには、`magento-cloud url` コマンドを使用してプロンプトに答えます。 または、[!DNL Cloud Console] の **Access site** の下で URL を見つけることができます。

## ストアオプションの設定

ストアオプションには次のものがあります。

* [B2B （Business-to-Business）モジュール](b2b-module.md)
* [カスタムテーマ](custom-theme.md)
* [拡張機能](extensions.md)
* [複数のサイト](multiple-sites.md)
* [支払いサービス](paypal.md)

## サービスと統合の設定

リモート環境に対する特定のデプロイメント動作を管理する特定の [ 設定ファイル ](../environment/overview.md) があります。 これらのトピックは、個別に確認できます。

* [アプリケーションのデプロイメント](../application/configure-app-yaml.md)
* [環境ビルドおよびデプロイアクション](../environment/configure-env-yaml.md)
* [受信リクエストルート](../routes/routes-yaml.md)
* [サポートされるサービス](../services/services-yaml.md)

## 設定管理

ストアオプション、サービス、統合を設定した後、設定管理を使用して、ダウンタイムを最小限に抑えながら、すべての環境にこのような設定を一貫してデプロイします。 [ 設定の管理 ](store-settings.md) を参照してください。
