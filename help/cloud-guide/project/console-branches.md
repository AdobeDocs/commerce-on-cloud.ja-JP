---
title: ' [!DNL Cloud Console]で分岐を管理'
description: ' [!DNL Cloud Console]を使用してAdobe Commerce on cloud infrastructureの環境ブランチを管理する方法について説明します。'
role: Developer
feature: Cloud, Install
exl-id: 2c254586-b670-4dd7-8f82-edcc139e9800
TQID: https://experienceleague.adobe.com/-9EfBaTgSBPQa6HspiaqngBtwURAeUGlNP9hREcXrQQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1638
ht-degree: 0%

---

# [!DNL Cloud Console]で分岐を管理

環境は、[!DNL Cloud Console]または`magento-cloud` CLIのいずれかを使用して管理できます。 プロジェクトファイルはGit リポジトリに保存されます。 Git コマンドを使用してコードを管理できますが、`magento-cloud` CLIはプラットフォーム機能を操作するように設計されていますが、Git コマンドは操作できません。 Cloud CLI トピックの[Git コマンド &#x200B;](../dev-tools/cloud-cli-overview.md#git-commands)を参照してください。

このトピックでは、[!DNL Cloud Console]を使用して次の操作を行う方法について説明します。

- 環境の追加または削除
- 親環境から同期（`git pull`）
- 親環境に（`git push`）を結合

>[!TIP]
>
>Pro ステージング環境と実稼動環境からブランチを作成することはできません。 分岐は`master` ブランチから行うことができます。

## 環境の作成

分岐戦略では、コードを開発し、開発ブランチに拡張機能を追加する共通のGit ワークフローを使用します。 [Starter](../architecture/starter-architecture.md)および[Pro](../architecture/starter-develop-deploy-workflow.md) アーキテクチャの概要を参照してください。

- Starterの場合は、`master` ブランチから`staging` ブランチを作成し、開発用に`staging`からブランチを作成します。
- Proの場合は、`Integration`環境から開発ブランチを作成します。

お客様のアカウントでは、限られた数の![&#x200B; アクティブなブランチ &#x200B;](../../assets/icon-active.png){width="32"} （アクティブ）と無制限の数の![非アクティブなブランチ &#x200B;](../../assets/icon-inactive.png){width="32"} （非アクティブ）開発ブランチをサポートしています。 [!DNL Cloud Console]またはCloud CLIのみを使用してブランチを追加または削除することで、アクティブなブランチと非アクティブなブランチを管理します。 ブランチを削除する前に、ブランチを非アクティブ化します。このブランチは、_環境_ リストに&#x200B;_非アクティブ_&#x200B;として残ります。 後でブランチを再アクティブ化するか、環境設定またはCloud CLIを使用して[&#x200B; ブランチ &#x200B;](../dev-tools/cloud-cli-overview.md#)を削除できます。

開発用に追加のアクティブ環境が必要な場合は、[&#x200B; サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信してください。

**ブランチを追加するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境を選択します。

   >[!TIP]
   >
   >新しいブランチがこの環境から複製されます。 作成する環境に類似した親環境を選択します。

1. **[!UICONTROL Branch]**&#x200B;をクリックします。

   ![&#x200B; ブランチを作成](../../assets/button-branch.png){width="150"}

1. 「_分岐から…_」フォームに、分岐名を入力します。

   環境&#x200B;_name_&#x200B;が環境&#x200B;_ID_&#x200B;と異なるのは、環境名にスペースまたは大文字を使用する場合のみです。 環境IDは、すべての小文字、数字、許可された記号で構成されます。 環境名の大文字はIDの小文字に変換され、環境名のスペースはダッシュに変換されます。

   環境名&#x200B;**には、Linux シェルまたは正規表現用に予約された文字を含めることはできません**。 禁止されている文字には、中括弧（`{ }`）、括弧、アスタリスク （`*`）、角括弧（`>`）、アンパサンド （`&`）、パーセント （<code>%が含まれます</code>）およびその他の文字。

1. **[!UICONTROL Environment type]**&#x200B;を選択します。

1. **[!UICONTROL Create Branch]**&#x200B;をクリックします。

1. 環境がデプロイされるまでお待ちください。

   デプロイメント中、環境のステータスは&#x200B;**処理中**&#x200B;です。 デプロイメントが成功すると、ステータスは&#x200B;**成功**&#x200B;の緑色のチェックマークに変わります。

## 非アクティブブランチを作成

Adobe Commerce Cloud コンソールまたはCLIから非アクティブなブランチを作成することはできません。 非アクティブなブランチを作成する場合は、Git リポジトリでブランチを作成し、コマンドの`environment.Parent` オプションを使用してプッシュします。

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## 環境の削除

環境を削除する前に、その環境を非アクティブ化する必要があります。 環境を非アクティブにすると、削除できます。

**環境を非アクティブ化するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. ナビゲーションバー&#x200B;_環境_ リストから環境を選択します。

1. 上部のナビゲーションバーの右側にある設定アイコンをクリックすると、環境設定が開きます。

1. 「_[!UICONTROL General]_」タブで、_[!UICONTROL Deactivate environment]_ セクションまでスクロールして「**[!UICONTROL Deactivate environment and delete data]**」をクリックし、指示に従います。

## 環境の同期

環境（またはブランチ）の同期は`git pull origin <parent>`と同じです。 更新されたコードは、親環境から同期できます。 この機能は、[!DNL Cloud Console]を通じてすべてのStarterおよびPro環境で使用できます。

Pro プランの場合、ステージングおよび実稼動から`master` ブランチに同期できます。 この同期では、データではなくコードの取得とプッシュのみが行われます。 データを同期するには、データベースデータをダンプして、別の環境のデータベースにプッシュします。 [静的ファイルとデータの移行とデプロイ &#x200B;](/help/cloud-guide/deploy/staging-production.md#migrate-static-files)を参照してください。

**環境を同期するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境リストで、同期するブランチの名前をクリックします。

1. クリック（同期）。

   ![環境を同期](../../assets/button-sync.png){width="150"}

1. 同期する項目を選択します。

   - データを置き換える – （データとファイル）は、データベースの変更と親ブランチのコンテンツファイルを同期します。
   - 結合 – （コード）は、更新されたコードを親ブランチから同期します。

   また、コピーして使用するためのCLI コマンドも作成されます。

1. **同期**&#x200B;をクリックします。

## 親環境と結合

環境（またはブランチ）の結合は`git push origin`と同じです。 更新されたコードを環境からその親環境にプッシュするには、マージします。 このコードを`master`に結合できます。 `merge` コマンドを使用して、ステージングおよび実稼動にデプロイできます。

**親環境と結合するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境リストで、結合するブランチの名前をクリックします。

1. クリック（結合）します。

   ![環境を結合](../../assets/button-merge.png){width="150"}

1. 「**結合**」をクリックして、アクションを確認します。

## ログを表示

[!DNL Cloud Console]を通じて、ビルド、デプロイ、デプロイメントの履歴など、環境に関する様々なログを確認できます。

**Starter**&#x200B;の場合、ビルドとデプロイのログとデプロイメント履歴を確認できます。 これらの環境には、`master` （実稼動環境）分岐とそれから作成されたすべての分岐が含まれます。

**Pro**&#x200B;の場合、各環境で次のログを確認できます。

- 統合：ビルドおよびデプロイとデプロイの履歴
- ステージング – ログとデプロイメント履歴をビルドします。 SSHを使用してサーバーにログインし、デプロイログを表示します。
- 実稼動 – ログとデプロイメント履歴を作成します。 SSHを使用してサーバーにログインし、デプロイログを表示します。

**ログを[!DNL Cloud Console]**&#x200B;で表示するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境を選択します。

   環境ビューには[&#x200B; アクティビティリスト &#x200B;](activity-stream.md)が表示され、_最近_&#x200B;件のイベント、同期、結合、分岐、バックアップなどのアクションごとに1つのエントリが表示されます。 完全なデプロイメント履歴については、**すべて**&#x200B;をクリックしてください。

1. ビルドログを表示するには、アカウントのデプロイメントレコードごとに「成功」または「失敗」リンクを選択します。

>[!TIP]
>
>ドロップダウンリストの「**フィルター**」アイコンをクリックし、表示するメッセージのタイプを選択します。

## プライベート Git リポジトリからコードを取得する

Adobe Commerce on cloud インフラストラクチャプロジェクトには、プライベート Git リポジトリのコードを含めることができます。 例えば、カスタムモジュールやテーマのコードをプライベートリポジトリに含めることができます。 これを行うには、プロジェクトの公開SSH キーをプライベート Git リポジトリに追加し、プロジェクト `composer.json` ファイルを更新する必要があります。

プライベート GitHub リポジトリにデプロイメントキーを追加するには、そのリポジトリの管理者である必要があります。 GitHubでは、デプロイキーを1つのリポジトリにのみ使用できます。

プロジェクトが複数のリポジトリにアクセスすることを希望する場合は、SSH キーを自動ユーザーアカウントに添付できます。 このアカウントは人間が使用していないため、[&#x200B; マシンユーザー](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)と呼ばれます。 マシンアカウントを共同作業者として追加するか、リポジトリにアクセスできるチームにマシンユーザーを追加します。

>[!INFO]
>
>Adobeでは、このコードをプロジェクト Git リポジトリに追加して結合することをお勧めします。 接続を設定しないと、ビルドの問題が発生する可能性があります。

**SSH公開鍵を検索するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 上部のナビゲーションバーの右側にある設定アイコンをクリックします。

1. _プロジェクト設定_&#x200B;で、**[!UICONTROL Deploy Key]**&#x200B;をクリックします。

1. デプロイキーをクリップボードにコピーして、次のいずれかのGit ベースの方法で使用します。

>[!BEGINTABS]

>[!TAB GitHub]

### GitHub デプロイキーを入力します

GitHubでは、デプロイキーはデフォルトで読み取り専用です。

**プロジェクトの公開鍵をGitHub デプロイキーとして入力するには**:

1. GitHub リポジトリに管理者としてログインします。
1. リポジトリ **[!UICONTROL Settings]** タブをクリックします。

   >[!NOTE]
   >
   >このオプションが表示されない場合は、リポジトリ管理者としてログインしておらず、このタスクを完了できません。 GitHub リポジトリ管理者に依頼します。

1. 左側のナビゲーションの「_設定_」タブで、**[!UICONTROL Deploy Keys]**&#x200B;をクリックします。
1. **[!UICONTROL Add deploy key]**&#x200B;をクリックします。
1. プロンプトに従います。

`composer.json`では、`<user>@<host>:<.git</code>`形式、または非標準ポートを使用している場合は`ssh://<user>@<host>:<port>/<path>.git`を使用します。

>[!TAB Bitbucket]

### Bitbucket デプロイ キーを入力します

**プロジェクトの公開鍵をBitbucket デプロイ キーとして入力するには**:

1. 管理者としてBitbucket リポジトリにログインします。
1. 左側のナビゲーションで、**[!UICONTROL Settings]**&#x200B;をクリックします。
1. 一般/ **[!UICONTROL Deployment Keys]**&#x200B;をクリックします。
1. **[!UICONTROL Add Key]**&#x200B;をクリックします。
1. プロンプトに従います。

>[!TAB GitLab]

### GitLab デプロイキーを入力します

**プロジェクトの公開SSH キーをGitLab デプロイキーとして追加するには**:

1. GitLab リポジトリに所有者としてログインします。
1. _パイプライン_ オプションがプロジェクトに対して有効になっていることを確認します。

   1. プロジェクト設定で、**[!UICONTROL Visibility, project, features, permissions]** セクションを展開します。
   1. 必要に応じて、**[!UICONTROL Pipelines]**&#x200B;をクリックしてオプションを有効にします。

1. 公開SSH キーをCI/CD設定に追加します。

   1. 左側のナビゲーションで、設定/ **[!UICONTROL CI / CD]**&#x200B;をクリックします。
   1. 「キーをデプロイ **展開**」をクリックして、キーを設定します。
   1. 「_キーをデプロイ_」フォームで、**[!UICONTROL Title]** フィールドにデプロイキー名を追加し、公開SSH キーを&#x200B;**[!UICONTROL Key]** フィールドに貼り付けます。
   1. **[!UICONTROL Add Key]**&#x200B;をクリックして設定を保存します。

>[!ENDTABS]

## セキュアな環境とブランチ

[!DNL Cloud Console]を使用して、Web ブラウザーを介して任意の場所からプロジェクトと環境にアクセスできます。 実稼動環境、ストア、サイトに対するセキュリティが設定されている場合があります。 このセクションでは、厳密に開発者、DBAなどに対して、統合環境とステージング環境を保護するのに役立ちます。

>[!WARNING]
>
>**次の方法を使用してPro ステージング環境と実稼動環境を保護しないでください。**&#x200B;これにより、Fastlyのキャッシュが破損します。 Fastly Adobe Commerce用CDNで利用可能な[&#x200B; ブロッキング &#x200B;](../cdn/fastly-vcl-blocking.md)機能を使用します。

**環境を保護するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。

1. _すべてのプロジェクト_ リストからプロジェクトを選択します。

1. 環境を選択し、ナビゲーションバーの設定アイコンをクリックします。

1. 環境設定&#x200B;_一般_ タブで、**[!UICONTROL HTTP access control enabled]**&#x200B;の&#x200B;**オン**&#x200B;をクリックして、安全なアクセスを有効にします。 アクセスをフィルタリングする資格情報またはIP アドレスを選択できます。

1. 資格情報でフィルタリングするには、**[!UICONTROL Add Login]**&#x200B;をクリックし、ユーザー名とパスワードを入力し、**[!UICONTROL Add Login]**&#x200B;をクリックして追加します。

1. IP アドレスでフィルタリングするには、`deny`または`allow`のリストにIP アドレスを入力します。 例：

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. **[!UICONTROL Save]**&#x200B;をクリックします。 これにより、環境が再配置され、セキュリティと設定が更新されます。 Adobeでは、セキュリティ設定を完了した後に環境をテストすることをお勧めします。
