---
title: 調査プレイブック
description: Adobe Commerce Traffic Insightsを使用して、CDN帯域幅の超過、検索ボットとweb クローラーの負荷、悪意のあるトラフィックを調査する方法と、エスカレーションするタイミングについて説明します。
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# 調査プレイブック

[!DNL Adobe Commerce Traffic Insights] アプリは、次の問題の調査に役立つように設計されています。

- 帯域幅の超過
- Web クローラー負荷
- 悪意のあるトラフィック

または、[Advanced Security: ネイティブボット管理、レイヤー7 DDoSとレート制限](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)、手動による緩和が不十分な場合のAdobeのネイティブエスカレーションパスをリクエストすることもできます。 各ステップは、症状を表示するウィジェットを参照するので、指標から具体的なアクションに移動できます。

>[!WARNING]
>
>このページの提案はガイドラインに過ぎません。 ブロッキングルールをデプロイする前に、必ず自分のトラフィックに対して検証してください。

## CDN帯域幅の超過

帯域幅の超過を考慮する前に、帯域幅の請求方法を理解してください。 **all**&#x200B;のFastly サービスのトラフィックは、実稼動&#x200B;**および**&#x200B;のステージング環境を含む[!DNL Adobe Commerce on Cloud Infrastructure] アカウントにバンドルされ、契約の年間許可に対する一般的な使用量にカウントされます。 **帯域幅>総帯域幅**&#x200B;から開始し、コンテンツの種類&#x200B;**および**&#x200B;帯域幅ドメインの詳細&#x200B;**で**&#x200B;帯域幅を持つボリュームを属性にします。

### メディアコンテンツ

一部のストアでは、カタログが原因で、帯域幅の大部分がメディアとして提供されています。 コンテンツ タイプ別&#x200B;**帯域幅**&#x200B;が大幅なメディア帯域幅を示す場合は、次の緩和策を検討してください。

- [Fastlyの非可逆変換](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion)を試して、より小さく低画質の画像を提供します。
- [Fastly Deep Image Optimization](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization)を調査して、コンテンツ配信ネットワーク（CDN）側でサイズ変更された画像を生成します。

### 大きなファイル

一部のサイトには、大きなファイルや、特定の多量の応答（ERP （エンタープライズリソースプランニング）の統合や書き出しなど）が含まれています。 帯域幅ごとの&#x200B;**URL**&#x200B;を使用して、**BW**&#x200B;列と&#x200B;**平均サイズ**&#x200B;列を確認し、これらの大きなファイルを見つけます。 **パスセグメント lvl 1を帯域幅**&#x200B;で使用すると、上位レベルのビューを表示できます。

### 重い404

Adobe Commerce **404 ページが見つかりません**&#x200B;は、通常、重いテーマ形式のページ（～1.5 MB）および&#x200B;**キャッシュ不可**&#x200B;であるため、404を繰り返すと異常なトラフィックが発生する可能性があります。 `favicon.ico`のような些細なリソースでも、小さなファイルではなく、重い`404` ページに変わる可能性があります。 **404**&#x200B;と&#x200B;**404 BW**&#x200B;列を&#x200B;**帯域幅ドメイン別の詳細**、**URL帯域幅の指定**、**帯域幅の上位IP**、**IP サブネットの統計**&#x200B;で使用すると、一貫して404 ボリュームを生成するクライアント、IP、URLを検索できます。 その後、アクセスを減らすか制限します。例えば、軽量`403`を返します。

### FPC ヒット率が低い

