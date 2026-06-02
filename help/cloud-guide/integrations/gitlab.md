---
title: GitLab統合
description: Adobe Commerce on cloud インフラストラクチャプロジェクトをGitLabと統合する方法について説明します。
feature: Cloud, Integration
exl-id: 24c2156f-0629-4e89-b5b1-ca144d6bfdae
TQID: https://experienceleague.adobe.com/WMHyRgeTet95WG-mQ8gqIxLMY4kwKRUop94MQ3mjtXc
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 791
ht-degree: 0%

---

# GitLab統合

コードの変更をプッシュすると、環境を自動的に構築およびデプロイするようにGitLab リポジトリを設定できます。 この統合により、GitLab リポジトリとAdobe Commerce on cloud infrastructure アカウントが同期されます。

{{private-repository}}

この統合により、次のことが可能になります。

- ブランチの作成時に環境を作成する
- プルリクエストをマージするときに環境を再デプロイする
- ブランチを削除するときに環境を削除する

プロセスを続行するには、GitLab トークンとWebhookを取得する必要があります。

## 前提条件

- Adobe Commerce オンクラウド インフラストラクチャ プロジェクトへの管理者アクセス
- ローカル環境の[`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) ツール
- GitLab アカウント
- GitLab リポジトリへの書き込みアクセスを持つGitLab個人アクセストークン。選択したスコープは`api`および`read_repository`以上である必要があります。

## リポジトリの準備

既存の環境からAdobe Commerce on cloud infrastructure プロジェクトを複製し、同じブランチ名を保持しながら、プロジェクトのブランチを新しい空のGitLab リポジトリに移行します。 同一のGit ツリーを保持することは&#x200B;**重要**&#x200B;であるため、Adobe Commerce on cloud infrastructure プロジェクトの既存の環境やブランチが失われることはありません。

1. ターミナルから、Adobe Commerce on cloud infrastructure プロジェクトにログインします。

   ```bash
   magento-cloud login
   ```

1. プロジェクトのリストを作成し、プロジェクト IDをコピーします。

   ```bash
   magento-cloud project:list
   ```

1. プロジェクトをローカル環境に複製します。

   ```bash
   magento-cloud project:get <project-id>
   ```

1. GitLab リポジトリをリモートとして追加します（GitLabがSaaS バージョンで使用されている場合）。

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   リモート接続の既定の名前は`origin`または`magento`です。 `origin`が存在する場合は、別の名前を選択するか、既存の参照の名前を変更または削除できます。 [git-remote ドキュメント ](https://git-scm.com/docs/git-remote)を参照してください。

1. GitLab リモートが正しく追加されていることを確認します。

   ```bash
   git remote -v
   ```

   期待される応答：

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. プロジェクトファイルを新しいGitLab リポジトリにプッシュします。 すべてのブランチ名を同じにしておくことを忘れないでください。

   ```bash
   git push -u origin master
   ```

   新しいGitLab リポジトリで始める場合は、リモートリポジトリがローカルコピーと一致しないため、`-f` オプションを使用する必要がある場合があります。

1. GitLab リポジトリにすべてのプロジェクトファイルが含まれていることを確認します。

## GitLab統合を有効にする

`magento-cloud integration` コマンドを使用してGitLab統合を有効にし、GitLab Webhookのペイロード URLを取得して、GitLabからAdobe Commerce on cloud infrastructure プロジェクトに更新を送信します。

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| オプション | 説明 |
| ------ | ----------- |
| `<project-ID>` | Adobe Commerce on cloud インフラストラクチャプロジェクト ID |
| `<your-GitLab-token>` | GitLab用に生成した個人アクセストークン |
| `--base-url` | GitLabのURL （`https://gitlab.com/` GitLabがSaaS バージョンで使用されている場合） |
| `--server-project` | GitLabのプロジェクト名（ベース URLの後の部分） |
| `--build-merge-requests` | 結合リクエストごとに新しい環境を構築するようにクラウドインフラストラクチャ上のAdobe Commerceに指示する&#x200B;_オプション_ パラメーター（デフォルトでは`true`） |
| `--merge-requests-clone-parent-data` | マージ要求の親環境のデータを複製するようにクラウドインフラストラクチャ上のAdobe Commerceに指示する&#x200B;_オプション_ パラメーター（デフォルトでは`true`） |
| `--fetch-branches` | クラウド インフラストラクチャ上のAdobe Commerceが、（非アクティブな環境として）リモート （`true`から）すべてのブランチを取得できるようにする&#x200B;_オプション_ パラメーター（デフォルト） |
| `--prune-branches` | クラウド インフラストラクチャ上のAdobe Commerceに、リモート （`true`既定）に存在しないブランチを削除するように指示する&#x200B;_オプション_ パラメーター |

>[!WARNING]
>
>`magento-cloud integration` コマンドは、Adobe Commerce on cloud infrastructure プロジェクトの&#x200B;_all_ コードを、GitLab リポジトリのコードで上書きします。 これには、`production` ブランチを含むすべてのブランチが含まれます。 この操作は瞬時におこなわれ、元に戻すことはできません。 ベストプラクティスとして、GitLab統合を追加する前に、Adobe Commerce on cloud infrastructure プロジェクトからすべてのブランチを複製し、GitLab リポジトリにプッシュすることが重要です。

**GitLab統合を有効にするには**:

1. ターミナルから、GitLab統合をAdobe Commerce on cloud infrastructure プロジェクトに追加します。

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. プロンプトが表示されたら、`y`と入力して統合を追加します。

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. 戻り出力で表示される&#x200B;**フック URL**&#x200B;をコピーします。

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

### GitLabでWebhookを追加する

プッシュリクエストや結合リクエストなどのイベントをCloud Git サーバーと通信するには、GitLab リポジトリ用のWebhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview)を[作成する必要があります

1. GitLab リポジトリで、「**設定**」タブをクリックします。

1. 左側のナビゲーションバーで、**Webhook**&#x200B;をクリックします。

1. _Webhook_ フォームで、次のフィールドを編集します。

   - **URL**: GitLab統合を有効にしたときに返された`Hook URL`を入力します。
   - **秘密鍵**：必要に応じて確認秘密鍵を入力します。
   - **トリガー**: ニーズに応じて`Merge request events`または`Push events`を確認してください。
   - **SSL検証を有効にする**：このオプションを選択する必要があります。

1. 「**Webhookを追加**」をクリックします。

### 統合をテストする

GitLab統合を設定した後、`magento-cloud` CLIを使用して統合が動作していることを確認できます。

```bash
magento-cloud integration:validate
```

GitLab リポジトリに簡単な変更をプッシュしてテストすることもできます。

1. テストファイルを作成します。

   ```bash
   touch test.md
   ```

1. 変更をコミットしてGitLab リポジトリにプッシュします。

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. [[!DNL Cloud Console]](../project/overview.md)にログインし、コミットメッセージが表示され、プロジェクトがデプロイされていることを確認します。

## Cloud ブランチの作成

`magento-cloud` CLI `environment:push` コマンドを使用して、新しい環境を作成し、アクティブ化します。 [ クラウドブランチの作成](bitbucket.md#create-a-cloud-branch)を参照してください。

## 統合の削除

`magento-cloud` CLI `integration:delete` コマンドを使用して、統合を削除します。 [統合の削除](bitbucket.md#remove-the-integration)を参照してください。
