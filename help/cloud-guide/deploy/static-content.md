---
title: 静的コンテンツデプロイメント
description: 画像、スクリプト、CSS などの静的コンテンツをクラウドインフラストラクチャプロジェクト上のAdobe Commerceにデプロイする方法について説明します。
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 静的コンテンツのデプロイメント戦略

静的コンテンツのデプロイメント（SCD）は、画像、スクリプト、CSS、ビデオ、テーマ、ロケール、web ページなどの生成するコンテンツの量と、コンテンツを生成するタイミングに依存するストアデプロイメントプロセスに大きな影響を与えます。 例えば、デフォルトの方法では、サイトがメンテナンスモードの場合、[ デプロイフェーズ ](process.md#deploy-phase-deploy-phase) の間、静的コンテンツが生成されます。ただし、この方法では、マウントされた `pub/static` ディレクトリにコンテンツを直接書き込むのに時間がかかります。 必要に応じて、デプロイメント時間を短縮するのに役立つオプションや戦略がいくつかあります。

## JavaScriptおよびHTML コンテンツの最適化

静的コンテンツのデプロイメント中にバンドルおよび縮小を使用して、最適化されたJavaScriptおよびHTML コンテンツを構築できます。

### コンテンツの縮小

デプロイメントディレクトリ内の静的ビューファイルのコピーをスキップし、必要に応じてHTMLを _縮小_ 生成すると、デプロイメントプロセス中の SCD の読み込み時間を短縮で `var/view_preprocessed` ます。 この機能を有効にするには、`.magento.env.yaml` ファイルで、グローバル環境変数 [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) を `true` に設定します。

>[!NOTE]
>
>`ece-tools` パッケージバージョン 2002.0.13 以降、SKIP_HTML_MINIFICATION 変数のデフォルト値は `true` に設定されています。

不要なテーマファイルの数を減らすことで、デプロイメント時間とディスク領域を **より** 節約できます。 例えば、`magento/backend` のテーマを英語でデプロイし、カスタムテーマを他の言語でデプロイすることができます。 これらのテーマ設定は、環境変数 [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix) を使用して設定できます。

## デプロイ方法の選択

デプロイメント戦略は、_ビルド_ フェーズ、_デプロイ_ フェーズ、または _オンデマンド_ で静的コンテンツを生成することを選択するかどうかによって異なります。 次のチャートに示すように、デプロイフェーズでは静的コンテンツを生成することが、最適な選択ではありません。 HTMLが縮小されていても、各コンテンツファイルをマウントされた `~/pub/static` ディレクトリにコピーする必要があり、時間がかかる場合があります。 静的コンテンツをオンデマンドで生成することは、最適な選択と思われます。 ただし、コンテンツファイルがキャッシュに存在しない場合は、リクエストされた瞬間に生成されるので、読み込み時間がユーザーエクスペリエンスに長くなります。 したがって、ビルドフェーズでは静的コンテンツを生成するのが最適です。

![SCD 荷重比較 ](../../assets/scd-load-times.png)

### ビルド時の SCD の設定

縮小されたHTMLを使用したビルドフェーズでの静的コンテンツの生成は、[**ダウンタイムなし** デプロイメント ](reduce-downtime.md)、または **理想的な状態** に最適な設定です。 マウントされたドライブにファイルをコピーする代わりに、`./init/pub/static` ディレクトリからシンボリックリンクを作成します。

静的コンテンツを生成するには、テーマとロケールにアクセスする必要があります。 Adobe Commerceは、テーマをファイルシステムに格納します。これはビルドフェーズでアクセスできますが、Adobe Commerceはデータベースにロケールを格納します。 データベースは、ビルドフェーズでは使用 _できません_。 ビルドフェーズで静的コンテンツを生成するには、`ece-tools` パッケージで `config:dump` コマンドを使用してロケールをファイルシステムに移動する必要があります。 ロケールが読み取られ、`app/etc/config.php` ファイルに保存されます。

>[!NOTE]
>`ece-tools` パッケージで `config:dump` コマンドを実行すると、`config.php` ファイルにダンプされた設定は [ 管理ダッシュボードではロック（グレー表示）されます ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin)。 管理者でこのような設定を更新する唯一の方法は、ファイルからローカルに設定を削除し、プロジェクトを再デプロイすることです。
>&#x200B;>また、インスタンスに新しいストア/ストアグループ/web サイトを追加するたびに、`config:dump` コマンドを実行してデータベースが同期されていることを確認する必要があります。 また、`config.php` ファイルに [ ダンプする設定 ](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en) を選択することもできます。
>&#x200B;>フィールドがグレー表示されていてこの手順を実行しなかったため、`config.php` ファイルからストア/ストアグループ/web サイト設定を削除した場合、ダンプされなかった新しいエンティティは、次のデプロイメント時にデータベースから削除されます。

**ビルド時に SCD を生成するようにプロジェクトを設定するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。
1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. ロケールをファイルシステムに移動し、[`config.php` ファイルを更新します ](../development/commerce-version.md#create-a-configphp-file)。

1. `.magento.env.yaml` 設定ファイルには、次の値を含める必要があります。

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) は `true` です
   - ビルド ステージの [SKIP_SCD](../environment/variables-build.md#skip_scd) は `false` です
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) は `compact` です

1. `.magento.app.yaml` ファイルでの [ デプロイ後のフック ](../application/hooks-property.md) の設定を確認します。

1. [ 理想的な状態のスマート ウィザード ](smart-wizards.md) を実行して、設定を確認します。

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### SCD のオンデマンド設定

SCD をオンデマンドで生成することは、統合環境における開発ワークフローに最適です。 デプロイメント時間が短縮されるので、実装をすばやく確認したり、統合テストを実行したりできます。 `.magento.env.yaml` ファイルのグローバルステージで [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) 環境変数を有効にします。 変数 SCD_ON_DEMAND は、SCD に関連するその他すべての設定をオーバーライドし、`~/pub/static` ディレクトリの既存のコンテンツを消去します。

SCD オンデマンド戦略を使用する場合、ホームページなど、要求するページをキャッシュにプリロードすると便利です。 `.magento.env.yaml` ファイルのデプロイ後のステージの [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) 環境変数に、必要なページのリストを追加します。

>[!WARNING]
>
>実稼動環境では SCD のオンデマンド戦略を使用しないでください。

### SCD をスキップしています

静的コンテンツの生成を完全にスキップすることもできます。 [SKIP_SCD](../environment/variables-build.md#skipscd) 環境変数をグローバルステージで設定して、SCD に関連する他の設定を無視できます。 これは、`~/pub/static` ディレクトリ内の既存のコンテンツには影響しません。
