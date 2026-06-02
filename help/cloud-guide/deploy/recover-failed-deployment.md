---
title: コンポーネントの障害からの復旧
description: Adobe Commerce on cloud infrastructureでコンポーネントを適切にデプロイできない場合の復旧方法について説明します。
feature: Cloud, Deploy
exl-id: f5e79366-5548-40dd-ac2a-56dc84c5d4e2
TQID: https://experienceleague.adobe.com/1krotAWilGcnp3OUMk-fl5iHFObGpWFUC8EpYSPjQfU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 0%

---

# コンポーネントの障害からの復旧

このトピックでは、コンポーネントが適切にデプロイできない場合の回復方法について説明します。 一般的な例としては、互換性のないPHPのバージョンなど、リモート環境で満たされない依存関係を持つコンポーネントが挙げられます。

次のいずれかの方法で、失敗したデプロイメントから回復できます。

- [バックアップの復元](../storage/snapshots.md#restore-a-snapshot)
- 以前の変更からプロジェクトとコードをクリーニングして再デプロイする

## クリーニング、削除、および再展開

以前のデプロイメントからクリーンアップするには、追加または更新されたコンポーネントを特定して削除します。 まず、リモート環境にログインし、`var` ディレクトリの内容を手動でクリアします。 次に、`composer.json` ファイルからコンポーネントを削除し、環境を再デプロイします。

**`var` ディレクトリを削除するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. `var` ディレクトリをクリアします。

   ```shell
   rm -rf var/*
   ```

1. ログアウトします。

**コンポーネントを削除するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. キャッシュをクリアします。

   ```bash
   composer clear-cache
   ```

1. コンポーネントを`composer.json` ファイルから削除します。

   ```bash
   composer remove <component-name>:<version>
   ```

   次のメッセージが表示された場合は、それ以上何もする必要はありません。

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. 依存関係が更新されるまでお待ちください。

1. コードの変更を追加、コミット、プッシュします。

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

バックアップなしで環境を復元する方法については、[環境を復元](../development/restore-environment.md)を参照してください。

{{stuck-deployment-tip}}
