---
title: Elasticsearch サービスの設定
description: Adobe CommerceのクラウドインフラストラクチャでElasticsearch サービスを有効にする方法について説明します。
feature: Cloud, Search, Services
exl-id: 238b9ed5-ce73-428f-9459-35de8573d5d8
TQID: https://experienceleague.adobe.com/RYv3SjF62YHhPtM9vFrlPD0MVwfPS7EIhHxQXaMEeuI
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 739
ht-degree: 0%

---

# Elasticsearch サービスの設定

[Elasticsearch](https://www.elastic.co)は、あらゆるソースからデータを取り込み、任意の形式で検索してリアルタイムで視覚化できるオープンソース製品です。

{{elasticsearch-support}}

Adobe Commerce バージョン 2.4.4以降については、[OpenSearch サービスの設定](opensearch.md)を参照してください。

- Elasticsearchでは、商品カタログ内の商品に対して、迅速かつ高度な検索を実行します
- Elasticsearch Analyzersは多言語をサポートしています
- ストップワードと類義語をサポート
- インデックス再作成操作が完了するまで、インデックス作成は顧客に影響しません

>[!TIP]
>
>Adobeでは、Adobe Commerce アプリケーションのサードパーティ検索ツールを設定する予定がある場合でも、Adobe Commerce on cloud infrastructure プロジェクトにElasticsearchを常に設定することをお勧めします。 Elasticsearchを設定すると、サードパーティの検索ツールが失敗した場合に備えて、フォールバックオプションが提供されます。

{{service-instruction}}

**Elasticsearchを有効にするには**:

1. スタータープロジェクトの場合は、`elasticsearch` サービスを`.magento/services.yaml` ファイルに追加し、Elasticsearch バージョンと割り当てられたディスク容量をMB単位で指定します。

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pro プロジェクトの場合、ステージング環境と実稼動環境でElasticsearch バージョンを変更するには、Adobe Commerce サポートチケットを送信する必要があります。

1. `.magento.app.yaml` ファイルで`relationships` プロパティを設定します。

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   これらの変更が環境にどのような影響を与えるかについては、[&#x200B; サービス &#x200B;](services-yaml.md)を参照してください。

1. デプロイメントプロセスが完了したら、SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. カタログ検索インデックスのインデックスを再作成します。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーニングします。

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Elasticsearch ソフトウェアの互換性

クラウドインフラストラクチャプロジェクト上のAdobe Commerceをインストールまたはアップグレードする場合は、常にElasticsearch サービスのバージョンとAdobe Commerce用[Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) クライアントとの互換性を確認してください。

- **初回設定**- `services.yaml` ファイルで指定されたElasticsearch バージョンが、Adobe Commerce用に設定されたElasticsearch PHP クライアントと互換性があることを確認します。

- **プロジェクトのアップグレード** – 新しいアプリケーションのバージョンのElasticsearch PHP クライアントが、クラウドインフラストラクチャにインストールされているElasticsearch サービスのバージョンと互換性があることを確認します。

Adobe Commerce on cloud infrastructureのサービスバージョンと互換性のサポートは、クラウドインフラストラクチャにデプロイされたバージョンによって決まり、Adobe Commerce オンプレミスのデプロイメントでサポートされているバージョンとは異なる場合があります。 [&#x200B; サービスバージョン &#x200B;](services-yaml.md#service-versions)を参照してください。

**Elasticsearch ソフトウェアの互換性を確認するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. アクティブな環境のElasticsearchの詳細を表示します。

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. また、SSHを使用してリモート環境にログインすることもできます。

   ```bash
   magento-cloud ssh
   ```

1. `elasticsearch/elasticsearch`のComposer パッケージのバージョンを確認してください。

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   応答で、`versions` プロパティのインストール済みバージョンを確認してください。

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   また、Elasticsearch PHP クライアントのバージョンは、環境ルートディレクトリの`composer.lock` ファイルにあります。

1. コマンドラインから、Elasticsearch サービス接続の詳細を取得します。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   応答で、Elasticsearch サービスエンドポイントのIP アドレスを見つけます。

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. インストール済みのElasticsearch サービス `version:number`をサービスエンドポイントから取得します。

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Elasticsearch サービスとPHP クライアントのバージョンの互換性を確認します。

   バージョンに互換性がない場合は、環境設定に次のいずれかの更新を行います。

   - Elasticsearch PHP クライアントを、Elasticsearch サービスのバージョンと互換性のあるバージョンに変更します。

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - `services.yaml` ファイルのElasticsearch サービスのバージョンを、Elasticsearch PHP クライアントと互換性のあるバージョンに変更します。

     {{pro-update-service}}

## Elasticsearch サービスを再起動します

[Elasticsearch](https://www.elastic.co) サービスを再起動する必要がある場合は、Adobe Commerce サポートにお問い合わせください。

## 追加の検索設定

- デフォルトでは、クラウド環境の検索設定は、デプロイするたびに再生成されます。 `SEARCH_CONFIGURATION` デプロイ変数を使用して、デプロイ間でカスタム検索設定を保持できます。 [変数のデプロイ &#x200B;](../environment/variables-deploy.md#search_configuration)を参照してください。

- プロジェクトにElasticsearch サービスを設定した後、管理者UIを使用してElasticsearch接続をテストし、Adobe CommerceのElasticsearch設定をカスタマイズします。

### Elasticsearch用プラグインの追加

オプションで、`.magento/services.yaml` ファイルのElasticsearch サービスに`configuration:plugins` セクションを追加することで、Elasticsearchのプラグインを追加できます。 例えば、次のコードでは、ICU解析と音声解析プラグインを有効にします。

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Elastic Suite サードパーティプラグインを使用する場合は、`ece-tools` パッケージ [&#128279;](../dev-tools/update-package.md)をバージョン 2002.0.19以降に更新する必要があります。
Elastic Suiteを設定する際に、設定設定を`ELASTICSUITE_CONFIGURATION` デプロイ変数に追加します。この設定は、デプロイメント間で設定を保存します。

### Elasticsearchのプラグインの削除

`.magento/services.yaml`の`elasticsearch:`からプラグイン エントリを削除しても、期待どおりにアンインストールも無効化もされません。 Elasticsearch データを再インデックスする必要があります。 この動作は、これらのプラグインに依存するデータの損失や破損を防ぐことを目的としています。

**Elasticsearch プラグインを削除するには**:

1. `.magento/services.yaml` ファイルからElasticsearch プラグインのエントリを削除します。
1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Cloud リポジトリに`.magento/services.yaml`の変更をコミットします。
1. カタログ検索インデックスのインデックスを再作成します。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーニングします。

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Adobe CommerceでのElastic Suite プラグインの使用またはトラブルシューティングについて詳しくは、[Elastic Suite ドキュメント &#x200B;](https://github.com/Smile-SA/elasticsuite)を参照してください。

