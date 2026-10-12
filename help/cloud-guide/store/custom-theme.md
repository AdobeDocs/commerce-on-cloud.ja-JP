---
title: カスタムテーマ
description: Adobe Commerce on cloud infrastructureを使用してカスタムテーマをインストールする方法について説明します。
feature: Cloud, Themes
exl-id: 3ae4b0d5-9179-42c4-bb07-8ec09bd057d0
TQID: 'https://experienceleague.adobe.com/rk-VP6z1tQSY-HMU-dD9hv6O9Wpesbp1o5KQMYapCJE'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 8b440f30-6794-5ed6-981f-391de4e9b0cc
    internal-label: Themes
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%
---
# カスタムテーマ

1つまたは複数のテーマをインストールして、プロジェクト内のストアやサイトの1つまたは全部に使用できます。 テーマには、画像、フォント、CSS、JavaScript、PHPなど、複数の静的ファイルが含まれており、ストアを完全にデザインできます。 テーマを追加するには、そのコードをファイルシステムに抽出するか、Composerを使用します。

## テーマの手動インストール

テーマを手動でインストールするには、圧縮アーカイブまたは次のようなディレクトリ構造にテーマのコードが含まれている必要があります。

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

1. ストアフロントテーマの`<Project root dir>/app/design/frontend`の下のテーマのコード、または管理者テーマの`<Project root dir>/app/design/adminhtml`をコピーします。 最上位のディレクトリが`<VendorName>`であることを確認してください。そうしないと、テーマが正しくインストールされません。

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 正しい場所にコピーしたテーマを確認します。

   * ストアフロントのテーマ：`ls <project-root>/app/design/frontend`
   * 管理者テーマ：`ls <project-root>/app/design/adminhtml`

   サンプルは次のとおりです。

   ExampleTheme Adobe Commerce

1. ファイルを追加してコミットします。

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. ファイルをブランチにプッシュします。

   ```bash
   git push origin <branch name>
   ```

1. デプロイメントが完了するのを待ちます。
1. Adminにログインします。
1. **コンテンツ**/デザイン/**テーマ**&#x200B;をクリックします。

   テーマが右側のペインに表示されます。

## Composerを使用したテーマのインストール

Composerを使用したテーマのインストールは、Composerを使用した他の拡張機能のインストールと同じです。 詳しくは、[ モジュールのインストール、管理、アップグレード ](extensions.md)を参照してください。

Composerを使用してテーマをインストールするには：

1. Commerce Marketplaceから購入。
1. テーマのコンポーザー名を取得します。
1. Adobe Commerceのルートディレクトリに移動し、次のコマンドを入力します。

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

1. Adminにログインします。
1. **コンテンツ**/デザイン/**テーマ**&#x200B;をクリックします。

   テーマが右側のペインに表示されます。

## 複数のテーマ

ロケールごとに異なるテーマなど、複数のテーマを使用する場合は、テーマのデプロイメントをカスタマイズするために`SCD_MATRIX`環境変数を確認してください。 [環境設定](../environment/configure-env-yaml.md)の[ ビルド ](../environment/variables-build.md#scd_matrix)または[ デプロイ ](../environment/variables-deploy.md#scd_matrix)のステージを参照してください。
