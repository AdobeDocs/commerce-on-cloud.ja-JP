---
title: クラウド CLI
description: magento-cloud CLI の概要と、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのローカル開発環境を管理する際にどう役立つかを説明します。
exl-id: 71a705f2-8672-4125-b539-b7b1621f2f64
source-git-commit: 82d89f442792baec995dd0be40f2a49cba168f76
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# クラウド CLI

`magento-cloud` CLI は、開発者とシステム管理者が、ローカルワークステーションからクラウドインフラストラクチャプロジェクトおよび環境のAdobe Commerceを管理できるようにするコマンドラインツールです。

このツールは、追加の自動化機能とプロジェクト管理機能への直接アクセスを提供することで、[[!DNL Cloud Console]](../../get-started/cloud-console.md) の機能を拡張します。 ツールをローカルにインストールした後、それを使用して Starter と Pro の両方の統合環境を管理できます。

>[!NOTE]
>
>これはローカルツールで、Unix ベースのオペレーティングシステムでのみサポートされます。 Windows はサポートされていません。 このページで説明している方法を使用して、クラウド環境（読み取り専用）にインストールすることはできません。 クラウド環境にモジュールをインストールするには、次のいずれかの **デプロイメントワークフロー** を使用します。
>
>- [Pro デプロイメントワークフロー ](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [ スターターデプロイメントのワークフロー ](https://experienceleague.adobe.com/ja/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**`magento-cloud` CLI をインストールするには**:

1. _ローカルワークステーション_ で、クラウドプロジェクトを複製するディレクトリと、[ ファイルシステムの所有者 ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=ja) が _書き込み_ アクセス権を持つディレクトリに変更します。

1. `magento-cloud` CLI をインストールします。

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. bash プロファイ `magento-cloud` に CLI を追加します。

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 更新した bash プロファイルを再読み込みします。

   ```bash
   . ~/.bash_profile
   ```

1. CLI を開始するには、`magento-cloud` を呼び出し、プロンプトが表示されたらクラウドアカウントの資格情報を入力します。

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. `magento-cloud` コマンドがパス内にあることを確認します。 次の例は、使用可能なコマンドを示しています。

   ```bash
   magento-cloud list
   ```

## 一般的なコマンド

Adobeでは、これらのコマンドを Cloud Integration Environment の管理用に設計しました。`magento-cloud` パラメーターを省略できるように、プロジェクトディレクトリから `-p <project-ID>` CLI を実行することをお勧めします。

一般的に使用される `magento-cloud` CLI コマンドの次のリストには、必要なオプションのみが含まれています。 `--help` オプションを任意のコマンドと共に使用すると、詳細を確認できます。

| コマンド | 説明 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | プロジェクトにログインします。 |
| `magento-cloud list` | CLI ツールで使用可能なコマンドを一覧表示します。 |
| `magento-cloud environment:list` | 現在のプロジェクトの環境をリストします。 |
| `magento-cloud environment:checkout` | 既存の環境をチェックアウトします。 |
| `magento-cloud environment:merge -e` | この環境の変更をその親と結合します。 |
| `magento-cloud variables` | この環境のリスト変数。 |
| `magento-cloud ssh` | SSH を使用してリモート環境に接続します。 |
| `magento-cloud url` | ブラウザーでAdobe Commerce ストアフロントを開きます。 |
| `magento-cloud web` | [!DNL Cloud Console] を開きます。 |

## 環境コマンド

環境 _name_ は、環境名にスペースまたは大文字を使用する場合にのみ、環境 _ID_ と異なります。 環境 ID は、すべての小文字、数字、許可された記号で構成されます。 環境名の大文字は、ID では小文字に変換され、環境名のスペースはダッシュに変換されます。

環境名には、Linux シェルまたは正規表現で予約されている文字を含めることができません _使用できません_。 禁止されている文字には、中括弧（`{ }`）、括弧、アスタリスク（`*`）、山括弧（`< >`）、アンパサンド（`&`）、パーセント（`%`）、その他の文字があります。

`magento-cloud environment:list` コマンドは環境階層を表示しますが、`git branch` は表示しません。 ネストされた環境がある場合は、次を使用します。

```bash
magento-cloud environment:list
```

### 環境の再デプロイ

プッシュを使用せずに再デプロイメントをトリガーします。 再デプロイする環境を検証し、確認します。 保留状態のビルドがある場合は、再デプロイを使用しません。

```bash
magento-cloud environment:redeploy
```

応答の例：

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git コマンド

これらのコマンドの一部は Git コマンドに似ていることに気付くかもしれません。 `magento-cloud` のコマンドは、追加機能を使用して Git ベースのクラウドプロジェクトに直接接続します。 `magento-cloud` CLI を使用せずにブランチを作成した場合、ブランチは「アクティブ化」されず、変更をリモート環境にプッシュしても自動的にビルドされません。 `magento-cloud` CLI コマンドにはアクティブ化が含まれます。

分岐を作成するには、`magento-cloud` コマンドを使用して分岐をアクティブにします。

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

分岐ステータスの場合：

- `magento-cloud env` コマンドを使用して、環境ブランチとそのステータス（アクティブまたは非アクティブ）のリストを表示します。
- `magento-cloud environment:activate` コマンドを使用して、環境ブランチをアクティベートします。

空の Git コミットをプッシュしてデプロイメントをトリガーにします。 例：

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

ユーザーの追加など、一部のアクションではデプロイメントは発生しません。

### 環境ブランチの作成

次の手順は、CLI コマンドと Git コマンドを交互に使用してローカル環境を管理する方法を示しています。

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. [ ファイルシステム所有者 ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=ja) に切り替えます。

1. プロジェクトにログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトのリストを作成します。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクト内の環境をリストします。 すべての環境には、コード、データベース、環境変数、設定およびサービスを含んだアクティブな Git ブランチが含まれています。

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >`magento-cloud environment:list` コマンドは環境階層を表示するのに対して、`git branch` コマンドは表示しないので、このコマンドを使用することが重要です。

1. オリジンブランチを取得して最新のコードを取得します。

   ```bash
   git fetch origin
   ```

1. 特定のブランチと環境をチェックアウトするか、切り替えます。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git コマンドは、Git ブランチのみをチェックアウトします。 `magento-cloud checkout` コマンドは、ブランチをチェックアウトし、アクティブな環境に切り替えます。

   >[!TIP]
   >
   >`magento-cloud environment:branch <environment-name> <parent-environment-ID>` のコマンド構文を使用して、環境ブランチを作成できます。 環境ブランチの作成とアクティブ化にはさらに時間がかかる場合があります。

1. 環境 ID を使用して、更新されたコードをローカルに取り込みます。 環境ブランチが新しい場合は、これは必要ありません。

   ```bash
   git pull origin <environment-ID>
   ```

1. （_オプション_）環境の [ スナップショット ](../storage/snapshots.md) をバックアップとして作成します。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## CLI の更新

ログイン時に `magento-cloud` CLI は利用可能な更新をチェックしますが、`self:update` コマンドを使用すると更新をチェックできます。 更新がある場合は、手順に従って CLI を更新します。

`magento-cloud` CLI が最新の場合は、次の応答が表示されます。

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
