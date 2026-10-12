---
title: ストアオプションと設定管理の概要
description: クラウド基盤でAdobe Commerceストアをカスタマイズ。
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: 'https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%
---
# ストアオプションと設定管理の概要

カスタムテーマの追加、拡張機能のインストール、クラウドインフラストラクチャ環境全体での特定の設定の適用など、ストアをカスタマイズする方法はたくさんあります。 ステージング環境と実稼動環境で、特定のサービスの設定を直接設定できます。 複数のweb サイトやストアを設定できます。 ストア設定を使用すると、ローカルワークステーションでこれらのオプションを設定し、環境全体に特定の設定をデプロイできます。

ストアフロントにアクセスするには、`magento-cloud url` コマンドを使用して、プロンプトに答えます。 または、**アクセス サイト**&#x200B;の[!DNL Cloud Console]でURLを見つけることができます。

## ストアオプションの設定

ストアオプションには次のものが含まれます。

* [企業間取引モジュール（B2B）](b2b-module.md)
* [カスタムテーマ](custom-theme.md)
* [拡張機能](extensions.md)
* [複数のサイト](multiple-sites.md)
* [決済サービス](paypal.md)

## サービスと統合の設定

リモート環境に対する特定のデプロイメント動作を管理する特定の[設定ファイル &#x200B;](../environment/overview.md)があります。 これらのトピックは個別に確認できます。

* [アプリケーションの展開](../application/configure-app-yaml.md)
* [環境のビルドとデプロイのアクション](../environment/configure-env-yaml.md)
* [受信リクエストルート](../routes/routes-yaml.md)
* [サポート対象サービス](../services/services-yaml.md)

## 設定管理

ストアのオプション、サービス、統合を設定した後は、構成管理を使用して、すべての環境に一貫した構成をデプロイし、ダウンタイムを最小限に抑えます。 [構成管理](store-settings.md)を参照してください。
