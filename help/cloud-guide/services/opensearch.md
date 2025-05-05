---
title: OpenSearch サービスの設定
description: クラウドインフラストラクチャー上でAdobe Commerceの OpenSearch サービスを有効にする方法について説明します。
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# OpenSearch サービスの設定

[OpenSearch](https://www.opensearch.org) サービスは、Elasticsearchのライセンスの変更に伴い、Elasticsearch7.10.2 のオープンソースのフォークになりました。 詳しくは、GitHub の [OpenSource プロジェクト ](https://github.com/opensearch-project) を参照してください。

{{elasticsearch-support}}

OpenSearch を使用すると、任意のソース、任意の形式からデータを取得し、リアルタイムで検索および視覚化できます。

- 製品カタログ内の製品に対するクイック検索と詳細検索
- OpenSearch アナライザーは複数の言語をサポートしています
- ストップワードおよび同義語のサポート
- インデックス再作成が完了するまで、インデックス作成は顧客に影響しません

{{service-instruction}}

>[!TIP]
>
>Adobeでは、Adobe Commerce アプリケーションにサードパーティの検索ツールを設定する予定がある場合でも、クラウドインフラストラクチャプロジェクトでAdobe Commerce用に常に OpenSearch を設定することをお勧めします。 OpenSearch を設定すると、サードパーティの検索ツールでエラーが発生した場合にフォールバックオプションを利用できます。

**OpenSearch を有効にするには**:

1. スターターおよび Pro 統合環境の場合は、適切なバージョンと割り当てられたディスク容量（MB 単位）を使用して `opensearch` サービスを `.magento/services.yaml` ファイルに追加します。 この場合、バージョン 2 が適切です。 クラウドインフラストラクチャでは最新バージョンの OpenSearch が使用されるので、マイナーバージョンは必要ありません。

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Pro プロジェクトの場合、ステージング環境と実稼動環境で OpenSearch バージョンを変更するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) する必要があります。

1. `.magento.app.yaml` ファイルの `relationships` プロパティを設定または確認します。

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   これらの変更が環境に与える影響について詳しくは、[ サービスの設定 ](services-yaml.md) を参照してください。

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

## OpenSearch ソフトウェアの互換性

クラウドインフラストラクチャプロジェクトでAdobe Commerceをインストールまたはアップグレードする際は、常に OpenSearch サービスのバージョンと、Adobe Commerce用の [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) クライアントとの互換性を確認してください。

- **初回セットアップ**- `services.yaml` ファイルで指定された OpenSearch バージョンが、Adobe Commerce用に設定された OpenSearch PHP クライアントと互換性があることを確認します。

- **プロジェクトのアップグレード** – 新しいアプリケーションバージョンの OpenSearch PHP クライアントが、クラウドインフラストラクチャにインストールされた OpenSearch サービスバージョンと互換性があることを確認します。

サービスのバージョンと互換性のサポートは、Cloud Infrastructure でテストおよびデプロイされたバージョンによって決まり、Adobe Commerceのオンプレミスデプロイメントでサポートされているバージョンとは異なる場合があります。 サポートされているバージョンの一覧については、『インストール ガイド _の [&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=ja) システム要件_ を参照してください。

**OpenSearch ソフトウェアの互換性を確認するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. アクティブな環境の OpenSearch の詳細を表示します。

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. または、SSH を使用してリモート環境にログインすることもできます。

   ```bash
   magento-cloud ssh
   ```

1. OpenSearch サービス接続の詳細を取得します。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   応答で、OpenSearch サービスエンドポイントの IP アドレスとポートを見つけます。

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. サービスエンドポイントから、インストールされている OpenSearch サービス `version:number` を取得します。

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## OpenSearch サービスを再起動します

OpenSearch サービスを再起動する必要がある場合は、Adobe Commerce サポートに連絡してください。

## 追加の検索設定

- デフォルトでは、クラウド環境の検索設定は、デプロイするたびに再生成されます。 `SEARCH_CONFIGURATION` デプロイ変数を使用して、デプロイメント間でカスタム検索設定を保持できます。 [ 変数のデプロイ ](../environment/variables-deploy.md#search_configuration) を参照してください。

- プロジェクトの OpenSearch サービスを設定した後、管理 UI を使用して OpenSearch 接続をテストし、Adobe Commerceの OpenSearch 設定をカスタマイズします。

### OpenSearch 用プラグインの追加

オプションで、`.magento/services.yaml` ファイルの OpenSearch サービスに `configuration:plugins` セクションを追加することで、OpenSearch 用のプラグインを追加できます。 例えば、次のコードは ICU 分析と音声分析プラグインを有効にします。

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

プラグインについて詳しくは、[OpenSearch プロジェクト ](https://github.com/opensearch-project) を参照してください。

### OpenSearch のプラグインを削除

`.magento/services.yaml` ファイルの `opensearch:` セクションからプラグイン エントリを削除しても、サービスはアンインストールまたは無効化 **されません**。 このサービスを完全に無効にするには、`.magento/services.yaml` ファイルからプラグインを削除した後、OpenSearch データのインデックスを再作成する必要があります。 この設計により、これらのプラグインに依存するデータの損失や破損を防ぐことができます。

**OpenSearch プラグインを削除するには**:

1. `.magento/services.yaml` ファイルから OpenSearch プラグインのエントリを削除します。
1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
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
