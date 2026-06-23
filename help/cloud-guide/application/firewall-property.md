---
title: ファイアウォールプロパティ
description: Commerce アプリケーション設定ファイルでfirewall プロパティを設定する方法の例を参照してください。
feature: Cloud, Configuration, Security
exl-id: 27704633-78c7-4b5a-b415-6ce5f02bd5e4
TQID: https://experienceleague.adobe.com/8b69VjT0HmEAwStlvvqYHAoINPKVBnvSUvsB5uC4T84
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: bd989d82-1e15-4534-88db-f1f51dd77ffaid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 861
ht-degree: 0%

---

# ファイアウォールプロパティ

>[!IMPORTANT]
>
>スタータープロジェクトのみ

スタータープロジェクトの場合、`firewall` プロパティは&#x200B;_アウトバウンド_ ファイアウォールをアプリケーションに追加します。 このファイアウォールは、受信リクエストには影響しません。 Adobe Commerce サイトを&#x200B;_離れる_&#x200B;ことができる`tcp`送信リクエストを定義します。 これはエグレスフィルタリングと呼ばれます。 アウトバウンドファイアウォールは、サイトを離脱またはエスケープする可能性のあるものをフィルタリングします。 エスケープできるものを制限すると、サーバーに強力なセキュリティツールが追加されます。

## デフォルトの制限ポリシー

ファイアウォールには、アウトバウンドトラフィックを制御するための2つのデフォルト ポリシーが用意されています：`allow`と`deny`。 `allow` ポリシー&#x200B;_では、すべてのアウトバウンドトラフィックをデフォルトで_&#x200B;許可しています。 `deny` ポリシー&#x200B;_は、デフォルトですべてのアウトバウンドトラフィックを_&#x200B;拒否します。 しかし、ルールを追加すると、デフォルトのポリシーが上書きされ、ルールで許可されていない&#x200B;**all**&#x200B;の送信トラフィックがファイアウォールでブロックされます。

スタータープランの場合、デフォルトのポリシーは`allow`に設定されます。 この設定では、出力フィルタールールを追加するまで、現在のすべてのアウトバウンドトラフィックがブロック解除されたままになります。 デフォルトのポリシーは、リクエストに応じて`deny`に設定できます。

**デフォルトポリシーを確認するには**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

ポリシーの`deny`を要求しない限り、コマンドはポリシーを`allow`に設定する必要があります。

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**重要ポイント**：送信ルールを追加すると、ルールに追加するドメイン、IP アドレス、またはポートを除くすべての送信トラフィックがブロックされます。 そのため、本番サイトに追加する前に、包括的なアウトバウンドリストを定義し、テストすることが重要です。

## ファイアウォールオプション

`.magento.app.yaml` ファイルの次の設定例は、出力フィルタリングのルールを追加するために使用できるすべての`firewall` オプションを示しています。

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## 出力フィルタールール

アウトバウンドファイアウォール設定は、ルールで構成されます。 必要なだけルールを定義できます。 ルールの要件は次のとおりです。

**各ルール：**

- ハイフン （`-`）で始める必要があります。 同じ行にコメントを追加すると、1つのルールを文書化し、視覚的に次のルールから分離できます。
- 次のいずれかのオプションを定義する必要があります：`domains`、`ips`、または`ports`。
- `tcp` プロトコルを使用する必要があります。 これは、すべてのルールのデフォルトのプロトコルなので、ルールから省略できます。
- `domains`または`ips`を定義できますが、両方を同じルールで定義することはできません。
- `yaml`件のコメント （`#`）と改行を含めて、許可されるドメイン、IP アドレス、およびポートを整理できます。

各ルールでは、次のプロパティを使用します。

### `domains`

`domains` オプションを使用すると、完全修飾ドメイン名（FQDN）のリストを許可できます。

ルールで`domains`が定義されていて`ports`が定義されていない場合、ファイアウォールでは、任意のポートでドメイン要求が許可されます。

### `ips`

`ips` オプションを使用すると、CIDR表記法でIP アドレスのリストを許可できます。 単一のIP アドレスまたはIP アドレスの範囲を指定できます。

1つのIP アドレスを指定するには、IP アドレスの末尾に`/32` CIDR プレフィックスを追加します。

```
172.217.11.174/32  # google.com
```

IP アドレスの範囲を指定するには、[IP Range to CIDR](https://ipaddressguide.com/cidr)計算機を使用します。

ルールで`ips`が定義されていて`ports`が定義されていない場合、ファイアウォールでは、任意のポートでIP要求が許可されます。

### `ports`

`ports` オプションを使用すると、1から65535までのポートのリストを許可できます。 この例のほとんどのルールでは、ポート `80`と`443`でHTTP要求とHTTPS要求の両方が許可されています。 ただし、New Relicの場合、このルールではポート `443`上のドメインとIP アドレスへのアクセスのみが許可されます。これは、[ ネットワークトラフィック ](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents)に関するNew Relic ドキュメントで推奨されています。

ルールが`ports`のみを定義する場合、ファイアウォールでは、定義されたポートのすべてのドメインとIP アドレスへのアクセスが許可されます。

>[!NOTE]
>
>メールを送信するSMTP ポートであるポート `25`は、例外なく常にブロックされます。

### `protocol`

前述したように、TCPはデフォルトであり、ルールに許可される唯一のプロトコルです。 UDPとそのポートは許可されていません。 このため、すべてのルールから`protocol` オプションを省略できます。 それでも含める場合は、例の最初のルールに示すように、値を`tcp`に設定する必要があります。

## 許可するドメイン名の検索

エグレス フィルタリング ルールに含めるドメインを特定するには、次のコマンドを使用してサーバーの`dns.log` ファイルを解析し、サイトがログに記録したすべてのDNS リクエストのリストを表示します。

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

このコマンドは、エグレス フィルタリング ルールによって作成されたがブロックされたDNS リクエストも表示します。 出力には、ブロックされたドメインが表示されず、リクエストが実行されたことが示されます。 出力には、IP アドレスを使用して行われたリクエストは表示されません。

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

IP アドレスとは対照的に、ドメインは通常、エグレス フィルタリングに対してより具体的で安全です。 例えば、CDNを使用するサービスにIP アドレスを追加する場合、CDNのIP アドレスを許可します。このIP アドレスは、他の数百または数千のドメインで使用できます。 1つのIP アドレスを使用すると、他の何千ものサーバーへのアウトバウンドアクセスを許可することができます。

## 出力フィルタリングルールのテスト

サイトに必要なドメインとIP アドレスのアクセスルールを収集して設定したら、プッシュとテストを実行します。

出力フィルタリングルールをテストするには：

1. ルール内のドメインとIP アドレスにアクセスするための`curl` コマンドのシェルスクリプトを作成します。 ブロックする必要があるドメインとIP アドレスへのアクセスをテストするコマンドを含めます。

1. スクリプトを実行するには、`.magento.app.yaml` ファイルで`post_deploy` フックを設定します。

1. `firewall`設定とテスト スクリプトを`integration` ブランチにプッシュします。

1. `curl` コマンドから`post_deploy`出力を確認します。

1. `firewall` ルールを絞り込み、`curl` スクリプトを更新し、コミット、プッシュ、および繰り返します。

### `curl` スクリプトの例

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy`の例

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```

