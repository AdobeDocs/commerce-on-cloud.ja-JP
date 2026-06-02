---
title: 複数のweb サイトや店舗の設定
description: Adobe Commerce on cloud infrastructure用に複数のweb サイトまたはストアを設定する方法について説明します。
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
TQID: https://experienceleague.adobe.com/532nrO6XkiqiNDfRMT6gZ4mVRqlv5PszegPJLuemmyc
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1094
ht-degree: 0%

---

# 複数のweb サイトや店舗の設定

Adobe Commerceでは、英語ストア、フランス語ストア、ドイツ語ストアなど、複数のweb サイトやストアを設定できます。 [Web サイト、ストア、ストアビューについて](best-practices.md#store-views)を参照してください。

>[!WARNING]
>
>web サイトや実店舗の数が増えるにつれて、カタログデータは増加します。 プロジェクトのアーキテクチャによっては、追加のストアを使用すると、インデックス作成プロセスが長くなり、キャッシュされていないカタログページの応答時間が遅くなる可能性があります。 Adobeでは、サイトのパフォーマンスを詳細に監視することをお勧めします。

複数のストアを設定するプロセスは、一意のドメインと共有ドメインのどちらを使用するかを選択します。

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
>サイトベース URLにストアビューを追加するには、複数のディレクトリを作成する必要はありません。 _設定ガイド_&#x200B;の「[ ストアコードをベース URL](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)に追加する」を参照してください。

## ドメインの追加

カスタムドメインは、Pro ステージング環境および実稼動環境に追加できます。統合環境に追加することはできません。

ドメインを追加するプロセスは、クラウドアカウントの種類によって異なります。

- プロ向けステージングと本番用

  新しいドメインをFastlyに追加するか、[ ドメインの管理](../cdn/fastly-custom-cache-configuration.md#manage-domains)を参照するか、サポートチケットを開いてサポートをリクエストしてください。 さらに、クラスターに新しいドメインを追加するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。

- スタータープロダクションのみ

  新しいドメインをFastlyに追加します。サポートをリクエストするには、[ ドメインの管理](../cdn/fastly-custom-cache-configuration.md#manage-domains)または[Adobe Commerce サポートチケットの送信](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を参照してください。 さらに、[!DNL Cloud Console]の&#x200B;**ドメイン** タブに新しいドメインを追加する必要があります：`https://<zone>.magento.cloud/projects/<project-ID>/edit`

## ローカルインストールの設定

複数のストアを使用するようにローカルインストールを設定するには、_設定ガイド_&#x200B;の[複数のweb サイトまたはストア ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html)を参照してください。

複数のストアを使用するローカルインストールを正常に作成してテストしたら、統合環境を準備する必要があります。

1. **ルートまたは場所を設定** – 受信URLをAdobe Commerceで処理する方法を指定します

   - [別々のドメインのルート](#configure-routes-for-separate-domains)
   - [共有ドメインの場所](#configure-locations-for-shared-domains)

1. **Web サイト、ストア、ストアビューを設定する**—Adobe Commerce管理UIを使用して設定します
1. **変数を変更** - `magento-vars.php` ファイルの`MAGE_RUN_TYPE`変数と`MAGE_RUN_CODE`変数の値を指定します
1. **環境のデプロイとテスト** - `integration` ブランチのデプロイとテスト

>[!TIP]
>
>ローカル環境を使用して、複数のweb サイトまたはストアを設定できます。 [複数のweb サイトまたはストアを設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites)する方法については、Cloud Dockerの手順を参照してください。

### Pro環境の設定の更新

{{pro-self-service-warning}}

### 個別のドメインのルートの設定

ルートは、受信URLを処理する方法を定義します。 一意のドメインを持つ複数のストアでは、`routes.yaml` ファイルで各ドメインを定義する必要があります。 ルートの設定方法は、サイトの操作方法によって異なります。

**統合環境でルートを設定するには**:

1. ローカル ワークステーションで、テキスト エディターで`.magento/routes.yaml` ファイルを開きます。

1. ドメインとサブドメインを定義します。 `mymagento` アップストリーム値は、`.magento.app.yaml` ファイルのname プロパティと同じ値です。

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. `routes.yaml` ファイルに変更を保存します。

1. 引き続き[Web サイト、ストア、ストアビューの設定](#set-up-websites-stores-and-store-views)を行います。

### 共有ドメインの場所の設定

ルート設定がURLの処理方法を定義する場合、`.magento.app.yaml` ファイルの`web` プロパティは、アプリケーションをWebに公開する方法を定義します。 Web _locations_&#x200B;では、受信リクエストをより詳細に指定できます。 例えば、ドメインが`store.com`の場合、ドメインを共有する2つの異なるストアへのリクエストに`/first` （デフォルトのサイト）と`/second`を使用できます。

**新しいWebの場所を設定するには**:

1. ルート （`/`）のエイリアスを作成します。 この例では、エイリアスは3行目の`&app`です。

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

1. Web サイト （`/website`）のパススルーを作成し、前の手順のエイリアスを使用してルートを参照します。

   エイリアスを使用すると、`website`はルートの場所から値にアクセスできます。 この例では、web サイト `passthru`は21行目にあります。

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

1. ルート （`/`）と静的（`/static`）の場所のエイリアスを作成します。

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

1. `pub` ディレクトリの下にweb サイトのサブディレクトリを作成します：`pub/<website>`

1. `pub/index.php` ファイルを`pub/<website>` ディレクトリにコピーし、`bootstrap` パス （`/../../app/bootstrap.php`）を更新します。

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

   - `pub/<website>/index.php` （このファイルが`.gitignore`にある場合、プッシュには強制オプションが必要になる場合があります）。
   - `.magento.app.yaml`

### Web サイト、ストア、ストアビューを設定する

_管理UI_&#x200B;で、Adobe Commerce **Web サイト**、**ストア**、**ストアビュー**&#x200B;を設定します。 _設定ガイド_&#x200B;の「](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)管理者に複数のweb サイト、ストア、ストアビューを設定する」を参照してください。[

ローカルインストールを設定する際には、管理者から表示されるweb サイト、ストア、ストアビューの同じ名前とコードを使用することが重要です。 `magento-vars.php` ファイルを更新する際には、これらの値が必要です。

### 変数を変更

NGINX仮想ホストを設定する代わりに、プロジェクトルートディレクトリの`magento-vars.php` ファイルを使用して`MAGE_RUN_CODE`変数と`MAGE_RUN_TYPE`変数を渡します。

**`magento-vars.php` ファイルを使用して変数を渡すには**:

1. テキストエディターで`magento-vars.php` ファイルを開きます。

   [ デフォルトの`magento-vars.php` ファイル ](https://github.com/magento/magento-cloud/blob/master/magento-vars.php)は次のようになります。

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

1. コメントされた`if` ブロックを移動して、`function` ブロックの&#x200B;_後_&#x200B;になり、コメントが解除されるようにします。

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

1. `if (isHttpHost("example.com"))` ブロック内の次の値を置き換えます。
   - `example.com` - _web サイト_&#x200B;のベース URL
   - `default` – お客様の&#x200B;_web サイト_&#x200B;または&#x200B;_ストアビュー_&#x200B;の一意のコード
   - `store` – 次のいずれかの値を持つ：
      - `website` - ストアフロントに&#x200B;_web サイト_&#x200B;を読み込みます
      - `store` - ストアフロントに&#x200B;_ストアビュー_&#x200B;を読み込む

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

   同じドメインを持つ複数のサイトの場合、_host_&#x200B;と&#x200B;_URI_&#x200B;を確認する必要があります。

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

1. `magento-vars.php` ファイルに変更を保存します。

### 統合サーバーでのデプロイとテスト

変更をAdobe Commerce on cloud infrastructure統合環境にプッシュして、サイトをテストします。

1. リモートブランチにコードの変更を追加、コミット、プッシュします。

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. デプロイメントが完了するのを待ちます。

1. デプロイメント後、Web ブラウザーでストア URLを開きます。

   一意のドメインで、次の形式を使用します：`http://<magento-run-code>.<site-URL>`

   例：`http://french.master-name-projectID.us.magentosite.cloud/`

   共有ドメインでは、次の形式を使用します：`http://<site-URL>/<magento-run-code>`

   例：`http://master-name-projectID.us.magentosite.cloud/french/`

1. サイトを徹底的にテストし、コードを`integration` ブランチにマージして、さらにデプロイします。

## ステージングおよび実稼動へのデプロイ

ステージングおよび実稼動環境](../deploy/staging-production.md)への[ デプロイのデプロイメントプロセスに従います。 Starter環境とPro環境の場合は、[!DNL Cloud Console]を使用して、環境全体にコードをプッシュします。

Adobeでは、実稼動環境にプッシュする前に、ステージング環境で完全にテストすることをお勧めします。 統合環境でコードを変更し、環境全体にデプロイするプロセスを再度開始します。

