---
title: デプロイ後の変数
description: Adobe Commerce on cloud infrastructureのデプロイ後のフェーズでアクションを制御する環境変数の一覧を参照してください。
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: 42523ff9-d8ca-470a-ac7b-d2ce21edd830
TQID: https://experienceleague.adobe.com/w60X0FgUZr-ff1cJJmo5y8frgYW5MlR0pyBy8tTFfSg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 516
ht-degree: 0%

---

# デプロイ後の変数

次の&#x200B;_デプロイ後_&#x200B;変数は、デプロイ後フェーズのアクションを制御し、[ グローバル変数](variables-global.md)から値を継承して上書きできます。 これらの変数を`.magento.env.yaml` ファイルの`post-deploy` ステージに挿入します。

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

ビルドおよびデプロイプロセスのカスタマイズについて詳しくは、次を参照してください。

- [デプロイメント設定](configure-env-yaml.md)
- [デプロイメントプロセス](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Default**— `[]` （空の配列）
- **バージョン** - Adobe Commerce 2.1.4以降

指定したページの&#x200B;_最初のバイトまでの時間_ （TTFB）テストを設定して、サイトのパフォーマンスをテストします。 テストを必要とする各ページに対して、絶対パス参照、つまりプロトコルとホストを含むURLを指定します。

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

変更をテストしてコミットするページを指定すると、デプロイ後のフェーズで&#x200B;_最初のバイトまでの時間_ テストが実行され、各パスの結果がクラウドログに投稿されます。

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

リダイレクトされたパスの場合、ログは環境変数で設定されたパスではなく、リダイレクト ターゲットのパスをレポートします。 無効なパスを指定すると、ログに警告メッセージが表示されます。

## `WARM_UP_CONCURRENCY`

- **既定**—_設定なし_
- **バージョン** - Adobe Commerce 2.1.4以降

キャッシュウォームアップ操作中に送信する同時リクエストの制限を指定して、サーバー負荷を軽減します。 この値は、並列接続の数を制限し、デプロイ後の`WARM_UP_PAGES`変数でキャッシュのプリロード用に複数のページが指定される環境設定に役立ちます。

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Default**— `index.php`
- **バージョン** - Adobe Commerce 2.1.4以降

`post_deploy` ステージでキャッシュのプリロードに使用するページのリストをカスタマイズします。 デプロイ後フックを設定する必要があります。 `.magento.app.yaml` ファイルの[ フックの節](../application/hooks-property.md)を参照してください。

- **単一ページ** - キャッシュに追加する単一ページを指定します。 デフォルトのベース URLを指定する必要はありません。 次の例は、`BASE_URL/index.php` ページをキャッシュします。

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **複数のドメイン** – 複数のURLを一覧表示します。 次の例では、2つのドメインからページをキャッシュします。

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **複数ページ** – 次の形式を使用して、特定の正規表現パターンに従って複数のページをキャッシュします。

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`：使用可能なバリアント `category`、`cms-page`、`product`、`store-page`
   - `pattern|url|product_sku`: `regexp` パターンまたは完全一致`url`を使用してURLをフィルタリングするか、すべてのページにアスタリスク （\*）を使用します。 `product` エンティティタイプに製品SKUを使用
   - `store_id|store_code`: ストアのIDまたはコード、またはすべてのストアのアスタリスク （\*）を使用します。複数のストア IDまたはコードを`|`で区切って渡すことができます

  次の例では、これらの条件に基づいて、`category`および`cms-page`のエンティティタイプをキャッシュします。
   - id `1`を持つストアのすべてのカテゴリ ページ
   - コード `store1`と`store2`を持つストアのすべてのカテゴリ ページ
   - コード `store_en`を持つストアのカテゴリ ページ `cars`
   - すべてのストアのCMS ページ `contact`
   - id `1`と`2`を持つストアのCMS ページ `contact`
   - `car_`を含み、ID 2のストアの`html`で終わるカテゴリーページ
   - コード `store_gb`を持つストアの`tires_`を含むカテゴリ ページ

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

  次の例では、これらの条件に基づいて`product` エンティティタイプをキャッシュします。
   - 全店舗のすべての商品（パフォーマンスの問題を回避するために、プログラムで1店舗あたり100個に制限されています）
   - ストア `store1`のすべての製品
   - すべてのストアの`sku1`を含む商品
   - コード `store1`と`store2`のストアの`sku1`を持つ商品
   - コード `store1`と`store2`のストアの`sku1`、`sku2`および`sku3`の製品

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

  次の例では、これらの条件に基づいて`store-page` エンティティタイプをキャッシュします。
   - すべてのストアのページ `/contact-us`
   - id `1`のストアのページ `/contact-us`
   - コード `code1`と`code2`を持つストアのページ `/contact-us`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
