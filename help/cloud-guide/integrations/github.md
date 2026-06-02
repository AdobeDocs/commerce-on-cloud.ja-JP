---
title: GitHubとの統合
description: Adobe Commerce on cloud インフラストラクチャプロジェクトをGitHubと統合する方法について説明します。
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00.000Z
exl-id: 08e569fa-5ab4-45c0-82e6-476f25c17fe0
TQID: https://experienceleague.adobe.com/7Lt2uWkU1kD8VoZAX--lGcJawdg3VEy9U39IS45dBvY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 972
ht-degree: 0%

---

# GitHubとの統合

GitHubとの統合により、Adobe Commerce on cloud infrastructure環境をGitHub リポジトリから直接管理できます。 この統合により、既にGitHubにあるコンテンツが管理され、Adobe Commerce on cloud infrastructureのコードリポジトリと同期されます。 本質的には、コードリポジトリはGitHub リポジトリのミラーになります。

{{private-repository}}

この統合により、次のことが可能になります。

- ブランチの作成時に環境を作成する
- プルリクエストをマージするときに環境を再デプロイする
- ブランチを削除するときに環境を削除する

プロセスを続行するには、GitHub トークンとWebhookを取得する必要があります。

## 前提条件

- Adobe Commerce オンクラウド インフラストラクチャ プロジェクトへの管理者アクセス
- GitHub リポジトリ
- GitHub個人アクセストークン

## GitHub トークンの生成

GitHub開発者設定で、従来の個人アクセストークンを作成します。 リポジトリに&#x200B;_プッシュ_&#x200B;できるように、GitHub リポジトリへの書き込みアクセス権を持つグループのメンバーである必要があります。 トークンを作成する際には、次のスコープを含めます。

- `admin:repo_hook` - web フックの作成
- `repo` – リポジトリとの統合
- `read:org` – 組織リポジトリとの統合

[GitHub: Create](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)を参照してください。

## リポジトリの準備

既存の環境からAdobe Commerce on cloud infrastructure プロジェクトを複製し、同じブランチ名を保持しながら、プロジェクトのブランチを新しい空のGitHub リポジトリに移行します。 同一のGit ツリーを保持することは&#x200B;**重要**&#x200B;であるため、Adobe Commerce on cloud infrastructure プロジェクトの既存の環境やブランチが失われることはありません。

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
   magento-cloud project:get <project-ID>
   ```

1. GitHub リポジトリをリモートとして追加します。

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   リモート接続の既定の名前は`origin`または`magento`です。 `origin`が存在する場合は、別の名前を選択するか、既存の参照の名前を変更または削除できます。 [git-remote ドキュメント &#x200B;](https://git-scm.com/docs/git-remote)を参照してください。

1. GitHub リモートが正しく追加されていることを確認します。

   ```bash
   git remote -v
   ```

   期待される応答：

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. プロジェクトファイルを新しいGitHub リポジトリにプッシュします。 すべてのブランチ名を同じにしておくことを忘れないでください。

   ```bash
   git push -u origin master
   ```

   新しいGitHub リポジトリで始める場合は、リモートリポジトリがローカルコピーと一致しないため、`-f` オプションを使用する必要がある場合があります。

1. GitHub リポジトリにすべてのプロジェクトファイルが含まれていることを確認します。

## GitHub統合を有効にする

開始する前に、プロジェクトコードと環境がGitHub リポジトリにある必要があります。 統合を有効にすると、GitHub リポジトリがコードソースになります。 コードの変更を元の`magento` リポジトリにプッシュすると、コードの変更をGitHub リポジトリにプッシュすると、統合によって上書きされます。

次に、GitHub統合を有効にし、Webhookの作成時に使用するペイロード URLを提供します。

>[!WARNING]
>
>次のコマンドは、`production` ブランチを含むすべてのブランチを含むGitHub リポジトリのコードで、Adobe Commerce on cloud infrastructure プロジェクトの&#x200B;_all_ コードを上書きします。 この操作は瞬時におこなわれ、元に戻すことはできません。 ベストプラクティスとして、GitHub統合を追加する前に、Adobe Commerce on cloud infrastructure プロジェクトからすべてのブランチを複製し、GitHub リポジトリ **にプッシュすることが重要です。**

`magento-cloud integration:add`を使用してCLI プロンプトをステップ実行するか、次のオプションを使用して統合コマンドを構築するかを選択できます。

| オプション | 必要ですか？ | 説明 |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | はい | サーバーのインストールのベース URLです。`https://github.com/`またはカスタムの場合があります。 リポジトリがパブリック Githubでホストされている場合、またはリポジトリがプライベートサーバーでホストされていない場合は、このオプションを省略します。 リポジトリ URLが`https://github.com/{account}/{repository-name}`と似ている場合は、このオプションを省略します。 これにより、`Unable to connect to GitHub: repository not found`などのエラーが発生する可能性があります。 |
| `--token` | はい | GitHub用に生成した個人アクセストークン |
| `--repository` | はい | リポジトリ名：`owner-or-organisation/repository` |
| `--build-pull-requests` | オプション | プルリクエストをマージした後に、クラウドインフラストラクチャ上のAdobe Commerceにデプロイを指示します（デフォルトは`true`） |
| `--fetch-branches` | オプション | ブランチを更新した後、クラウドインフラストラクチャ上のAdobe Commerceでブランチを追跡してデプロイする（デフォルトでは`true`） |
| `--prune-branches` | オプション | リモートに存在しない分岐を削除します（`true` デフォルト） |

