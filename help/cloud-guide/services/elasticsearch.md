---
title: Elasticsearchサービスの設定
description: クラウドインフラストラクチャー上でAdobe CommerceのElasticsearchサービスを有効にする方法について説明します。
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Elasticsearchサービスの設定

[Elasticsearch](https://www.elastic.co) は、あらゆるソース、あらゆる形式からデータを取得し、リアルタイムで検索および視覚化できるオープンソース製品です。

{{elasticsearch-support}}

Adobe Commerce バージョン 2.4.4 以降については、[OpenSearch サービスの設定 ](opensearch.md) を参照してください。

- Elasticsearchが商品カタログ内の商品に対してクイック検索と詳細検索を実行します
- Elasticsearchアナライザーは複数の言語をサポートしています
- ストップワードおよび同義語のサポート
- インデックス再作成が完了するまで、インデックス作成は顧客に影響しません

>[!TIP]
>
>Adobeでは、Adobe Commerce アプリケーションにサードパーティの検索ツールを設定する予定がある場合でも、クラウドインフラストラクチャプロジェクトでAdobe CommerceのElasticsearchを常に設定することをお勧めします。 Elasticsearchの設定では、サードパーティの検索ツールでエラーが発生した場合に備えてフォールバックオプションを提供しています。

{{service-instruction}}

**Elasticsearchを有効にするには**:

1. スタータープロジェクトの場合、Elasticsearchのバージョンと割り当てられたディスク領域（MB 単位）を指定して `elasticsearch` サービスを `.magento/services.yaml` ファイルに追加します。

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pro プロジェクトの場合、ステージング環境と実稼動環境でElasticsearchのバージョンを変更するには、Adobe Commerce サポートチケットを送信する必要があります。

1. `.magento.app.yaml` ファイルの `relationships` プロパティを設定します。

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   これらの変更が環境に与える影響について詳しくは、[ サービス ](services-yaml.md) を参照してください。

1. デプロイメントプロセスが完了したら、SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. カタログ検索インデックスを再インデックス化します。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーンアップします。

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Elasticsearchソフトウェアの互換性

クラウドインフラストラクチャプロジェクトにAdobe Commerceをインストールまたはアップグレードする際は、ElasticsearchサービスのバージョンとAdobe Commerce用の [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) クライアントとの互換性を常に確認してください。

- **初回セットアップ**- `services.yaml` ファイルで指定されたElasticsearchバージョンが、Adobe Commerce用に設定されたElasticsearch PHP クライアントと互換性があることを確認します。

- **プロジェクトのアップグレード** – 新しいアプリケーションバージョンのElasticsearch PHP クライアントが、クラウドインフラストラクチャにインストールされたElasticsearchサービスのバージョンと互換性があることを確認します。

クラウドインフラストラクチャにおけるAdobe Commerceのサービスバージョンと互換性のサポートは、クラウドインフラストラクチャにデプロイされたバージョンによって決まり、Adobe Commerceのオンプレミスデプロイメントでサポートされているバージョンとは異なる場合があります。 [ サービスバージョン ](services-yaml.md#service-versions) を参照してください。

**Elasticsearchソフトウェアの互換性を確認するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. アクティブなElasticsearchの環境の詳細を表示します。

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. または、SSH を使用してリモート環境にログインすることもできます。

   ```bash
   magento-cloud ssh
   ```

1. `elasticsearch/elasticsearch` の Composer パッケージのバージョンを確認します。

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   応答で、`versions` プロパティのインストールされているバージョンを確認します。

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

   また、Elasticsearchのルートディレクトリの `composer.lock` ファイルに、環境の PHP クライアントのバージョンがあります。

1. コマンドラインから、Elasticsearchサービス接続の詳細を取得します。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   応答で、Elasticsearchサービスエンドポイントの IP アドレスを見つけます。

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

1. サービスエンドポイントから、インストールされているElasticsearchサービス `version:number` を取得します。

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

1. Elasticsearchサービスと PHP クライアントのバージョンの互換性を確認します。

   バージョンに互換性がない場合は、環境設定を次のいずれかの更新をおこないます。

   - Elasticsearch PHP クライアントを、Elasticsearchサービスのバージョンと互換性のあるバージョンに変更します。

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - `services.yaml` ファイルのElasticsearchサービスのバージョンを、Elasticsearchの PHP クライアントと互換性のあるバージョンに変更します。

     {{pro-update-service}}

## Elasticsearchサービスを再起動します

[Elasticsearch](https://www.elastic.co) サービスを再起動する必要がある場合は、Adobe Commerce サポートに連絡してください。

## 追加の検索設定

- デフォルトでは、クラウド環境の検索設定は、デプロイするたびに再生成されます。 `SEARCH_CONFIGURATION` デプロイ変数を使用して、デプロイメント間でカスタム検索設定を保持できます。 [ 変数のデプロイ ](../environment/variables-deploy.md#search_configuration) を参照してください。

- プロジェクトのElasticsearchサービスを設定した後、管理 UI を使用してElasticsearchElasticsearchをテストし、Adobe Commerceの接続設定をカスタマイズします。

### Elasticsearch用プラグインの追加

オプションで、`.magento/services.yaml` ファイルのElasticsearchサービスに `configuration:plugins` セクションを追加することで、Elasticsearch用のプラグインを追加できます。 例えば、次のコードは ICU 分析と音声分析プラグインを有効にします。

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Elastic Suite サードパーティプラグインを使用する場合は、[`ece-tools` パッケージをバージョン 2002.0.19 以降に更新 ](../dev-tools/update-package.md) する必要があります。
Elastic Suite を設定する際に、`ELASTICSUITE_CONFIGURATION` のデプロイ変数に設定を追加します。 この設定により、デプロイメント全体で設定が保存されます。

### Elasticsearch用プラグインを削除

`.magento/services.yaml` の `elasticsearch:` からプラグインエントリを削除しても、期待どおりにアンインストールや無効化は行われません。 Elasticsearchデータを再インデックスする必要があります。 この動作は、これらのプラグインに依存するデータの損失や破損の可能性を防ぐことを目的としています。

**Elasticsearchプラグインを削除するには**:

1. `.magento/services.yaml` ファイルからElasticsearchプラグインのエントリを削除します。
1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. `.magento/services.yaml` の変更をクラウドリポジトリにコミットします。
1. カタログ検索インデックスを再インデックス化します。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーンアップします。

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Adobe Commerceでの Elastic Suite プラグインの使用またはトラブルシューティングについて詳しくは、[Elastic Suite のドキュメント ](https://github.com/Smile-SA/elasticsuite) を参照してください。

## トラブルシューティング

Elasticsearchの問題のトラブルシューティングについては、以下のAdobe Commerce サポートの記事を参照してください。

- [Elasticsearch 5 が設定されましたが、「Fielddata が無効です…」エラーで検索ページが読み込まれません ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html?lang=ja)
- [Adobe CommerceのElasticsearchのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Elasticsearch インデックスの状態は `yellow` または `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html?lang=ja) です
