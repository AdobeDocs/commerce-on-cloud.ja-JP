---
title: カスタムテーマ
description: クラウドインフラストラクチャ上のAdobe Commerceにカスタムテーマをインストールする方法を説明します。
feature: Cloud, Themes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# カスタムテーマ

プロジェクト内の 1 つまたはすべてのストアとサイトに使用する 1 つまたは複数のテーマをインストールできます。 テーマには、画像、フォント、CSS、JavaScript、PHP など、ストアを完全にデザインするための複数の静的ファイルが含まれます。 テーマを追加するには、コードをファイルシステムに抽出するか、Composer を使用します。

## テーマを手動でインストール

テーマを手動でインストールするには、テーマのコードを圧縮されたアーカイブまたは次のようなディレクトリ構造にする必要があります。

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**テーマを手動でインストールするには**:

1. ストアフロントテーマの場合は `<Project root dir>/app/design/frontend` の下に、管理テーマの場合は `<Project root dir>/app/design/adminhtml` の下にテーマのコードをコピーします。 最上位のディレクトリが `<VendorName>` であることを確認します。そうでない場合、テーマが正しくインストールされません。

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 正しい場所にコピーされたテーマを確認します。

   * Storefront テーマ：`ls <project-root>/app/design/frontend`
   * 管理テーマ：`ls <project-root>/app/design/adminhtml`

   次に例を示します。

   ExampleTheme Adobe Commerce

1. ファイルの追加とコミット

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. ファイルをブランチにプッシュします。

   ```bash
   git push origin <branch name>
   ```

1. デプロイメントが完了するまで待ちます。
1. 管理者にログインします。
1. **コンテンツ**/デザイン/**テーマ** をクリックします。

   テーマが右側のパネルに表示されます。

## Composer を使用したテーマのインストール

Composer を使用してテーマをインストールする方法は、Composer を使用して他の拡張機能をインストールする方法と同様です。 詳しくは、[ モジュールのインストール、管理、アップグレード ](extensions.md) を参照してください。

Composer を使用してテーマをインストールするには：

1. テーマをCommerce Marketplaceから購入します。
1. テーマのコンポーザー名を取得します。
1. Adobe Commerceのルートディレクトリに移動して、コマンドを入力します。

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   以下に例を挙げます。

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. 依存関係が更新されるのを待ちます。
1. 次のコマンドを入力します。

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. 管理者にログインします。
1. **コンテンツ**/デザイン/**テーマ** をクリックします。

   テーマが右側のパネルに表示されます。

## 複数のテーマ

ロケールごとに異なるテーマなど、複数のテーマを使用する場合は、テーマのデプロイメントをカスタマイズするための `SCD_MATRIX` 環境変数を確認します。 [ 環境設定 ](../environment/configure-env-yaml.md) の [ ビルド ](../environment/variables-build.md#scd_matrix) または [ デプロイ ](../environment/variables-deploy.md#scd_matrix) ステージを参照してください。
