---
title: CLI によるブランチの管理
description: Cloud CLI を使用して、クラウドインフラストラクチャ上のAdobe Commerceの環境ブランチを管理する方法について説明します。
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# CLI によるブランチの管理

`magento-cloud` CLI をインストールするには、[Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md) を参照してください。 `magento-cloud` CLI をインストールし、クラウドインフラストラクチャへのリモートアクセス用の SSH キーを設定したら、`magento-cloud` CLI コマンドを使用してプロジェクトの環境を管理できます。 環境アーキテクチャについては、「[&#x200B; スターターアーキテクチャ &#x200B;](../architecture/starter-architecture.md) または [Pro アーキテクチャ &#x200B;](../architecture/pro-architecture.md)」を参照してください。

[!DNL Cloud Console] を使用してブランチと環境を管理するには、[&#x200B; を使用したブランチの管理  [!DNL Cloud Console]](../project/console-branches.md) を参照してください。

## CLI コマンドの使用

`magento-cloud` の CLI コマンドは、Git コマンドに似ています。 これらを使用して、プロジェクトに接続し、環境を管理できます。 コマンドは任意のディレクトリから実行できますが、プロジェクトディレクトリから実行することをお勧めします。 プロジェクトディレクトリから実行する場合は、`-p <project-ID>` パラメーターを省略できます。 [Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md) を参照してください。

## プロジェクトのクローン

次の手順では、CLI コマンドと Git コマンド `magento-cloud` 組み合わせて、プロジェクトをローカルワークステーションに複製します。 CLI コマンドの完全なリスト `magento-cloud` 表示するには、`magento-cloud list` コマンドを使用します。

>[!IMPORTANT]
>
>一部の Git コマンドでは、クラウドインフラストラクチャプロジェクト上のAdobe Commerceでアクションを完了できません。 例えば、Git コマンドを使用してブランチを作成できますが、新しい環境を作成してアクティブ化することはできません。 環境を _アクティブ_ にするには、`magento-cloud environment:branch <branch-name>` コマンドを使用して環境を作成する必要があります。 または、[!DNL Cloud Console] を使用してアクティブな環境を作成できます。 [Cloud CLI リファレンス &#x200B;](../dev-tools/cloud-cli-overview.md#git-commands) を参照してください。

**プロジェクトを環境 `master` 複製するには**:

1. [&#x200B; ファイル・システム所有者 &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=ja) アカウントを使用して、ローカル・ワークステーションにログインします。

1. Web サーバーまたは仮想ホスト _docroot_ ディレクトリに移動します。

1. `magento-cloud` CLI を使用してログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトのリストを作成します。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクトのクローン。

   ```bash
   magento-cloud project:get <project-ID>
   ```

   プロンプトが表示されたら、ディレクトリ名を指定します。

1. `magento2` ディレクトリに移動します。

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

1. 更新されたコードを取り込みます。

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>クラウドインフラストラクチャー上のAdobe Commerceで Git ベースのホスティングサービスを使用する方法については、「[&#x200B; 統合 &#x200B;](../integrations/overview.md)」を参照してください。

## 開発用のブランチの作成

プロジェクトを複製し、Adobe Commerce管理者アカウントの設定を更新したら、開発用に分岐できます。 前述のように、環境を _アクティブ_ にするには、`magento-cloud environment:branch <branch-name>` コマンドまたは [!DNL Cloud Console] を使用して環境を作成する必要があります。

- [&#x200B; スターター &#x200B;](../architecture/starter-develop-deploy-workflow.md#clone-and-branch) の場合は、`staging` 用のブランチを作成した後、`staging` ブランチに基づいて開発ブランチを作成することを検討します。
- [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow) の場合は、`Integration` ブランチに基づいて開発ブランチを作成します。

**開発ブランチを作成するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. プロジェクトワークフローに推奨されるブランチに基づいて環境を作成します。

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 依存関係を更新します。

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_オプション_] 環境の [&#x200B; バックアップ &#x200B;](../storage/snapshots.md) を作成します。

### ブランチの結合

開発が完了したら、このブランチを親に結合します。

1. コードの変更をコミットしてプッシュします。

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 親環境と結合します。

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 環境の削除

環境が不要になったことが確実な場合にのみ、環境を削除します。 環境を削除した後は、その環境を復元できません。

>[!WARNING]
>
>プロジェクトの `master` ブランチは削除できません。

このタスクを実行するには、プロジェクト管理者、環境管理者、またはアカウント所有者である必要があります。 詳しくは、[&#x200B; クラウドプロジェクトへのユーザーアクセスの管理 &#x200B;](../project/user-access.md) を参照してください。

環境を削除すると、その環境は _非アクティブ_ に設定されます。 コードは引き続き Git ブランチで使用できますが、サービスやデータベースは含まれなくなりました。 環境を完全に削除するには、対応するリモート Git ブランチも削除する必要があります。

**環境を削除するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. リモートサーバーから更新を取得します。

   ```bash
   git fetch
   ```

1. 環境ブランチを削除します。

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   オプションとして、delete コマンドに複数の環境 ID を追加することで、一度に複数の環境を削除できます。

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. プロンプトに応答して、ローカル環境と対応するリモート環境を削除します。

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   環境を削除すると、環境は _非アクティブ_ 状態になります。

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
>非アクティブな環境をアクティブにするには、`magento-cloud environment:activate` コマンドを使用します。

## リモート環境とのインタラクション

[SSH キーを設定 &#x200B;](../development/secure-connections.md) したら、[&#x200B; ローカルワークスペースからリモート環境に接続 &#x200B;](../development/secure-connections.md#connect-to-a-remote-environment) し、プロジェクトサービスとやり取りし、設定を変更できます。
