---
title: OpenSearch サービスの設定
description: クラウドインフラストラクチャー上でAdobe Commerceの OpenSearch サービスを有効にする方法について説明します。
feature: Cloud, Search, Services
exl-id: e704ab2a-2f6b-480b-9b36-1e97c406e873
source-git-commit: 5a190471f4ccc23eb1c311f3082af1948cf1c68d
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 0%

---

# OpenSearch サービスの設定

[OpenSearch](https://www.opensearch.org) サービスは、Elasticsearch 7.10.2 のオープンソースのフォークで、Elasticsearchのライセンスの変更に従っています。 詳しくは、GitHub の [OpenSource プロジェクト &#x200B;](https://github.com/opensearch-project) を参照してください。

{{elasticsearch-support}}

OpenSearch を使用すると、任意のソース、任意の形式からデータを取得し、リアルタイムで検索および視覚化できます。

- 製品カタログ内の製品に対するクイック検索と詳細検索
- OpenSearch アナライザーは複数の言語をサポートしています
- ストップワードおよび同義語のサポート
- インデックス再作成が完了するまで、インデックス作成は顧客に影響しません

{{service-instruction}}

>[!TIP]
>
>[Live Search](https://experienceleague.adobe.com/en/docs/commerce/live-search/overview) を使用しないクラウドインフラストラクチャプロジェクトのAdobe Commerceの場合、Adobeでは、[!DNL OpenSearch] を設定してサードパーティの検索ツールのフォールバックオプションを提供することをお勧めします。 ただし、同じCommerce インスタンスで [!DNL OpenSearch] と [!DNL Live Search] の両方を有効にすることはできません。

**OpenSearch を有効にするには**:

1. 統合環境の場合、適切なバージョンと割り当てられたディスク領域（MB 単位）を使用して `opensearch` サービスを `.magento/services.yaml` ファイルに追加します。 この場合、バージョン 2 が適切です。 マイナーバージョンは必要ありません。

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Pro プロジェクトの場合、ステージング環境と実稼動環境で OpenSearch バージョンを変更するには、[Adobe Commerce サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) する必要があります。

1. `relationships` ファイルの `.magento.app.yaml` プロパティを設定または確認します。

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

   これらの変更が環境に与える影響について詳しくは、[&#x200B; サービスの設定 &#x200B;](services-yaml.md) を参照してください。

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

サービスのバージョンと互換性のサポートは、Cloud Infrastructure でテストおよびデプロイされたバージョンによって決まり、Adobe Commerceのオンプレミスデプロイメントでサポートされているバージョンとは異なる場合があります。 サポートされているバージョンの一覧については、『インストール ガイド [&#x200B; の &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)_システム要件_ を参照してください。

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

- デフォルトでは、クラウド環境の検索設定は、デプロイするたびに再生成されます。 `SEARCH_CONFIGURATION` デプロイ変数を使用して、デプロイメント間でカスタム検索設定を保持できます。 [&#x200B; 変数のデプロイ &#x200B;](../environment/variables-deploy.md#search_configuration) を参照してください。

- プロジェクトの OpenSearch サービスを設定した後、管理 UI を使用して OpenSearch 接続をテストし、Adobe Commerceの OpenSearch 設定をカスタマイズします。

### OpenSearch 用プラグインの追加

オプションで、`configuration:plugins` ファイルの OpenSearch サービスに `.magento/services.yaml` セクションを追加することで、OpenSearch 用のプラグインを追加できます。 例えば、次のコードは ICU 分析と音声分析プラグインを有効にします。

>[!NOTE]
>
>これは、統合環境とスターター環境にのみ適用されます。 実稼動クラスターまたはステージング環境にプラグインをインストールするには、[&#x200B; サポートリクエストを送信 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) します。


```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

プラグインについて詳しくは、[OpenSearch プロジェクト &#x200B;](https://github.com/opensearch-project) を参照してください。

### OpenSearch のプラグインを削除

`opensearch:` ファイルの `.magento/services.yaml` セクションからプラグイン エントリを削除しても、サービスはアンインストールまたは無効化 **されません**。 このサービスを完全に無効にするには、`.magento/services.yaml` ファイルからプラグインを削除した後、OpenSearch データのインデックスを再作成する必要があります。 この設計により、これらのプラグインに依存するデータの損失や破損を防ぐことができます。


**OpenSearch プラグインを削除するには**:

>[!NOTE]
>
>この変更は、統合およびスターター環境にのみ適用されます。 実稼動クラスターまたはステージング環境でプラグインを削除するには、[&#x200B; サポートチケットを送信 &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) する必要があります。

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
1. カタログ検索インデックスを再インデックス化します（すべての環境：統合、スターター、Pro ステージング、実稼動クラスター）。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーンアップします。

   ```bash
   bin/magento cache:clean
   ```
