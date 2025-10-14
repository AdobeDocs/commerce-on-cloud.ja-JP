---
title: システム固有の設定の管理例
description: クラウドインフラストラクチャ環境ですべてのAdobe Commerceでストア設定を管理および同期する方法の例を確認します。
hidefromtoc: true
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---


# システム固有の設定の管理例

この例では、構成管理を使用して、すべての環境でストア設定の一貫性を維持する方法を示します。

この例では、[&#x200B; ストア設定 &#x200B;](store-settings.md) で定義された次のプロシージャを使用します。

1. 統合環境ストア管理者に設定を入力します。
1. `config.php` ファイルを作成し、ローカル ワークステーションに転送します。
1. `config.php` をリモート統合環境にプッシュします。
1. 設定が管理者で編集できないことを確認します。
1. 必要な変更を加えます。

   * 統合環境の構成設定の変更
   * 設定を追加するには、コマンドを実行して `config.php` を再度作成します。 新しい設定がファイルに追加されます。
   * 既存の設定を削除または編集するには、ファイルを手動で編集します。
   * コミットしてプッシュします。

例えば、次の設定が必要な場合があります。

* 統合環境でロケールと静的ファイル最適化設定を無効にする
* ステージング環境および実稼動環境での静的ファイル最適化の有効化
* ステージング環境および実稼動環境での Fastly の設定に、それぞれの特定の資格情報を使用する

_静的ファイル最適化_ とは、JavaScriptとカスケードスタイルシートの結合と縮小、およびHTMLテンプレートの縮小を意味します。 [&#x200B; 静的コンテンツデプロイメント戦略 &#x200B;](../deploy/static-content.md) を参照してください。

## 前提条件

これらの設定管理タスクを完了するには、以下が必要です。

* [&#x200B; 環境「管理者」 &#x200B;](../project/user-access.md) 権限を持つプロジェクトリーダーの役割
* 統合、ステージングおよび実稼動環境の管理者 URL と資格情報

## Commerce Admin の設定

統合環境では、管理者にログインして、ストア、Web サイト、モジュールまたは拡張機能のシステム設定、静的ファイル最適化、静的コンテンツのデプロイメントに関連するシステム値を変更できます。 [&#x200B; 設定データ &#x200B;](store-settings.md#scd-performance) を参照してください。

**ロケールと静的ファイル最適化設定を変更するには**:

1. 統合環境管理者にログインします。 この URL には、[[!DNL Cloud Console]](../project/overview.md) からアクセスできます。
1. **ストア**/設定/**設定**/一般/**一般** に移動します。
1. ページナビゲーションで、「ロケールオプション **を展開** ます。
1. **ロケール** リストからロケールを変更します。 後で元に戻すことができます。

   ![&#x200B; ロケールの変更 &#x200B;](../../assets/locale-options.png)

1. 「**設定を保存**」をクリックします。
1. プロンプトが表示されたら、[&#x200B; キャッシュをフラッシュ &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/tools/cache-management) します。
1. 管理者からログアウトします。

## 値をエクスポートし、config.php をローカルシステムに転送します。

この手順では、ローカルマシンで実行するコマンドを使用して、統合環境上に `config.php` 設定ファイルを作成し、転送します。

この手順は、[&#x200B; 推奨手順 &#x200B;](store-settings.md) の手順 2 に対応します。 作成した `config.php` は、Git に追加できるように、ローカルシステムに転送します。

**ア`config.php`** ットを作成して転送するには：

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. 統合環境に変更します。

   ```bash
   magento-cloud environment:checkout integration
   ```

1. リモート・データベースのローカル・ダンプを作成します。

   ```bash
   magento-cloud db:dump
   ```

`config.php` の次のスニペットは、デフォルトのロケールを `en_GB` に変更し、静的ファイル最適化設定を変更する例を示しています。

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## config.php のプッシュと環境へのデプロイ

`config.php` を作成してローカルシステムに転送したので、Git にコミットして環境にプッシュします。 この手順は、[&#x200B; 推奨手順 &#x200B;](store-settings.md) の手順 3 および 4 に対応します。

次のコマンドは、追加、コミットおよび `master` 分岐へのプッシュを行います。

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

ステージング環境および実稼動環境へのコードのデプロイメントを完了します。 スターターの場合は、`staging` 分岐と `master` 分岐にプッシュします。 デプロイメントコマンドについて詳しくは、[&#x200B; ストアのデプロイ &#x200B;](../deploy/staging-production.md) を参照してください。

すべての環境でデプロイメントが完了するまで待ちます。

## 設定の変更を確認します

`config.php` を環境にプッシュした後、変更した値は、管理者では読み取り専用にする必要があります。 この例では、変更されたデフォルトのロケールと静的ファイル最適化設定は、管理者で編集できません。 これらの設定は、`config.php` で指定します。

設定の変更を確認するには：

1. いずれかの環境で管理者からログアウトします。
1. 管理者に再度ログインします。
1. **ストア**/設定/**設定**/一般/**一般** をクリックします。
1. 右側のウィンドウで「**ロケールオプション**」を展開します。

   次のサンプルに示すように、いくつかのフィールドは編集できないことに注意してください。 これらの設定は、`config.php` で管理されます。

   ![&#x200B; 一部の値は管理者では編集できなくなりました &#x200B;](../../assets/locale-options-disabled.png)

1. 管理者からログアウトします。

## システム固有の構成設定の変更および更新

これらの設定を変更する必要がある場合は、テキストエディターを使用して `config.php` ファイルを手動で変更します。 編集または削除を完了したら、前の手順に従ってコミットしてリモート環境にプッシュできます。

設定を追加するには、統合環境を変更し、コマンドを再度実行してファイルを生成します。 新しい設定はすべて、ファイルのコードに追加されます。 前の手順に従って、Git にプッシュします。

この例では、静的ファイル最適化設定を変更し、JavaScriptの新しい設定を追加します。

### 統合での設定の追加

統合環境に設定値を追加するには、Admin. この例では、JavaScript ファイルを結合します。

1. 統合管理者からログアウトします。
1. 統合管理者に再度ログインします。
1. **ストア**/設定/**設定**/**詳細**/**開発者** をクリックします。
1. 右側のウィンドウで、「**JavaScript設定**」を展開します。
1. **JavaScript ファイルを結合** リストで、「はい **をクリックし** す。
1. 「**設定を保存**」をクリックします。
1. プロンプトが表示されたら、[&#x200B; キャッシュをフラッシュ &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-admin/systems/tools/cache-management) します。
1. 管理者からログアウトします。

dump コマンドを再度実行すると、新しい設定がファイルに追加されます。

```bash
magento-cloud db:dump
```

### 新しい設定で config.php を編集します。

ローカルで、テキストエディターを使用して、更新された `app/etc/config.php` ファイルを編集します。 これらの設定を編集して、JavaScript、HTML、CSS ファイルの縮小を有効にします。

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

縮小を許可するように設定を変更するには、`'minify_html'` および各 `'minify_files'` オプションの `'0'` を `'1'` に編集します。

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

変更内容をファイルに保存します。

### 変更を Git にプッシュ

変更をプッシュするには、次を入力します。

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

デプロイメントが完了するまで待ちます。

デプロイメントプロセスを繰り返して、コードをすべての環境にプッシュします。
