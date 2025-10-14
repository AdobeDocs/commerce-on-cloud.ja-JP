---
title: GitHub 統合
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceを GitHub と統合する方法を説明します。
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# GitHub 統合

GitHub 統合を使用すると、クラウドインフラストラクチャ環境でAdobe Commerceを GitHub リポジトリから直接管理できます。 この統合では、既に GitHub に存在するコンテンツを管理し、クラウドインフラストラクチャコードリポジトリ上のAdobe Commerceと同期します。 基本的に、コードリポジトリは GitHub リポジトリのミラーになります。

{{private-repository}}

この統合により、次のことが可能になります。

- ブランチの作成時に環境を作成します
- プルリクエストを結合する際に環境を再デプロイ
- ブランチを削除したら、環境を削除します

プロセスを続行するには、GitHub トークンと Webhook を取得する必要があります。

## 前提条件

- Adobe Commerce on cloud infrastructure プロジェクトへの管理者アクセス
- GitHub リポジトリ
- GitHub 個人用アクセストークン

## GitHub トークンの生成

GitHub 開発者設定で、従来の個人用アクセストークンを作成します。 リポジトリに _プッシュ_ できるように、GitHub リポジトリへの書き込みアクセス権を持つグループのメンバーである必要があります。 トークンを作成する際には、次の範囲を含めます。

- `admin:repo_hook` - Web フックの作成
- `repo` - リポジトリとの統合
- `read:org` – 組織リポジトリとの統合

詳しくは、「[GitHub：作成 &#x200B;](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)」を参照してください。

## リポジトリを準備

既存の環境からクラウドインフラストラクチャー上のAdobe Commerce プロジェクトのクローンを作成し、同じブランチ名を保持したまま、プロジェクトのブランチを新しい空の GitHub リポジトリに移行します。 Adobe Commerce on cloud infrastructure プロジェクトで既存の環境やブランチが失われないように、同一の Git ツリーを保持することが **重要** です。

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
   magento-cloud project:get <project-ID>
   ```

1. GitHub リポジトリをリモートとして追加します。

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   リモート接続の既定の名前は `origin` または `magento` です。 `origin` が存在する場合は、別の名前を選択するか、既存の参照の名前を変更または削除できます。 [git-remote ドキュメント &#x200B;](https://git-scm.com/docs/git-remote) を参照してください。

1. GitHub リモートが正しく追加されていることを確認します。

   ```bash
   git remote -v
   ```

   期待される応答：

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. プロジェクトファイルを新しい GitHub リポジトリにプッシュします。 すべてのブランチ名は同じにしておくことを忘れないでください。

   ```bash
   git push -u origin master
   ```

   新しい GitHub リポジトリで開始する場合は、リモートリポジトリがローカルコピーと一致しないので、`-f` オプションを使用する必要がある場合があります。

1. GitHub リポジトリにすべてのプロジェクトファイルが含まれていることを確認します。

## GitHub 統合の有効化

開始する前に、プロジェクトコードと環境を GitHub リポジトリに配置する必要があります。 統合を有効にすると、GitHub リポジトリがコードソースになります。 コードの変更を元の `magento` リポジトリにプッシュした場合、コードの変更を GitHub リポジトリにプッシュすると、統合によって上書きされます。

以下は GitHub 統合を有効にし、Webhook の作成時に使用するペイロード URL を指定します。

>[!WARNING]
>
>次のコマンドは、`production` ブランチを含むすべてのブランチを含む GitHub リポジトリのコードで、クラウドインフラストラクチャプロジェクトのAdobe Commerceの _すべて_ コードを上書きします。 このアクションは即座に実行され、元に戻すことはできません。 ベストプラクティスとして、クラウドインフラストラクチャプロジェクト上のAdobe Commerceからすべてのブランチをクローンし、GitHub リポジトリにプッシュすることが重要です **事前に**。

`magento-cloud integration:add` を使用して CLI プロンプトをステップ実行するか、次のオプションを使用して統合コマンドを構築するかを選択できます。

| オプション | 必須？ | 説明 |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | はい | サーバーインストールのベース URL （`https://github.com/` またはカスタム）。 リポジトリがパブリック Github でホストされている場合、またはリポジトリがプライベートサーバーでホストされていない場合は、このオプションを省略します。 リポジトリー URL が `https://github.com/{account}/{repository-name}` に似ている場合は、このオプションを省略します。 これにより、`Unable to connect to GitHub: repository not found` などのエラーが発生する可能性があります。 |
| `--token` | はい | GitHub 用に生成した個人用アクセストークン |
| `--repository` | はい | リポジトリ名：`owner-or-organisation/repository` |
| `--build-pull-requests` | オプション | は、プルリクエストを結合した後にデプロイするように、クラウドインフラストラクチャ上のAdobe Commerceに指示します（デフォルトでは `true`）。 |
| `--fetch-branches` | オプション | ブランチを更新した後、クラウドインフラストラクチャ上のAdobe Commerceでブランチをトラッキングしデプロイします（デフォルトでは `true`）。 |
| `--prune-branches` | オプション | リモートに存在しないブランチを削除します（デフォルトでは `true`）。 |

その他にも様々なオプションがあり、ヘルプ オプションを使用して確認できます。

```bash
magento-cloud integration:add --help
```

**GitHub 統合を有効にするには**:

1. 統合を有効にします。

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **例 1**：個人用プライベートリポジトリに対して GitHub 統合を有効にする：

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **例 2**：組織リポジトリの GitHub 統合を有効にする：

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. プロンプトが表示されたら、必要な情報を入力します。

1. 戻り出力で表示された **ペイロード URL** をコピーします。

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## GitHub での Webhook の追加

プッシュなどのイベントをクラウド Git サーバーと通信するには、GitHub リポジトリの Webhook を作成する必要があります。

1. GitHub リポジトリで、「**設定**」タブをクリックします。

1. 左側のナビゲーションバーで、「**Webhook**」をクリックします。

1. _Webhook_ パネルで、「**Webhook を追加**」をクリックします。

1. _Webhook/Add Webhook_ フォームで、次のフィールドを編集します。

   - **ペイロード URL**:GitHub 統合を有効にした際に返された URL を入力します。
   - **コンテンツタイプ**：リストから **application/json** を選択します。
   - **秘密鍵**：検証の秘密鍵を入力します。
   - **この Webhook をトリガーにするイベントはどれですか？**: **すべて送信** を選択します。
   - 「**アクティブ**」チェックボックスを選択します。

1. **Webhook を追加** をクリックします。

## 統合のテスト

GitHub 統合を設定したら、`magento-cloud` の CLI を使用して、統合が動作していることを確認できます。

```bash
magento-cloud integration:validate
```

または、GitHub リポジトリに簡単な変更をプッシュしてテストできます。

1. テストファイルを作成します。

   ```bash
   touch test.md
   ```

1. 変更をコミットして GitHub リポジトリにプッシュします。

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. [[!DNL Cloud Console]](../project/overview.md) にログインし、コミットメッセージが表示され、プロジェクトがデプロイされていることを確認します。

## 統合の削除

コードに影響を与えることなく、プロジェクトから GitHub 統合を安全に削除できます。

**GitHub 統合を削除するには**:

1. ターミナルから、クラウドインフラストラクチャプロジェクトのAdobe Commerceにログインします。

1. 統合のリストを作成します。 次の手順を完了するには、GitHub 統合 ID が必要です。

   ```bash
   magento-cloud integration:list
   ```

1. 統合を削除します。

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

また、GitHub アカウントにログインし、リポジトリの「_Webhook_」タブで Web フックを削除することで、GitHub の統合を削除できます _設定_。
