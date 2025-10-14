---
title: プロジェクトのアップグレードのベストプラクティス
description: プロジェクト ファイルのアップグレードに関するベスト プラクティスの一覧を参照してください。
feature: Cloud, Best Practices, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# プロジェクトのアップグレードのベストプラクティス

ビルドおよびデプロイメントのベストプラクティスに従い、[&#x200B; アップグレードとパッチ &#x200B;](../development/commerce-version.md) ワークフローを使用してアプリケーションをアップグレードします。 次のガイドラインに従って、アップグレードとアップグレード後の作業を計画します。

- **プロジェクトをバックアップする** - Adobe Commerceおよびサードパーティ製またはカスタムのエクステンションをアップグレードする前に、統合環境、ステージング環境および実稼動環境でデータベースをバックアップします。 [&#x200B; データベースのバックアップ &#x200B;](../development/commerce-version.md#project-backup) を参照してください。

- **互換性の問題の確認**-

   - カスタムテーマが新しいAdobe Commerce バージョンと互換性があることを確認します

   - サードパーティおよびカスタム拡張機能をアップグレードした後、`magento-cloud local:build` コマンドを使用して、デプロイ前に Composer の依存関係を検証します。

   - Adobe Commerceのリリースノートと拡張機能のドキュメントを参照して、アップグレードされたAdobe Commerce バージョンおよび拡張機能に関連する既知の機能問題やバグに対処するために必要な回避策や設定変更が実装されていることを確認します。

   - インストールされているサービスバージョンが新しいAdobe Commerce バージョンと互換性があることを確認し、必要に応じてサービスをアップグレードします。 [&#x200B; サービス &#x200B;](../services/services-yaml.md) を参照してください。

   - データベースをテストして、Adobe Commerceのバージョンと拡張機能の更新で発生した問題に対処します。

   - リモート環境にデプロイする前に、環境固有の設定に対して必要な更新を行います。

   - 検索サービスのバージョンが、PHP クライアントのバージョンと互換性があることを確認します。 [Elasticsearchの設定 &#x200B;](../services/elasticsearch.md) または [OpenSearch の設定 &#x200B;](../services/opensearch.md) を参照してください。

- **リモート環境のデータベース接続と使用可能なストレージの確認**-

   - SSH を使用してリモートサーバーにログインし、MySQL データベースへの接続を確認します。 [&#x200B; データベースへの接続 &#x200B;](../services/mysql.md#connect-to-the-database) を参照してください。

   - リモート環境で使用可能なストレージを確認 – `disk free` コマンドを使用して、クラウド環境で使用可能なディスク領域を表示および管理します。 [&#x200B; ディスク容量の管理 &#x200B;](../storage/manage-disk-space.md) を参照してください。

      - アップグレードしたデータベースのサイズを確認し、`services.yaml` ファイルに十分なディスク領域が割り当てられていることを確認します。

      - ディスク領域を解放する – キャッシュをクリアし、`/log` および `/tmp` ディレクトリをデプロイ前にクリーンアップします。

- **ステージング環境にデプロイする前に、ローカル環境と統合環境で正常なアップグレードを計画し実行します**- アップグレード後に、デプロイメントをテストし、問題を解決します。

- **コードをステージング環境に、次に実稼動環境に結合** – 変更を実稼動環境にプッシュする前に、ステージング環境の問題をテストして解決します。

- **アップグレード後のタスクの完了**-

   - SSH を使用してリモートサーバーにログインし、以下を確認します。

      - インデクサーのステータスを確認し、必要に応じてインデックスを再作成します。 _設定ガイド_ の [&#x200B; インデクサーの管理 &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=ja) を参照してください。

      - Adobe Commerce データベースの `cron` ログと `cron_schedule` テーブルを確認して cron ステータスを確認し、必要に応じて cron ジョブを再実行します。
[&#x200B; 設定ガイド &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=ja#logging) の _ログ_ を参照してください。

   - ステージング環境および実稼動環境でアップグレード後のユーザー受け入れテスト UAT を完了し、サードパーティおよびカスタム拡張機能のアップグレードに関連する問題を修正します。
