---
title: サンプルデータ
description: Adobe Commerce on cloud infrastructureを使用してサンプルデータをインストールする方法について説明します。
exl-id: 59e23cfa-d364-4e70-8a86-644c339778cc
TQID: https://experienceleague.adobe.com/hEowDeOnXfkdlB-GCNRylKYrJLtXITR11-zsveyIA0Y
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 205
ht-degree: 0%

---

# サンプルデータ

ストアの開発時にサンプルデータが必要な場合は、サンプルデータをインストールできます。 このデータは、顧客や商品などのデータを含む、アクティブなAdobe Commerceストアをシミュレートします。 このサンプルデータは、新しいAdobe Commerce on cloud インフラストラクチャテンプレートのインストールで最も効果的に機能します。

ベストプラクティスとして、開発環境と統合環境にサンプルデータをインストールします。 ステージングまたは実稼動環境でサンプルデータを使用する場合は、本番稼働前に情報と製品を[削除](#reset-or-uninstall-sample-data)する必要があります。

## サンプルデータのインストール

サンプルデータをインストールするには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. ルートで次のコマンドを入力して、サンプルデータを追加します。

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. コンポーネントが更新されるのを待ちます。

1. 変更をコミットしてプッシュします。

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. プロジェクトがデプロイされるのを待ちます。

1. 統合環境でストアフロントページに移動し、インストールが成功したことを確認します。 [!DNL Cloud Console]を通じてストアフロントへのURL リンクを見つけることができます。

1. ご使用の環境のスナップショットを撮ります。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

ライブデータを使用して開発テストを開始できます。

## サンプルデータのリセットまたはアンインストール

サンプルデータのインストールと同じ手順で、サンプルデータをリセットまたは削除できます。

- サンプルデータの削除：`./bin/magento sampledata:remove`
- サンプルデータのリセット：`./bin/magento sampledata:reset`
