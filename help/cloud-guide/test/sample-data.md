---
title: サンプルデータ
description: クラウドインフラストラクチャー上でAdobe Commerceを使用してサンプルデータをインストールする方法を説明します。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# サンプルデータ

ストアの開発時にサンプルデータが必要な場合は、サンプルデータをインストールできます。 このデータは、顧客、商品、その他のデータを含むアクティブなAdobe Commerce ストアをシミュレーションします。 このサンプルデータは、クラウドインフラストラクチャテンプレート上の新しいAdobe Commerceインストールで最も適しています。

ベストプラクティスとして、サンプルデータを開発環境と統合環境にインストールします。 ステージング環境または実稼動環境でサンプルデータを使用する場合は、情報と製品を [ 削除 ](#reset-or-uninstall-sample-data) してから運用を開始する必要があります。

## サンプルデータのインストール

サンプルデータをインストールするには：

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. ルートで、次のコマンドを入力してサンプルデータを追加します。

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. コンポーネントが更新されるのを待ちます。

1. 変更をコミットし、プッシュします。

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. プロジェクトがデプロイされるのを待ちます。

1. 統合環境のストアフロントページに移動して、インストールが成功したことを確認します。 ストアフロントへの URL リンクは、[!DNL Cloud Console] を通じて見つけることができます。

1. 環境のスナップショットを作成します。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

ライブデータを使用して開発のテストを開始できます。

## サンプルデータのリセットまたはアンインストール

サンプルデータのインストールと同じ手順に従って、サンプルデータをリセットまたは削除できます。

- サンプル データの削除：`./bin/magento sampledata:remove`
- サンプル データのリセット：`./bin/magento sampledata:reset`
