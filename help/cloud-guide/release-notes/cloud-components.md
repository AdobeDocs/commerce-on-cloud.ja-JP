---
title: Commerceのクラウドコンポーネント
description: Cloud Components パッケージの最新の機能強化の一覧を参照してください。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00.000Z
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
TQID: https://experienceleague.adobe.com/8vlWv-N4EaBbzemw60LYmE3MuVpFyKn4b1ZU081WriU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 740
ht-degree: 0%

---

# Commerceのクラウドコンポーネント

[Cloud Components](https://github.com/magento/magento-cloud-components) パッケージは、Cloud インフラストラクチャにデプロイされたサイトに対して、拡張Adobe Commerce コア機能を提供します。 このパッケージは、ECE-Tools パッケージの依存関係です。 このリリースノートでは、[Cloud Tools Suite for Commerce](cloud-tools-suite.md)のコンポーネントであるこのパッケージの最新の機能強化について説明します。

`magento/magento-cloud-components` パッケージでは、次のバージョン シーケンスが使用されています：`<major>.<minor>.<patch>`

リリースノートには以下が含まれます。

- ![新しいアイコン &#x200B;](../../assets/new.svg)新機能
- ![修正アイコン &#x200B;](../../assets/fix.svg)修正と機能強化

<!--Add release notes below-->

## v1.1.4 {#latest}

リリース日：2026年3月5日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.5** - PHP 8.5のサポートを追加しました。<!-- MCLOUD-14182-->

## v1.1.3

リリース日：2025年8月7日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.4** - PHP 8.4の機能テストと修正を追加しました。<!-- MCLOUD-13313 -->

## v1.1.2

リリース日：2025年6月3日（PT）

- ![&#x200B; アイコンの修正](../../assets/fix.svg) **2.4.8**&#x200B;との互換性を向上させるために、2.4.8<!-- MCLOUD-13707	 - -->との互換性を向上させるために、サードパーティ製ライブラリを更新しました

## v1.1.1

リリース日：2025年2月6日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.4** - PHP 8.4のサポートを追加しました。<!-- MCLOUD-13148	 - -->
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **キャッシュウォームアップの修正** - キャッシュウォームアップ中のカテゴリ URLに関する問題を修正しました。<!-- MCLOUD-12454 - -->


## v1.1.0

リリース日：2024年10月7日（PT）

- ![fix icon](../../assets/fix.svg) **リファクタリング コード** – 古いPHP バージョン 7.4、7.3、7.2および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 - -->
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **Monolog バージョン**&#x200B;のアップグレード - monolog 3.6.<!-- MCLOUD-12855 - -->のサポートを追加しました

## v1.0.14

リリース日：2024年4月8日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP** - PHP 8.3のサポートを追加しました。

## v1.0.13

リリース日：2023年3月10日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.2**&#x200B;の拡張サポート – 特定のPHP 8.2.x バージョンでCommerce 2.4.6をサポートする際の互換性の問題を修正しました。

## v1.0.12

リリース日：2022年9月13日（PT）

- ![&#x200B; アイコン &#x200B;](../../assets/fix.svg) **ウォームアップ**&#x200B;のエラー – ページの表示が管理者で&#x200B;[**個別に表示されない**](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure)に設定されている場合に[&#x200B; ウォームアップ &#x200B;](../environment/variables-post-deploy.md#warm_up_pages)を試みた問題を修正し、デプロイメントログに`ERROR: Warming up failed: <link to page>`個のエラーが発生しました。<!-- MCLOUD-9134 -->

## v1.0.11

リリース日：2022年8月4日（PT）

- ![&#x200B; アイコンの修正](../../assets/fix.svg) **Symfony 5.4との互換性のサポートを追加** - Symfony 5.4との互換性の修正。<!-- AC-3550 -->

## v1.0.10

リリース日：2022年3月10日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.1**&#x200B;をサポート - PHP 8.1のサポートを追加し、PHP 7.1のサポートを終了しました。

## v1.0.9

リリース日：2021年10月25日（PT）

- ![修正アイコン &#x200B;](../../assets/fix.svg) **モノログの更新**-`monolog` パッケージに必要な最小バージョンを`^2.3`に更新しました。<!-- ACMP-1263 -->

## v1.0.8

リリース日：2021年7月29日（PT）

- ![修正アイコン &#x200B;](../../assets/fix.svg) **自動生成URLから末尾のスラッシュを削除** - キャッシュ ウォームアップ中に生成されたカテゴリーページ URLから末尾のスラッシュを削除しました。<!--MCLOUD-7192-->

## v1.0.7

リリース日：2020年9月9日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **改善のログ記録** - `cache.log` ファイルのサイズを小さくして、パフォーマンスを向上させます。<!--MCLOUD-6859-->

- ![&#x200B; アイコンを修正](../../assets/fix.svg) `php bin/magento cache:evict` CLI コマンドが失敗する原因となったキャッシュ設定値のタイプ エラーを修正しました。

## v1.0.6

リリース日：2020年8月5日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **Redis パフォーマンスの向上** – 有効期限が切れたRedis キーを削除する`./bin/magento cache:evict` コマンドを追加しました。これにより、Redis メモリの使用を減らし、パフォーマンスを向上させることができます。<!--MCLOUD-6023-->

- ![fix icon](../../assets/fix.svg) パフォーマンスの問題を修正するために、コンテキスト *の* New Relic ログのサポートを削除しました。<!--MCLOUD-6422-->

## v1.0.5

リリース日：2020年6月25日（PT）

- ![&#x200B; アイコンを修正](../../assets/fix.svg) magento/magento-cloud-components バージョン 1.0.4で導入された問題を修正しました。この問題により、デプロイメントフェーズ中にフラッシュキャッシュ操作が失敗し、デプロイメントプロセスが中断されます。

## v1.0.4

リリース日：2020年6月25日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **コンテキストにNew Relic ログを実装**—Adobe Commerceによって生成されたアプリケーションログが、New Relic内でトレース表示されるようになりました。トラブルシューティング機能が向上しました。<!--MCLOUD-6029-->

- ![新しいアイコン &#x200B;](../../assets/new.svg) **改善されたログ** - キャッシュの無効化と完全なインデックス再作成イベントを追跡するためのログを追加しました。<!--MCLOUD-6157-->

## v1.0.3

リリース日：2020年2月27日（PT）

- ![&#x200B; アイコンを修正](../../assets/fix.svg)古いバージョンのPHPを使用する`ece-tools` 2002.0.x リリースをサポートするための互換性の問題を修正しました。

## v1.0.2

リリース日：2020年2月6日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg)特定の製品ページのキャッシュのプリロードをサポートするために、`WARM_UP_PAGES`環境変数の機能を拡張しました。 詳細な機能の説明については、[&#x200B; デプロイ後の変数](../environment/variables-post-deploy.md#warm_up_pages)のトピックを参照してください。<!--MAGECLOUD-4444-->

- ![&#x200B; アイコンの修正](../../assets/fix.svg)無効なストア URLを使用して`WARM_UP_PAGES`機能を使用してキャッシュに入力すると、デプロイ後のフックが失敗する問題を修正しました。 この問題は、URLの書き換えが無効になった場合にのみ発生しました。<!-- MAGECLOUD-4094 -->

## v1.0.1

リリース日：2019年7月23日（PT）

- ![&#x200B; アイコンを修正](../../assets/fix.svg) デフォルトのストア URLを使用する&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;機能に影響する問題を修正しました。 これで、`config:show:default-url` コマンドでベース URLを取得できない場合は、MAGENTO_CLOUD_ROUTES変数のURLが使用されます。<!-- MAGECLOUD-3866 -->

## v1.0.0

リリース日：2019年6月12日（PT）

これは、[`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) パッケージの最初のリリースで、`ece-tools` パッケージ バージョン 2002.0.20以降の新しい依存関係です。

- ![新しいアイコン &#x200B;](../../assets/new.svg)正規表現パターンを使用して、単一ページ、複数ドメイン、複数ページをキャッシュするように&#x200B;**WARM_UP_PAGES**&#x200B;環境変数を設定する機能を追加しました。 [&#x200B; デプロイ後の変数](../environment/variables-post-deploy.md#warm_up_pages)を参照<!--MAGECLOUD-3258-->
