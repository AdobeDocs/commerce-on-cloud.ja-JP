---
title: Launch チェックリスト
description: サイトの起動のチェックリスト項目を確認します。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# Launch チェックリスト

実稼動環境にデプロイする前に、[Launch チェックリスト ](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) をダウンロードし、以下の手順に従って使用することで、必要な設定とテストをすべて完了していることを確認します。 Starter と Pro の完全なデプロイメントプロセスの概要については、[ ストアのデプロイ ](../deploy/staging-production.md) を参照してください。

## 実稼動環境で完全にテスト

サイト、ストア、環境のすべての側面をテストする方法については、[ デプロイメントのテスト ](../test/staging-and-production.md) を参照してください。 これらのテストには、Fastly の検証、ユーザー受け入れテスト（UAT）、パフォーマンステストが含まれます。

## TLS と Fastly

Adobeは、各環境に対して SSL/TLS 証明書を暗号化しましょう提供します。 この証明書は、HTTPS で安全なトラフィックを提供するために Fastly に必要です。

この証明書を使用するには、ドメインの検証を完了してAdobeに証明書を適用できるように、DNS 構成を更新する必要があります。 各環境には、その環境にデプロイされたクラウドインフラストラクチャサイト上のAdobe Commerceのドメインをカバーする一意の証明書があります。 [Fastly セットアッププロセス ](../cdn/fastly-configuration.md) の間に、および設定の更新を完了することをお勧めします。

## 実稼働設定で DNS 設定を更新します

サイトを起動する準備が整ったら、実稼動環境から Fastly サービスを通じてトラフィックをルーティングするように、DNS 設定を更新する必要があります。

**前提条件：**

