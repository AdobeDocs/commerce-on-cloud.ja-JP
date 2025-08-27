---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Rake ライブラリファイル

このディレクトリには、機能別に整理された Rake タスク定義が含まれています。 Rake は、このディレクトリ内のすべての `.rake` ファイルを自動的に読み込みます。

## ファイルの構成

### `adobe-docs-tasks.rake`

Experience League上のAdobe Commerceに関する一般的な要件、共有機能、名前空間に依存しないタスクが含まれています。

- `whatsnew` - ニュースダイジェストのデータを生成（デフォルト：最終更新以降）
- `render` - テンプレートファイルのレンダリングとインクルードの管理

### `includes.rake`

`:includes` 名前空間で整理されたインクルード管理タスクが含まれます。

- `includes:maintain_relationships` - Markdown ファイルでのインクルード関係の検出と維持
- `includes:maintain_timestamps` - インクルードファイルの変更に基づいてタイムスタンプを追加/更新します
- `includes:maintain_all` – 両方の操作を順番に実行します
- `includes:unused` – 未使用のインクルードファイルを検索する

### `images.rake`

`:images` 名前空間で整理された画像管理タスクが含まれます。

- `images:optimize` – 変更されたコミットされていないファイルの画像を最適化する
- `images:unused` - プロジェクト内の未使用の画像を検索する

## 仕組み

Rake は、`.rake` ディレクトリ内の `rakelib/` ファイルを自動的に検出して読み込みます。 これにより、次のことが可能になります。

1. **機能別にタスクを整理** – 関連タスクをグループ化します
2. **メインの Rakefile をクリーンに** - コアプロジェクトタスクに焦点を当てます。
3. **新しいタスクグループを簡単に追加** – 新しい `.rake` ファイルを作成するだけです
4. **関心の分離を管理** – 各ファイルで特定のドメインを処理します

## 新しいタスクグループの追加

関連タスクの新しいグループを追加するには：

1. このディレクトリに新しい `.rake` ファイルを作成します。
2. 説明的な名前空間（画像関連タスクの `namespace :images` など）を使用します
3. 名前空間内でタスクを定義します
4. Rake は自動的にそれらを読み込みます

## 構造の例

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## 使用状況

タスクは作成後すぐに使用できます。

```bash
rake your_namespace:task_name
```

## 利点

- **モジュール性**：各ファイルは、特定の領域にフォーカスします
- **メンテナンス性**：特定のタスクグループを見つけて変更しやすくなります
- **スケーラビリティ**：メインの Rakefile を混乱させずに、多数のタスクグループを追加できる
- **チーム開発**：異なるデベロッパーが異なるタスクグループで作業できます
