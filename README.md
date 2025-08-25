---
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 1%

---
# クラウドインフラストラクチャー上のAdobe Commerce

このサイトには、クラウドインフラストラクチャー上のCommerceに関する最新の開発者向けドキュメントが含まれています。

- [ クラウドインフラストラクチャー上のCommerceガイド ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/overview)
- クラウドインフラストラクチャー上での [Commerceの概要 ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/overview)

## Adobe オープン Source行動規範

このプロジェクトでは、[Adobe オープン Source行動規範 ](code-of-conduct.md) を採用しています。 詳しくは、[投稿](contributing.md)の記事を参照してください。

## Adobe コンテンツへの投稿について

[Adobe ドキュメント投稿者ガイドを参照してください ](https://experienceleague.adobe.com/en/docs/contributor/contributor-guide/introduction)。

投稿方法は、投稿者と、投稿したい変更の種類に応じて異なります。

### 軽微な変更

軽微な変更をコントリビューションする場合は、記事にアクセスして記事の下部に表示されるフィードバックエリアをクリックし、**詳細なフィードバックオプション** をクリックします。次に、**編集の提案** をクリックして、GitHub の Markdown ソースファイルに移動します。 GitHub UI を使用して更新を行います。

このリポジトリのドキュメントおよびコード例について投稿者が送信した軽微な修正や説明は、Adobeの利用規約の対象となります。

### コミュニティメンバーによる大幅な変更または新しい記事

Adobe コミュニティのメンバーが新しい記事を作成したり、大きな変更をコントリビューションしたりする場合は、Git リポジトリーの「イシュー」タブを使用してイシューを送信し、ドキュメントチームとのやり取りを開始してください。 計画に同意したら、公開および非公開リポジトリでの作業を組み合わせて新しいコンテンツを取り込むために、従業員と協力する必要があります。

### Adobe社員からの大きな変化

Adobe Experience Cloud ソリューションの製品チームのテクニカルライター、プログラムマネージャー、または開発者で技術記事の投稿または作成を担当している場合は、`https://git.corp.adobe.com/AdobeDocs` のプライベートリポジトリを使用する必要があります。

## ツールと設定

コミュニティのコントリビューターは、基本的な編集を行う場合は GitHub UI を使用し、大きな変更を加える場合はリポジトリをフォークします。

詳しくは、[Adobe ドキュメント投稿者ガイド ](https://experienceleague.adobe.com/en/docs/contributor/contributor-guide/introduction) を参照してください。

## Markdown を使用してトピックを書式設定する方法

このリポジトリ内の記事はすべて、GitHub Flavored Markdown を使用しています。 Markdown について詳しくは、以下を参照してください。

- [Markdown の基本 ](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [ 印刷用 Markdown チートシート ](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## テンプレート

一部のトピックでは、データファイルとテンプレートを使用して、公開済みコンテンツを生成します。 このアプローチのユースケースを次に示します。

- プログラムで生成された大きなコンテンツセットの公開
- 統合のために、YAML などの機械読み取り可能なファイル形式を必要とする複数のシステム（クラウド CLI ツール、サービス設定など）をまたいで、顧客に単一の情報源を提供する

テンプレート化されたコンテンツの例としては、次のようなものがあります（ただし、これに限定されません）。

- [Cloud CLI リファレンス](help/templated/cloud-cli-ref.md)
- [クラウドパッケージ](help/templated/cloud-packages.md)
- [ECE ツールリファレンス](help/templated/ece-tools.md)
- [クラウド用 PHP 拡張機能](help/templated/php-extensions-cloud.md)

### テンプレートコンテンツの生成

通常、ほとんどのライターは、製品可用性テーブルとシステム要件テーブルにリリースバージョンを追加するだけで済みます。 その他のテンプレート化されたコンテンツのメンテナンスは、すべて専任のチームメンバーが自動化または管理します。 これらの説明は、ほとんどのライターを対象としています。

>**メモ：**
>
>- テンプレート化されたコンテンツを生成するには、ターミナルのコマンドラインで作業する必要があります。
>- レンダリングスクリプトを実行するには、Ruby がインストールされている必要があります。 必要なバージョンについては [_jekyll/.ruby-version] (_jekyll/.ruby-version) を参照してください。

テンプレート化されたコンテンツのファイル構造について詳しくは、次を参照してください。

- `_jekyll` - テンプレート化されたトピックと必要なアセットが含まれます
- `_jekyll/_data` - テンプレートのレンダリングに使用される機械読み取り可能なファイル形式が含まれます
- `_jekyll/templated` – 液体テンプレート言語を使用するHTMLベースのテンプレートファイルが含まれます
- `help/_includes/templated` - テンプレート化されたコンテンツの生成された出力をファイル形式で含 `.md`、Experience League トピックで公開できるようにします。レンダリング・スクリプトは、生成された出力をこのディレクトリに自動的に書き込みます

テンプレート化されたコンテンツを更新するには：

1. テキストエディターで、`_jekyll/_data` ディレクトリにあるデータファイルを開きます。 例：

   - [Cloud CLI リファレンス ](help/templated/cloud-cli-ref.md):`_jekyll/_data/cloud-cli-ref.yaml`
   - [ クラウドパッケージ ](help/templated/cloud-packages.md):`_jekyll/_data/cloud-packages.yaml`
   - [ECE ツールリファレンス ](help/templated/ece-tools.md):`_jekyll/_data/ece-tools.yaml`

2. 既存の YAML 構造を使用してエントリを作成します。

3. `_jekyll` ディレクトリに移動します。

   ```bash
   cd _jekyll
   ```

4. テンプレート化されたコンテンツを生成し、出力を `help/_includes/templated` ディレクトリに書き込みます。

   ```bash
   rake render
   ```

   >**注意：** スクリプトは `_jekyll` ディレクトリから実行する必要があります。 スクリプトを初めて実行する場合は、`bundle install` コマンドを使用して Ruby の依存関係をインストールする必要があります。

5. `root` ディレクトリに戻ります。

   ```bash
   cd ..
   ```

6. 期待される `help/_includes/templated` ファイルが変更されたことを確認します。

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

[ データファイル ](https://jekyllrb.com/docs/datafiles)、[ 液体フィルター ](https://jekyllrb.com/docs/liquid/filters/) およびその他の機能について詳しくは、Jekyll のドキュメントを参照してください。
