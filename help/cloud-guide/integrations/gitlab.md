---
title: GitLab との統合
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceを GitLab と統合する方法を説明します。
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# GitLab との統合

コードの変更をプッシュするときに環境を自動的にビルドおよびデプロイするように、GitLab リポジトリを設定できます。 この統合により、GitLab リポジトリがクラウドインフラストラクチャアカウント上のAdobe Commerceと同期されます。

{{private-repository}}

この統合により、次のことが可能になります。

- ブランチの作成時に環境を作成します
- プルリクエストを結合する際に環境を再デプロイ
- ブランチを削除したら、環境を削除します

プロセスを続行するには、GitLab トークンと Webhook を取得する必要があります。

## 前提条件

- Adobe Commerce on cloud infrastructure プロジェクトへの管理者アクセス
- ローカル環境での [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) ツール
- GitLab アカウント
- GitLab リポジトリへの書き込みアクセス権を持つ GitLab 個人用アクセストークン。選択した範囲は、少なくとも `api` と `read_repository` である必要があります。

## リポジトリを準備

既存の環境からクラウドインフラストラクチャプロジェクト上にAdobe Commerceのクローンを作成し、同じブランチ名を保持したまま、プロジェクトのブランチを新しい空の GitLab リポジトリに移行します。 Adobe Commerce on cloud infrastructure プロジェクトで既存の環境やブランチが失われないように、同一の Git ツリーを保持することが **重要** です。

1. ターミナルから、クラウドインフラストラクチャプロジェクトのAdobe Commerceにログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトをリストして、プロジェクト ID をコピーします。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクトのクローンをローカル環境に作成します。

   ```bash
   magento-cloud project:get <project-id>
   ```

1. GitLab リポジトリをリモートとして追加します（SaaS バージョンで GitLab が使用されている場合）。

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   リモート接続の既定の名前は `origin` または `magento` です。 `origin` が存在する場合は、別の名前を選択するか、既存の参照の名前を変更または削除できます。 [git-remote ドキュメント &#x200B;](https://git-scm.com/docs/git-remote) を参照してください。

1. GitLab リモートが正しく追加されていることを確認します。

   ```bash
   git remote -v
   ```

   期待される応答：

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. プロジェクトファイルを新しい GitLab リポジトリにプッシュします。 すべてのブランチ名は同じにしておくことを忘れないでください。

   ```bash
   git push -u origin master
   ```

   新しい GitLab リポジトリで開始する場合、リモートリポジトリがローカルコピーと一致しないので、`-f` オプションを使用する必要がある場合があります。

1. GitLab リポジトリにすべてのプロジェクトファイルが含まれていることを確認します。

## GitLab 統合の有効化

`magento-cloud integration` コマンドを使用して GitLab 統合を有効にし、GitLab Webhook のペイロード URL を取得して、GitLab からクラウドインフラストラクチャプロジェクトのAdobe Commerceに更新を送信します。

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| オプション | 説明 |
| ------ | ----------- |
| `<project-ID>` | クラウドインフラストラクチャー上のAdobe Commerce プロジェクト ID |
| `<your-GitLab-token>` | GitLab 用に生成した個人用アクセストークン |
| `--base-url` | GitLab の URL （GitLab を SaaS バージョンで使用している場合の `https://gitlab.com/`） |
| `--server-project` | GitLab のプロジェクト名（ベース URL の後の部分） |
| `--build-merge-requests` | すべての結合リクエストで新しい環境を構築するようにクラウドインフラストラクチャ上のAdobe Commerceに指示する _オプション_ パラメーター（デフォルトでは `true`） |
| `--merge-requests-clone-parent-data` | 結合リクエスト用に親環境のデータを複製するようにクラウドインフラストラクチャ上のAdobe Commerceに指示する _オプション_ パラメーター（デフォルトでは `true`） |
| `--fetch-branches` | Adobe Commerce on Cloud Infrastructure がリモートから（非アクティブな環境として）すべてのブランチを取得できるようにする _オプション_ パラメーター（デフォルトでは `true`） |
| `--prune-branches` | リモートに存在しないブランチを削除するようクラウドインフラストラクチャー上のAdobe Commerceに指示する _オプション_ パラメーター（デフォルトでは `true`） |

>[!WARNING]
>
>`magento-cloud integration` コマンドは、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのコード _すべて_ を、GitLab リポジトリのコードで上書きします。 これには、`production` ブランチを含むすべてのブランチが含まれます。 このアクションは即座に実行され、元に戻すことはできません。 ベストプラクティスとして、GitLab 統合を追加する前に、クラウドインフラストラクチャプロジェクト上のAdobe Commerceからすべてのブランチをクローンし、GitLab リポジトリにプッシュすることが重要です。

**GitLab 統合を有効にするには**:

1. ターミナルから、Adobe Commerce on cloud infrastructure プロジェクトに GitLab 統合を追加します。

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. プロンプトが表示されたら、`y` と入力して統合を追加します。

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. 返り出力で表示されている **フック URL** をコピーします。

   ```
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### GitLab での Webhook の追加

イベント（プッシュリクエストや結合リクエストなど）をクラウド Git サーバーと通信するには、GitLab リポジトリの [Webhook を作成 &#x200B;](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) する必要があります

1. GitLab リポジトリで、「**設定**」タブをクリックします。

1. 左側のナビゲーションバーで、「**Webhook**」をクリックします。

1. _Webhook_ フォームで、次のフィールドを編集します。

   - **URL**:GitLab 統合を有効にした際に返される `Hook URL` を入力します。
   - **秘密鍵トークン**：必要に応じて、検証秘密鍵を入力します。
   - **トリガー**：必要に応じて、`Merge request events` や `Push events` を確認してください。
   - **SSL 検証を有効にする**：このオプションを選択する必要があります。

1. **Webhook を追加** をクリックします。

### 統合のテスト

GitLab 統合を設定したら、`magento-cloud` の CLI を使用して、統合が動作していることを確認できます。

```bash
magento-cloud integration:validate
```

または、GitLab リポジトリに簡単な変更をプッシュしてテストできます。

1. テストファイルを作成します。

   ```bash
   touch test.md
   ```

1. 変更をコミットして GitLab リポジトリにプッシュします。

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. [[!DNL Cloud Console]](../project/overview.md) にログインし、コミットメッセージが表示され、プロジェクトがデプロイされていることを確認します。

## クラウドブランチの作成

`magento-cloud` CLI `environment:push` コマンドを使用して、新しい環境を作成し、アクティブにします。 [&#x200B; クラウドブランチの作成 &#x200B;](bitbucket.md#create-a-cloud-branch) を参照してください。

## 統合の削除

`magento-cloud` CLI `integration:delete` コマンドを使用して、統合を削除します。 [&#x200B; 統合の削除 &#x200B;](bitbucket.md#remove-the-integration) を参照してください。
