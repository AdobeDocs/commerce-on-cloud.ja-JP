---
title: OpenSearch サービスの設定
description: Adobe Commerce オンクラウドインフラストラクチャでOpenSearch サービスを有効にする方法について説明します。
feature: Cloud, Search, Services
exl-id: e704ab2a-2f6b-480b-9b36-1e97c406e873
TQID: https://experienceleague.adobe.com/DIH1i-hJKlsoFFmDsws-w6iuJ56B7dcdiJP5Zh1iRII
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 788
ht-degree: 0%

---

# OpenSearch サービスの設定

[OpenSearch](https://www.opensearch.org) サービスは、Elasticsearchのライセンス変更に続く、Elasticsearch 7.10.2のオープンソース フォークです。 GitHubの[OpenSource プロジェクト ](https://github.com/opensearch-project)を参照してください。 [必要システム構成](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)には、サポートされているバージョンが一覧表示されます。

{{elasticsearch-support}}

OpenSearchなら、あらゆるソースやフォーマットからデータを取得し、リアルタイムで検索して可視化できます。

- 商品カタログ内の商品をすばやく高度に検索
- OpenSearch Analyzersは多言語をサポートしています
- ストップワードと類義語をサポート
- インデックス再作成操作が完了するまで、インデックス作成は顧客に影響しません

{{service-instruction}}

>[!TIP]
>
>[ ライブサーチ ](https://experienceleague.adobe.com/en/docs/commerce/live-search/overview)を使用していないCloud Infrastructure プロジェクトのAdobe Commerceの場合、Adobeでは、サードパーティの検索ツールにフォールバックオプションを提供するように[!DNL OpenSearch]を設定することをお勧めします。 ただし、[!DNL OpenSearch]と[!DNL Live Search]の両方を同じCommerce インスタンスで有効にすることはできません。

**OpenSearch**&#x200B;を有効にするには：

1. 統合環境の場合は、適切なバージョンと割り当てられたディスク容量がMBの`.magento/services.yaml` ファイルに`opensearch` サービスを追加します。 この場合、バージョン 3が適切です。 マイナーバージョンは必要ありません。

   ```yaml
   opensearch:
       type: opensearch:3
       disk: 1024
   ```

   Pro プロジェクトの場合、ステージング環境と実稼動環境でOpenSearch バージョンを変更するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。

1. `.magento.app.yaml` ファイルの`relationships` プロパティを設定または検証します。

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. コードの変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   これらの変更が環境にどのような影響を与えるかについては、[ サービスの設定](services-yaml.md)を参照してください。

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

## OpenSearch ソフトウェアの互換性

Adobe Commerce on cloud infrastructure プロジェクトをインストールまたはアップグレードする場合は、常にOpenSearch サービスのバージョンとAdobe Commerce用の[OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) クライアントとの互換性を確認してください。

- **初回設定**- `services.yaml` ファイルで指定されたOpenSearch バージョンが、Adobe Commerce用に設定されたOpenSearch PHP クライアントと互換性があることを確認します。

- **プロジェクトのアップグレード** – 新しいアプリケーション バージョンのOpenSearch PHP クライアントが、クラウド インフラストラクチャにインストールされているOpenSearch サービス バージョンと互換性があることを確認します。

サービスのバージョンと互換性のサポートは、クラウドインフラストラクチャでテストおよびデプロイされたバージョンによって決まり、Adobe Commerce オンプレミスのデプロイメントでサポートされているバージョンとは異なる場合があります。 サポートされているバージョンの一覧については、_インストールガイド_&#x200B;の[必要システム構成](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)を参照してください。

**OpenSearch ソフトウェアの互換性を確認するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. アクティブな環境のOpenSearchの詳細を表示します。

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. また、SSHを使用してリモート環境にログインすることもできます。

   ```bash
   magento-cloud ssh
   ```

1. OpenSearch サービス接続の詳細を取得します。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   応答で、OpenSearch サービスエンドポイントのIP アドレスとポートを見つけます。

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
   | type                                     | opensearch:3                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. インストールされたOpenSearch サービス `version:number`をサービス エンドポイントから取得します。

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
        "number" : "3.1.0",
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

OpenSearch サービスを再起動する必要がある場合は、Adobe Commerce サポートにお問い合わせください。

## 追加の検索設定

- デフォルトでは、クラウド環境の検索設定は、デプロイするたびに再生成されます。 `SEARCH_CONFIGURATION` デプロイ変数を使用して、デプロイ間でカスタム検索設定を保持できます。 [変数のデプロイ ](../environment/variables-deploy.md#search_configuration)を参照してください。

- プロジェクトにOpenSearch サービスを設定したら、Admin UIを使用してOpenSearch接続をテストし、Adobe CommerceのOpenSearch設定をカスタマイズします。

### OpenSearchのプラグインを追加

オプションとして、`.magento/services.yaml` ファイルのOpenSearch サービスに`configuration:plugins` セクションを追加することで、OpenSearch用のプラグインを追加できます。 例えば、次のコードでは、ICU解析と音声解析プラグインを有効にします。

>[!NOTE]
>
>これは、統合環境とスターター環境にのみ適用されます。 Pro ステージングまたは実稼動クラスターにプラグインをインストールするには、[ サポートリクエストを送信](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)してください。


```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

プラグインについて詳しくは、[OpenSearch プロジェクト ](https://github.com/opensearch-project)を参照してください。

### OpenSearchのプラグインを削除

`.magento/services.yaml` ファイルの`opensearch:` セクションからプラグインエントリを削除すると、**not**&#x200B;がサービスをアンインストールまたは無効にしません。 サービスを完全に無効にするには、`.magento/services.yaml` ファイルからプラグインを削除した後、OpenSearch データを再インデックスする必要があります。 このデザインは、これらのプラグインに依存するデータの可能性のある損失や破損を防ぎます。


**OpenSearch プラグインを削除するには**:

>[!NOTE]
>
>この変更は、統合環境とスターター環境にのみ適用されます。 Pro ステージングまたは実稼動クラスターでプラグインを削除するには、[ サポートチケット ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)を送信する必要があります。

1. OpenSearch プラグインのエントリを`.magento/services.yaml` ファイルから削除します。
1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. `.magento/services.yaml`の変更をクラウドリポジトリにコミットします。
1. カタログ検索インデックスのインデックスを再作成します（すべての環境：統合、スターター、プロステージング、実稼動クラスター）。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. キャッシュをクリーニングします。

   ```bash
   bin/magento cache:clean
   ```
