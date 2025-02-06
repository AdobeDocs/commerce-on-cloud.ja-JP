---
title: Commerceのクラウドコンポーネント
description: クラウドコンポーネントパッケージの最新の改善点のリストを確認します。
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: 33f89e5c9af7c172ad0592b61343e285b456fc1a
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# Commerceのクラウドコンポーネント

[ クラウドコンポーネント ](https://github.com/magento/magento-cloud-components) パッケージは、クラウドインフラストラクチャにデプロイされたサイトに対して、拡張されたAdobe Commerce コア機能を提供します。 このパッケージは、ECE-Tools パッケージの依存関係です。 これらのリリースノートでは、[Commerce用 Cloud Tools Suite](cloud-tools-suite.md) のコンポーネントであるこのパッケージの最新の改善点について説明します。

`magento/magento-cloud-components` パッケージでは、次のバージョン シーケンスを使用します：`<major>.<minor>.<patch>`

リリースノートには次のものが含まれます。

- ![ 新しいアイコン ](../../assets/new.svg) 新機能
- ![ 修正アイコン ](../../assets/fix.svg) 修正点および改善点

<!--Add release notes below-->

## v1.1.1 {#latest}


リリース日：2025 年 2 月 6 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg)**PHP 8.4**—PHP 8.4 のサポートを追加しました。<!-- MCLOUD-13148	 - -->
- ![ 修正アイコン ](../../assets/fix.svg)**キャッシュウォームアップの修正** - キャッシュウォームアップ中のカテゴリ URL の問題を修正しました。<!-- MCLOUD-12454 - -->


## v1.1.0

リリース日：2024 年 10 月 7 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg)**リファクタリングされたコード** – 古い PHP バージョン 7.4、7.3、7.2 および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 - -->
- ![ 修正アイコン ](../../assets/fix.svg)**アップグレードされた Monolog バージョン**—Monolog 3.6.<!-- MCLOUD-12855 - --> のサポートを追加

## v1.0.14

リリース日：2024 年 4 月 8 日（PT）

- ![new icon](../../assets/new.svg) **PHP**—PHP 8.3 のサポートを追加しました。

## v1.0.13

リリース日：2023 年 3 月 10 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg)**PHP 8.2 のサポートの強化**—Commerce 2.4.6 をサポートするために、特定の PHP 8.2.x バージョンとの互換性の問題を修正しました。

## v1.0.12

リリース日：2022 年 9 月 13 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg)**ウォームアップ時のエラー** – 管理者でページの表示が [ 個別に表示されない ](../environment/variables-post-deploy.md#warm_up_pages) に設定されている場合に、デプロイメントログに `ERROR: Warming up failed: <link to page>` エラーが表示される [**ウォームアップ**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) を実行しようとした問題を修正しました。<!-- MCLOUD-9134 -->

## v1.0.11

リリース日：2022 年 8 月 4 日（PT）

- ![fix icon](../../assets/fix.svg)**Symfony 5.4 互換性のサポートを追加**—Symfony 5.4.<!-- AC-3550 --> との互換性を修正

## v1.0.10

リリース日：2022 年 3 月 10 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg)**PHP 8.1 のサポート**—PHP 8.1 のサポートを追加し、PHP 7.1 のサポートを終了しました。

## v1.0.9

リリース日：2021 年 10 月 25 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg)**モノログを更新** - `monolog` パッケージに必要な最小バージョンを `^2.3` に更新しました。<!-- ACMP-1263 -->

## v1.0.8

リリース日：2021 年 7 月 29 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg)**自動生成された URL から末尾のスラッシュを削除** - キャッシュウォームアップ中に生成されたカテゴリページ URL から末尾のスラッシュを削除しました。<!--MCLOUD-7192-->

## v1.0.7

リリース日：2020 年 9 月 9 日（PT）

- ![ 新規アイコン ](../../assets/new.svg)**ログの改善** - パフォーマンスを向上させるために `cache.log` ファイルのサイズを縮小します。<!--MCLOUD-6859-->

- ![fix icon](../../assets/fix.svg)`php bin/magento cache:evict` CLI コマンドが失敗する原因となったキャッシュ設定値のタイプエラーを修正しました。

## v1.0.6

リリース日：2020 年 8 月 5 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg)**Redis パフォーマンスの向上** – 期限切れの Redis キーを削除する `./bin/magento cache:evict` コマンドを追加しました。これにより、Redis メモリの使用量が減り、パフォーマンスが向上します。<!--MCLOUD-6023-->

- ![ 修正アイコン ](../../assets/fix.svg) パフォーマンスの問題を修正するために、*コンテキストのNew Relic ログ* のサポートを削除しました。<!--MCLOUD-6422-->

## v1.0.5

リリース日：2020 年 6 月 25 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg) magento/magento-cloud-components バージョン 1.0.4 で発生した、デプロイフェーズ中にフラッシュキャッシュ操作が失敗し、デプロイメントプロセスが中断される問題を修正しました。

## v1.0.4

リリース日：2020 年 6 月 25 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg)**コンテキストでの実装されたNew Relic ログ** - Adobe Commerceで生成されたアプリケーションログがNew Relic内のトレースに表示され、トラブルシューティング機能が向上しました。<!--MCLOUD-6029-->

- ![ 新しいアイコン ](../../assets/new.svg)**ログの改善** - キャッシュの無効化と完全な再インデックスイベントを追跡するログを追加しました。<!--MCLOUD-6157-->

## v1.0.3

リリース日：2020 年 2 月 27 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg) 古いバージョンの PHP を使用する `ece-tools` 2002.0.x リリースをサポートするための互換性の問題を修正しました。

## v1.0.2

リリース日：2020 年 2 月 6 日（PT）

- ![ 新しいアイコン ](../../assets/new.svg) 特定の製品ページのキャッシュのプリロードをサポートするために、`WARM_UP_PAGES` 環境変数の機能が拡張されました。 機能の説明について詳しくは、[ デプロイ後変数 ](../environment/variables-post-deploy.md#warm_up_pages) のトピックを参照してください。<!--MAGECLOUD-4444-->

- ![ 修正アイコン ](../../assets/fix.svg) 無効なストア URL を使用すると、`WARM_UP_PAGES` 機能を使用してキャッシュにデータを入力する際に、デプロイ後のフックが失敗する問題を修正しました。 この問題は、URL の書き換えが無効になった場合にのみ発生していました。<!-- MAGECLOUD-4094 -->

## v1.0.1

リリース日：2019 年 7 月 23 日（PT）

- ![ 修正アイコン ](../../assets/fix.svg) デフォルトのストア URL を使用する [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) 機能に影響する問題を修正しました。 これで、`config:show:default-url` コマンドでベース URL を取得できない場合は、MAGENTO_CLOUD_ROUTES 変数からの URL が使用されます。<!-- MAGECLOUD-3866 -->

## v1.0.0

リリース日：2019 年 6 月 12 日

これは [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) パッケージの最初のリリースで、`ece-tools` パッケージバージョン 2002.0.20 以降の新しい依存関係です。

- ![ 新規アイコン ](../../assets/new.svg) 正規表現パターンを使用して、単一ページ、複数ドメインおよび複数ページをキャッシュするように **WARM_UP_PAGES** 環境変数を設定する機能が追加されました。 [ デプロイ後の変数 ](../environment/variables-post-deploy.md#warm_up_pages) を参照してください。<!--MAGECLOUD-3258-->
