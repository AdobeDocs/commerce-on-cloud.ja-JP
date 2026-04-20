---
title: プロジェクトをアップグレードするためのベストプラクティス
description: プロジェクトファイルをアップグレードするためのベストプラクティスの一覧を参照してください。
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
source-git-commit: 31494a956babaf15320d0ffa86fcba9e845d53a1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# プロジェクトをアップグレードするためのベストプラクティス

ビルドとデプロイメントに関するベストプラクティスに従い、[ アップグレードとパッチ ](../development/commerce-version.md) ワークフローを使用してアプリケーションをアップグレードします。 アップグレードとアップグレード後の作業を計画するには、次のガイドラインを使用します。

- **プロジェクトのバックアップ**- Adobe Commerceとサードパーティまたはカスタム拡張機能をアップグレードする前に、Integration、Staging、およびProduction環境でデータベースをバックアップしてください。 [ データベースのバックアップ ](../development/commerce-version.md#project-backup)を参照してください。

- **互換性の問題を確認**-

   - カスタムテーマが新しいAdobe Commerce バージョンと互換性があることを確認します

   - サードパーティおよびカスタム拡張機能をアップグレードした後、デプロイする前に`magento-cloud local:build` コマンドを使用してComposerの依存関係を検証し、[互換性のアップグレード ツール ](#use-the-upgrade-compatibility-tool)を実行して、現在のバージョンとターゲットバージョン間のコードレベルの非互換性を特定します。 次に、[ アップグレード互換性ツール ](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool)を使用して、統合、ステージング、実稼動にデプロイする前に、コードレベルの非互換性を特定し、優先順位を付けます。

   - Adobe Commerce リリースノートと拡張機能ドキュメントを確認して、アップグレードされたAdobe Commerceのバージョンと拡張機能に関連する既知の機能上の問題やバグに対処するために必要な回避策や設定変更を実装していることを確認します。

   - インストールされているサービスのバージョンが新しいAdobe Commerceのバージョンと互換性があることを確認し、必要に応じてサービスをアップグレードします。 [ サービス ](../services/services-yaml.md)を参照してください。

   - データベースをテストして、Adobe Commerceのバージョンと拡張機能のアップデートによって発生した問題に対処します。

   - リモート環境にデプロイする前に、必要な環境固有の設定の更新を行います。

   - 検索サービスのバージョンがPHP クライアントバージョンと互換性があることを確認します。 [Elasticsearchの設定](../services/elasticsearch.md)または[OpenSearchの設定](../services/opensearch.md)を参照してください。

- **リモート環境のデータベース接続と利用可能なストレージを確認する**-

   - SSHを使用してリモートサーバーにログインし、MySQL データベースへの接続を確認します。 [ データベースへの接続](../services/mysql.md#connect-to-the-database)を参照してください。

   - リモート環境で使用可能なストレージを確認します。`disk free` コマンドを使用して、クラウド環境で使用可能なディスク領域を表示および管理します。 [ ディスク領域の管理](../storage/manage-disk-space.md)を参照してください。

      - アップグレードされたデータベースのサイズを確認し、`services.yaml` ファイルに十分なディスク領域が割り当てられていることを確認します。

      - ディスク領域を解放します。キャッシュをクリアし、デプロイする前に`/log`および`/tmp` ディレクトリをクリーニングします。

- **ローカル環境と統合環境でアップグレードを計画し、正常に実行します。ステージング環境にデプロイする前に**- アップグレード後に、デプロイメントをテストして問題を解決します。

- **コードをステージングにマージしてから、実稼動環境にマージします** – 実稼動環境に変更をプッシュする前に、ステージング環境の問題をテストして解決します。

- **アップグレード後のタスクを完了**-

   - SSHを使用してリモートサーバーにログインし、次の点を確認します。

      - インデクサーステータスを確認し、必要に応じてインデックスを再作成します。 _設定ガイド_&#x200B;の「[ インデクサーの管理](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html)」を参照してください。

      - Adobe Commerce データベースの`cron` ログと`cron_schedule` テーブルを確認して、cron ステータスを確認し、必要に応じてcron ジョブを再実行します。
_設定ガイド_&#x200B;の「[ ログ ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging)」を参照してください。

   - ステージング環境および実稼動環境でのアップグレード後のユーザー受け入れテスト UATを完了し、サードパーティおよびカスタム拡張機能のアップグレードに関連する問題を修正します。

## 互換性のアップグレード ツールの使用

アップグレード前の分析の一環としてアップグレード互換性ツール（UCT）を実行して、アップグレードの範囲と影響を把握します。

- UCTは、現在のインスタンスを対象のAdobe Commerce バージョンと比較し、アップグレード前に修正する必要がある重大な問題、エラー、警告のリストを返します。
- `--coming-version (-c)`を使用して予定されているターゲットバージョンと比較し、`--ignore-current-version-compatibility-issues`を使用して、アップグレードによって導入された新しい問題のみに焦点を当てます。
- UCT HTML レポートは、拡張機能の互換性、サービス バージョン、データベース チェックと並べて、アップグレード チェックリストへの入力として扱います。

設定と使用の詳細については、次を参照してください。

- [アップグレード互換性ツールの概要](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [互換性のアップグレード ツールの実行](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

Site-Wide Analysis Toolを使用するCloud マーチャントの場合は、ダッシュボードからUCTをトリガーし、ウィジェットから直接HTML レポートをダウンロードすることもできます。 [ サイト全体の分析ツールの統合](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool)を参照してください。
