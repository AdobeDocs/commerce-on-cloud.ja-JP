---
source-git-commit: 305df8acb8c41369b34e2c20d9c7df6824222931
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---
# 貢献

ご協力いただきありがとうございます。

このプロジェクトに投稿する際のガイドラインを次に示します。

## 行動規範

このプロジェクトはアドビ[行動規範](code-of-conduct.md)を遵守しています。参加することにより、
この行動規範を遵守することが求められます。 受け入れがたい行動を見かけた場合は
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com)。

## 投稿者ガイドドキュメント

詳しくは、[&#x200B; 投稿者ガイド &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=ja) を参照してください。

## 質問がある場合

まず、イシューを入力します。 このプロジェクトの既存のコミッターは、次の項目に到達するよう取り組みます
イシュースレッド内のプロジェクトの方向性とイシューの解決策に関する合意
（該当する場合）。

## コントリビューター使用許諾契約

このプロジェクトへのサードパーティ投稿者には、署名済みの投稿者が関連付けられている必要があります
使用許諾契約。 これにより、Adobeに投稿を再配布する権限が付与されます
プロジェクトの一環として。 [&#x200B; アドビの CLA に署名してください &#x200B;](https://opensource.adobe.com/cla.html)。 あなた
Adobe CLA の送信は 1 回だけでかまいません。したがって、以前に送信したことがある場合は、
行ってもよろしい。

## コードレビュー

すべての送信は、プルリクエスト形式でおこなわれ、レビューする必要があります
プロジェクト コミッター別 [GitHub のプルリクエストドキュメントをお読みください &#x200B;](https://docs.github.com/articles/about-pull-requests/)
プルリクエストの送信に関する詳細情報。

最後に、次の場合は [&#x200B; プルリクエストテンプレート &#x200B;](PULL_REQUEST_TEMPLATE.md) に従ってください
プルリクエストを送信しています！

## 投稿者からコミッターへ

アドビはコミュニティからの投稿を歓迎しています。 コントリビューターから一歩進みたい場合
そして、プロジェクトにおける完全な書き込みアクセス権と発言権を持つコミッターになる必要があります
プロジェクトに招待される。 既存のコミッターは、内部ノミネーションを採用しています
招待状の前に怠惰なコンセンサス（沈黙は承認）に到達しなければならないプロセス
が発行されました。 自分に適性があり、さらに深く関わりたいと思われるなら、
自由に既存のコミッターに連絡して、それについて話し合ってください。

## セキュリティ上の問題

セキュリティ上の問題は、このイシュートラッカーでは報告しないでください。 代わりに、[&#x200B; セキュリティの専門家に問題を提起してください &#x200B;](https://helpx.adobe.com/jp/security/alertus.html)

## 新機能

変更によって、強調表示する必要のある新しいトピック、重要な更新、修正が導入された場合は、プルリクエストの本文から [&#x200B; 新機能 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/overview#whats-new) のセクションに簡単な説明を追加できます。

新機能のハイライトを追加するには：

1. `whatsnew` タグと適切な説明を、最後にプルリクエスト本文に含めます。 説明には、変更に関するコンテキストと、ターゲットのトピックまたはトピックへのリンクを指定する必要があります。 次の形式を使用します（コードブロックの引用符は表示専用であり、プルリクエスト本文に含めないでください）。

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html).
   ```

   または、複数のトピックがある場合は、次の操作を行います。

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/third-target-topic.html).
   ```

   複数のハイライトにリストを使用することもできます。

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

1. 変更のタイプを示す、サポートされるラベルを追加します。 サポートされているラベルには、変更のタイプごとに次のようなラベルが含まれています。

   - `new-topic` – 新しいトピック用
   - `major-update` - コンテンツ、構造、機能に大幅な変更を含む可能性のある大規模な更新の場合
   - `technical` - メジャーアップデートとは見なされないが、注意が必要な技術的な変更

**重要：**

1. `whatsnew` 部は、`whatsnew` タグから始めて、プルリクエスト本文の最後になければなりません。
1. 変更の説明には、作業リンクを含める必要があります。 リンクが正しいことを確認し、目的のトピックに導いてください。 トピックが新規の場合は、プルリクエストを結合して新しいトピックを公開した後、リンクが機能していることを確認します。 プルリクエストが結合された後は、リンクを修正しても問題ありません。

例えば、リポジトリ内でクローズドなプルリクエストを検索して既存のハイライトの書式を確認し、[&#x200B; 新機能 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-cloud-service/user-guide/overview#whats-new) の節と比較して、ドキュメントにどのように表示されるかを確認します。
