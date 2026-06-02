---
title: CLIを使用した分岐の管理
description: Cloud CLIを使用して、Adobe Commerce on cloud infrastructureの環境ブランチを管理する方法を説明します。
role: Developer
feature: Cloud, Install
exl-id: d67e8802-8137-451f-b468-8b788afb01ea
TQID: https://experienceleague.adobe.com/hCfTF-Vl9LLKgUet4hS3JZN3kX7ZF6BDJ4tsYnr44Fs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 688
ht-degree: 0%

---

# CLIを使用した分岐の管理

`magento-cloud` CLIをインストールするには、[Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md)を参照してください。 `magento-cloud` CLIをインストールし、クラウドインフラストラクチャへのリモートアクセス用にSSH キーを設定したら、`magento-cloud` CLI コマンドを使用してプロジェクトの環境を管理できます。 環境アーキテクチャについて詳しくは、[&#x200B; スターターアーキテクチャ &#x200B;](../architecture/starter-architecture.md)または[&#x200B; プロアーキテクチャ &#x200B;](../architecture/pro-architecture.md)を参照してください。

[!DNL Cloud Console]を使用してブランチと環境を管理するには、[を使用してブランチを管理 [!DNL Cloud Console]](../project/console-branches.md)を参照してください。

## CLI コマンドの使用

`magento-cloud` CLI コマンドは、Git コマンドに似ています。 プロジェクトに接続し、環境を管理するために使用できます。 任意のディレクトリからコマンドを実行できますが、プロジェクト ディレクトリからコマンドを実行することをお勧めします。 プロジェクトディレクトリから実行する場合、`-p <project-ID>` パラメーターを省略できます。 [Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md)を参照してください。

## プロジェクトの複製

次の手順では、`magento-cloud` CLI コマンドとGit コマンドを組み合わせて、プロジェクトをローカル ワークステーションに複製します。 `magento-cloud` CLI コマンドの完全なリストを表示するには、`magento-cloud list` コマンドを使用します。

>[!IMPORTANT]
>
>Git コマンドの中には、Adobe Commerce on cloud インフラストラクチャプロジェクトでアクションを実行できないものもあります。 例えば、Git コマンドを使用してブランチを作成できますが、新しい環境を作成してアクティブ化することはできません。 環境を&#x200B;_アクティブ_&#x200B;にするには、`magento-cloud environment:branch <branch-name>` コマンドを使用して環境を作成する必要があります。 または、[!DNL Cloud Console]を使用してアクティブな環境を作成することもできます。 [Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md#git-commands)を参照してください。

**プロジェクト `master`環境**&#x200B;を複製するには：

1. [&#x200B; ファイルシステム所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) アカウントでローカル ワークステーションにログインします。

1. Web サーバーまたは仮想ホスト _docroot_ ディレクトリに変更します。

1. `magento-cloud` CLIを使用してログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトのリストを作成。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクトの複製。

   ```bash
   magento-cloud project:get <project-ID>
   ```

   プロンプトが表示されたら、ディレクトリ名を入力します。

1. `magento2` ディレクトリに変更します。

1. プロジェクトで使用可能な環境のリスト。

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >`magento-cloud environment:list` コマンドは環境階層を表示しますが、`git branch` コマンドは表示しません。

1. リモートブランチを取得します。

   ```bash
   git fetch origin
   ```

1. 更新されたコードを取得します。

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Adobe Commerce クラウドインフラストラクチャでのGit ベースのホスティングサービスの使用について詳しくは、[統合](../integrations/overview.md)を参照してください。

## 開発用ブランチの作成

プロジェクトを複製し、Adobe Commerce管理者アカウント設定を更新したら、開発のために分岐できます。 前述したように、環境を&#x200B;_アクティブ_&#x200B;にするには、`magento-cloud environment:branch <branch-name>` コマンドまたは[!DNL Cloud Console]を使用して環境を作成する必要があります。

- [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch)の場合は、`staging`のブランチを作成してから、`staging` ブランチに基づいて開発ブランチを作成することを検討してください。
- [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow)の場合、`Integration` ブランチに基づいて開発ブランチを作成します。

**開発ブランチを作成するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. プロジェクトワークフローで推奨されるブランチに基づいて環境を作成します。

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 依存関係を更新します。

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_オプション_]&#x200B;環境の[&#x200B; バックアップ &#x200B;](../storage/snapshots.md)を作成します。

### 分岐を結合

開発が完了したら、このブランチを親にマージします。

1. コミットとプッシュのコード変更：

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 親の環境と結合：

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 環境の削除

環境を削除する必要がなくなったことがわかっている場合にのみ、環境を削除します。 環境を削除した後に回復することはできません。

>[!WARNING]
>
>プロジェクトの`master` ブランチは削除できません。

このタスクを実行するには、プロジェクト管理者、環境管理者、またはアカウント所有者である必要があります。 [&#x200B; クラウドプロジェクトへのユーザーアクセスの管理](../project/user-access.md)を参照してください。

環境を削除すると、環境は&#x200B;_非アクティブ_&#x200B;に設定されます。 コードは引き続きGit ブランチで利用できますが、サービスやデータベースは含まれていません。 環境を完全に削除するには、対応するリモート Git ブランチも削除する必要があります。

**環境を削除するには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. リモートサーバーから更新を取得します。

   ```bash
   git fetch
   ```

1. 環境ブランチを削除します。

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   オプションとして、複数の環境IDをdelete コマンドに追加することで、複数の環境を一度に削除できます。

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. プロンプトに応答して、ローカル環境と対応するリモート環境を削除します。

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   環境を削除すると、_非アクティブ_&#x200B;状態になります。

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   リモート Git ブランチを削除すると、プロジェクトから環境が削除されます。

1. 環境が削除されるのを待ちます。

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>非アクティブな環境をアクティブ化するには、`magento-cloud environment:activate` コマンドを使用します。

## リモート環境の操作

SSH キー[&#128279;](../development/secure-connections.md)を[&#x200B; セットアップした後、ローカル ワークスペースからリモート環境](../development/secure-connections.md#connect-to-a-remote-environment)に接続し、プロジェクト サービスと対話して設定を変更できます。
