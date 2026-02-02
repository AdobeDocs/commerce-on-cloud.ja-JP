---
source-git-commit: ee2ed3a78939af36572f0baa6672b2e9f9a2557f
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---
# 画像の最適化のプリコミットフック

このディレクトリには、画像がリポジトリにコミットされる前に自動的に最適化される事前コミットフックが含まれています。

## フックの機能

- **自動検出** ステージングされた画像ファイル（PNG、JPG、JPEG、GIF）
- **`image_optim`** を実行して画像を圧縮および最適化
- **最適化された画像を自動的に再ステージ**
- **すべてのコミット済み画像が適切に最適化されていることを確認**

## 利点

- リポジトリサイズの縮小
- ドキュメントのページ読み込みの高速化
- すべてのコントリビューターで一貫した画質
- 手動での最適化は不要です

## 前提条件

- Ruby 3.0 以降
- バンドラー
- Git

## 設定

### 自動セットアップ （推奨）

```bash
.githooks/setup-hooks.sh
```

### 手動設定

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### プロジェクト設定の完了

1. リポジトリのクローンを作成します。

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. プリコミットフックを有効にする：

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Jekyll 依存関係をインストールします。

   ```bash
   cd _jekyll
   bundle install
   ```

## フックのテスト

1. リポジトリへの画像ファイルの追加
2. ステージング：`git add <image-file>`
3. コミットを試みます：`git commit -m 'test'`
4. フックは自動的に画像を最適化する必要があります

### 期待される出力

```bash
Found 1 staged image(s). Running optimization...
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## 画像のガイドライン

- **PNG**：スクリーンショットと UI 要素に使用（自動的に最適化されます）
- **JPEG**：写真に使用します（自動的に最適化されます）
- **GIF**: アニメーションに使用（自動的に最適化されます）
- **SVG**: アイコンおよびシンプルなグラフィックに使用（フックによって処理されず、そのままコミットされます）

プリコミットフックは、コミット時に PNG、JPEG、GIF画像を自動的に最適化します。

## 手動での最適化

手動での画像の最適化の場合：

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## 設定

フックは、設定ファイル `_jekyll/.image_optim.yml` を使用して最適化設定をカスタマイズします。

- **PNG**:`advpng`、`optipng`、`pngquant` を使用します
- **JPEG**: `jhead`、`jpegoptim`、`jpegtran` を使用します
- **GIF**: `gifsicle` を使用します
- **SVG**：処理されません（ベクターグラフィックおよびアニメーションを保持するために検出から除外されます）

## トラブルシューティング

### フックが動作していません

- フック設定の確認：`git config core.hooksPath`
- フック ファイルが実行可能であることを確認します。`chmod +x .githooks/pre-commit`
- ディレクトリが指定された正しいリポジトリに属していることを確認 `_jekyll` ます

### 最適化の失敗

- `bundle install` ディレクトリで `_jekyll` が実行されていることを確認します。
- `adobe-comdox-exl-rake-tasks` gem がインストールされていることを確認します（`image_optim` を提供）。
- `.image_optim.yml` 設定ファイルを確認します。

### パフォーマンスの問題

- `_jekyll/.image_optim.yml` でのスレッド数の調整
- エラー情報 `DEBUG=1` 詳細な環境変数の設定

## 仕組み

1. **プリコミットトリガー**:`git commit` を実行すると、フックが自動的に実行されます
2. **画像検出**：ステージングされたファイルをスキャンして画像拡張子を調べます
3. **最適化**：ステージングされた各画像で `image_optim` を実行します
4. **再ステージング**：最適化された画像をステージング領域に自動的に追加して戻します
5. **コミットが続行**：最適化に成功した場合、コミットは通常どおり続行されます

## サポートされる画像形式

- **PNG** （`.png`） – 可逆および非可逆圧縮
- **JPEG** （`.jpg`、`.jpeg`） – メタデータのクリーンアップによる非可逆圧縮
- **GIF** （`.gif`） – アニメーションと静的な最適化
- **SVG** （`.svg`） – フックによって処理されません（品質を保持するためにそのままコミットします）

## ベストプラクティス

1. **フックのテスト**：最初に小さなイメージをコミットして、それが機能することを確認します
2. **変更を確認**:Git の差分を確認して最適化結果を確認する
3. **パフォーマンスの監視**：大きな画像の最適化には時間がかかる場合があります
4. **バージョン管理**: フックはこの `.githooks/` ディレクトリに格納されます

## サポート

プリコミットフックの問題の場合：

1. エラーメッセージのフック出力を確認します
2. `image_optim` 設定が機能していることを確認します
3. 最初に手動のレイクタスクでテストします
4. フックログと設定を確認します。
5. フックの設定を確認します（`git config core.hooksPath`）。
