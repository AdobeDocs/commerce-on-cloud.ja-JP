---
title: ファイアウォールプロパティ
description: Commerce アプリケーション設定ファイルでファイアウォールプロパティを設定する方法の例を参照してください。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# ファイアウォールプロパティ

>[!IMPORTANT]
>
>スタータープロジェクトのみ

スタータープロジェクトの場合、`firewall` プロパティは、アプリケーションに _アウトバウンド_ ファイアウォールを追加します。 このファイアウォールは、受信リクエストには影響しません。 Adobe Commerce サイトから _退出_ できるアウトバウンドリクエストの `tcp` を定義します。 これは、エグレスフィルタリングと呼ばれます。 アウトバウンドファイアウォールは、サイトから出たり、サイトから出たりできるものをフィルタリングします。 エスケープできるものを制限すると、サーバーに強力なセキュリティツールが追加されます。

## デフォルトの制限ポリシー

ファイアウォールには、送信トラフィックを制御する 2 つのデフォルトのポリシー（`allow` と `deny`）が用意されています。 `allow` ポリシー _許可_ は、デフォルトですべての送信トラフィックを許可します。 また、`deny` ポリシーはデフォルトですべての送信トラフィックを _拒否_ します。 ただし、規則を追加すると、既定のポリシーは上書きされ、ファイアウォールは規則で許可されていない送信トラフィック **すべて** をブロックします。

スタータープランの場合、デフォルトのポリシーは `allow` に設定されています。 この設定により、出力フィルタリングルールを追加するまで、現在のすべての送信トラフィックがブロック解除されたままになります。 デフォルトのポリシーは、リクエストに応じて `deny` に設定できます。

**デフォルトポリシーを確認するには**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

ポリシーに対して `deny` をリクエストしない限り、コマンドはポリシーが `allow` に設定されていることを表示します。

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**キーテイクアウト**：送信規則を追加すると、その規則に追加したドメイン、IP アドレス、またはポートを除くすべての送信トラフィックがブロックされます。 そのため、実稼動サイトに追加する前に、完全なアウトバウンドリストを定義しテストしておくことが重要です。

## ファイアウォールオプション

`.magento.app.yaml` ファイルにある次の設定例には、エグレスフィルタリングのルールを追加するために使用できる `firewall` のすべてのオプションが示されています。

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

送信ファイアウォール設定は、ルールで構成されています。 必要な数のルールを定義できます。 ルールの要件は次のとおりです。

**各ルール：**

- ハイフン（`-`）で始める必要があります。 同じ行にコメントを追加すると、ドキュメントを作成して、ルールを次のルールと視覚的に区別するのに役立ちます。
- `domains`、`ips`、`ports` のオプションのうち、少なくとも 1 つを定義する必要があります。
- `tcp` プロトコルを使用する必要があります。 これは、すべてのルールのデフォルトのプロトコルなので、ルールから省略できます。
- `domains` または `ips` を定義できますが、両方を同じルールに含めることはできません。
- `yaml` コメント（`#`）と改行を含めて、許可されるドメイン、IP アドレス、ポートを整理できます。

各ルールでは、次のプロパティを使用します。

### `domains`

`domains` オプションを使用すると、完全修飾ドメイン名（FQDN）の一覧が許可されます。

ルールで `ports` が定義されていない場合 `domains`、ファイアウォールでは任意のポートでのドメインリクエストが許可されます。

### `ips`

`ips` オプションを使用すると、CIDR 表記で IP アドレスのリストが許可されます。 単一の IP アドレスまたは IP アドレスの範囲を指定できます。

1 つの IP アドレスを指定するには、IP アドレスの末尾に `/32` の CIDR プレフィックスを追加します。

```
172.217.11.174/32  # google.com
```

IP アドレスの範囲を指定するには、[IP 範囲から CIDR](https://ipaddressguide.com/cidr) 計算ツールを使用します。

規則によって `ports` が定義され `ips` いない場合、ファイアウォールは任意のポートでの IP 要求を許可します。

### `ports`

`ports` オプションを使用すると、1 から 65535 までのポートのリストが許可されます。 この例のほとんどのルールでは、ポート `80` および `443` は HTTP と HTTPS の両方のリクエストを許可します。 ただし、New Relicの場合、ルールでは、[&#x200B; ネットワークトラフィック &#x200B;](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents) に関するNew Relicのドキュメントで推奨されているように、ポート `443` のドメインおよび IP アドレスへのアクセスのみが許可されます。

ルールで `ports` のみが定義されている場合、ファイアウォールでは、定義されているポートのすべてのドメインと IP アドレスへのアクセスが許可されます。

>[!NOTE]
>
>メールを送信する SMTP ポートであるポート `25` は、例外なく常にブロックされます。

### `protocol`

前述のように、TCP はデフォルトであり、ルールに使用できる唯一のプロトコルです。 UDP とそのポートは許可されていません。 このため、すべてのルールから `protocol` オプションを省略できます。 それでも含める場合は、例の最初のルールに示すように、値を `tcp` に設定する必要があります。

## 許可するドメイン名の検索

出力フィルタリングルールに含めるドメインを特定しやすくするには、次のコマンドを使用してサーバーの `dns.log` ファイルを解析し、サイトがログ記録したすべての DNS リクエストのリストを表示します。

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

このコマンドは、送信フィルタリングルールによってブロックされた DNS リクエストも表示します。 出力には、ブロックされたドメインは表示されず、リクエストが行われただけです。 IP アドレスを使用して行われたリクエストは出力に表示されません。

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

IP アドレスとは異なり、ドメインは通常、エグレスフィルタリングに対してより具体的で安全です。 例えば、CDN を使用するサービスの IP アドレスを追加する場合、CDN の IP アドレスを許可します。これは、数百または数千の他のドメインで使用できます。 1 つの IP アドレスで、他の何千ものサーバーへのアウトバウンドアクセスを許可できます。

## 出力フィルタールールのテスト

サイトで必要なドメインおよび IP アドレスのアクセスルールを収集して設定したら、プッシュしてテストします。

出力フィルタリングルールをテストするには：

1. ルール内のドメインと IP アドレスにアクセスするための `curl` コマンドのシェルスクリプトを作成します。 ブロックする必要があるドメインおよび IP アドレスへのアクセスをテストするコマンドを含めます。

1. `.magento.app.yaml` ファイルに `post_deploy` フックを設定して、スクリプトを実行します。

1. `firewall` 設定とテストスクリプトを `integration` ブランチにプッシュします。

1. `curl` コマンドからの `post_deploy` 出力を調べます。

1. `firewall` ルールを調整し、`curl` スクリプトを更新し、コミット、プッシュおよび繰り返します。

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

### `post_deploy` 例

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