[!DNL Adobe]では、メインのCDN キャッシュアグリゲーターがオリジンを提供し、クライアントに最も近いローカルのPoint of Presence （[POPs](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)）からリクエストが少なくなるように、Fastly [&#x200B; シールド &#x200B;](https://www.fastly.com/documentation/guides/concepts/shielding/)を有効にすることをお勧めします。 [設定の確認](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding)を参照してください。

POP-to-client トラフィックとshield-to-POP トラフィックは個別にカウントされ、クライアントのレスポンスは圧縮されますが、shield-to-POP トラフィック [は圧縮されません](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) Edge Side Includes （[ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/)）のサポートを維持します。 つまり、フルページキャッシュ（FPC）のヒット率が低いと、動的なページの帯域幅が大幅に増加します。 **FPC ヒット率**、**FPC Stats By Domain**、**CDN ネットワークセグメント帯域幅**&#x200B;で症状を確認します。

ヒット率の低下は、多くの場合、大量の検索エンジンweb クローラーによって引き起こされます（[検索ボットとweb クローラー](#search-bots-and-crawlers)を参照）。 また、利用可能な場合は、[古いキャッシュをweb クローラー](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/)に提供する必要があります。 原因が頻繁にキャッシュが無効化される場合は、**Cache Invalidation By Tags**&#x200B;と&#x200B;**FPC Age By Top URLs**&#x200B;を使用して、解約されたタグ/URLを見つけます。

## ボットとweb クローラーの検索

Web クローラーの影響を測定するには、**Known Bots By Bandwidth**&#x200B;および&#x200B;**Known Bots Impact Details**&#x200B;から開始して、最もアクティブなボットを確認し、そのリクエストのみを調査するために特定のボットで[&#x200B; フィルター](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use)を実行します。

### リクエストが多すぎます

検索ボットが送信するリクエストが多すぎる最も一般的な原因は、`<meta name="robots" content="index,follow">`を含むページの解析中に発生します。 ボットは、トップナビゲーションと階層化されたナビゲーションリンクを、ほぼ無限のループでフォローできます。 この問題に対処するには、次のオプションを検討してください。

>[!WARNING]
>
> Web クローラーの行動を制限する前に、検索エンジン最適化（SEO）の専門家に相談してください。 リトレーニングはSEOに悪影響を与える可能性があります。

- `nofollow`をトップナビゲーションおよび階層化ナビゲーションリンクに追加します（例：`<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`）。
- ページのメタタグを`index,nofollow`に変更します。共通の[&#x200B; デザイン構成設定](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt)として、またはカスタム拡張機能を使用してページタイプごとに変更します。 `sitemap.xml`を正確に保ち、ボットが常に最新のページのリストをインデックスに追加できるようにします。
- `robots.txt`を更新して、パスとリソースボットがアクセスできないようにします。
- `crawl-delay` ディレクティブは公式のRobots Exclusion Protocolの一部ではありませんが、Bingbot、Slurp、SEMrushBotなどの一部のボットでは機能します。 Googlebotはこのディレクティブを無視します。
- レート制限ルールの追加。 Fastly モジュールには[不正なweb クローラー対策](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection)が組み込まれています。 より細かい制御のために、[&#x200B; カスタム Varnish Configuration Language （VCL）スニペット &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets)は、個別のレート制限を持つユーザーエージェント正規表現に対して`429` （リクエストが多すぎます）または`405` （メソッドが許可されていません）を返すことができます。 Web クローラーのドキュメントで、推奨される方法と応答コードを確認します。 Fastlyの[&#x200B; レート制限VCL ガイダンス &#x200B;](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/)を参照してください。
- AIとLLM （大規模言語モデル）web クローラーは、増加する特殊事例です。 VCLは常に自分自身を識別するわけではないので、VCLのユーザーエージェントのルールは遅れることがあります。 Adobeの[高度なセキュリティ &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security) アドオンには、[&#x200B; ネイティブボット管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)があり、VCLだけでは対応できない、エッジのAIの疑いのあるweb クローラーやフェッチャーと検証済みのボットを区別できます。

### 不要なweb クローラーのブロック

特定の検索エンジンが大きなトラフィックを生み出し、ビジネスにとって重要ではない場合、完全にブロックすることができます。

- 一部のボットは、解析ルールを再読み取りして更新した後、1 ～ 2日後に`robots.txt`の変更に従います。
- Web クローラーが`robots.txt`を無視する場合は、カスタム VCL スニペット（[例](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent)）でブロックします。 一部のweb クローラーは、これを周波数制御の好ましい方法または唯一の方法として明示的に文書化しています。

## 悪意のあるスクリプトとスクレイパー

Traffic Insights アプリを使用して、攻撃の一般的な方向を特定し、必要に応じてフォーカス領域でフィルタリングします。 赤いフラグが付いたリクエストが、主に特定のIP、サブネット、または地域から来る場合（**リクエスト数で上位IPs**、**IP サブネットで統計**、**国で統計**）、カスタム Fastly VCLでそれらをブロックすることを検討してください。

すべてのCloud Infrastructure プロジェクトには、設定に関係なく、自動保護のベースラインが既に用意されています。 同梱のWeb Application Firewall （WAF）は、50 リクエスト/分、350 リクエスト/10分、または1,800 リクエスト/時間を超えると、SQL インジェクションと既知の悪意のあるIP信号（バックドア、攻撃ツール、CMDEXE、Log4J-JNDI、トラバーサル、XSS）を即座にブロックし、その他の悪意のあるIPをレート制限します。 このベースラインは、**Requests By WAF Response**&#x200B;と、このアプリのテーブルのWAF シグナル列が示しているものです。 これらの列のスパイクは、必ずしも保護されていないことを意味しません。

- 資格情報の詰め込み、アカウントの乗り継ぎ、偽アカウントの作成、カードテスト、コンテンツスクレイピング、在庫/カートの保管などに注意してください。 これらのボット主導の不正使用パターンは、**ボットのアクティビティとリクエスト分析** タブに表示されます。 ログイン、アカウント、チェックアウト、またはカタログのエンドポイントにヒットする大量で多様性の少ないトラフィックは、**リクエスト数による上位IP**&#x200B;と&#x200B;**既知のボットへの影響の詳細**&#x200B;で検索する署名です。
- [Google reCAPTCHA](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/captcha/security-google-recaptcha)を使用して、チェックアウトおよびチェックアウト API エンドポイントをボット攻撃から保護します。
- Fastly モジュールのネイティブ レート制限[&#x200B; パス保護](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection)を使用します。
- コンマ区切りの`Sigsci_Tags` フィールドで[次世代WAF シグナル &#x200B;](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/)を確認し、関連するシグナルの一致をターゲット ブロック ルールに組み合わせます。 不審なリクエストの値は`BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`のようになります。 WAFは、IPが自動的にブロックを開始する前に、`SITE-FLAGGED-IP`のしきい値までというラベルを付けます。 **WAF Attack &amp; Anomaly Signals**、**WAF Bots Signals**、**Requests By WAF Response** ウィジェット、およびIP、サブネット、カントリーテーブルのWAF列は、これらを表示します。
- 一般的なアプローチについては、[Fastly レベルでのAdobe Commerceの悪意のあるトラフィックのブロック &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level)に関するAdobeの記事を参照してください。
- 手動ブロッキングが有効なオプションではない複雑なシナリオ（継続ボットキャンペーン、多数のIP/APIに広がる攻撃、レイヤ 7分散型サービス拒否（DDoS）など）の場合は、最初にAdobeの[Advanced Security](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security) アドオンを検討してください（[&#x200B; ネイティブボット管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)を参照）。 ストアフロントと同じFastly Edge上で動作します。 そのスコープ外の機能が必要な場合は、[Datadome](https://docs.datadome.co/docs/module-fastly)や[HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) （旧PerimeterX）などのネイティブ Fastly統合を備えたサードパーティのマネージドボット軽減サービスが推奨されます。 これらのオプションはすべて追加コストです。

## 高度なセキュリティ：ネイティブボット管理、レイヤー7 DDoS、レート制限

ここでは、Traffic Insights アプリのデータと手動のFastly VCLで何ができるかを説明します。 継続または進化するボットキャンペーン、レイヤー7 （アプリケーションレイヤー） DDoS、または悪用が多くのIPとAPI エンドポイントにわずかに広がる場合、Adobeは[高度なセキュリティ &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)を提供します。

Advanced Securityは、[!DNL Adobe Commerce on Cloud Infrastructure]用の有料アドオンで、既にストアフロントにサービスを提供している同じFastly プラットフォームで、エッジボット管理（AIweb クローラーとフェッチャー検出を含む）、レイヤー7 DDoS対策、高度なレート制限を追加します。 完全な機能、現在の制限、およびそのリクエスト方法については、[高度なセキュリティ &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)を参照してください。

購入して有効化したら、Traffic Insights アプリを使用して、高度なセキュリティが機能していることを確認します。 その決定は、**WAF Attack &amp; Anomaly Signals**、**WAF Bots Signals**、**WAF Response**&#x200B;のリクエストの背後にある同じ`Sigsci_Tags`および`Agent_response` フィールドを通じて報告されます。 有効化する前と有効化した後のウィジェットを比較して、トラフィックに対してアクティブにアクションを実行していることを確認します。
