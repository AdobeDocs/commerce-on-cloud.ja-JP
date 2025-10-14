---
title: デプロイ後変数
description: Adobe Commerce on cloud infrastructure のデプロイ後のフェーズで、アクションを制御する環境変数のリストを参照してください。
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# デプロイ後変数

次の _デプロイ後_ 変数は、デプロイ後のフェーズでのアクションを制御し、[&#x200B; グローバル変数 &#x200B;](variables-global.md) の値を継承および上書きできます。 `.magento.env.yaml` ファイルの `post-deploy` のステージに、次の変数を挿入します。

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズに関する詳細情報：

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **デフォルト** - `[]` （空の配列）
- **バージョン** - Adobe Commerce 2.1.4 以降

サイトのパフォーマンスをテストするために、指定したページの _最初のバイトまでの時間_ （TTFB）テストを設定します。 テストが必要な各ページに対して、絶対パス参照、またはプロトコルとホストを含む URL を指定します。

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

変更をテストしてコミットするページを指定すると、デプロイ後のフェーズで _最初のバイトまでの時間_ テストが実行され、各パスの結果がクラウドログに投稿されます。

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

リダイレクトされたパスの場合、ログには、環境変数で設定されたパスではなく、リダイレクトターゲットのパスが報告されます。 無効なパスを指定すると、ログに警告メッセージが表示されます。

## `WARM_UP_CONCURRENCY`

- **Default**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4 以降

サーバー負荷を軽減するために、キャッシュウォームアップ操作中に送信する同時リクエストの制限を指定します。 この値は、並列接続の数を制限し、`WARM_UP_PAGES` のデプロイ後変数がキャッシュのプリロード用に複数のページを指定する環境設定で役立ちます。

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **デフォルト**— `index.php`
- **バージョン** - Adobe Commerce 2.1.4 以降

`post_deploy` ステージでキャッシュのプリロードに使用するページのリストをカスタマイズします。 デプロイ後フックを設定する必要があります。 `.magento.app.yaml` ファイルの [hooks セクション &#x200B;](../application/hooks-property.md) を参照してください。

- **単一ページ** - キャッシュに追加する単一ページを指定します。 デフォルトのベース URL を指定する必要はありません。 次の例では、`BASE_URL/index.php` ページをキャッシュします。

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **複数のドメイン** – 複数の URL をリストします。 次の例では、2 つのドメインからページをキャッシュします。

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **複数のページ** – 特定の正規表現パターンに従って複数のページをキャッシュするには、次のフォーマットを使用します：

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`：使用可能なバリアント `category`、`cms-page`、`product`、`store-page`
   - `pattern|url|product_sku`:`regexp` パターンまたは完全一致 `url` を使用して URL をフィルタリングするか、すべてのページにアスタリスク（\*）を使用します。 `product` エンティティ タイプに製品 SKU を使用する
   - `store_id|store_code`：ストアの ID またはコード、またはすべてのストアでアスタリスク（\*）を使用します。複数のストア ID またはコードを `|` で区切って渡すことができます

  次の例では、これらの条件に基づいて、`category` および `cms-page` エンティティタイプのをキャッシュします。
   - id が `1` のストアのすべてのカテゴリページ
   - コード `store1` および `store2` を含むストアのすべてのカテゴリページ
   - コード `store_en` を含むストアのカテゴリ ページ `cars`
   - すべてのストアの cms ページ `contact`
   - id `1` および `2` を持つストアの CMS ページ `contact`
   - id 2 のストアで `car_` を含み、`html` で終わるカテゴリページ
   - コード `store_gb` を含むストアの `tires_` を含むカテゴリ ページ

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  次の例では、これらの条件に基づいて、`product` エンティティタイプのをキャッシュします。
   - すべてのストアのすべての製品（パフォーマンスの問題を回避するために、プログラムによってストアあたり 100 個に制限）
   - 店舗 `store1` 用全製品
   - すべての店舗で `sku1` を使用する製品
   - コード `store1` および `store2` を含むストアの `sku1` を使用した製品
   - `sku1`、`sku2`、`sku3` を使用した、コード `store1` および `store2` を含むストア向けの製品

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  次の例では、これらの条件に基づいて、`store-page` エンティティタイプのをキャッシュします。
   - すべてのストアのページ `/contact-us`
   - id `1` を含むストアのページ `/contact-us`
   - コード `code1` と `code2` を含むストアのページ `/contact-us`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
