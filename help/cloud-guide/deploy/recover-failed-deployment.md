---
title: コンポーネント障害からのリカバリ
description: コンポーネントがクラウドインフラストラクチャ上のAdobe Commerceに適切にデプロイされない場合の復元方法を説明します。
feature: Cloud, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# コンポーネント障害からのリカバリ

このトピックでは、コンポーネントの適切なデプロイに失敗した場合の回復方法について説明します。 一般的な例としては、互換性のない PHP バージョンなど、リモート環境で満たされない依存関係を持つコンポーネントが挙げられます。

デプロイメントの失敗から回復するには、次のいずれかの方法があります。

- [バックアップの復元](../storage/snapshots.md#restore-a-snapshot)
- 以前の変更からプロジェクトとコードを削除し、再デプロイします

## クリーンアップ、削除、および再デプロイ

以前のデプロイメントからクリーンアップするには、追加または更新されたコンポーネントを特定して削除します。 まず、リモート環境にログインし、`var` ディレクトリの内容を手動で消去します。 次に、`composer.json` ファイルからコンポーネントを削除し、環境を再デプロイします。

**`var` ディレクトリをクリーンアップするには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. SSH を使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. `var` ディレクトリをクリアします。

   ```shell
   rm -rf var/*
   ```

1. ログアウトします。

**コンポーネントを削除するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. キャッシュをクリアします。

   ```bash
   composer clear-cache
   ```

1. `composer.json` ファイルからコンポーネントを削除します。

   ```bash
   composer remove <component-name>:<version>
   ```

   次のメッセージが表示された場合は、それ以上何もする必要はありません。

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. 依存関係を更新しています。しばらくお待ちください。

1. コードの変更を追加、コミットおよびプッシュします。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

バックアップを使用しない環境の復元について詳しくは、[ 環境の復元 ](../development/restore-environment.md) を参照してください。

{{stuck-deployment-tip}}
