---
title: Cloud CLI
description: magento-cloud CLIと、Adobe Commerce on cloud インフラストラクチャプロジェクトのローカル開発環境を管理する方法について説明します。
exl-id: 71a705f2-8672-4125-b539-b7b1621f2f64
source-git-commit: 82d89f442792baec995dd0be40f2a49cba168f76
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Cloud CLI

`magento-cloud` CLIは、開発者とシステム管理者がローカル ワークステーションからクラウド インフラストラクチャ プロジェクトおよび環境でAdobe Commerceを管理できるようにするコマンドライン ツールです。

このツールは、追加の自動化機能とプロジェクト管理機能への直接アクセスを提供することで、[[!DNL Cloud Console]](../../get-started/cloud-console.md)の機能を拡張します。 ツールをローカルにインストールした後は、それを使用してStarter環境とPro統合環境の両方を管理できます。

>[!NOTE]
>
>これはローカルツールであり、Unix ベースのオペレーティングシステムでのみサポートされています。 Windowsはサポートされていません。 このページで説明されている方法を使用して、クラウド環境（読み取り専用）にインストールすることはできません。 次のいずれかの&#x200B;**デプロイメントワークフロー**&#x200B;を介して、クラウド環境にモジュールのみをインストールできます。
>
>- [Pro デプロイメントワークフロー](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [ スターターデプロイメントワークフロー](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**`magento-cloud` CLI**&#x200B;をインストールするには：

1. _ローカルワークステーション_&#x200B;で、クラウドプロジェクトを複製するディレクトリに変更します。このディレクトリでは、[ ファイルシステム所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)が&#x200B;_書き込み_ アクセスできます。

1. `magento-cloud` CLIをインストールします。

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Bash プロファイルに`magento-cloud` CLIを追加します。

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 更新したbash プロファイルをリロードします。

   ```bash
   . ~/.bash_profile
   ```

1. CLIを開始するには、`magento-cloud`を呼び出し、プロンプトが表示されたらCloud アカウントの資格情報を入力します。

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. `magento-cloud` コマンドがパス内にあることを確認します。 次の例は、使用可能なコマンドの一覧です。

   ```bash
   magento-cloud list
   ```

## 共通コマンド

Adobeは、Cloud統合環境を管理するためにこれらのコマンドを設計し、`-p <project-ID>` パラメーターを省略できるように、プロジェクトディレクトリから`magento-cloud` CLIを実行することをお勧めします。

一般的に使用される`magento-cloud` CLI コマンドの次のリストには、必要なオプションのみが含まれています。 任意のコマンドで`--help` オプションを使用すると、詳細を確認できます。

| コマンド | 説明 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | プロジェクトにログインします。 |
| `magento-cloud list` | CLI ツールで使用できるコマンドのリストを表示します。 |
| `magento-cloud environment:list` | 現在のプロジェクト内の環境を一覧表示します。 |
| `magento-cloud environment:checkout` | 既存の環境をチェックアウトします。 |
| `magento-cloud environment:merge -e` | この環境の変更を親と結合します。 |
| `magento-cloud variables` | この環境で変数をリストします。 |
| `magento-cloud ssh` | SSHを使用してリモート環境に接続します。 |
| `magento-cloud url` | ブラウザーでAdobe Commerce ストアフロントを開きます。 |
| `magento-cloud web` | [!DNL Cloud Console]を開きます。 |

## 環境コマンド

環境&#x200B;_name_&#x200B;が環境&#x200B;_ID_&#x200B;と異なるのは、環境名にスペースまたは大文字を使用する場合のみです。 環境IDは、すべての小文字、数字、許可された記号で構成されます。 環境名の大文字はIDの小文字に変換され、環境名のスペースはダッシュに変換されます。

環境名&#x200B;_には、Linux シェルまたは正規表現用に予約された文字を含めることはできません_。 使用できない文字には、中括弧（`{ }`）、括弧、アスタリスク （`*`）、角括弧（`< >`）、アンパサンド （`&`）、% （`%`）などの文字が含まれます。

`magento-cloud environment:list` コマンドは環境階層を表示しますが、`git branch`は表示しません。 ネストされた環境がある場合は、次を使用します。

```bash
magento-cloud environment:list
```

### 環境の再デプロイ

プッシュ通知を使用せずに再デプロイメントをトリガーします。 再デプロイする環境を確認して確認します。 保留状態のビルドがある場合は、redeployを使用しないでください。

```bash
magento-cloud environment:redeploy
```

回答サンプル：

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git コマンド

これらのコマンドのいくつかはGit コマンドに似ていることにお気づきでしょう。 `magento-cloud` コマンドは、追加機能を使用して、Git ベースのCloud プロジェクトに直接接続します。 `magento-cloud` CLIを使用せずにブランチを作成した場合、ブランチは「アクティブ化」されず、リモート環境に変更をプッシュしても自動的にビルドされません。 `magento-cloud` CLI コマンドには、アクティブ化が含まれています。

ブランチを作成するには、`magento-cloud` コマンドを使用して、ブランチをアクティブ化します。

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

ブランチステータスの場合：

- `magento-cloud env` コマンドを使用して、環境ブランチのリストとそのステータス（アクティブまたは非アクティブ）を表示します。
- 環境ブランチをアクティブ化するには、`magento-cloud environment:activate` コマンドを使用します。

空のGit コミットをデプロイメントのトリガーにプッシュします。 例：

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

ユーザーの追加など、一部のアクションはデプロイメントに至りません。

### 環境ブランチの作成

次の手順では、CLI コマンドとGit コマンドを交互に使用してローカル環境を管理する方法を示します。

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. [ ファイルシステム所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)に切り替えます。

1. プロジェクトにログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトのリストを作成。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクト内の環境のリスト。 あらゆる環境には、コード、データベース、環境変数、設定、サービスを含むアクティブなGit ブランチが含まれます。

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >環境階層が表示されますが、`git branch` コマンドは表示されないため、`magento-cloud environment:list` コマンドを使用することが重要です。

1. オリジン分岐を取得して最新のコードを取得します。

   ```bash
   git fetch origin
   ```

1. 特定のブランチや環境をチェックアウトするか、切り替えます。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git コマンドはGit ブランチをチェックアウトするだけです。 `magento-cloud checkout` コマンドはブランチをチェックアウトし、アクティブな環境に切り替えます。

   >[!TIP]
   >
   >環境ブランチは、`magento-cloud environment:branch <environment-name> <parent-environment-ID>` コマンド構文を使用して作成できます。 環境ブランチの作成とアクティブ化にはさらに時間がかかる場合があります。

1. 環境IDを使用して、更新されたコードをローカルに取り込みます。 環境ブランチが新しい場合、これは必要ありません。

   ```bash
   git pull origin <environment-ID>
   ```

1. （_オプション_）環境の[ スナップショット ](../storage/snapshots.md)をバックアップとして作成します。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## CLIの更新

`magento-cloud` CLIは、ログイン時に利用可能な更新を確認しますが、`self:update` コマンドを使用して更新を確認できます。 利用可能なアップデートがある場合は、手順に従ってCLIをアップデートします。

`magento-cloud` CLIが最新の場合、次の応答が表示されます。

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
