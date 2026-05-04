---
title: バックアップ管理
description: Adobe Commerce on cloud infrastructure プロジェクトのバックアップを手動で作成および復元する方法について説明します。
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: 1114b6001bd171bdb41423df697c7b168ae6fe19
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 0%

---

# バックアップ管理

アクティブなStarter環境の手動バックアップは、[!DNL Cloud Console]の&#x200B;**[!UICONTROL Backup]** ボタンまたは`magento-cloud snapshot:create` コマンドを使用して、いつでも実行できます。

バックアップまたは&#x200B;_スナップショット_&#x200B;は、実行中のサービス（MySQL データベース）からのすべての永続的なデータと、マウントされたボリューム（var、pub/media、アプリなど）に保存されているすべてのファイルを含む環境データの完全なバックアップです。 コードは既にGit ベースのリポジトリに保存されているため、スナップショットにはコードが&#x200B;_not_&#x200B;含まれていません。 スナップショットのコピーをダウンロードすることはできません。

>[!WARNING]
>
>通常、バックアップには、`pub/media`などのパブリック web ディレクトリを含むマウントされたディレクトリの内容が含まれますが、バックアップ出力ファイルを`pub/media`や`pub/static`などのパブリック web ディレクトリに移動しないでください。

バックアップ/スナップショット機能は、**not**&#x200B;がPro ステージング環境および実稼動環境に適用されます。これらの環境では、災害復旧のために定期的なバックアップがデフォルトで提供されます。 詳しくは、[Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery)を参照してください。 Pro ステージング環境および実稼動環境での自動ライブバックアップとは異なり、バックアップは&#x200B;**自動**&#x200B;ではありません。 StarterまたはPro統合環境のバックアップを定期的に作成するには、手動でバックアップを作成するか、cron ジョブを設定する必要があります。_お客様の_&#x200B;の責任です。

## 手動バックアップの作成

アクティブなStarter環境とintegration Pro環境の手動バックアップを[!DNL Cloud Console]から作成するか、Cloud CLIからスナップショットを作成できます。 環境には[管理者ロール &#x200B;](../project/user-access.md)が必要です。

>[!NOTE]
>
>ターミナルで次のコマンドを実行して、含める/除外するフォルダー/パスに合わせてコードを調整することで、Pro実稼動クラスターおよびステージングクラスターで直接コードのバックアップを作成できます。
>
>```bash
>mkdir -p var/support
>/usr/bin/nice -n 15 /bin/tar -czhf var/support/code-$(date +"%Y%m%d%H%M%p").tar.gz app bin composer.* dev lib pub/*.php pub/errors setup vendor --exclude='pub/media'
>```

**Pro環境**&#x200B;のデータベースバックアップを作成するには：

ステージングと実稼動を含むPro環境のデータベースダンプを作成するには、[&#x200B; データベースダンプの作成](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud) ナレッジベース記事を参照してください。

**[!DNL Cloud Console]**&#x200B;を使用して任意のStarter環境のバックアップを作成するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。
1. プロジェクトのナビゲーションバーから環境を選択します。 環境はアクティブである必要があります。
1. _バックアップ_ ビューで、**[!UICONTROL Backup]**&#x200B;をクリックします。 このオプションは、Pro環境では使用できません。

   ![&#x200B; バックアップ &#x200B;](../../assets/button-backup.png){width="150"}

**[!DNL Cloud Console]**&#x200B;を使用して統合環境のバックアップを作成するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。
1. プロジェクトのナビゲーションバーから統合/開発環境を選択します。 環境はアクティブである必要があります。
1. 右上のメニューで「**[!UICONTROL Backup]**」オプションを選択します。 このオプションは、スターター環境とPro環境の両方で使用できます。
1. 「**[!UICONTROL Yes]**」ボタンをクリックします。

**`magento-cloud` CLI**&#x200B;を使用してスナップショットを作成するには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。
1. スナップショットに対する環境ブランチを確認します。
1. スナップショットの作成：

   ```bash
   magento-cloud snapshot:create --live
   ```

   または、`magento-cloud backup` short コマンドを使用することもできます。 `--live` オプションを使用すると、ダウンタイムを回避するために環境を実行したままになります。 オプションの完全なリストを表示するには、`magento-cloud snapshot:create --help`と入力します。

   回答サンプル：

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. 最新のスナップショットを確認します。

   ```bash
   magento-cloud snapshot:list
   ```

   このリストは、スナップショットのステータスに関する情報を返します。

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 手動バックアップの復元

環境への[管理者アクセス &#x200B;](../project/user-access.md)が必要です。 手動バックアップは、最大&#x200B;**7日間**～_復元_&#x200B;です。 バックアップを復元しても、現在のGit ブランチのコードは変更されません。 この方法でバックアップを復元することは、Proのステージング環境と実稼動環境には適用されません。[Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery)を参照してください。

復元時間は、データベースのサイズによって異なります。

- 大規模なデータベース（200 GB以上）では5時間かかる場合があります
- 中程度のデータベース（150 GB）では、2/2時間かかることがあります
- 小さなデータベース（60 GB）には1時間かかる場合があります

>[!TIP]
>
>バックアップなしで復元：
>
>- 前のコードにロールバックしたり、環境に追加された拡張機能を削除したりするには、[&#x200B; コードをロールバック &#x200B;](#roll-back-code)するを参照してください。
>- _not_&#x200B;にバックアップがある不安定な環境を復元するには、[環境の復元](../development/restore-environment.md)を参照してください。

**[!DNL Cloud Console]**&#x200B;を使用してバックアップを復元するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)にログインします。
1. プロジェクトのナビゲーションバーから環境を選択します。
1. _バックアップ_ ビューで、_保存_ リストからバックアップを選択します。 バックアップ機能は&#x200B;**not**&#x200B;がPro環境に適用されます。
1. ![詳細](../../assets/icon-more.png){width="32"} （_詳細_）メニューで、**復元**&#x200B;をクリックします。
1. バックアップから復元の情報を確認し、**はい、復元**&#x200B;をクリックします。

**Cloud CLI**&#x200B;を使用してスナップショットを復元するには：

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。
1. 復元する環境ブランチを確認してください。
1. 使用可能なすべてのスナップショットを一覧表示します。

   ```bash
   magento-cloud snapshot:list
   ```

   リストは、使用可能なスナップショットに関する情報を返します。

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. リストからスナップショット IDを使用してスナップショットを復元します。

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## 災害復旧スナップショットの復元

Pro ステージング環境および実稼動環境で災害復旧スナップショットを復元するには、[&#x200B; データベース ダンプをサーバーから直接インポートします](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3)。

## コードをロールバック

バックアップとスナップショットには、コードのコピーが&#x200B;_not_&#x200B;含まれています。 コードは既にGit ベースのリポジトリに保存されているため、Git ベースのコマンドを使用してコードをロールバック（または元に戻す）できます。 例えば、`git log --oneline`を使用して以前のコミットをスクロールし、[`git revert`](https://git-scm.com/docs/git-revert)を使用して特定のコミットからコードを復元します。

また、_非アクティブ_ ブランチにコードを保存することもできます。 `magento-cloud` コマンドを使用する代わりに、Git コマンドを使用してブランチを作成します。 Cloud CLI トピックの[Git コマンド &#x200B;](../dev-tools/cloud-cli-overview.md#git-commands)について参照してください。

## 関連情報

- [データベースのバックアップ](database-dump.md)
- Pro実稼動クラスターとステージング クラスターの[&#x200B; バックアップと災害復旧](../architecture/pro-architecture.md#backup-and-disaster-recovery)
