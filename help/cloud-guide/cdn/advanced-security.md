---
title: Adobe Commerceの高度なセキュリティ
description: Advanced Securityが、Adobe Commerce on Cloud Infrastructureにボット管理、高度なレート制限、およびレイヤ 7 DDoS対策をどのように追加するかをご覧ください。
feature: Cloud, Configuration, Security
exl-id: 7aeb189f-be69-45d5-8163-4748424083c0
source-git-commit: 0b3ef117f85c990c2a01ecb655c930b8c4f61acb
workflow-type: tm+mt
source-wordcount: '2474'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security]は、[!DNL Adobe Commerce on Cloud Infrastructure]と連携して、オンラインストアを高速で利用しやすく、安全に保つ製品です。 これにより、売上を保護し、ダウンタイムを削減し、トラフィックイベントや自動攻撃のピーク時においても顧客の信頼を維持することができます。

[!DNL Adobe Commerce on Cloud Infrastructure]には、[ レイヤ 3および4 DDoS対策](./fastly.md#ddos-protection)と[Web Application Firewall （WAF） ](./fastly-waf-service.md)が組み込まれています。 [共有責任モデル ](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)の下では、レイヤ 7 DDoS検出、ボット保護、およびプロアクティブ IP ブロッキングが加盟店の責任であり、[!DNL Adobe Commerce Advanced Security]が対処するように設計されています。

[!DNL Advanced Security]は、Fastlyを搭載したエッジ セキュリティ機能を通じてストアフロント保護を拡張します。この機能は、ネットワーク エッジでのスケーラビリティ、パフォーマンス、セキュリティを組み合わせた統合エッジ プラットフォームの一部として、ボット管理、高度なレート制限、およびレイヤ 7 DDoS保護機能を提供します。

>[!NOTE]
>
>[!DNL Advanced Security]は、[!DNL Adobe Commerce on Cloud Infrastructure] （PaaS） プロジェクトでのみ使用できます。

## 主な能力

[!DNL Adobe Commerce Advanced Security]には、次の追加の保護が含まれています。

- **[ボット管理](https://docs.fastly.com/products/bot-management)**:Web アプリケーション上の不要なボット アクティビティを特定して緩和します。 Bot Management サービスは、正規のボット（検索エンジンweb クローラー、ソーシャルメディアボット）と悪意のあるボットを区別し、ネットワークエッジでリアルタイムの分類を提供し、トラフィックをブロック、許可、チャレンジ、レート制限するオプションを提供します。

- **[DDoS保護](https://docs.fastly.com/products/fastly-ddos-protection)** – すべての[!DNL Adobe Commerce on Cloud Infrastructure] プロジェクトに含まれている既存のレイヤ 3および4保護を超えて、レイヤ 7 （アプリケーション レイヤー） DDoS保護を提供します。 DDoS保護サービスは、大規模なボリューム攻撃を吸収し、分散型サービス拒否（DDoS）イベント中に継続的にアプリケーションの可用性を確保し、トラフィックのピーク時には収益を保護します。

- **[高度なレート制限](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** – 特定のURL、API エンドポイント、およびアプリケーションリソースを不正使用から保護する、設定可能なレート制限ルールを提供します。 Advanced Rate Limiting サービスは、Fastly CDN モジュールを通じて利用可能な[基本的なレート制限](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)を超えて、特定のトラフィックパターンと攻撃ベクトルをターゲットにし、インフラストラクチャの負担とクラウドコストを削減します。

>[!NOTE]
>
>現在、[!DNL Advanced Security]設定でサポートチケットの送信が必要です。 管理UIによるセルフサービス設定は、今後のリリースで計画されています。 詳しくは、[ リクエスト  [!DNL Advanced Security]](#request-advanced-security)を参照してください。

>[!IMPORTANT]
>
>**現在の制限**
>
>2026年第3四半期末まで、お客様はボット管理ルールを直接変更または管理することはできません。
>
>ルールの追加、変更、調整については、[ サポートチケット ](https://experienceleague.adobe.com/home?support-tab=home#support)を通じてAdobe Commerce サポートにお問い合わせください。 サポートチームは、要求された変更を実装します。
>
>Fastlyは、2026年第4四半期以降、Commerceの管理パネルでボット管理ルールを管理できるアドオン機能をリリースする予定です。

## デフォルトのルールと保護

次のデフォルトのルールと保護は、[!DNL Advanced Security]で使用できます。

### レイヤー7 DDoS

- DDoSしきい値はFastly CDN プラットフォームに組み込まれており、現在は顧客ごとにカスタマイズすることはできません。
- DDoS対策によってブロックされたトラフィックのログは、顧客に直接表示されません。
- リクエストに応じて、Adobe Commerce サポートは、ブロックされたDDoS トラフィックに関連する詳細を提供できます。
- ネイティブのDDoS ログ転送機能は、今後のリリースで提供される予定です。

### ボット管理

以下のベースラインボット管理の保護は、FastlyのSignal Sciences ダッシュボードを通じて利用できます。

| ルールタイプ | ステータス | 表示 |
|---|---|---|
| 不正なBOTと疑われるタグ付けされたトラフィックをブロック | オンボーディング中にデフォルトで有効 | `sigsci_tags`の下のNew Relic ログに表示 |
| 特定のタグ（sigsci タグ）にもとづくトラフィックのブロック | お客様と共同で必要な場合にのみ設定 | `sigsci_tags`の下のNew Relic ログに表示 |
| 特定のAPIやURL パターンに対するレート制限 | お客様と共同で必要な場合にのみ設定 | ブロックされたトラフィックは、`Agent_response`の下のNew Relic ログに表示されます |
| 特定のAPIやURL パターンに対する動的な課題 | お客様と共同で必要な場合にのみ設定 | ブロックされたトラフィックは、`Agent_response`の下のNew Relic ログに表示されます |
| ブラウザーチャレンジ | お客様と共同で必要な場合にのみ設定 | ブロックされたトラフィックは、`Agent_response`の下のNew Relic ログに表示されます |

## オブザーバビリティ：ボット保護とNGWAF アクティビティの監視

CDN ログは、お客様のNew Relic アカウントに自動的に転送されます。 詳細については、[ ログ管理](../monitor/log-management.md)を参照してください。

CDN ログには、Signal Sciences （Bot Protection / Next-Generation WAF）の組み込みのテレメトリが含まれており、お客様はNew Relic内で直接セキュリティイベントを監視できます。

主なフィールドは次のとおりです。

- **`Sigsci_Tags`** - Signal Sciencesによって適用される分類とタグを示します。
- **`Agent_response`** - ボット保護/NGWAF エージェントによって実行されたアクションを示します。

例：

- ボット保護またはNGWAF ルールによってブロックされたトラフィックを識別するには：

  `Agent_response:"406"`

  応答コード 406は、リクエストがセキュリティ制御によってブロックされたことを示します。

- 不正なボットとしてタグ付けされたリクエストを識別するには：

  `Sigsci_Tags:"*SUSPECTED-BAD-BOT*"`

これらのフィールドを使用して、New Relic内でダッシュボード、アラート、調査を作成し、ボットのアクティビティ、ブロックされたリクエスト、その他のセキュリティ関連イベントをモニタリングします。

## 既存のVCL機能は変更されません

[!DNL Advanced Security] アドオンを有効にしても、既存のFastly VCL ベースのセキュリティ制御を変更または置き換えることはできません。

次の既存のVCL ブロック機能は、変更なしで引き続き機能します。

- IP ベースのブロッキング
- 位置情報のブロック
- ユーザーエージェントベースのブロック
- JA3署名ベースのブロッキング
- JA4署名ベースのブロッキング

お客様は、既存のカスタム VCL設定とセキュリティルールを[!DNL Advanced Security] アドオン機能と一緒に引き続き使用できます。

[!DNL Advanced Security] アドオンは、標準のFastly CDNと、既に[!DNL Adobe Commerce on Cloud Infrastructure]で利用可能な既存のVCL保護に加えて動作します。

## 脅威のカバレッジ

[!DNL Advanced Security]は、一連の自動化およびアプリケーション層の脅威からストアフロントを保護します。

![Adobe Commerce セキュリティスタックにおける高度なセキュリティの位置付け](../../assets/advanced-security.svg)

### ボット主導の不正利用

- **資格情報の詰め込み** – データ侵害から盗まれた資格情報を使用してログインを自動的に試みます。
- **アカウントの乗っ取り** – 顧客アカウントへの不正アクセスを試みるボット。
- **アカウント作成の不正行為** – 詐欺または不正使用を目的とした偽アカウントの自動作成。
- **カードテスト** – 盗難したクレジットカード番号を決済代行会社に対してテストするボット。
- **コンテンツスクレイピング**：ストアフロントから商品データ、価格設定、コンテンツを自動的に抽出します。
- **在庫の保管** – 商品をカートに入れて購入を妨げるボット。

### AI ボット管理

- **AI web クローラー検出**：同意なしにコンテンツをスクレイピングして大規模言語モデルをトレーニングするAI web クローラーを特定および管理します。
- **AI フェッチャー制御** - リアルタイムAI 検索結果で使用されるAI フェッチャーを制御します。
- **設定可能なAI ボットポリシー** - ポリシー適用用に設定可能な信号タイプを使用して、検証済みAI ボットと疑わしいAI ボットを区別します。

### アプリケーション層の攻撃

- **レイヤ 7 DDoS攻撃** – 組み込みのレイヤ 3および4保護をバイパスするアプリケーション レイヤーをターゲットとする分散攻撃。 [!DNL Advanced Security]は、オリジン サーバーに到達する前に、エッジでこれらのボリューム攻撃を吸収します。
- **URLおよびAPIの不正使用** – 特定のURLまたはAPI エンドポイントをターゲットにした攻撃が多数のIP アドレスに広がっており、個々のIP ブロッキングが効果的ではありません。
- **キャッシュバスティング攻撃** - CDN キャッシュをバイパスし、オリジンサーバーを圧倒するように設計された、操作されたクエリパラメーターを使用したリクエスト。

### 追加機能

- **動的な課題** – 疑わしいトラフィックに最適な課題を自動的に割り当てます。 プライベートアクセストークン（PAT）を活用して、ユーザーエクスペリエンスに影響を与えることなく、リクエストの一部をシームレスに検証します。
- **詐欺テクノロジー**：攻撃者に誤った情報を返すことで、アカウントの乗っ取りを試みることに対処し、大規模な運用能力を妨害しながら、攻撃を軽減します。

## 適切な保護方法の選択

次のガイダンスを使用して、[!DNL Advanced Security]がストアフロント保護ニーズに適したソリューションであるか、既存の保護または代替ソリューションがより適切かどうかを判断します。

### [!DNL Advanced Security]の使用条件

次のシナリオは、[!DNL Advanced Security]で最適に対処できます。

| シナリオ | [!DNL Advanced Security]がどのように役立つか |
|---|---|
| 資格情報の詰め込み、コンテンツスクレイピング、インベントリの保管など、ボット主導の攻撃を受けるサイトです | ボット管理により、アプリケーションに到達する前に、エッジでの自動化された脅威を特定および軽減できます |
| 組み込みのレイヤ 3および4のカバレッジを超えて、レイヤ 7のDDoS保護対策が必要です | DDoS保護機能は、ネットワークレベルの保護をバイパスするアプリケーション層の攻撃を吸収します |
| 特定のURLまたはAPI エンドポイントは、IPでブロックできない大量の分散トラフィックのターゲットとなります | 高度なレート制限は、特定のエンドポイントとトラフィックパターンに対して詳細な制御を提供します |
| ストアフロントコンテンツにアクセスするAIweb クローラーとフェッチャーを管理したい場合 | ボット管理には、設定可能なAI ボット検出と適用ポリシーが含まれます |
| 既存のFastly CDNに統合された、Adobeサポートのエッジセキュリティソリューションが必要です | [!DNL Advanced Security]は、既にストアフロントにサービスを提供している同じFastly edge プラットフォームで実行されます |

### 既存の保護を利用するケース

次のシナリオは、既存の保護で最適に対処できます。

| シナリオ | 推奨されるアプローチ |
|---|---|
| 単一のIPまたは識別可能なIPの小さなセットが、web サイトに次々とリクエストを送っています | Commerce管理者またはFastly APIを使用してIPをブロックします。 組み込みの[ レイヤー3/4 DDoS保護機能](./fastly.md#ddos-protection)と既存の[IP保護ブロックリスト](./fastly-vcl-blocking.md) VCL スニペットを使用します。 |
| SQL インジェクション、クロスサイトスクリプティング（XSS）またはその他のOWASPの上位10個の脅威をブロックする必要があります | 含まれている[WAF サービス ](./fastly-waf-service.md)は、これらの脅威を自動的にブロックします。 |
| DDoS攻撃パターンは、基本的なVCL ブロッキングルールで制御できます | Adobe Commerceで既に利用可能な既存の[ カスタム VCL スニペット ](./fastly-vcl-custom-snippets.md)を使用します。 |

### 代替保護を使用する場合

次のシナリオは、[!DNL Advanced Security]を補完できる代替保護で最適に対処されます。

| シナリオ | 推奨されるアプローチ |
|---|---|
| トランザクションレベルの不正スコアリングや支払い詐欺防止が必要です | 専用の詐欺防止プラットフォームを利用する。 [!DNL Advanced Security]はエッジ ネットワーク レベルで保護され、個々の支払いトランザクションは評価されません。 |
| IDとアクセス管理（IAM）が必要です | 専用のIAM ソリューションを実装する。 ユーザーの認証とセッション管理は、引き続きお客様の責任です。 |
| 静的または動的なアプリケーションセキュリティテスト（SAST/DAST）が必要 | 専用のアプリケーションセキュリティテストツールを使用できます。 コードレベルの脆弱性スキャンは提供されていません。 |
| レート制限を超えた包括的なAPI セキュリティ（スキーマ検証やAPI ゲートウェイ機能など）が必要です | 専用のAPI セキュリティプラットフォームの導入。 |
| PCI スキャンやSOC レポートなどの規制遵守ツールが必要です | 専用のコンプライアンス管理ツールを利用する。 |

>[!TIP]
>
>現在、サードパーティのボット保護プロバイダーを使用している場合、[!DNL Advanced Security]に統合すると、操作の複雑さが軽減され、プロバイダー間で一貫性のないセキュリティカバレッジが排除されます。 プロジェクトの[!DNL Advanced Security]を評価するには、Adobe アカウントチームにお問い合わせください。

## セキュリティスタックの位置

[!DNL Advanced Security]は、エッジベースの保護の追加レイヤーとして、より広範なAdobe Commerce セキュリティアーキテクチャに適合します。 これは、既に[!DNL Adobe Commerce on Cloud Infrastructure]に含まれているWAFおよびLayer 3/4 DDoS保護機能と同じように機能し、置き換えるものではありません。 次のセクションでは、既存の保護と、顧客に残る責任との関係を明確にします。

### 含まれる保護

[!DNL Adobe Commerce on Cloud Infrastructure]には、次のセキュリティ機能が含まれています。

- **[Web Application Firewall （WAF）](./fastly-waf-service.md)**:SQL インジェクション、クロスサイトスクリプティング （XSS）、その他のOpen Web Application Security Project （OWASP）に対する管理済み保護の上位10個の脅威。 実稼動環境でのみ使用できます。
- **[レイヤ 3および4 DDoS保護](./fastly.md#ddos-protection)**:SYN フラッド、UDP フラッド、ICMP ベースの攻撃、TCP レベルの攻撃などのネットワーク層の攻撃に対する組み込みの保護機能。 Fastly CDNで自動的に有効になります。
- **[SSL/TLS証明書](./fastly-configuration.md#provision-ssltls-certificates)** – 安全なHTTPS トラフィック用のドメイン検証済み暗号化証明書。
- **[オリジンクローキング](./fastly.md#origin-cloaking)**—Fastlyを介するすべてのトラフィックルートを確保し、オリジンサーバーへの直接アクセスをブロックします。
- **[VCL ベースのセキュリティ スニペット](./fastly-vcl-custom-snippets.md)** - IP ブロッキング、許可リストに加える、およびリクエスト フィルタリング用のカスタム Varnish Configuration Language （VCL）ルール。

### [!DNL Advanced Security]

[!DNL Advanced Security]は、[!DNL Adobe Commerce on Cloud Infrastructure]に含まれている組み込みの保護機能を超えて保護を強化していますが、追加の費用が発生します。

- **ボット管理** - Edge ベースのボット検出と、AI ボット管理による緩和。
- **レイヤ 7 DDoS対策** - アプリケーション層のDDoS吸収と防御。
- **高度なレート制限** - URLとAPI エンドポイントの詳細なレート制御。
- **動的な課題と詐欺テクノロジー** – 自動課題の割り当てとアカウント乗っ取りの緩和。

### 顧客の責任

- **不正防止** - トランザクションレベルの不正行為のスコアリングと不正支払いの検出。
- **IDとアクセス管理** – 顧客認証、認証、セッション管理。
- **アプリケーションセキュリティテスト**—SAST/DASTおよび脆弱性のスキャン。
- **カスタム セキュリティ設定** - VCL ベースのルール、IP アドレスの設定、およびIP アドレスのブロックリスト。
- **コンプライアンス ツール** - PCI スキャン、SOC コンプライアンス レポート、規制監査ツール。
- **アプリケーションレベルの強化** - トークンベースのAPI認証、クエリパラメーターの正規化、およびキャッシュ戦略の設計。

Adobeとカスタマーセキュリティの責任の詳細については、[共有責任モデル ](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)を参照してください。

## 一般的な攻撃パターンと保護

次の表は、一般的な攻撃パターンを、Adobe Commerce セキュリティスタック内の適切な保護レイヤーにマッピングしたものです。

| 攻撃パターン | タイプ | 保護 |
|---|---|---|
| 単一のIP、または多数のリクエストを送信する識別可能なIPのセット | DDoS + ボット | Commerce管理者またはFastly APIを使用してIPをブロックします。 組み込みのレイヤ 3/4 DDoS保護機能は、このトラフィックをネットワーク エッジでフィルタリングします。 |
| 特定のURLやAPIに対する攻撃は、多数のIPに分散しています | DDoS + ボット | **[!DNL Advanced Security]**：高度なレート制限は、URLごとにリクエスト量を制限します。 ボット管理は、分散したボットトラフィックを識別してブロックします。 |
| 適切な認証なしにREST API エンドポイントに対する自動攻撃 | ボット + DDoS | API エンドポイントでトークンベースの認証が使用されていることを確認します。 トークンが侵害された場合は、認証情報をローテーションします。 **[!DNL Advanced Security]**：高度なレート制限により、公開されたエンドポイントを保護できます。 |
| 操作されたクエリパラメーターを使用したキャッシュバスティング攻撃 | ボット + DDoS | キャッシュキーから必須ではないクエリパラメーターを除外します。 アプリケーションレベルでのクエリパラメータの正規化と制限。 **[!DNL Advanced Security]**: ボット管理は、自動キャッシュ バスト トラフィックを検出してブロックします。 |
| SQL インジェクションまたはクロスサイトスクリプティング（XSS）の試行 | WAF | 含まれている[WAF サービス ](./fastly-waf-service.md)は、管理されたセキュリティ ルールを使用して、これらの脅威を自動的にブロックします。 |

### WAFのブロック動作

次のWAF ビヘイビアーは、[!DNL Advanced Security]が有効であるかどうかに関係なく、すべての[!DNL Adobe Commerce on Cloud Infrastructure] プロジェクトに適用されます。 付属のWAF サービスでは、一般的な攻撃信号に対して次のブロック動作を使用します。

- **SQL インジェクション**&#x200B;要求は、1つの一致する要求であっても、直ちにブロックされます。
- 既知の悪意のあるIPからの次の脅威シグナルで識別されたリクエストは、Backdoor、Attack Tooling、CMDEXE、Log4J JNDI、Traversal、およびXSSですぐにブロックされます。
- 上記の脅威シグナルを示す非悪意のあるIPからのリクエストは、次のしきい値を超えるとブロックされます。

| 間隔 | しきい値 | 頻度を確認 |
|---|---|---|
| 1分 | 50件のリクエスト | 20秒ごと |
| 10分 | 350 リクエスト | 3分ごとに |
| 1時間 | 1,800件のリクエスト | 20分ごと |

## リクエスト [!DNL Advanced Security]

[!DNL Advanced Security]をリクエストするには：

>[!NOTE]
>
>[!DNL Advanced Security]は追加料金で利用でき、アクティブな[!DNL Adobe Commerce on Cloud Infrastructure] （PaaS）サブスクリプションが必要です。

1. お客様のプロジェクトの[!DNL Advanced Security]について詳しくは、Adobe アカウントチームまたはAdobeの営業担当者にお問い合わせください。

1. [!DNL Advanced Security]を購入した後、[Adobe Commerce サポートチケットを送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)し、[!DNL Advanced Security]の有効化をリクエストします。 [!DNL Adobe Commerce on Cloud Infrastructure] プロジェクト IDと、有効化が必要な環境（実稼動環境やステージングなど）を含めます。

1. Adobeは、Fastly サービスで[!DNL Advanced Security]をアクティブ化し、初期の保護ポリシーを設定します。 イネーブルメントは、通常、チケット提出から数営業日以内に完了します。

1. [!DNL Advanced Security]がアクティブであることを確認するメッセージと、環境で有効になっている保護の詳細が表示されます。

>[!NOTE]
>
>[!DNL Advanced Security]への設定の変更には、現在[ サポートチケットの送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)が必要です。 管理UIによるセルフサービス設定は、今後のリリースで計画されています。

## 制限

[!DNL Advanced Security]は、エッジ層のストアフロント保護を提供します。 次の機能は利用できず、補完的なソリューションで最適に対処できます。

- **トランザクションレベルの不正行為スコアリング**—[!DNL Advanced Security]では、不正行為のリスクに対する個々の支払いトランザクションは評価されません。 トランザクションレベルのスコアリングには、専用の不正利用防止プラットフォームを使用します。
- **IDおよびアクセス管理（IAM）**—[!DNL Advanced Security]は、ユーザー認証、認証、またはセッション管理を管理しません。 これらは引き続き顧客の責任です。
- **静的および動的アプリケーション セキュリティ テスト （SAST/DAST）**—[!DNL Advanced Security]には、コード レベルの脆弱性のスキャンまたは侵入テストは含まれていません。
- **API セキュリティ** – 高度なレート制限によりAPI エンドポイントを不正使用から保護できますが、スキーマ検証やAPI ゲートウェイ管理などの包括的なAPI セキュリティ機能は提供されていません。
- **完全な不正防止**—[!DNL Advanced Security]は、エッジ層のストアフロント保護に焦点を当てており、完全な不正管理プラットフォームではありません。
- **コンプライアンス ツール**—[!DNL Advanced Security]には、PCI スキャン、SOC コンプライアンス レポート、規制監査の機能はありません。
