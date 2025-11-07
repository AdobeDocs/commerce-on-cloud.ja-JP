---
title: Commerceのクラウドパッチ
description: クラウドパッチパッケージの最新の改善点のリストを確認します。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: 4f96ed89edbbc148c5558050368d8366bd89053a
workflow-type: tm+mt
source-wordcount: '2498'
ht-degree: 0%

---

# Commerceのクラウドパッチ

[&#x200B; クラウドパッチ &#x200B;](https://github.com/magento/magento-cloud-patches) パッケージには、すべてのAdobe Commerce バージョンとクラウド環境の統合を改善し、重要な修正の迅速な配信をサポートする必須のパッチのセットが用意されています。

Cloud Patches for Commerce パッケージは、ECE-Tools パッケージの依存関係であり、ECE-Tools パッケージをインストールまたは更新するとインストールおよび更新されます。 また、Commerce as a スタンドアロンパッケージとしてクラウドパッチを使用および管理して、Cloud Platform 上にないAdobe Commerce プロジェクトにパッチを適用することもできます。 これらのリリースノートでは、このパッケージの最新の改善点について説明します。

>[!TIP]
>
>プロジェクトに必要なパッチがすべて含まれていることを確認するには、[&#x200B; 最新バージョンの ece-tools](../dev-tools/update-package.md) にアップデートしてください。

>[!NOTE]
>
>プロジェクトにパッチを適用する手順については、[&#x200B; パッチの適用 &#x200B;](../development/apply-patches.md) を参照してください。

`magento/magento-cloud-patches` パッケージでは、次のバージョン シーケンスを使用します：`<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.11 {#latest}

リリース日：2025 年 9 月 9 日（PT）

- ![fix icon](../../assets/fix.svg)**WebAPI**-CVE-2025-54236 の修正。<!-- MCLOUD-14016 -->

## v1.1.10

リリース日：2025 年 8 月 7 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP 8.4** – 機能テストを追加しました。<!-- MCLOUD-13312 -->

## v1.1.9

リリース日：2025 年 6 月 9 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**カテゴリ表示の改善** – カテゴリ表示の改善 CVE-2025-47109<!-- MCLOUD-13752	 - -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**管理キャッシュの改善**-Improve-admin-cache-efficiency CVE-2025-47110<!-- MCLOUD-13753	 - -->

## v1.1.8

リリース日：2025 年 6 月 3 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**2.4.8 との互換性の向上** – サードパーティライブラリを更新し、2.4.8 との互換性を向上 <!-- MCLOUD-13707	 - -->

## v1.1.7

リリース日：2025 年 5 月 5 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Commerce 2.4.4 から 2.4.8 への更新されたパッチ** – これは、1.1.7 でリリースされた [CVE-2025-24434](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch) の更新されたパッチです <!-- MCLOUD-13619 -->

## v1.1.6

リリース日：2025 年 4 月 24 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Commerce 2.4.4 から 2.4.7 への更新されたパッチ** – これは、1.1.4 でリリースされた [CVE-2025-24434](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08) の更新されたパッチです <!-- MCLOUD-13240 -->

## v1.1.5

リリース日：2025 年 4 月 15 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**B2B 1.5.2 の追加されたパッチ**—B2B モジュール 1.5.2 および MariaDB 10.6 を使用した ACP2E-3833 の問題を修正しました <!-- MCLOUD-13605	-->

## v1.1.4

リリース日：2025 年 2 月 13 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**Commerce 2.4.4 から 2.4.7 への追加されたパッチ** – この更新パッチ [CVE-2025-24434](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## v1.1.3

リリース日：2025 年 2 月 6 日（PT）

- ![&#x200B; 新しいアイコン &#x200B;](../../assets/new.svg)**PHP 8.4**—PHP 8.4 のサポートを追加しました。<!-- MCLOUD-13149	 - -->

## v1.1.2

リリース日：2024 年 11 月 5 日（PT）

- ![fix icon](../../assets/fix.svg)**Commerce 2.4.4 から 2.4.7 へのパッチの追加** – この更新プログラムは、B2B モジュールを使用する場合のAdobe Commerceの重大な [CVE-2024-45115](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) 脆弱性を修正します。<!-- MCLOUD-12980 - -->

## v1.1.1

リリース日：2024 年 11 月 5 日（PT）

- ![fix icon](../../assets/fix.svg)**Commerce 2.4.4 から 2.4.7 への追加されたパッチ** – このアップデートは、重大な [CVE-2024-34102](https://experienceleague.adobe.com/ja/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting の脆弱性にパッチを適用します。<!-- MCLOUD-12980 - -->

## v1.1.0

リリース日：2024 年 10 月 7 日（PT）

- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**リファクタリングされたコード** – 古い PHP バージョン （7.4、7.3、7.2）および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 - -->
- ![&#x200B; 修正アイコン &#x200B;](../../assets/fix.svg)**アップグレードされた Monolog バージョン**—Monolog 3.6.<!-- MCLOUD-12855 - --> のサポートを追加
- ![fix icon](../../assets/fix.svg)**Patch for Application Server** - GraphQL Application Server の既知の問題を解決します。 特に、バージョン 2.4.7 の `CatalogGraphQl\\Model\\Config\\AttributeReader` には、古い属性の設定に基づいて応答を取得するGraphQL リクエストを引き起こす可能性があるバグが含まれていまし <!-- ACPT-1876 -->。

## v1.0.27

リリース日：2024 年 5 月 21 日（PT）

- **PHP 8.3 のサポート** – このパッチは、php 8.3 と composer パッケージバージョン間の互換性エラーを解決します。

## v1.0.26

リリース日：2024 年 4 月 8 日（PT）

- ![new icon](../../assets/new.svg) **PHP** — PHP 8.3 のサポートを追加しました。

## v1.0.25

リリース日：2024 年 1 月 16 日（PT）

- **キャッシュの改善** – このパッチは、Adobe Commerce バージョン 2.4.4 以降で、レイアウトキャッシュの効率を高め、メモリ使用量を大幅に削減します。<!-- MCLOUD-11514 -->
- **CRON ジョブの改善** – このパッチは、実行されなかったジョブがAdobe Commerce バージョン 2.4.4 以降の cron ジョブロックを不必要に待つ問題を修正します。<!-- MCLOUD-11329 -->

## v1.0.24

リリース日：2023 年 9 月 15 日（PT）

- **パフォーマンスの向上** – このパッチは、Adobe Commerce 2.4.6 と同じデプロイメント設定の読み込み回数を 2.4.6-p1 に減らすことで、パフォーマンスに影響を与える問題を修正し <!-- MCLOUD-10604 --> す。

## v1.0.23

リリース日：2023 年 7 月 31 日（PT）

- **パッチ MCLOUD-10604** を削除しました。このパッチは QPT に移動されました。<!-- MCLOUD-10736 -->

## v1.0.22

リリース日：2023 年 6 月 19 日（PT）

- **拡張 QPT CLI ウィザード/出力**:QPT CLI ウィザード/出力に、依存関係がある場合にパッチの詳細と要件を確認することを促す警告を追加しました。<!-- ACP2E-1963 -->
- **Commerce 2.4.6 用にパッチを追加しました：**
   - `regexp cache tag` の検証を修正しました。<!-- MCLOUD-10226 -->
   - 同じデプロイメント設定の読み込み回数を減らすことで、パフォーマンスを向上しました。<!-- MCLOUD-10604 -->
- **Commerce 2.3.7 から 2.4.6 へのパッチの追加** - `catalog_product_entity_*` テーブルで、1 ずつ増分するのではなく、ランダムな値で増分する問題を修正しました。<!-- MCLOUD-10032 -->
- **Commerce 2.4.0 から 2.4.6 へのパッチの追加** - Admin から JS/CSS キャッシュをフラッシュする際に発生した `The file can't be deleted. Warning!unlink: No such file or directory` というエラーを修正しました。<!-- MCLOUD-10279 -->

## v1.0.21

リリース日：2023 年 3 月 10 日（PT）

- **PHP 8.2 のサポートの強化** - Commerce 2.4.6 をサポートするために、特定の PHP 8.2.x バージョンとの互換性の問題を修正しました。

## v1.0.20

リリース日：2022 年 10 月 27 日（PT）

- **L2 キャッシュ改善パッチの追加** – このパッチは、Commerce バージョン 2.4.0 および 2.4.1.<!-- MCLOUD-7845 --> のローカル L2 キャッシュをフラッシュする際の問題を修正します。

## v1.0.19

リリース日：2022 年 9 月 13 日（PT）

- **PHP 8.1 の拡張サポート** – 特定の PHP 8.1.x バージョンとの互換性の問題を修正しました。

## v1.0.18

リリース日：2022 年 8 月 11 日（PT）

Adobe Commerce 2.4.5 の重要なパッチ：

- **Braintree支払いを使用した注文に関する問題** – このパッチは、管理者が新しい注文や再注文を行うのを妨げる重要な問題を解決します。<!-- MCLOUD-9137 -->

[Braintree支払いが有効な場合、管理者が注文の作成や並べ替えができない &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html?lang=ja) を参照してください。

## v1.0.17

リリース日：2022 年 5 月 24 日（PT）

`patches.json` ファイル内のセキュリティパッチの制約を修正しました。

## v1.0.16

リリース日：2022 年 3 月 31 日（PT）

Adobe Commerce 2.3.3-p1 以降のバージョン用の重要なパッチ：

認証されていないリモートコードの実行を引き起こす **重大** な脆弱性を解決するためにパッチを更新しました。<!-- MCLOUD-8479 -->

[Adobe セキュリティ速報 APSB22-12](https://helpx.adobe.com/jp/security/products/magento/apsb22-12.html) を参照してください。

## v1.0.15

リリース日：2022 年 3 月 10 日（PT）

- **PHP 8.1 のサポート**—PHP 8.1 のサポートを追加し、PHP 7.0 および 7.1 のサポートを終了しました。
- **Adobe Commerce 2.3.3 にパッチを追加** – 商品ページに表示される固定通貨。

## v1.0.14

リリース日：2022 年 2 月 13 日（PT）

Adobe Commerce 2.3.3-p1 以降のバージョン用の重要なパッチ：

認証されていないリモートコードが実行される **重大** な）脆弱性を解決するためのパッチを追加しました。<!-- MCLOUD-8461 -->

[Adobe セキュリティ速報 APSB22-12](https://helpx.adobe.com/jp/security/products/magento/apsb22-12.html) を参照してください。

## v1.0.13

リリース日：2021 年 10 月 25 日（PT）

- **モノログを更新** - `monolog` パッケージに必要な最小バージョンを `^2.3` に更新しました。<!-- ACMP-1263 -->
- **互換性のない PHP 方式** - Adobe Commerce バージョン 2.4.3 および 2.3.7-p1.<!-- AC-384 --> での互換性のない PHP 方式を修正しました。
- **PHP エラー** - パッチの適用中に発生した `PHP error 'Undefined variable: errorMessage' ...` エラーを修正しました。<!-- ACP2E-138 -->

## v1.0.12

リリース日：2021 年 8 月 12 日（PT）

Adobe Commerce 2.4.3 および 2.3.7-p1 の重要なパッチ：

- **API レート制限の問題** – このパッチは、配列に 20 項目を超える項目を含むリクエストを Web API が処理できなかったデフォルトのレート制限を修正します。 このパッチは、レート制限のデフォルト値を引き上げます。 Adobe Commerce [2.4.3 リリースノート &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 --> を参照してください。

## v1.0.11

リリース日：2021 年 7 月 29 日（PT）

- **B2B レイヤーナビゲーションパッチを適用した場合に発生する問題を修正しました** - B2B レイヤーナビゲーションパッチを適用したお客様の場合、この修正により、ストアビューを切り替えた後に検索ページに表示される `Undefined offset` エラーが解決されます。<!--MCLOUD-5287-->

- **Paypal チェックアウトパッチ** - PayPal Express でAdobe Commerce 2.3.7 の問題が修正され、以前に注文した価格が表示されます。<!--MC-42674-->

- **パッチカテゴリサポート** – 品質パッチに割り当てられたパッチカテゴリと接触チャネルソースの処理に対するサポートを追加しました。 カテゴリを使用すると、お客様は [&#x200B; 品質向上パッチツール &#x200B;](https://github.com/magento/quality-patches) および Site-wide Analysis Tool （SWAT）を使用する際に、フィルターと並べ替えを使用してパッチをより迅速に見つけることができます。<!--MC-38577-->

## v1.0.10

リリース日：2021 年 5 月 10 日（PT）

- **Adobe Commerce 2.3.7 との互換性** - Adobe Commerce 2.3.7 にインストールする際の Composer の依存関係の競合を解決しました。<!--MC-42131-->
- **バンドルされたパッチを複数回適用することで発生する問題を修正しました** - バンドルされたパッチ（他の非推奨パッチが含まれているパッチ）を複数回適用すると、含まれている非推奨パッケージが元に戻る可能性があります。 すべてのパッチが 1 回だけ適用されるようになりました。 同じパッケージを再度適用しようとすると、パッチが既に適用されていることを示すメッセージが表示されます。<!--MC-41912-->
- **B2B 階層ナビゲーションパッチ** - ユーザーが B2B 共有カタログを有効にすると、階層ナビゲーションですべての製品オプションが表示されない別の問題を修正しました。<!--MCLOUD-7742-->

## v1.0.9

リリース日：2021 年 2 月 1 日（PT）

- **B2B 階層ナビゲーションパッチ** - B2B 共有カタログが有効な場合に、階層ナビゲーションですべての製品オプションが表示されない問題を修正しました。<!--MCLOUD-6923-->
- **PHP 7.4 との互換性**—PHP 7.4 とのクラウドパッチの互換性の問題を修正しました。<!--MCLOUD-7367-->
- **非推奨パッチが表示される** – 非推奨パッチの内容全体を含む代替パッチを適用すると、非推奨パッチがパッチテーブルに表示されるクラウドパッチの問題を修正しました。 これは、他の複数のパッチを組み合わせたパッチを適用した場合に発生する可能性があります。<!--MC-40626-->
- **パッチ適用時のサイレント・エラー** - `git apply` コマンドが一部の環境でパッチの適用にサイレント・エラーで失敗するクラウド・パッチの問題を修正しました。<!--MC-40529-->

## v1.0.8

リリース日：2020 年 10 月 14 日

- **magento/magento-cloud-patches の互換性アップデート** - Adobe Commerce 2.4.1 以降のリリースとの互換性を保つために、`symfony` ファイルの `semver` および `composer.json` バージョンの制約を更新しました。<!--MCLOUD-7111-->

## v1.0.7

リリース日：2020 年 10 月 14 日

- **Adobe Commerce 2.3.0 から 2.3.5、2.4.0 用の Redis パッチ** - レベル 2 キャッシュを実装する際のカテゴリへの製品追加をサポートするために、Redis パッチを更新しました。<!--MCLOUD-6659-->

- **Braintree VBE パッチ** – 管理者がBraintree和解報告書を表示しようとした際にエラーが発生した問題を修正します。<!--MCLOUD-6684-->

- 現在、`ece-patches apply` コマンドは、Unix `patch` コマンドを使用して、ホストシステムで Git が使用できない場合にパッチを適用します。<!--MCLOUD-7069-->

## v1.0.6

リリース日：

- **Adobe Commerce 2.3.0 ～ 2.3.4 用 Redis パッチ** – コミュニケーションを最適化し、パフォーマンスを向上
   - Redis とAdobe Commerce間のネットワーク転送のサイズを削減
   - Redis の読み込みと書き込み操作の競合状態を修正
   - 保存時にエラーを処理するための基本キャッシュ アダプターの書き換え
   - Redis CPUの消費量を減らします <!--MCLOUD-6139-->

- **Adobe Commerce 2.3.0 ～ 2.3.5 用 Redis パッチ** - パフォーマンスの向上とエラーの修正
   - 無限ロックを防ぐためのキャッシュロックの実装の修正
   - 現在のロック機構の改善
   - 署名済みロックを実装して、並列リクエストからのロック解除を防ぐ
   - Redis 書き込み操作で発生した次のエラーを修正します。`OOM command not allowed when used memory > maxmemory`
   - 製品のアップデート中に実行されるタグ `cat_p` よるクリーンキャッシュの処理を修正しました <!--MCLOUD-6110-->

- このモジュールを含まないAdobe Commerce v2.2.6 または 2.3.5 を使用しているクラウドインフラストラクチャプロジェクトで、Adobe Commerceに必要な `amzn/amazon-pay-module` パッチを適用する際にエラーが発生する問題を修正しました。 現在は、モジュールがインストールされていない場合、パッチ適用プロセスは `amzn/amazon-pay-module` パッチをスキップします。<!--MCLOUD-6588-->

## v1.0.5

リリース日：2020 年 6 月 26 日（PT）

- **Redis パフォーマンスの向上** - Adobe Commerce バージョン 2.3.3 および 2.3.4 に Redis 最適化機能を追加しました。これらの修正は、Adobe Commerce バージョン 2.3.5 リリースに含まれていました。<!--MCLOUD-5771-->

- **New Relic ログのエンリッチメント** - Commerce バージョン 1.0.4 のクラウドコンポーネントで導入されたNew Relic ログ機能の強化をサポートするために必要な Monolog ProcessorInterface を追加します。このパッチは、Adobe Commerce 2.1.x をデプロイするために必要です。パッチが適用されていない場合、ビルドは `di:compile` のプロセス中に失敗します。<!--MCLOUD-6029-->

## v1.0.4

リリース日：2020 年 5 月 12 日（PT）

- **Amazon Pay チェックアウト** - Amazon Pay 支払いウィジェットの問題を修正しました。この問題により、お客様がチェックアウトプロセス中に _レビューと支払い_ ステップで支払い方法を変更できません。<!--MCLOUD-5930-->

- **カテゴリページでの製品表示** - _すべてのページを表示_ 表示で製品がカテゴリページに表示されない問題を修正しました。<!--MCLOUD-5684-->

- **ページビルダー画像のアップロード** – 画像を画像ギャラリーにアップロードする際に次のエラーが発生する場合があるページビルダーインターフェイスの問題を修正しました。`Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **不要なサイトマップ生成警告を抑制** - サイトマップの生成中にエラーが発生した場合は再試行を試行し、エラーを自動的に復元できる場合は顧客の電子メール通知をスキップします。<!--MCLOUD-3025-->

- **サイト・パフォーマンスの向上**:`Magento\Framework\App\DeploymentConfig\Reader::load` 機能のパフォーマンスに関する問題を修正します。この機能は、サイトのパフォーマンスに影響を及ぼすロード時間が定期的に長くなるという問題を解決します。<!--MCLOUD-5650-->

- 支払方法パッチのパッチ割当を更新し、支払モジュールが存在する場合にのみ支払パッチが適用されるように、Magentoベース・パッケージ（magento/magento2-base）のかわりに支払モジュールをターゲットにします。<!--MCLOUD-5666-->

- Magento Open Source.<!--MCLOUD-5701--> との互換性を確保するためのパッチを更新しました。

## v1.0.3

リリース日：2020 年 4 月 28 日（PT）

- Adobe Commerce 2.3.5 をサポートするための「FPC がデプロイメント中に無効になる」パッチの修正を追加しました。

## v1.0.2

リリース日：2020 年 2 月 27 日（PT）

このリリースには、次のパッチと重要な修正が含まれています。

- **magento/magento-cloud-patches の互換性のアップデート**

   - Adobe Commerce 2.4 以降のリリースとの互換性を保つために、`symfony` ファイルの `semver` と `composer.json` のバージョンの制約を更新しました。<!--MAGECLOUD-5127-->

   - `composer.json` 2002.0.22 以降の 2002.0.x リリースとの互換性を確保するために、`ece-tools` の制約を更新しました。

- **PayPal Express Checkout** - 2020 年 2 月 12 日（PT）に公開されたこのパッチは、PayPal Express Checkout で発注された注文に影響を与える問題を解決します。この問題では、注文の配送先住所が、「配送」ページのドロップダウンメニューから選択するのではなく、テキストフィールドに手動で入力された国地域を指定します。 詳しくは、パッチのダウンロードページの完全なパッチの説明を参照してください。

- **アプリケーション配置修正** – 配置プロセス中にページ全体のキャッシュが無効になる問題を修正するためのパッチが追加されました。 このパッチは、Adobe Commerce 2.3.2 以降のリリースに適用されます。

- **Async/Bulk API のスコープパラメーター** - `composer.json` ファイルの構文エラーを修正するために、このパッチを更新しました。 このパッチは、Magento Open Source 2.3.1 および 2.3.2 に適用されます。詳しくは、パッチのダウンロードページの完全なパッチの説明を参照してください。

## v1.0.1

リリース日：2020 年 2 月 6 日（PT）

magento/magento-cloud-patches v1.0.1 リリースのソフトウェアダウンロードページからすべてのMagento Open Source 2.x パッチを取り込みました。 以前にプロジェクトにパッチをコピーした場合は、競合を避けるために削除します。

このリリースには、次のパッチと重要な修正が含まれています。

- **cron デッドロックの修正と cron ロックの改善**—

   - `cron_schedule` テーブルのステータス値が正しくないことが原因で、一部の cron ジョブが実行されない問題を修正しました。 ここでは、`cron_schedule` テーブルを使用する代わりに、Adobe Commerce lock フレームワークを使用して、cron ジョブステータスをチェックおよび更新します。 エラーステータスで終了した Cron ジョブは、24 時間待たずに、次回の Cron 実行時に再試行されます。

   - _テーブル内のデータの更新中にデッドロックを回避するための_ retry`cron_schedule` 操作を追加します。

- **Magento Open Source 2.x で使用可能なすべてのパッチを含めるように `magento/magento-cloud-patches` を更新** - ソフトウェアのダウンロードページで使用可能なすべてのMagento Open Source 2.x パッチを含めるように magento/magento-cloud-patches パッケージを更新しました。 Magento Open Sourceのパッチをクラウドインフラストラクチャプロジェクト上のAdobe Commerceに以前にコピーした場合は、競合を避けるために削除します。<!--MAGECLOUD-4606-->

- **Elasticsearch カタログのページネーションの修正** - magento/magento-cloud-patches v1.0 で提供されているElasticsearch カタログのページネーションパッチを、より効果的な修正に置き換えました。<!--MAGECLOUD-4847-->

- **ページビルダーパッチ** - Commerce 1.0.0 用の Cloud Patches では、ページビルダーの既知のリモートコード実行（RCE）の脆弱性に対処するため、ページビルダーパッチをバンドルしました。初期修正はAdobe Commerce 2.3.3 に基づいています。問題を修正するための複数の最適化を含む、Adobe Commerce 2.3.4.に基づくより安定した実装で、これらのパッチを更新しました。<!--MAGECLOUD-4884-->

  magento/magento-cloud-patches 1.0.0 パッケージを使用している場合でも、ページビルダー RCE の脆弱性の問題から保護されます。 1.0.1 以降にアップデートすると、同じ修正をより適切に実装できます。

## v1.0.0

リリース日：2019 年 11 月 14 日（PT）

これは [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) パッケージの最初のリリースで、`ece-tools` パッケージバージョン 2002.0.22 以降のリリースの新しい依存関係です。

このリリースには、次のパッチと重要な修正が含まれています。

- **Page Builder 2.3.1.x および 2.3.2.x リリース用のセキュリティパッチ** - Page Builder プレビューで、未認証ユーザーがネットワーク（RCE）上での任意のコード実行をトリガーするために使用され、グローバルな情報漏洩を引き起こす可能性があるテンプレートメソッドにアクセスできるようにする問題を修正しました。 この問題は、Adobe Commerce バージョン 2.3.1 および 2.3.2.<!--MAGECLOUD-4649--> で、サポートされていないバージョンのページビルダーを使用している場合に発生する可能性があります

- **MSI パッチ** - デフォルトのインベントリ設定を使用して在庫を管理する際に、インデックス作成エラーやパフォーマンスの問題が発生する問題を修正しました。<!--MAGECLOUD-4428-->

- **新しいメールインターフェイスの後方互換性**-Adobe Commerce v2.3.3 で導入された `Magento\Framework\Mail\EmailMessageInterface` PHP インターフェイスに起因する後方非互換性の問題を修正しました。このパッチの適用範囲では、新しい `EmailMessageInterface` は古い `MessageInterface` から継承され、Adobe Commerce コアモジュールは `MessageInterface`.<!--MAGECLOUD-4422--> に依存するように戻されます。

- **Elasticsearch 6.x では、カタログのページネーションが機能しない** - Elasticsearch 6.x をカタログ検索エンジンとして使用する顧客に影響を与える、検索結果のページネーションに関する重大な問題を修正しました。<!--MAGECLOUD-4448-->
