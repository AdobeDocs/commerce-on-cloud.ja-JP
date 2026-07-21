---
source-git-commit: 7c3f2965f89503cc94a31aabca3d317a9330a4f6
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---
# 画像の最適化のためのプリコミットフック

このディレクトリには、画像がリポジトリにコミットされる前に自動的に最適化される事前コミットフックが含まれています。

## フックの機能

- **ステージングされた画像ファイル（**）を自動検出（PNG、JPEG、GIF、SVG）
- **`image_optim`**&#x200B;を実行してラスター画像（PNG、JPEG、GIF）を圧縮および最適化する
- **最適化された画像**&#x200B;を自動的に再ステージング
- **コミットされたすべてのラスター画像**&#x200B;が適切に最適化されていることを確認します
- **ステージング済みSVG**&#x200B;をサイズ制限に照らし合わせて確認し、SVGが制限を超えた場合はコミットを中止します

## Adobe Workfrontの利点

- リポジトリサイズの削減
- ドキュメントのページ読み込みを高速化
- あらゆる貢献者で一貫した画質
- 手作業による最適化は不要です

## 前提条件

- Ruby 3.0以上
- バンドラー
- Git

## 設定

### 自動設定（推奨）

```bash
.githooks/setup-hooks.sh
```

### 手動設定

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### プロジェクト設定の完了

1. リポジトリを複製します。

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. プリコミットフックを有効にする：

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Jekyllの依存関係をインストールします。

   ```bash
   cd _jekyll
   bundle install
   ```

## フックのテスト

1. リポジトリへの画像ファイルの追加
2. ステージング：`git add <image-file>`
3. コミットを試みます：`git commit -m 'test'`
4. フックは自動的に画像を最適化する必要があります

### 予想される出力

```bash
Found 1 staged image(s). Running optimization...
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## 画像ガイドライン

- **PNG**: スクリーンショットとUI要素に使用（自動的に最適化されます）
- **JPEG**：写真用に使用（自動最適化されます）
- **GIF**: アニメーションに使用（自動的に最適化されます）
- **SVG**: アイコンとシンプルなグラフィックに使用します（最適化されていませんが、サイズ制限に照らし合わせてチェックされます。制限を超えるとコミットに失敗します）

プリコミットフックは、コミット時にPNG、JPEG、GIFの画像を自動的に最適化し、ステージングされたSVGをサイズ制限（140 KB）に照らし合わせてチェックします。

ステージングされたSVGが制限を超えた場合、コミットは中止されます。 代わりにPNGに変換します。

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=path/to/image.svg
```

## 手作業による最適化

手動画像の最適化：

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## 設定

フックは、最適化の設定をカスタマイズするために設定ファイル `_jekyll/.image_optim.yml`を使用します。

- **PNG**: `advpng`、`optipng`、`pngquant`を使用
- **JPEG**: `jhead`、`jpegoptim`、`jpegtran`を使用
- **GIF**: `gifsicle`を使用
- **SVG**：最適化されていませんが（`image_optim`から除外してベクターグラフィックとアニメーションを保持）、140 KBのサイズ制限に照らし合わせてチェックしました

## トラブルシューティング

### フックが作動しない

- フック設定の確認：`git config core.hooksPath`
- フックファイルが実行可能であることを確認します：`chmod +x .githooks/pre-commit`
- `_jekyll` ディレクトリがある正しいリポジトリにいることを確認します

### 最適化の失敗

- `bundle install`が`_jekyll` ディレクトリで実行されたことを確認します
- `adobe-comdox-exl-rake-tasks` gemがインストールされていることを確認します（`image_optim`を提供）
- `.image_optim.yml`設定ファイルを確認します

### SVGがサイズ制限を超えています

- ステージングされたSVGが140 KBを超えると、コミットは中止されます
- SVGをPNGに変換：`cd _jekyll && bundle exec rake images:svg_to_png path=path/to/image.svg`
- 次に、SVGの代わりにPNGをステージングし、再度コミットします

### パフォーマンスの問題

- `_jekyll/.image_optim.yml`でのスレッド数の調整
- エラー情報の詳細については、`DEBUG=1`環境変数を設定してください

## 仕組み

1. **プリコミットトリガー**: `git commit`を実行すると、フックが自動的に実行されます
2. **画像検出**：画像の拡張機能を検索してステージングされたファイルをスキャンします
3. **最適化**：各ステージングされたPNG、JPEG、またはGIFで`image_optim`を実行します
4. **再ステージング**：最適化された画像をステージング領域に自動的に追加します
5. **SVG サイズ チェック**：各ステージング済みSVGを140 KBのサイズ制限と比較します
6. **コミットの続行**：最適化が成功し、SVGがサイズ制限を超えない場合、コミットは通常どおりに続行されます。そうでない場合、コミットは中止されます

## サポートされる画像形式

- **PNG** （`.png`） – 可逆圧縮と非可逆圧縮
- **JPEG** （`.jpg`, `.jpeg`） – メタデータクリーンアップによる非可逆圧縮
- **GIF** （`.gif`） – アニメーションと静的最適化
- **SVG** （`.svg`） – 最適化されていませんが（画質を保持するために現状のままコミットします）、140 KBのサイズ制限に照らし合わせてチェックします。制限を超えると、コミットは中止されます

## ベストプラクティス

1. **フックをテストする**：小さな画像を最初にコミットして、動作することを確認します
2. **変更を確認**: Gitの差分を確認して最適化結果を確認します
3. **パフォーマンスを監視**：大きな画像を最適化するには時間がかかる場合があります
4. **バージョン管理**: フックはこの`.githooks/` ディレクトリに保存されます

## サポート

プリコミットフックに関する問題の場合：

1. エラーメッセージのフック出力を確認する
2. `image_optim`のセットアップが機能していることを確認します
3. 最初に手動レイクタスクでテストします
4. フックログと設定を確認する
5. フック設定を確認してください：`git config core.hooksPath`
