---
title: 複数の web サイトまたはストアを設定
description: クラウドインフラストラクチャー上でAdobe Commerce用に複数の web サイトまたはストアを設定する方法について説明します。
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# 複数の web サイトまたはストアを設定

英語のストア、フランス語のストア、ドイツ語のストアなど、複数の web サイトやストアを持つようにAdobe Commerceを設定できます。 詳しくは [Web サイト、ストア、ストア表示について ](best-practices.md#store-views) を参照してください。

>[!WARNING]
>
>Web サイトやストアの数を増やすと、カタログデータは大きくなります。 プロジェクトアーキテクチャによっては、追加のストアによって、インデックス作成プロセスが長くなり、キャッシュされていないカタログページの応答時間が遅くなる可能性があります。 Adobeでは、サイトのパフォーマンスを詳細に監視することをお勧めします。

複数のストアを設定するプロセスは、一意のドメインと共有ドメインのどちらを使用するかによって異なります。

一意のドメインを持つ複数のストア：

```
https://first.store.com/
https://second.store.com/
```

同じドメインを持つ複数のストア：

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>ストアビューをサイトベース URL に追加する場合、複数のディレクトリを作成する必要はありません。 [ 設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) の _ベース URL へのストアコードの追加_ を参照してください。

## ドメインの追加

カスタムドメインは、ステージング環境および実稼動環境に追加できます。統合環境に追加することはできません。

ドメインを追加するプロセスは、クラウドアカウントのタイプによって異なります。

- ステージングおよび実稼動用

  新しいドメインを Fastly に追加する、[ ドメインの管理 ](../cdn/fastly-custom-cache-configuration.md#manage-domains) を参照する、またはサポートチケットを開いてサポートをリクエストします。 また、クラスターに追加する新しいドメインをリクエストするには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) する必要があります。

- スターター実稼動用のみ

  新しいドメインを Fastly に追加する方法については、[ ドメインの管理 ](../cdn/fastly-custom-cache-configuration.md#manage-domains) または [Adobe Commerce サポートチケットの送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) を参照してください。 さらに、次の手順で新しいドメインを **ドメイン** タブに追加する必要があり [!DNL Cloud Console] す。`https://<zone>.magento.cloud/projects/<project-ID>/edit`

## ローカルインストールの設定

複数のストアを使用するようにローカルインストールを設定するには、[ 設定ガイド ][config-multiweb] の _複数の web サイトまたはストア_ を参照してください。

ローカルインストールを正常に作成およびテストして複数のストアを使用したら、統合環境の準備を行う必要があります。

1. **ルートまたは場所を設定** – 受信 URL がAdobe Commerceでどのように処理されるかを指定します。

   - [個別のドメインのルート](#configure-routes-for-separate-domains)
   - [共有ドメインの場所](#configure-locations-for-shared-domains)

1. **Web サイト、ストア、ストア表示の設定** - Adobe Commerce管理 UI を使用して設定します
1. **変数の変更** - `MAGE_RUN_TYPE` ファイルの `MAGE_RUN_CODE` 変数と `magento-vars.php` 変数の値を指定します
1. **環境の導入とテスト**:`integration` ブランチの導入とテスト

>[!TIP]
>
>ローカル環境を使用して、複数の web サイトやストアを設定できます。 詳しくは、Cloud Docker の手順 [ 複数の web サイトまたはストアの設定 ](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites) を参照してください。

### Pro 環境の設定アップデート

{{pro-self-service-warning}}

### 個別のドメインのルートの設定

ルートは、受信 URL の処理方法を定義します。 一意のドメインを持つ複数のストアでは、`routes.yaml` ファイルで各ドメインを定義する必要があります。 ルートを設定する方法は、サイトの動作方法によって異なります。

**統合環境でルートを設定するには**:

1. ローカルワークステーションで、`.magento/routes.yaml` ファイルをテキストエディターで開きます。

1. ドメインとサブドメインを定義します。 `mymagento` のアップストリーム値は、`.magento.app.yaml` ファイルの name プロパティと同じ値です。

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. 変更内容を `routes.yaml` ファイルに保存します。

1. 続けて [web サイト、ストア、ストア表示の設定 ](#set-up-websites-stores-and-store-views) を行います。

### 共有ドメインの場所の設定

routes 設定で URL の処理方法を定義する場合、`web` ファイルの `.magento.app.yaml` プロパティで、アプリケーションが web に公開される方法を定義します。 Web _場所_ を使用すると、受信リクエストの精度を高めることができます。 例えば、ドメインが `store.com` の場合、ドメインを共有する 2 つの異なるストアへのリクエストには、`/first` （デフォルトサイト）と `/second` を使用できます。

**新しい web サイトを設定するには**:

1. ルート（`/`）のエイリアスを作成します。 この例では、エイリアスは 3 行目の `&app` です。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Web サイト（`/website`）のパススルーを作成し、前の手順のエイリアスを使用してルートを参照します。

   エイリアスを使用 `website` ると、ルートの場所から値にアクセスできます。 この例では、web サイト `passthru` は 21 行目にあります。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**別のディレクトリを使用して場所を設定するには**:

1. ルート（`/`）と静的（`/static`）の場所にエイリアスを作成します。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Web サイトのサブディレクトリを `pub` ディレクトリの下に作成します。`pub/<website>`

1. `pub/index.php` ファイルを `pub/<website>` ディレクトリにコピーし、`bootstrap` パス（`/../../app/bootstrap.php`）を更新します。

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. `index.php` ファイルのパススルーを作成します。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. 変更したファイルをコミットしてプッシュします。

   - `pub/<website>/index.php` （このファイルが `.gitignore` の場合、プッシュには強制オプションが必要な場合があります。）
   - `.magento.app.yaml`

### Web サイト、ストア、ストアビューの設定

_管理 UI_ で、Adobe Commerce **Web サイト**、**ストア**、**ストアビュー** を設定します。 [ 設定ガイド ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) の「管理者での複数の web サイト、ストア、ストア表示の設定 _を参照してください_。

ローカルインストールを設定する際には、管理者が web サイト、ストア、ストアビューと同じ名前とコードを使用することが重要です。 これらの値は、`magento-vars.php` ファイルを更新する際に必要になります。

### 変数の変更

NGINX 仮想ホストを設定する代わりに、プロジェクトのルートディレクトリにある `MAGE_RUN_CODE` ファイルを使用して、`MAGE_RUN_TYPE` 変数と `magento-vars.php` 変数を渡します。

**`magento-vars.php` ファイルを使用して変数を渡すには**:

1. `magento-vars.php` ファイルをテキストエディターで開きます。

   [ デフォルトの `magento-vars.php` ファイル ](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) は次のようになります。

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. コメントされた `if` ブロックを、_ブロックの_ 後 `function` に移動して、コメントされなくなるようにします。

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. `if (isHttpHost("example.com"))` ブロック内の次の値を
   - `example.com` - _web サイトのベース URL_
   - `default` - _web サイト_ または _ストアビューの一意のコード_
   - `store` – 次のいずれかの値を持ちます：
      - `website` - ストアフロントに _web サイト_ を読み込みます
      - `store` - ストアフロントに _ストア表示_ をロードします

   一意のドメインを使用する複数のサイトの場合：

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   同じドメインを持つ複数のサイトの場合は、_host_ と _URI_ を確認する必要があります。

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. 変更内容を `magento-vars.php` ファイルに保存します。

### 統合サーバーへのデプロイとテスト

変更内容をAdobe Commerce on cloud infrastructure integration environment にプッシュし、サイトをテストします。

1. コードの変更をリモートブランチに追加、コミットおよびプッシュします。

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. デプロイメントが完了するまで待ちます。

1. デプロイメント後、Web ブラウザーでストア URL を開きます。

   一意のドメインでは、`http://<magento-run-code>.<site-URL>` の形式を使用します。

   例：`http://french.master-name-projectID.us.magentosite.cloud/`

   共有ドメインでは、`http://<site-URL>/<magento-run-code>` の形式を使用します。

   例：`http://master-name-projectID.us.magentosite.cloud/french/`

1. サイトを徹底的にテストし、コードを `integration` ブランチに結合して、さらにデプロイメントします。

## ステージング環境および実稼動環境にデプロイ

デプロイメントプロセス [ ステージング環境および実稼動環境へのデプロイ ](../deploy/staging-production.md) に従います。 スターター環境および Pro 環境の場合は、[!DNL Cloud Console] を使用して環境全体にコードをプッシュします。

Adobeでは、実稼動環境にプッシュする前に、ステージング環境で完全にテストすることをお勧めします。 統合環境でコードを変更し、環境全体にデプロイするプロセスを再度開始します。

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
