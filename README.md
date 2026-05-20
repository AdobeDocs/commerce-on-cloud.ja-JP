---
source-git-commit: 55206749cd121ef6d6139a71af6ff905d4109859
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 0%

---
# Adobe Commerce on Cloud Infrastructure

このサイトには、Cloud Infrastructure上のCommerceの最新の開発者向けドキュメントが含まれています。

- [Commerce オンクラウドインフラストラクチャガイド](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/overview)
- [&#x200B; クラウドインフラストラクチャ上のCommerce](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/start/overview)の基本を学ぶ

## AdobeオープンSource行動規範

このプロジェクトでは、[Adobe オープン Source行動規範](code-of-conduct.md)を採用しています。 詳しくは、[寄付](contributing.md)の記事を参照してください。

## Adobe コンテンツへのコントリビューションについて

[Adobe Docs Contributor Guide](https://experienceleague.adobe.com/ja/docs/contributor/contributor-guide/introduction)を参照してください。

貢献の方法は、貢献するユーザーと変更の種類によって異なります。

### 軽微な変更

マイナーアップデートを投稿する場合は、記事にアクセスし、記事の下部に表示されるフィードバック領域をクリックし、**詳細なフィードバックオプション**&#x200B;をクリックしてから、**編集を提案**&#x200B;をクリックして、GitHubのマークダウンソースファイルに移動します。 GitHub UIを使用して更新します。

このリポジトリのドキュメントやコード例に対して提出する軽微な修正や明確化は、Adobe利用条件の対象となります。

### コミュニティメンバーからの主な変更点や新しい記事

Adobe コミュニティに参加していて、新しい記事を作成したり、大きな変更を送信したりしたい場合は、Git リポジトリの「イシュー」タブを使用してイシューを送信し、ドキュメントチームとの会話を開始してください。 計画に同意したら、公開リポジトリと非公開リポジトリの作業を組み合わせて、新しいコンテンツを取り込むために従業員と協力する必要があります。

### Adobe社員の主な変化

テクニカルライター、プログラムマネージャー、またはAdobe Experience Cloud ソリューションのプロダクトチームの開発者で、技術記事の投稿や作成を担当する場合は、`https://github.com/Adobe-Enterprise-Docs/commerce-on-cloud.ja-JP`のプライベートリポジトリを使用する必要があります。

## ツールと設定

コミュニティのコントリビューターは、GitHub UIを使用して基本的な編集をおこなったり、リポジトリをフォークして主要なコントリビューションを作成したりできます。

詳しくは、[Adobe Docs Contributor Guide](https://experienceleague.adobe.com/ja/docs/contributor/contributor-guide/introduction)を参照してください。

## Markdownを使用してトピックを書式設定する方法

このリポジトリ内のすべての記事では、GitHubのマークダウンを使用しています。 Markdownに詳しくない場合は、以下を参照してください。

- [Markdownの基本](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [印刷可能なマークダウンのチートシート](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## テンプレート

一部のトピックでは、データファイルとテンプレートを使用して、公開されたコンテンツを生成します。 このアプローチのユースケースは、次のとおりです。

- プログラムで生成された大規模なコンテンツセットの公開
- 統合のためにYAMLなどの機械読み取り可能なファイル形式（クラウド CLI ツール、サービス設定など）を必要とする複数のシステムをまたいで、顧客に信頼できる唯一の情報源を提供する

テンプレート化されたコンテンツの例としては、次のようなものがありますが、これらに限定されません。

- [Cloud CLI リファレンス](help/templated/cloud-cli-ref.md)
- [クラウドパッケージ](help/templated/cloud-packages.md)
- [ECE ツールリファレンス](help/templated/ece-tools.md)
- [クラウド向けPHP拡張機能](help/templated/php-extensions-cloud.md)

### テンプレート化されたコンテンツの生成

一般的に、ほとんどのライターは、製品可用性テーブルとシステム要件テーブルにリリースバージョンを追加するだけで済みます。 その他のテンプレート化されたコンテンツのメンテナンスは、自動化されるか、専用のチームメンバーによって管理されます。 これらの手順は、ほとんどの作成者を対象としています。

>**メモ：**
>
>- テンプレート化されたコンテンツを生成するには、ターミナルのコマンドラインで作業する必要があります。
>- レンダリングスクリプトを実行するには、Rubyがインストールされている必要があります。 必要なバージョンについては、[_jekyll/.ruby-version] (_jekyll/.ruby-version)を参照してください。

テンプレート化されたコンテンツのファイル構造については、次を参照してください。

- `_jekyll` - テンプレート化されたトピックと必要なアセットが含まれています
- `_jekyll/_data` - テンプレートのレンダリングに使用される機械読み取り可能なファイル形式が含まれます
- `_jekyll/templated` - Liquid テンプレート言語を使用するHTML ベースのテンプレートファイルが含まれます
- `help/_includes/templated` - テンプレート化されたコンテンツの生成された出力が`.md` ファイル形式で含まれるため、Experience League トピックで公開できます。レンダリングスクリプトは、生成された出力をこのディレクトリに自動的に書き込みます

テンプレート化されたコンテンツを更新するには：

1. テキストエディターで、`_jekyll/_data` ディレクトリのデータファイルを開きます。 例：

   - [Cloud CLI リファレンス &#x200B;](help/templated/cloud-cli-ref.md): `_jekyll/_data/cloud-cli-ref.yaml`
   - [&#x200B; クラウドパッケージ &#x200B;](help/templated/cloud-packages.md): `_jekyll/_data/cloud-packages.yaml`
   - [ECE ツール参照](help/templated/ece-tools.md): `_jekyll/_data/ece-tools.yaml`

2. 既存のYAML構造を使用して、エントリを作成します。

3. `_jekyll` ディレクトリに移動します。

   ```bash
   cd _jekyll
   ```

4. テンプレート化されたコンテンツを生成し、出力を`help/_includes/templated` ディレクトリに書き込みます。

   ```bash
   bundle exec rake render
   ```

   >**メモ：** スクリプトを`_jekyll` ディレクトリから実行する必要があります。 スクリプトを初めて実行する場合は、まず`bundle install` コマンドを使用してRuby依存関係をインストールする必要があります。 コアレイクタスクと依存関係（Jekyll、Rake、Image Optimization）は、Adobe Commerce ドキュメントリポジトリ全体のメンテナンス性を向上させるために`adobe-comdox-exl-rake-tasks` gemによって提供されます。 このリポジトリに固有のカスタムタスクは、`Rakefile`に実装されます。

5. `root` ディレクトリに戻ります。

   ```bash
   cd ..
   ```

6. 予想される`help/_includes/templated` ファイルが変更されたことを確認します。

   ```bash
   git status
   ```

   次のような出力が表示されます。

   ```bash
   modified:   _data/cloud-cli-ref.yaml
   modified:   help/_includes/templated/cloud-cli-ref.md
   ```

7. 変更をプッシュします。

   ```bash
   git add .
   git commit -m "descriptive message of the intended commit"
   git push
   ```

[&#x200B; データファイル &#x200B;](https://jekyllrb.com/docs/datafiles)、[液体フィルター](https://jekyllrb.com/docs/liquid/filters/)およびその他の機能について詳しくは、Jekyllのドキュメントを参照してください。

## 使用可能なレイク タスク

このリポジトリは、`adobe-comdox-exl-rake-tasks` gemが提供するrake タスクを使用します。 使用可能なすべてのタスクを表示するには、次を実行します。

```bash
cd _jekyll
bundle exec rake --tasks
```

## 画像の最適化のためのプリコミットフック

このリポジトリには、コミット前に画像を最適化する自動のプリコミットフックが含まれています。 **すべてのコントリビューターは、一貫した画像の最適化とリポジトリサイズの削減を確実にするために、これらのフック**&#x200B;を有効にする必要があります。

### クイック設定

リポジトリのクローンを作成したら、次を実行します。

```bash
.githooks/setup-hooks.sh
```

### フックの機能

- ステージングされた画像ファイルを自動検出（PNG、JPG、JPEG、GIF、SVG）
- `image_optim`を実行して画像を圧縮および最適化
- 最適化された画像を自動的にリステージ
- コミットされたすべての画像が適切に最適化されていることを確認します

### Adobe Workfrontの利点

- リポジトリサイズの削減
- ドキュメントのページ読み込みを高速化
- あらゆる貢献者で一貫した画質
- 手作業による最適化は不要です

セットアップ手順、トラブルシューティング、設定の詳細については、[`.githooks/README.md`](.githooks/README.md)を参照してください。
