---
source-git-commit: eab32e2f5155e3aa6c0924fa332cbef6e4dc502d
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---
# 貢献

ご協力いただきありがとうございます！

以下は、このプロジェクトに貢献する際に従うべき一連のガイドラインです。

## 行動規範

このプロジェクトは、Adobe [行動規範](code-of-conduct.md)に準拠しています。 参加することで，
あなたはこの暗号を守るべきだ。 許容できない動作を次に報告してください
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com)。

## コントリビューターガイドのドキュメント

[&#x200B; コントリビューターガイド &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html)を参照してください。

## よくある質問と？

問題を報告することから始めます。 このプロジェクトの既存のコミッターは
プロジェクトの方向性に関するコンセンサスと、イシュースレッド内のイシューソリューション
（適切な場合）。

## コントリビューターライセンス契約

このプロジェクトへのすべての第三者による寄付には、署名済みの寄稿者が同行する必要があります
ライセンス契約： これにより、Adobeにコントリビューションを再配布する権限が付与されます
必要だということです。 [CLAに署名](https://opensource.adobe.com/cla.html)。 あなた
Adobe CLAを1回送信するだけで済むため、以前に送信したことがある場合，
行くといいね。

## コードレビュー

すべての提出物はプルリクエストの形式で提出され、レビューする必要があります
見つける必要がありました。 [GitHubのプルリクエストドキュメントを読む](https://docs.github.com/articles/about-pull-requests/)
プルリクエストの送信について詳しくは、こちらを参照してください。

最後に、[&#x200B; プルリクエストテンプレート &#x200B;](PULL_REQUEST_TEMPLATE.md)に従ってください。
プルリクエストを送信しています！

## コントリビューターからコミッターへ

私たちのコミュニティからの寄付が大好きです！ コントリビューターから一歩進んで
プロジェクトで完全な書き込みアクセスと発言権を持つコミッターになるには、次のことが必要です
プロジェクトに招待されます。 既存のコミッターは内部指名を採用する
招待する前に遅延コンセンサスに達する必要があるプロセス（無音は承認です）
が発行されます。 自分が適格であり、もっと深く関わりたいと思うのであれば，
ぜひ既存のコミッターにご連絡いただき、それについてご相談ください。

## セキュリティの問題

セキュリティ上の問題は、この問題トラッカーで報告しないでください。 代わりに、[&#x200B; セキュリティのエキスパートに問題を報告してください](https://helpx.adobe.com/security/alertus.html)

## 新機能ハイライト

変更によって新しいトピック、重要な更新、または修正がハイライト表示される必要がある場合は、プルリクエストの本文から[新機能](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/overview#whats-new)に簡単な説明を追加できます。

新機能ハイライトを追加するには：

1. 最後に、プルリクエスト本文に適切な説明を含めた`whatsnew` タグを含めます。 説明では、変更に関するコンテキストと、ターゲットのトピックまたはトピックへのリンクを提供する必要があります。 次の形式を使用します（コードブロックの引用符は表現用のみです。プルリクエスト本文には含めないでください）。

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/target-topic.html).
   ```

   または、複数のトピックがある場合：

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/third-target-topic.html).
   ```

   複数のハイライトにリストを使用することもできます。

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/second-topic.html).
   ```

1. 変更のタイプを示すサポートされているラベルを追加します。 サポートされているラベルには、次のような変更の各タイプのラベルが含まれます。

   - `new-topic` – 新しいトピック用
   - `major-update` - コンテンツ、構造、または機能の大幅な変更が含まれる可能性があるメジャーアップデートの場合
   - `technical` - メジャーアップデートとは見なされないが、注意が必要な技術変更の場合

**重要：**

1. `whatsnew`部分は`whatsnew` タグから開始し、プルリクエスト本文の最後にある必要があります。
1. 変更内容の説明には、作業リンクが含まれている必要があります。 リンクが正しいことを確認し、意図されたトピックにつながってください。 トピックが新しい場合は、プルリクエストをマージして新しいトピックを公開した後、リンクが機能していることを確認します。 プルリクエストがマージされた後にリンクを修正しても問題ありません。

例えば、リポジトリ内のクローズしたプルリクエストで検索して、既存のハイライトがどのようにフォーマットされているかを確認し、[新機能](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/overview#whats-new)と比較して、ドキュメントでどのように表示されるかを確認します。
