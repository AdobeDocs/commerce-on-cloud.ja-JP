---
title: チェックリストを起動
description: サイト立ち上げ時のチェックリストの項目を確認する。
exl-id: efc97d4a-a9f3-49fa-b977-061282765e90
source-git-commit: ca2d94364787695398b2b8af559733fe52ec2949
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# チェックリストを起動

実稼動環境にデプロイする前に、[Launch チェックリスト &#x200B;](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf)をダウンロードし、これらの手順を使用して、必要なすべての設定とテストが完了したことを確認します。 StarterとProのデプロイメントプロセスの概要（[&#x200B; ストアのデプロイ &#x200B;](../deploy/staging-production.md)）を参照してください。

## 本番環境での完全テスト

サイト、ストア、環境のあらゆる側面をテストするには、[&#x200B; デプロイメントのテスト &#x200B;](../test/staging-and-production.md)を参照してください。 これらのテストには、Fastlyの検証、ユーザー受け入れテスト（UAT）およびパフォーマンステストが含まれます。

## TLSとFastly

Adobeでは、各環境にLet&#39;s Encrypt SSL/TLS証明書を提供しています。 FastlyがHTTPS経由で安全なトラフィックを提供するには、この証明書が必要です。

この証明書を使用するには、Adobeがドメインの検証を完了し、証明書を環境に適用できるように、DNS設定を更新する必要があります。 各環境には、その環境にデプロイされたAdobe Commerce on cloud infrastructure サイトのドメインをカバーする一意の証明書があります。 [Fastlyのセットアップ プロセス &#x200B;](../cdn/fastly-configuration.md)中に完了し、設定を更新することをお勧めします。

## 実稼動設定を使用したDNS設定の更新

サイトを立ち上げる準備ができたら、DNS設定を更新して、Fastly サービスを通じて実稼動環境からトラフィックをルーティングする必要があります。

**前提条件：**

- [開発環境でのFastlyのセットアップとテスト](../cdn/fastly-configuration.md#)

- 実稼動環境の設定は、必要なすべてのドメインで更新されました

  通常、顧客テクニカルアドバイザーと協力して、ストアに必要なすべてのトップレベルドメインとサブドメインを追加します。 実稼動環境のドメインを追加または変更するには、[Adobe Commerce サポートチケットを送信](https://support.magento.com/hc/en-us/articles/360019088251)してください。 プロジェクト設定が更新されたという確認を待ちます。

  スタータープロジェクトでは、ドメインをプロジェクトに追加する必要があります。 [&#x200B; ドメインの管理](../cdn/fastly-custom-cache-configuration.md#manage-domains)を参照してください。

- 本番環境用にプロビジョニングされたSSL/TLS証明書。

  Fastlyのセットアッププロセス中に実稼動ドメインのACME チャレンジレコードを追加した場合、Adobeは、DNS設定を更新してトラフィックをFastly サービスにルーティングすると、SSL/TLS証明書を実稼動環境に自動的にアップロードします。 証明書を事前にプロビジョニングしなかった場合、またはドメインを更新した場合、Adobeはドメインの検証を完了して証明書をプロビジョニングする必要があります。これには最大で12時間かかる場合があります。

### サイト起動用のDNS設定を更新するには：

1. 実稼動サイトの次のDNS設定を更新します。

   - 特に、既存のサイトから移行する場合は、必要なすべてのリダイレクトを設定します

   - ゾーンのルートリソースレコードを設定して、ホスト名を指定します

   - DNS情報を更新して顧客を正しい実稼動ストアに誘導するためのTTL （Time-to-Live）の値を下げます

     DNS レコードを切り替える場合は、TTL値を大幅に下げることをお勧めします。 この値は、DNS レコードをキャッシュする時間をDNSに伝えます。 短縮すると、DNSをより速く更新します。 例えば、サイトを更新する際に、TTLの値を3日から10分に変更できます。 TTL値を短くすると、DNS インフラストラクチャに負荷がかかります。 サイトの立ち上げ後、以前の高い値を復元します。


1. 実稼動環境のサブドメインをFastly サービス `prod.magentocloud.map.fastly.net`にポイントするCNAME レコードを追加します。例：

   | ドメインまたはサブドメイン | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. 必要に応じて、AおよびAAAA レコードを追加して、apex ドメイン（`<domain-name>.com`）を次のFastly IP アドレスにマッピングします。

   | Apex ドメイン | 穴名 | 阿穴目 |
   | --------------- | ----------------- | -------- |
   | `<domain-name>.com` | `151.101.1.124` | 2a04:4e42:200::380 |
   | `<domain-name>.com` | `151.101.65.124` | 2a04:4e42:400::380 |
   | `<domain-name>.com` | `151.101.129.124` | 2a04:4e42:600::380 |
   | `<domain-name>.com` | `151.101.193.124` | 2a04:4e42::380 |

>[!IMPORTANT]
>
>[RFC1034](https://www.rfc-editor.org/rfc/rfc1912) （**セクション 2.4**）のDNS手順には、次の内容が記載されています。>_CNAME レコードは、他のどのデータとも共存できません。 言い換えれば、suzy.podunk.xxがsue.podunk.xxのエイリアスである場合、suzy.podunk.eduのMX レコード、A レコード、さらにはTXT レコードも持つことはできません。_
>
>このため、DNS レコードは、サブドメインの場合は`CNAME`とapex ドメイン （ルート ドメイン）の場合は`A`と入力する必要があります。 このルールを破棄すると、MXやNSなどの他のレコードを追加する機能が失われるため、メールサービスやDNSの伝搬が中断される可能性があります。 一部のDNS プロバイダーは、内部カスタマイズを使用することによってこれを回避できますが、標準に従うことで、安定性と柔軟性（DNS プロバイダーの変更など）が確保されます。

1. ベース URLを更新します。

   - SSHを使用して本番環境にログインします。

     ```bash
     magento-cloud ssh -e production
     ```

   - CLIを使用して、ストアのベース URLを変更します。

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **メモ**：管理者からベース URLを更新することもできます。 _Adobe Commerce ストアおよび購入エクスペリエンスガイド_&#x200B;の[&#x200B; ストア URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=ja)を参照してください。

1. サイトが更新されるまで数分待ちます。

1. サイトのテスト。

## 実稼動設定の確認

1つ以上のストアの実稼動設定を検証するには、最終パスを作成します。 本番環境で設定を更新できます。 設定が読み取り専用の場合は、SSH接続を開き、CLI コマンドを使用して設定を変更したり、ローカル環境で設定を変更したりする必要がある場合があります。 更新が完了したら、ステージング環境と実稼動環境に変更をデプロイできます。

次の変更とチェックをお勧めします。

- [送信メールテストを完了しました](../project/outgoing-emails.md)

- [管理者資格情報とベース管理者URLのセキュアな設定](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/security/security-admin)

- [Web用のすべての画像を最適化する](../cdn/fastly-image-optimization.md)

- [HTML、JavaScriptおよびCSSの最小化設定を確認する](../deploy/static-content.md)

## Fastlyのキャッシュを確認

- Fastlyのキャッシュが実稼動サイトで正しく機能することをテストおよび検証します。 詳細なテストとチェックについては、[Fastly testing](../test/staging-and-production.md#check-fastly-caching)を参照してください。

- [最新バージョンのFastly CDN Module for Commerceが本番環境にインストールされていることを確認します](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Fastly VCL コードの最新バージョンがアップロードされていることを確認します](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## パフォーマンステスト

ローンチ前の準備プロセスの一環として、[Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)のオプションを確認することをお勧めします。

また、次のサードパーティオプションを使用してテストすることもできます。

- [包囲攻撃](https://www.joedog.org/siege-home/): ストアを制限にプッシュするためのトラフィック形成およびテスト ソフトウェア。 設定可能な数のシミュレートされたクライアントで、サイトにアクセスします。 Siegeは、基本認証、Cookie、HTTP、HTTPS、およびFTP プロトコルをサポートしています。

- [Jmeter](https://jmeter.apache.org/): フラッシュセールスのように、スパイクしたトラフィックのパフォーマンスを測定するのに役立つ優れた負荷テストです。 サイトに対して実行するカスタムテストを作成します。

- [New Relic](https://support.newrelic.com/s/) （提供）: データ、クエリ、Redisなどの送信アクションごとに費やされた時間を追跡して、パフォーマンスの低下を引き起こすサイトのプロセスと領域を見つけるのに役立ちます。

- [WebPageTest](https://www.webpagetest.org/)と[Pingdom](https://www.pingdom.com/)：サイトページのリアルタイム分析は、異なるオリジンの場所で時間を読み込みます。 Pingdomには料金がかかる場合があります。 WebPageTestは無料のツールです。

## セキュリティ設定

- [セキュリティスキャンの設定](overview.md#set-up-the-security-scan-tool)

- [管理者ユーザーのセキュアな設定](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/security/security-admin)

- [管理者URLのセキュアな設定](https://experienceleague.adobe.com/ja/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Adobe Commerce on cloud infrastructure プロジェクトで使用されなくなったユーザーを削除](../project/user-access.md)

- [二段階認証の設定](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## パフォーマンス監視

New Relic サービスを使用して、ProおよびStarter環境のパフォーマンスモニタリングを行うことができます。 Pro プランのアカウントでは、New Relic APMおよびインフラストラクチャエージェントを使用してアプリケーションとインフラストラクチャのパフォーマンスをモニタリングする、Adobe CommerceのManaged Alerts ポリシーを提供します。 これらのサービスの使用について詳しくは、[管理済みアラートを使用したパフォーマンスの監視](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)を参照してください。

### 次のステップ

[ローンチステップ](steps.md)
