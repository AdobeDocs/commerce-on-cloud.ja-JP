---
title: Commerce用のクラウドパッチ
description: Cloud Patches パッケージの最新の改善点のリストを参照してください。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00.000Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
TQID: https://experienceleague.adobe.com/ZN1TwgU2EFiIezQcZZT-CglLQGY1xZcXoi-BslV3sGQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 2889
ht-degree: 0%

---

# Commerce用のクラウドパッチ

[Cloud Patches](https://github.com/magento/magento-cloud-patches) パッケージには、すべてのAdobe Commerce バージョンとCloud環境との統合を改善し、重要な修正の迅速な提供をサポートする、必要なパッチのセットが用意されています。

Cloud Patches for Commerce パッケージは、ECE-Tools パッケージの依存関係であり、ECE-Tools パッケージのインストール時または更新時にインストールおよび更新されます。 また、CommerceのCloud Patchesをスタンドアロンパッケージとして使用して管理し、Cloud Platform上にないAdobe Commerce プロジェクトにパッチを適用することもできます。 これらのリリースノートでは、このパッケージの最新の機能強化について説明します。

>[!TIP]
>
>プロジェクトに必要なすべてのパッチが適用されていることを確認するには、[最新バージョンのece-tools](../dev-tools/update-package.md)にアップデートしてください。

>[!NOTE]
>
>プロジェクトにパッチを適用する手順については、[&#x200B; パッチの適用](../development/apply-patches.md)を参照してください。

`magento/magento-cloud-patches` パッケージでは、次のバージョン シーケンスが使用されています：`<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.14 {#latest}

リリース日：2026年5月6日（PT）

- ![修正アイコン &#x200B;](../../assets/fix.svg) **パッチバージョンの表示** – クラウド環境での修正クラウドパッチバージョンの表示<!--MCLOUD-14221 -->
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **PHPUnit クリーンアップ** – 固定PHPUnit通知<!--MCLOUD-14717 -->
- ![新しいアイコン &#x200B;](../../assets/new.svg)**化粧品の修正** – 化粧品の改善を追加しました。<!--MCLOUD-14686 -->

## v1.1.13

リリース日：2026年3月5日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.5** - PHP 8.5のサポートを追加しました。<!-- MCLOUD-14181 -->
- ![fix icon](../../assets/fix.svg) **PHP 8.x**&#x200B;の機能テストを更新しました。従来のPHP 7.x テストを削除し、PHP 8.1と8.2の新しいテストカバレッジを追加し、Adobe Commerce バージョンを更新しました。<!-- MCLOUD-14203 -->

## v1.1.12

リリース日：2025年11月13日（PT）

- ![&#x200B; アイコンを修正](../../assets/fix.svg) **Symfony パッケージ** – 最新のSymfony YAML パッケージのサポートを追加しました。<!-- MCLOUD-14020 -->
- ![fix icon](../../assets/fix.svg) **Patch** - JSの縮小とバンドルが有効になっている場合、[&#x200B; チェックアウトの修正が失敗する](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27997)問題について、*Commerce ナレッジベース*&#x200B;で説明しています。
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **カテゴリ表示**&#x200B;の改善 – MCLOUD-13752: カテゴリ表示の改善<!-- MCLOUD-13752 | MCLOUD-14139  -->

## v1.1.11

リリース日：2025年9月9日（PT）

- ![修正アイコン &#x200B;](../../assets/fix.svg) **WebAPI**-CVE-2025-54236.<!-- MCLOUD-14016 -->の修正

## v1.1.10

リリース日：2025年8月7日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.4** – 機能テストを追加しました。<!-- MCLOUD-13312 -->

## v1.1.9

リリース日：2025年6月9日（PT）

- ![&#x200B; アイコンの修正](../../assets/fix.svg) **カテゴリ表示の改善** – カテゴリ表示の改善<!-- MCLOUD-13752	 - -->
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **管理者キャッシュの改善**-Improve-admin-cache-efficiency CVE-2025-47110.<!-- MCLOUD-13753	 - -->

## v1.1.8

リリース日：2025年6月3日（PT）

- ![&#x200B; アイコンの修正](../../assets/fix.svg) **2.4.8**&#x200B;との互換性を向上させるために、2.4.8<!-- MCLOUD-13707	 - -->との互換性を向上させるために、サードパーティ製ライブラリを更新しました

## v1.1.7

リリース日：2025年5月5日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **Commerce 2.4.4から2.4.8**&#x200B;へのパッチを更新しました。これは、1.1.7<!-- MCLOUD-13619 -->にリリースされた[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch)のパッチです

## v1.1.6

リリース日：2025年4月24日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **Commerce 2.4.4から2.4.7**&#x200B;へのパッチを更新しました。これは、1.1.4<!-- MCLOUD-13240 -->にリリースされた[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)のパッチです

## v1.1.5

リリース日：2025年4月15日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **B2B 1.5.2**&#x200B;のパッチを追加 – B2B モジュール 1.5.2およびMariaDB 10.6<!-- MCLOUD-13605	-->を使用したACP2E-3833の問題を修正

## v1.1.4

リリース日：2025年2月13日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **Commerce 2.4.4から2.4.7**&#x200B;へのパッチが追加されました。この更新プログラムは、[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)にパッチを適用します。<!-- MCLOUD-13240	 - -->

## v1.1.3

リリース日：2025年2月6日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP 8.4** - PHP 8.4のサポートを追加しました。<!-- MCLOUD-13149	 - -->

## v1.1.2

リリース日：2024年11月5日（PT）

- ![fix icon](../../assets/fix.svg) **Commerce 2.4.4から2.4.7**&#x200B;へのパッチが追加されました。このアップデートにより、B2B モジュールを使用する際のAdobe Commerceに関する重要な[CVE-2024-45115](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73)脆弱性が修正されました。<!-- MCLOUD-12980 - -->

## v1.1.1

リリース日：2024年11月5日（PT）

- ![fix icon](../../assets/fix.svg) **Commerce 2.4.4から2.4.7**&#x200B;へのパッチが追加されました。このアップデートは、重要な[CVE-2024-34102](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicStingの脆弱性<!-- MCLOUD-12980 - -->にパッチを適用します。

## v1.1.0

リリース日：2024年10月7日（PT）

- ![fix icon](../../assets/fix.svg) **リファクタリング コード** – 古いPHP バージョン（7.4、7.3、7.2）および関連ライブラリのサポートを削除しました。<!-- MCLOUD-9278 - -->
- ![&#x200B; アイコンの修正](../../assets/fix.svg) **Monolog バージョン**&#x200B;のアップグレード - monolog 3.6.<!-- MCLOUD-12855 - -->のサポートを追加しました
- ![fix icon](../../assets/fix.svg) **Patch for Application Server**:GraphQL Application Serverの既知の問題を解決します。 具体的には、バージョン 2.4.7の`CatalogGraphQl\\Model\\Config\\AttributeReader`には、古い属性の設定に基づいてGraphQL リクエストが応答を取得する可能性があるバグが含まれていました。<!-- ACPT-1876 -->

## v1.0.27

リリース日：2024年5月21日（PT）

- **PHP 8.3**&#x200B;のサポート – このパッチは、php 8.3とコンポーザーパッケージバージョンの互換性エラーを解決します。

## v1.0.26

リリース日：2024年4月8日（PT）

- ![新しいアイコン &#x200B;](../../assets/new.svg) **PHP** — PHP 8.3のサポートを追加しました。

## v1.0.25

リリース日：2024年1月16日（PT）

- **キャッシュの改善** – このパッチは、Adobe Commerce バージョン 2.4.4以降のレイアウトキャッシュの効率性を高め、メモリ使用量を大幅に削減します。<!-- MCLOUD-11514 -->
- **CRON ジョブの機能向上** – このパッチは、Adobe Commerce バージョン 2.4.4以降のCRON ジョブのロックを見逃したジョブが不必要に待つ問題を修正します。<!-- MCLOUD-11329 -->

## v1.0.24

リリース日：2023年9月15日（PT）

- **パフォーマンスの向上** – このパッチは、Adobe Commerce 2.4.6に対して同じデプロイメント設定が読み込まれる回数を2.4.6 ～ 2.4.6-p1<!-- MCLOUD-10604 -->に減らすことで、パフォーマンスに影響を与える問題を修正します

## v1.0.23

リリース日：2023年7月31日（PT）

- **パッチ MCLOUD-10604**&#x200B;を削除しました – このパッチはQPT<!-- MCLOUD-10736 -->に移動しました

## v1.0.22

リリース日：2023年6月19日（PT）

- **拡張QPT CLI ウィザード/出力** - QPT CLI ウィザード/出力に警告を追加しました。依存関係がある場合は、パッチの詳細と要件を確認してください。<!-- ACP2E-1963 -->
- **Commerce 2.4.6のパッチを追加しました：**
   - `regexp cache tag`検証を修正しました。<!-- MCLOUD-10226 -->
   - 同じデプロイメント設定が読み込まれる回数を減らすことにより、パフォーマンスが向上しました。<!-- MCLOUD-10604 -->
- **Commerce 2.3.7から2.4.6**&#x200B;へのパッチを追加しました。`catalog_product_entity_*` テーブルに対して、増分1ではなくランダム値による増分が発生する問題を修正しました。<!-- MCLOUD-10032 -->
- **Commerce 2.4.0のパッチを2.4.6**&#x200B;に追加しました。管理者からJS/CSS キャッシュをフラッシュする際に発生した`The file can't be deleted. Warning!unlink: No such file or directory`というエラーを修正しました。<!-- MCLOUD-10279 -->

## v1.0.21

リリース日：2023年3月10日（PT）

- **PHP 8.2**&#x200B;の拡張サポート - Commerce 2.4.6をサポートする特定のPHP 8.2.x バージョンの互換性の問題を修正しました。

## v1.0.20

リリース日：2022年10月27日（PT）

- **L2 キャッシュの機能強化パッチ**&#x200B;を追加しました。このパッチは、Commerce バージョン 2.4.0および2.4.1のローカル L2 キャッシュのフラッシュに関する問題を修正します。<!-- MCLOUD-7845 -->

## v1.0.19

リリース日：2022年9月13日（PT）

- **PHP 8.1**&#x200B;の拡張サポート – 特定のPHP 8.1.x バージョンとの互換性の問題を修正しました。

## v1.0.18

リリース日：2022年8月11日（PT）

Adobe Commerce 2.4.5のクリティカルパッチ：

- **Braintree支払いを使用した注文に関する問題** – 管理者が新しい注文や再注文を行うことができない重大な問題を解決します。<!-- MCLOUD-9137 -->

Braintree支払いが有効になっている場合、[管理者は注文/再注文を作成できません](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html)を参照してください。

## v1.0.17

リリース日：2022年5月24日（PT）

`patches.json` ファイルのセキュリティパッチの制約を修正しました。

## v1.0.16

リリース日：2022年3月31日（PT）

Adobe Commerce 2.3.3-p1以降のバージョンのクリティカルパッチ：

未認証のリモート コード実行の原因となる&#x200B;**critical**&#x200B;脆弱性を解決するためのパッチを更新しました。<!-- MCLOUD-8479 -->

[Adobe セキュリティ情報APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html)を参照してください。

## v1.0.15

リリース日：2022年3月10日（PT）

- **サポート PHP 8.1** - PHP 8.1のサポートを追加し、PHP 7.0および7.1のサポートを終了しました。
- **Adobe Commerce 2.3.3**&#x200B;のパッチを追加しました – 商品ページに表示される固定通貨。

## v1.0.14

リリース日：2022年2月13日（PT）

Adobe Commerce 2.3.3-p1以降のバージョンのクリティカルパッチ：

**critical**&#x200B;脆弱性を解決するためのパッチを追加しました。これにより、未認証のリモート コードが実行されます。<!-- MCLOUD-8461 -->

[Adobe セキュリティ情報APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html)を参照してください。

## v1.0.13

リリース日：2021年10月25日（PT）

- **モノログの更新** - `monolog` パッケージに必要な最小バージョンを`^2.3`に更新しました。<!-- ACMP-1263 -->
- **互換性のないPHP メソッド** - Adobe Commerce バージョン 2.4.3および2.3.7-p1の互換性のないPHP メソッドを修正しました。<!-- AC-384 -->
- **PHP エラー** - パッチの適用中に発生した`PHP error 'Undefined variable: errorMessage' ...` エラーを修正しました。<!-- ACP2E-138 -->

## v1.0.12

リリース日：2021年8月12日（PT）

Adobe Commerce 2.4.3および2.3.7-p1のクリティカルパッチ：

- **API レート制限に関する問題** – このパッチは、Web APIが配列内の20個を超えるアイテムを含むリクエストを処理できないようにデフォルトのレート制限を修正します。 このパッチは、レート制限のデフォルト値を上げます。 Adobe Commerce [2.4.3 リリースノート &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting)を参照してください。<!-- MC-43048 -->

## v1.0.11

リリース日：2021年7月29日（PT）

- **B2B階層化ナビゲーションパッチを適用したことによる問題を修正**- B2B階層化ナビゲーションパッチを適用したお客様の場合、ストア表示を切り替えた後に検索ページに表示される`Undefined offset` エラーを解決します。<!--MCLOUD-5287-->

- **Paypal チェックアウトパッチ** – 以前に配置された注文価格が表示されるPayPal ExpressのAdobe Commerce 2.3.7の問題を修正します。<!--MC-42674-->

- **パッチカテゴリのサポート** – 品質パッチに割り当てられたパッチカテゴリとオリジンソースの処理のサポートを追加しました。 このカテゴリを使用すると、[品質パッチツール &#x200B;](https://github.com/magento/quality-patches)とサイト全体の分析ツール（SWAT）を使用する際に、フィルターと並べ替えを使用してパッチをより迅速に見つけることができます。<!--MC-38577-->

## v1.0.10

リリース日：2021年5月10日（PT）

- **Adobe Commerce 2.3.7**&#x200B;との互換性 – Adobe Commerce 2.3.7へのインストールに関するコンポーザー依存関係の競合を解決しました。<!--MC-42131-->
- **バンドルパッチを複数回適用することで発生する問題を修正しました** - バンドルパッチ（他の非推奨パッチを含むもの）を複数回適用すると、含まれている非推奨パッケージが元に戻る可能性があります。 すべてのパッチが1回だけ適用されるようになりました。 同じパッケージを再度適用しようとすると、パッチが既に適用されているというメッセージが表示されます。<!--MC-41912-->
- **B2B レイヤーナビゲーションパッチ** - ユーザーがB2B共有カタログを有効にした場合に、レイヤーナビゲーションがすべての製品オプションを表示できない別の問題を修正しました。<!--MCLOUD-7742-->

## v1.0.9

リリース日：2021年2月1日（PT）

- **B2B レイヤーナビゲーションパッチ** - B2B共有カタログが有効になっている場合に、レイヤーナビゲーションがすべての製品オプションを表示できない問題を修正しました。<!--MCLOUD-6923-->
- **PHP 7.4**&#x200B;との互換性 – PHP 7.4とのクラウドパッチの互換性の問題を修正しました。<!--MCLOUD-7367-->
- **非推奨パッチが表示される** – 非推奨パッチの内容全体を含む置換パッチを適用した後、非推奨パッチがパッチテーブルに表示されるクラウドパッチの問題を修正しました。 これは、他の複数のパッチを組み合わせたパッチを適用した場合に発生する可能性があります。<!--MC-40626-->
- **パッチ適用時のサイレントエラー** - `git apply` コマンドが一部の環境でパッチの適用にサイレントエラーが発生するクラウドパッチの問題を修正しました。<!--MC-40529-->

## v1.0.8

リリース日：2020年10月14日（PT）

- **magento/magento-cloud-patches**&#x200B;の互換性アップデート - Adobe Commerce 2.4.1以降のリリースとの互換性を保つため、`composer.json` ファイルの`symfony`および`semver` バージョンの制約を更新しました。<!--MCLOUD-7111-->

## v1.0.7

リリース日：2020年10月14日（PT）

- **Adobe Commerce 2.3.0から2.3.5、2.4.0**&#x200B;のRedis パッチ – レベル 2のキャッシュを実装する際に、商品をカテゴリに追加することをサポートするようにRedis パッチを更新しました。<!--MCLOUD-6659-->

- **Braintree VBE パッチ** – 管理者がBraintree決済レポートを表示しようとしたときにエラーが発生する問題を修正します。<!--MCLOUD-6684-->

- これで、`ece-patches apply` コマンドはUnix `patch` コマンドを使用して、Gitがホストシステムで使用できない場合にパッチを適用します。<!--MCLOUD-7069-->

## v1.0.6

リリース日：

- **Adobe Commerce 2.3.0 ～ 2.3.4**&#x200B;のRedis パッチ – コミュニケーションを最適化し、パフォーマンスを向上
   - RedisとAdobe Commerce間のネットワーク転送のサイズを小さくする
   - Redisの読み込みと書き込み操作に関する競合条件の修正
   - 保存時のエラーを処理するためのベース キャッシュ アダプターの書き換え
   - Redis CPUの消費量を減らす<!--MCLOUD-6139-->

- **Adobe Commerce 2.3.0 ～ 2.3.5**&#x200B;のRedis パッチ – パフォーマンスの向上とエラーの修正
   - キャッシュロックの実装を修正して、無限ロックを防ぐ
   - 現在のロック機構を改善する
   - 署名済みロックを実装して、並列リクエストからのロック解除を防ぐ
   - Redis書き込み操作で発生する次のエラーを修正します：`OOM command not allowed when used memory > maxmemory`
   - 製品の更新中に実行される`cat_p` タグによるクリーンキャッシュの処理を修正<!--MCLOUD-6110-->

- このモジュールを含まないAdobe Commerce v2.2.6または2.3.5を使用して、必要な`amzn/amazon-pay-module` パッチをAdobe Commerce クラウドインフラストラクチャプロジェクトに適用する際にエラーが発生する問題を修正しました。 モジュールがインストールされていない場合、パッチ適用プロセスは`amzn/amazon-pay-module` パッチをスキップします。<!--MCLOUD-6588-->

## v1.0.5

リリース日：2020年6月26日（PT）

- **Redis パフォーマンスの向上** - Redis最適化機能をAdobe Commerce バージョン 2.3.3および2.3.4に追加します。 これらの修正は、Adobe Commerce バージョン 2.3.5 リリースに含まれていました。<!--MCLOUD-5771-->

- **New Relic log enricher** - Commerce バージョン 1.0.4のCloud Componentsで導入されたNew Relic ログ機能の改善をサポートするために必要なMonolog ProcessorInterfaceを追加します。 このパッチは、Adobe Commerce 2.1.xのデプロイに必要です。 パッチが適用されない場合、`di:compile` プロセス中にビルドが失敗します。<!--MCLOUD-6029-->

## v1.0.4

リリース日：2020年5月12日（PT）

- **Amazon Pay チェックアウト** - Amazon Pay支払いウィジェットで、チェックアウトプロセス中に&#x200B;_Review &amp; Payments_ ステップでお客様が支払い方法を変更できなかった問題を修正します。<!--MCLOUD-5930-->

- **カテゴリーページでの製品表示** - _すべてのページを表示_ ビューでカテゴリーページに製品を表示できない問題を修正します。<!--MCLOUD-5684-->

- **ページビルダー画像のアップロード** – 画像を画像ギャラリーにアップロードする際に次のエラーが発生することがあるページビルダーのインターフェイスの問題を修正します。`Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **不要なサイトマップ生成警告を抑制** – サイトマップ生成中にエラーが発生した場合に再試行を追加し、エラーが自動的に回復できる場合に顧客のメール通知をスキップします。<!--MCLOUD-3025-->

- **サイトパフォーマンスの向上** - サイトパフォーマンスに影響を与える長い読み込み時間が定期的に発生していた`Magento\Framework\App\DeploymentConfig\Reader::load`関数のパフォーマンスの問題を修正します。<!--MCLOUD-5650-->

- 支払い方法のパッチ割り当てを更新しました。Magentoの基本パッケージ（magento/magento2-base）ではなく支払い方法をターゲットにして、支払い方法が存在する場合にのみ支払いパッチが適用されるようにしました。<!--MCLOUD-5666-->

- Magento Open Sourceと互換性のあるパッチを更新しました。<!--MCLOUD-5701-->

## v1.0.3

リリース日：2020年4月28日（PT）

- Adobe Commerce 2.3.5をサポートするための「デプロイ中にFPCが無効になる」パッチの修正を追加しました。

## v1.0.2

リリース日：2020年2月27日（PT）

このリリースには、次のパッチと重要な修正が含まれています。

- **magento/magento-cloud-patches**&#x200B;の互換性アップデート

   - Adobe Commerce 2.4以降のリリースとの互換性を保つため、`composer.json` ファイルの`symfony`および`semver` バージョンの制約を更新しました。<!--MAGECLOUD-5127-->

   - 2002.0.22以降の2002.0.x リリースと互換性を保つため、`composer.json`の制約を更新しました。`ece-tools`

- **PayPal Express Checkout** - 2020年2月12日に公開されたこのパッチは、PayPal Express Checkoutでの注文に影響する問題を解決します。注文の配送先住所で、配送ページのドロップダウンメニューから選択するのではなく、テキストフィールドに手動で入力された国の地域を指定します。 パッチの完全な説明については、パッチのダウンロードページを参照してください。

- **アプリケーション展開の修正** – 展開プロセス中に完全なページキャッシュを無効にした問題を修正するためのパッチを追加しました。 このパッチは、Adobe Commerce 2.3.2以降のリリースに適用されます。

- 非同期/バルク API **の** スコープ パラメーター – `composer.json` ファイルの構文エラーを修正するためにこのパッチを更新しました。 このパッチは、Magento Open Source 2.3.1および2.3.2に適用されます。 パッチの完全な説明については、パッチのダウンロードページを参照してください。

## v1.0.1

リリース日：2020年2月6日（PT）

magento/magento-cloud-patches v1.0.1 リリースのソフトウェアダウンロードページから、すべてのMagento Open Source 2.x パッチを含めました。 以前にパッチをプロジェクトにコピーしたことがある場合は、パッチを削除して競合を回避します。

このリリースには、次のパッチと重要な修正が含まれています。

- **cron デッドロックを修正し、cron ロックを改善**—

   - `cron_schedule` テーブルのステータス値が正しくないため、一部のcron ジョブが実行されない問題を修正しました。 ここでは、Adobe Commerce ロックフレームワークを使用して、`cron_schedule` テーブルを使用する代わりに、cron ジョブのステータスを確認および更新します。 エラーステータスで終了したCron ジョブは、24時間待つのではなく、次のcron実行時に再試行されます。

   - `cron_schedule` テーブルのデータの更新中にデッドロックを回避するために、_再試行_&#x200B;操作を追加します。

- **Magento Open Source 2.x**&#x200B;のすべての利用可能なパッチを含めるように`magento/magento-cloud-patches`を更新しました – magento/magento-cloud-patches パッケージを更新して、ソフトウェアのダウンロードページで利用可能なすべてのMagento Open Source 2.x パッチを含めました。 以前にAdobe Commerce on cloud infrastructure プロジェクトにMagento Open Sourceのパッチをコピーした場合は、それらを削除して競合を回避します。<!--MAGECLOUD-4606-->

- **Elasticsearch カタログのページネーション修正** —magento/magento-cloud-patches v1.0で配信されたElasticsearch カタログのページネーション パッチを、より効果的な修正に置き換えました。<!--MAGECLOUD-4847-->

- **Page Builder パッチ** - Commerce 1.0.0のCloud パッチでは、既知のPage Builder リモートコード実行（RCE）の脆弱性に対処するためのPage Builder パッチがバンドルされ、最初の修正はAdobe Commerce 2.3.3に基づいています。 Adobe Commerce 2.3.4に基づいてより安定した実装で、これらのパッチを更新しました。これには、問題を修正するための複数の最適化が含まれています。<!--MAGECLOUD-4884-->

  magento/magento-cloud-patches 1.0.0 パッケージがある場合でも、ページビルダーのRCE脆弱性の問題から保護されます。 1.0.1以降にアップデートした場合は、同じ修正をより適切に実装できます。

## v1.0.0

リリース日：2019年11月14日（PT）

これは、[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) パッケージの最初のリリースで、`ece-tools` パッケージ バージョン 2002.0.22以降のリリースの新しい依存関係です。

このリリースには、次のパッチと重要な修正が含まれています。

- **Page Builderの2.3.1.xおよび2.3.2.x リリース**&#x200B;のセキュリティパッチ - Page Builder プレビューで、未認証のユーザーが一部のテンプレートメソッドにアクセスできる問題を修正しました。このテンプレートメソッドは、ネットワーク （RCE）経由での任意のコード実行をトリガーするために使用でき、グローバルな情報漏洩が発生します。 この問題は、Adobe Commerce バージョン 2.3.1および2.3.2でサポートされていないバージョンのPage Builderを使用している場合に発生する可能性があります。<!--MAGECLOUD-4649-->

- **MSI パッチ** – 在庫の管理に既定の在庫設定を使用する際にインデックス作成エラーやパフォーマンスの問題が発生する問題を修正します。<!--MAGECLOUD-4428-->

- **新しいメールインターフェイスの後方互換性**- Adobe Commerce v2.3.3で導入された`Magento\Framework\Mail\EmailMessageInterface` PHP インターフェイスに起因する後方互換性の問題を修正します。 このパッチの適用範囲では、新しい`EmailMessageInterface`は古い`MessageInterface`から継承し、Adobe Commerce コアモジュールは`MessageInterface`に依存するように元に戻されます。<!--MAGECLOUD-4422-->

- **Elasticsearch 6.x**&#x200B;でカタログページネーションが機能しない – Elasticsearch 6.xをカタログ検索エンジンとして使用しているお客様に影響を与える検索結果のページネーションに関する重大な問題を修正します。<!--MAGECLOUD-4448-->
