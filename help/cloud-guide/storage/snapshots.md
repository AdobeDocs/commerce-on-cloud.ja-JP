---
title: バックアップ管理
description: クラウドインフラストラクチャプロジェクト上のAdobe Commerceのバックアップを手動で作成および復元する方法について説明します。
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: b9bbbb9b83ed995951feaa9391015f02a9661206
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---

# バックアップ管理

アクティブなスターター環境の手動バックアップは、[!DNL Cloud Console] の [**[!UICONTROL Backup]**] ボタンを使用するか、`magento-cloud snapshot:create` コマンドを使用して、いつでも実行できます。

バックアップまたは _スナップショット_ は、実行中のサービス（MySQL データベース）のすべての永続的なデータと、マウントされたボリューム（var、pub/media、app/etc）に保存されたすべてのファイルを含む、環境データの完全なバックアップです。 コードは Git ベースのリポジトリに既に保存されているので、スナップショットにはコードが含まれていま _ん_。 スナップショットのコピーはダウンロードできません。

>[!WARNING]
>
>バックアップには通常、マウントされたディレクトリ（`pub/media` などのパブリック web ディレクトリを含む）の内容が含まれていますが、バックアップ出力ファイルを `pub/media` や `pub/static` などのパブリック web ディレクトリに移動しないでください。

バックアップ/スナップショット機能は、ステージング環境および実稼動環境には適用 **されません**。これらの環境は、デフォルトで災害復旧用の通常のバックアップを受け取ります。 詳細は、「[Pro バックアップと障害回復 ](../architecture/pro-architecture.md#backup-and-disaster-recovery)」を参照してください。 ステージング環境および実稼動環境での自動ライブバックアップとは異なり、バックアップは自動 **ではありません**。 バックアップを手動で作成する _、または Starter または Pro の統合環境のバックアップを定期的に作成する cron ジョブをセットアップするのは_ ユーザーの責任です。

## 手動バックアップの作成

アクティブなスターター環境と Integration Pro 環境の手動バックアップを [!DNL Cloud Console] から作成するか、Cloud CLI からスナップショットを作成できます。 環境の [ 管理者の役割 ](../project/user-access.md) が必要です。

**Pro 環境のデータベースバックアップを作成するには**:
ステージングや実稼働を含む Pro 環境のデータベースダンプを作成するには、ナレッジベースの記事 [ データベースダンプの作成 ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud) を参照してください。

**[!DNL Cloud Console]** を使用してスターター環境のバックアップを作成するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。
1. プロジェクトナビゲーションバーから環境を選択します。 環境がアクティブである必要があります。
1. _バックアップ_ ビューで、「**[!UICONTROL Backup]**」をクリックします。 このオプションは、Pro 環境では使用できません。

   ![ バックアップ ](../../assets/button-backup.png){width="150"}

**[!DNL Cloud Console]** を使用して統合環境のバックアップを作成するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。
1. プロジェクトナビゲーションバーから統合/開発環境を選択します。 環境がアクティブである必要があります。
1. 右上のメニューで「**[!UICONTROL Backup]**」オプションを選択します。 このオプションは、Starter 環境と Pro 環境の両方で使用できます。
1. **[!UICONTROL Yes]** ボタンをクリックします。

**`magento-cloud` CLI を使用してスナップショットを作成するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。
1. スナップショットを作成する環境ブランチをチェックアウトします。
1. スナップショットを作成します。

   ```bash
   magento-cloud snapshot:create --live
   ```

   または、`magento-cloud backup` の短いコマンドを使用することもできます。 `--live` オプションを指定すると、ダウンタイムを避けるために環境は実行されたままになります。 オプションの完全なリストを表示するには、`magento-cloud snapshot:create --help` と入力します。

   応答の例：

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. 最新のスナップショットを検証します。

   ```bash
   magento-cloud snapshot:list
   ```

   リストは、スナップショットのステータスに関する情報を返します。

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 手動バックアップの復元

環境に対する [ 管理者アクセス ](../project/user-access.md) が必要です。 手動バックアップを **リストアする** には、最大で _7 日間_ かかります。 バックアップを復元しても、現在の Git ブランチのコードは変更されません。 この方法でのバックアップの復元は、Pro ステージング環境および実稼動環境には適用されません。[Pro バックアップおよび障害回復 ](../architecture/pro-architecture.md#backup-and-disaster-recovery) を参照してください。

復元時間は、データベースのサイズによって異なります。

- 大規模なデータベース（200 GB 以上）では 5 時間かかる場合がある
- 中程度のデータベース（150 GB）で 2 1/2 時間かかる場合がある
- 小規模なデータベース（60 GB）では 1 時間かかる場合がある

>[!TIP]
>
>バックアップなしでリストアする：
>
>- 以前のコードにロールバックしたり、環境で追加した拡張機能を削除したりするには、[ コードのロールバック ](#roll-back-code) を参照してください。
>- バックアップがある _ない_ 不安定な環境を復元するには、[ 環境の復元 ](../development/restore-environment.md) を参照してください。

**[!DNL Cloud Console]** を使用してバックアップを復元するには：

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) にログインします。
1. プロジェクトナビゲーションバーから環境を選択します。
1. _バックアップ_ ビューで、「保存 _リストからバックアップを選択_ ます。 バックアップ機能は Pro 環境には適用 **されません**。
1. ![ その他 ](../../assets/icon-more.png){width="32"} （_その他_）メニューで、「**復元**」をクリックします。
1. バックアップからのリストア情報を確認し、[ はい、リストア **をクリックします**。

**Cloud CLI を使用してスナップショットを復元するには**:

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。
1. 復元する環境ブランチをチェックアウトします。
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

1. リストのスナップショット ID を使用してスナップショットを復元します。

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## 障害回復スナップショットの復元

ステージング環境および実稼動環境で障害回復スナップショットを復元するには、[ データベースダンプをサーバーから直接読み込みます ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3)。

## コードをロールバック

バックアップとスナップショットには、コードのコピーは含まれません __。 コードは既に Git ベースのリポジトリに保存されているので、Git ベースのコマンドを使用してコードをロールバック（または元に戻す）できます。 例えば、以前のコミットをスクロールするには `git log --oneline` を使用し、特定のコミットからコードを復元するには [`git revert`](https://git-scm.com/docs/git-revert) を使用します。

また、コードを _非アクティブ_ ブランチに保存することもできます。 `magento-cloud` のコマンドを使用する代わりに、Git コマンドを使用してブランチを作成します。 Cloud CLI トピックの [Git コマンド ](../dev-tools/cloud-cli-overview.md#git-commands) についてを参照してください。