さらに多くのオプションがあり、ヘルプオプションを使用して確認できます。

```bash
magento-cloud integration:add --help
```

**GitHub統合を有効にするには**:

1. 統合を有効にします。

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **例1**：個人のプライベートリポジトリに対してGitHub統合を有効にする：

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **例2**：組織リポジトリのGitHub統合を有効にします。

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. プロンプトが表示されたら、必要な情報を入力します。

1. 戻り出力で表示される&#x200B;**ペイロード URL**&#x200B;をコピーします。

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## GitHubにWebhookを追加する

プッシュなどのイベントをCloud Git サーバーと通信するには、GitHub リポジトリ用のWebhookを作成する必要があります。

1. GitHub リポジトリで、「**設定**」タブをクリックします。

1. 左側のナビゲーションバーで、**Webhook**&#x200B;をクリックします。

1. _Webhook_ ペインで、「**Webhookを追加**」をクリックします。

1. _Webhook/Add webhook_ フォームで、次のフィールドを編集します。

   - **ペイロード URL**: GitHub統合を有効にしたときに返されたURLを入力します。
   - **コンテンツの種類**: リストから&#x200B;**application/json**&#x200B;を選択します。
   - **秘密鍵**：確認の秘密鍵を入力します。
   - **このWebhookをトリガーするイベントはどれですか？**:「**すべてを送信**」を選択します。
   - 「**アクティブ**」チェックボックスを選択します。

1. 「**Webhookを追加**」をクリックします。

## 統合をテストする

GitHub統合を設定した後、`magento-cloud` CLIを使用して統合が動作していることを確認できます。

```bash
magento-cloud integration:validate
```

GitHub リポジトリに簡単な変更をプッシュしてテストすることもできます。

1. テストファイルを作成します。

   ```bash
   touch test.md
   ```

1. 変更をコミットし、GitHub リポジトリにプッシュします。

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. [[!DNL Cloud Console]](../project/overview.md)にログインし、コミットメッセージが表示され、プロジェクトがデプロイされていることを確認します。

## 統合の削除

コードに影響を与えることなく、GitHub統合をプロジェクトから安全に削除できます。

**GitHub統合を削除するには**:

1. ターミナルから、Adobe Commerce on cloud infrastructure プロジェクトにログインします。

1. 統合機能のリストアップ。 次のステップを完了するには、GitHub統合IDが必要です。

   ```bash
   magento-cloud integration:list
   ```

1. 統合を削除します。

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

また、GitHub アカウントにログインし、リポジトリ _設定_&#x200B;の「_Webhook_」タブにあるweb フックを削除することで、GitHub統合を削除できます。
