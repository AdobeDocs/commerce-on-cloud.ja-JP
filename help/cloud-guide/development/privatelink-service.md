---
title: PrivateLink サービス
description: PrivateLink サービスを使用して、同じリージョンのプライベートクラウドとAdobe Commerce クラウドプラットフォームの間に安全な接続を確立する方法を説明します。
feature: Cloud, Iaas, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# PrivateLink サービス

クラウドインフラストラクチャー上のAdobe Commerceは、[AWS PrivateLink](https://aws.amazon.com/privatelink/) または [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/) サービスとの統合をサポートしています。 PrivateLink を使用すると、クラウドインフラストラクチャ環境上のAdobe Commerceと、外部システムでホストされるサービスおよびアプリケーションとの間に、安全なプライベート通信を確立できます。 Adobe Commerce アプリケーションと外部システムの両方に、同じクラウドリージョン内の同じクラウドプラットフォーム（AWSまたは Azure）上に設定された Virtual Private Cloud （VPC）エンドポイントを通じてアクセスできる必要があります。

>[!TIP]
>
>PrivateLink は、データベースやファイル転送などの非 HTTP （S）統合用の接続を保護するのに最適です。 アプリケーションをAdobe Commerce API と統合する場合は、「Adobe Developer App Builderの API メッシュ [&#128279;](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) の AdobeAPI メッシュを作成する方法 _を参照してください_。

## 機能とサポート

クラウドインフラストラクチャプロジェクト上のAdobe Commerce向け PrivateLink サービス統合には、次の機能およびサポートが含まれます。

- お客様の Virtual Private Cloud （VPC）と、同じクラウドリージョン内の同じクラウドプラットフォーム（AWSまたは Azure）上のAdobe VPCとの安全な接続。
- AdobeVPC とお客様 VPC のエンドポイントサービス間での一方向性または双方向通信のサポート。
- サービス有効化：

   - クラウドインフラストラクチャー上のAdobe Commerceで必要なポートを開きます。
   - お客様とAdobeの VPC 間の初期接続の確立
   - イネーブルメント中の接続の問題のトラブルシューティング

## 制限事項

- PrivateLink のサポートは、実稼動環境およびステージング環境でのみ使用できます。 ローカル環境、統合環境、スタータープロジェクトでは使用できません。
- PrivateLink を使用して SSH 接続を確立することはできません。 [SSH キーの有効化 ](secure-connections.md) を参照してください。
- Adobe Commerce サポートでは、最初のイネーブルメント以降のAWS PrivateLink の問題のトラブルシューティングには対応しません。
- お客様は、独自のVPCの管理に関連するコストについて責任を負います。
- [Fastly オリジンクローキング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html) が原因で、HTTPS プロトコル（ポート 443）を使用して、Azure プライベートリンク経由でクラウドインフラストラクチャ上のAdobe Commerceに接続することはできません。 この制限は、AWS PrivateLink には適用されません。
- PrivateDNS は使用できません。

## PrivateLink 接続タイプ

次のネットワーク図に示すように、2 つの PrivateLink 接続タイプを使用して、ストアとクラウド環境の外部でホストされる外部システムとの間の安全な通信を確立できます。

![PrivateLink のネットワーク図 ](../../assets/privatelink-architecture-diagram.png)

クラウドインフラストラクチャ環境のAdobe Commerceに最適な PrivateLink 接続タイプを次の中から 1 つ選びます。

- **単方向 PrivateLink** - クラウドインフラストラクチャストア上のAdobe Commerceから安全にデータを取得する場合は、この設定を選択します。
- **双方向 PrivateLink** - クラウドインフラストラクチャ環境で、Adobe Commerce外部のシステムとの間に安全な接続を確立する場合に選択します。 双方向オプションには、次の 2 つの接続が必要です。

   - お客様のVPCとAdobeのVPCの間の接続
   - AdobeVPCとカスタマーVPC間の接続

>[!TIP]
>
>PrivateLink 接続タイプの選択に関するヘルプや、VPCの設定と管理に関するヘルプは、ネットワーク管理者またはクラウドプラットフォームプロバイダーにお問い合わせください。 詳しくは、Cloud Platform PrivateLink のドキュメント [AWS PrivateLink](https://aws.amazon.com/privatelink/) または [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/) を参照してください。

## PrivateLink 有効化のリクエスト

>[!WARNING]
>
>PrivateLink の有効化には、最大 _5_ 営業日かかる場合があります。 不完全な情報や不正確な情報を提供すると、プロセスが遅れる場合があります。

### 前提条件

![ チェック ](../../assets/fix.svg) クラウドインフラストラクチャインスタンス上のAdobe Commerceと同じリージョンにあるクラウドアカウント（AWSまたは Azure）。

![ チェック ](../../assets/fix.svg)PrivateLink 経由で接続するサービスをホストする、お客様の環境内のVPC。 VPCのセットアップのヘルプについては、AWSまたは Azure のドキュメントを参照するか、ネットワーク管理者にお問い合わせください。

![ チェック ](../../assets/fix.svg) 双方向の PrivateLink 接続の場合、PrivateLink 有効化をリクエストする前に、アプリケーションまたはサービスのエンドポイントサービス設定を作成し、VPC環境でエンドポイントを作成する必要があります。 [ 双方向の PrivateLink 接続用の設定 ](#set-up-for-bidirectional-privatelink-connections) を参照してください。

PrivateLink の有効化に必要な次のデータを収集します。

- **お客様のクラウドアカウント番号** （AWSまたは Azure） – クラウドインフラストラクチャインスタンス上のAdobe Commerceと同じリージョンにある必要があります
- **クラウド地域** – 検証目的でアカウントがホストされているクラウド地域を指定します
- **サービスおよび通信ポート** - VPC 間のサービス通信を有効にするには、Adobeがポートを開く必要があります（SQL ポート 3306、SFTP ポート 2222 など）。
- **プロジェクト ID** - Adobe Commerce on cloud infrastructure Pro プロジェクト ID を指定します。 次の [Cloud CLI](../dev-tools/cloud-cli-overview.md) コマンドを使用すると、プロジェクト ID およびその他のプロジェクト情報を取得できます。`magento-cloud project:info`
- **接続タイプ** – 接続タイプに単方向または双方向を指定します
- **エンドポイントサービス** – 双方向の PrivateLink 接続の場合、Adobeが接続する必要があるVPC エンドポイントサービスの DNS URL を指定します（例：`com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`）。
- **エンドポイントサービスへのアクセス権の付与** – 外部サービスに接続するには、エンドポイントサービスから次のAWS アカウントプリンシパルへのアクセスを許可します。`arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >エンドポイントサービスへのアクセス権が提供されていない場合、VPCのサービスへの双方向の PrivateLink 接続が追加 **されない** ので、設定が遅れます。

#### Azure プライベートリンクの有効化に固有のその他の前提条件

- クラスター ID を指定します。SSH を使用して、リモートにログインし、コマンド `cat /etc/platform_cluster` を使用します。
- 外部サービスをAdobe Commerce Pro クラスターに接続するには、次のものが必要です。

   - 新しい外部プライベートエンドポイントに公開する Pro クラスター上のポートのリスト
   - プライベートエンドポイント接続の Azure サブスクリプション ID のリスト

- Adobe Commerce Pro クラスターを外部サービスに接続するには、次が必要です。

   - ターゲットサービスのリソース ID のリスト。 外部プライベートリンクサービス ID は次のようになります。

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### イネーブルメントワークフロー

次のワークフローでは、クラウドインフラストラクチャー上のAdobe Commerceと PrivateLink 統合のイネーブルメントプロセスの概要を説明します。

1. **顧客** 件名行 `PrivateLink support for <company>` で PrivateLink の有効化をリクエストするサポートチケットを送信します。 [ イネーブルメントに必要なデータ ](#prerequisites) をチケットに含めます。 Adobeは、サポートチケットを使用して、イネーブルメントプロセス中のコミュニケーションを調整します。

1. **Adobe** カスタマーアカウントがAdobeVPCの endpoint service にアクセスできるようにします。

   - Adobeエンドポイントサービス設定を更新して、カスタマーAWSまたは Azure アカウントから開始されたリクエストを受け入れます。
   - サポートチケットを更新して、接続先のAdobeVPC エンドポイントのサービス名（例：`com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`）を指定します。

1. **顧客** Cloud アカウント（AWSまたは Azure）にAdobeエンドポイントサービスを追加します。これにより、Adobeへの接続リクエストがトリガーされます。 手順については、Cloud Platform のドキュメントを参照してください。

   - AWSについては、[ インターフェイスエンドポイント接続リクエストの承認と却下 ] を参照してください。
   - Azure については、[ 接続リクエストの管理 ] を参照してください。

1. **Adobe** が接続リクエストを承認します。

1. 接続リクエストの承認後、VPCとAdobe VPCの間の **お客様**&#x200B;[ 接続を検証 ](#test-vpc-endpoint-service-connection) します。

1. 双方向接続を有効にするための追加手順：

   - **Adobe** Adobeアカウントプリンシパル（AWSまたは Azure アカウントのルートユーザー）を提供し、customer VPC エンドポイントサービスへのアクセスをリクエストします。
   - **顧客** カスタマーVPCのエンドポイントサービスへのAdobeアクセスを有効にします。 これは、前述の **Endpoint service access granted** 前提条件で説明したように、Adobeアカウントプリンシパルが `arn:aws:iam::402592597372:root` へのアクセス権を持っていることを前提としています。

      - Adobeアカウントから開始されたリクエストを受け入れるように、カスタマーエンドポイントサービス設定を更新します。 手順については、Cloud Platform のドキュメントを参照してください。

         - AWSについては、[ エンドポイントサービスの権限の追加と削除 ] を参照してください。
         - Azure については、「[ プライベートエンドポイント接続の管理 ]」を参照してください。

      - カスタマーVPCのエンドポイントサービス名でAdobeを指定します。

   - **Adobe** カスタマーエンドポイントサービスをAdobeプラットフォームアカウント（AWSまたは Azure）に追加します。これにより、カスタマーVPCへの接続リクエストがトリガーされます。
   - **お客様**、Adobeからの接続リクエストを承認して設定を完了します。
   - **お客様** AdobeVPCから [ 接続を検証 ](#test-vpc-endpoint-service-connection) します。

## VPC Endpoint Service 接続のテスト

Telnet アプリケーションを使用して、VPC エンドポイントサービスへの接続をテストできます。

**VPC エンドポイントサービスへの接続をテストするには**:

1. PrivateLink エンドポイントサービスにアクセスするように設定されたステージング環境または実稼動環境を、プロジェクトのルートディレクトリから **チェックアウト** します。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 次の CURL コマンドを実行します。

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   例：

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   成功した応答のサンプル：

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   失敗した応答のサンプル：

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. サービスが VM をリッスンしていることを確認します。

   ```bash
   netstat -na | grep <port>
   ```

1. パッケージフローを確認します。

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   次の内部設定をチェックして、設定が有効であることを確認します。

   - エンドポイントとエンドポイントサービスの設定
   - ネットワーク ロード バランサー（NLB）の設定
   - NLB 内のターゲット グループは正常であることを確認します
   - 各 VM の netcat/curl エンドポイント URL （上記を参照）

   接続に関する問題のトラブルシューティングについて詳しくは、次の記事を参照してください。

   - [AWS: エンドポイント サービス接続のトラブルシューティング ]
   - [Amazon: Azure プライベート リンク接続の問題のトラブルシューティング ]

   エラーを解決できない場合は、Adobe Commerce サポートチケットを更新して、接続の確立に関するヘルプをリクエストします。

## PrivateLink 設定の変更

既存の PrivateLink 設定を変更するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) します。 例えば、次のような変更をリクエストできます。

- Cloud infrastructure Pro 実稼動環境またはステージング環境のAdobe Commerceから PrivateLink 接続を削除します。
- Adobeエンドポイントサービスにアクセスするための顧客の Cloud Platform アカウント番号を変更します。
- Adobe VPCから、カスタマーVPC環境で使用可能な他のエンドポイントサービスへの PrivateLink 接続を追加または削除します。

## 双方向の PrivateLink 接続用の設定

双方向の PrivateLink 接続をサポートするには、お客様のVPCに次のリソースが必要です。

- ネットワーク ロード バランサー（NLB）
- 顧客VPCからのアプリケーションまたはサービスへのアクセスを有効にするエンドポイントサービス設定
- VPCでホストされているエンドポイントサービスにAdobeが接続できる [ インターフェイスエンドポイント ] （AWS）または [ プライベートエンドポイント ] （Azure）

これらのリソースがカスタマーVPCで使用できない場合は、Cloud Platform アカウントにサインインして設定を追加する必要があります。

- Amazon VPC コンソール - `https://console.aws.amazon.com/vpc/`
- Azure portal- `https://portal.azure.com`

PrivateLink の設定手順については、Cloud Platform のドキュメントを参照してください。

- **AWS PrivateLink のドキュメント**
   - [ ネットワークロードバランサーの作成 ]
   - [ エンドポイントサービス設定の作成 ]
   - [ インターフェイスエンドポイントの作成 ]
   - [ インターフェイス エンドポイントのライフサイクル ]

- **Azure PrivateLink のドキュメント**
   - [ ロードバランサーの作成 ]
   - [Azure プライベートリンクのワークフロー ]

<!--Link definitions-->

[インターフェイスエンドポイント接続リクエストの承認と却下]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[エンドポイントサービスの権限の追加と削除]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon:Azure プライベートリンク接続の問題のトラブルシューティング]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS：エンドポイントサービス接続のトラブルシューティング]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Azure プライベートリンクのワークフロー]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[ロードバランサーの作成]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[ネットワークロードバランサーの作成]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[エンドポイントサービス設定の作成]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[インターフェイスエンドポイントの作成]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[インターフェイス エンドポイント]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[プライベートエンドポイント接続の管理]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[接続リクエストの管理]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[プライベートエンドポイント]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