- [開発環境で Fastly を設定しテストする](../cdn/fastly-configuration.md#)

- 実稼動環境の設定が、必要なすべてのドメインで更新されました

  通常は、お客様のテクニカルアドバイザーと協力して、ストアに必要なすべての最上位ドメインとサブドメインを追加します。 実稼動環境用のドメインを追加または変更するには、[Adobe Commerce サポートチケットを送信 ](https://support.magento.com/hc/en-us/articles/360019088251) します。 プロジェクト設定が更新されたことを確認するまで待ちます。

  スタータープロジェクトでは、ドメインをプロジェクトに追加する必要があります。 [ ドメインの管理 ](../cdn/fastly-custom-cache-configuration.md#manage-domains) を参照してください。

- 実稼動環境用にプロビジョニングされた SSL/TLS 証明書。

  Fastly のセットアッププロセス中に実稼動ドメインの ACME チャレンジレコードを追加した場合、Fastly サービスへのトラフィックをルーティングするように DNS 設定を更新すると、Adobeは SSL/TLS 証明書を実稼動環境に自動的にアップロードします。 証明書を事前にプロビジョニングしなかった場合、またはドメインを更新した場合、Adobeはドメインの検証を完了して証明書をプロビジョニングする必要があります（最大で 12 時間かかることがあります）。

### サイトを起動するための DNS 設定を更新するには：

1. 実稼動サイトに対して次の DNS 設定を更新します。

   - 特に既存のサイトから移行する場合は、必要なリダイレクトをすべて設定します

   - ホスト名をアドレス指定するようにゾーンのルートリソースレコードを設定する

   - 顧客が正しい実稼動ストアを参照するように DNS 情報を更新するには、有効期間（TTL）の値を小さくします

     DNS レコードを切り替える際は、TTL 値を大幅に低くすることをお勧めします。 この値は、DNS レコードをキャッシュする期間を DNS に指示します。 短縮すると、DNS の更新が速くなります。 例えば、サイトの更新時に TTL 値を 3 日から 10 分に変更できます。 TTL 値を短くすると、DNS インフラストラクチャに負荷がかかることに注意してください。 サイトの起動後に、以前の値より高い値を復元します。


1. CNAME レコードを追加して、実稼動環境のサブドメインを Fastly サービス `prod.magentocloud.map.fastly.net` ーバーに指すようにします。次に例を示します。

   | ドメインまたはサブドメイン | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. 必要に応じて、A レコードを追加して、apex ドメイン（`<domain-name>.com`）を次の Fastly IP アドレスにマッピングします。

   | Apex ドメイン | ANAME |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>[RFC1034](https://www.rfc-editor.org/rfc/rfc1912) （**2.4**）の DNS 命令には次の内容が含まれています。
>_CNAME レコードは、他のデータと共存できません。 つまり、suzy.podunk.xx が sue.podunk.xx のエイリアスである場合、suzy.podunk.eduの MX レコード、A レコード、さらには TXT レコードも持つことはできません。_
>
>このため、DNS レコードは、サブドメインの場合はタイプ `CNAME`、apex ドメイン（ルートドメイン）の場合はタイプ `A` である必要があります。 このルールを破棄すると、MX や NS などの他のレコードを追加する機能が失われるため、メールサービスや DNS の伝播が中断される可能性があります。 一部の DNS プロバイダーは、内部のカスタマイズを使用することでこれを回避する場合がありますが、標準に従うと、安定性と柔軟性が確保されます（DNS プロバイダーの変更など）。

1. ベース URL を更新します。

   - SSH を使用して実稼動環境にログインします。

     ```bash
     magento-cloud ssh -e production
     ```

   - CLI を使用して、ストアのベース URL を変更します。

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **注意**：管理者からベース URL を更新することもできます。 [2&rbrace;Adobe Commerce ストアと購入エクスペリエンスガイド ](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html) の &lbrace;URL のストア _を参照してください。_

1. サイトが更新されるまで数分待ちます。

1. サイトをテストします。

## 実稼動設定の確認

最終パスを作成して、1 つ以上のストアの実稼動設定を検証します。 実稼動環境で設定を更新できます。 設定が読み取り専用の場合は、SSH 接続を開き、CLI コマンドを使用して構成を変更するか、ローカル環境で構成を変更する必要があります。 更新が完了したら、変更をステージング環境と実稼動環境にデプロイできます。

推奨される変更およびチェックを次に示します。

- [送信メールのテストを完了しました](../project/outgoing-emails.md)

- [ 管理者資格情報とベース管理者 URL のセキュリティで保護された設定 ](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [Web 用にすべての画像を最適化](../cdn/fastly-image-optimization.md)

- [HTML、JavaScriptおよび CSS の縮小設定の確認](../deploy/static-content.md)

## Fastly キャッシュの検証

- 実稼動サイトで Fastly キャッシュが正しく動作していることをテストし確認します。 詳細なテストとチェックについては、[Fastly テスト ](../test/staging-and-production.md#check-fastly-caching) を参照してください。

- [最新バージョンの Fastly CDN Module for Commerceが実稼動環境にインストールされていることを確認します](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Fastly VCL コードの最新バージョンがアップロードされていることを確認してください](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## パフォーマンステスト

ローンチ前の準備プロセスの一環として、[Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) オプションを確認することをお勧めします。

また、次のサードパーティオプションを使用してテストすることもできます。

- [Siege](https://www.joedog.org/siege-home/)：トラフィックのシェーピングとテストを行うソフトウェアで、ストアを限界まで押し上げます。 設定可能な数のシミュレーションクライアントを使用して、サイトをヒットします。 Siege では、基本認証、Cookie、HTTP、HTTPS、および FTP プロトコルをサポートしています。

- [Jmeter](https://jmeter.apache.org/)：優れた負荷テストにより、フラッシュセールスなどのスパイクトラフィックのパフォーマンスを測定できます。 サイトに対して実行するカスタムテストを作成します。

- [New Relic](https://support.newrelic.com/s/) （提供）：データ、クエリ、Redis などの送信に費やした時間が追跡されるので、パフォーマンスが低下するサイトのプロセスや領域を見つけるのに役立ちます。

- [WebPageTest](https://www.webpagetest.org/) および [Pingdom](https://www.pingdom.com/)：サイトページのリアルタイム分析により、様々なオリジンの場所で時間を読み込みます。 Pingdom は手数料がかかる場合があります。 WebPageTest は無料のツールです。

## セキュリティ設定

- [セキュリティスキャンの設定](overview.md#set-up-the-security-scan-tool)

- [ 管理者ユーザーのセキュリティで保護された設定 ](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [ 管理者 URL のセキュア設定 ](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [クラウドインフラストラクチャプロジェクトでAdobe Commerceを終了したユーザーをすべて削除](../project/user-access.md)

- [ 二要素認証の設定 ](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## パフォーマンス監視

Pro および Starter 環境でのパフォーマンスモニタリングには、New Relic サービスを使用できます。 Pro プランアカウントでは、New Relic APM およびインフラストラクチャエージェントを使用してアプリケーションとインフラストラクチャのパフォーマンスを監視する、Adobe Commerceのアラートポリシーのマネージドアラートを提供しています。 これらのサービスの使用方法については、[ 管理されたアラートによるパフォーマンスの監視 ](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) を参照してください。

### 次の手順

[ローンチ手順](steps.md)
