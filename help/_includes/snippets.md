---
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# クラウドスニペット

## Elasticsearch警告 {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11 以降は、クラウドインフラストラクチャー上のAdobe Commerceではサポートされていません。 Adobe Commerce バージョン 2.3.7 ～ p3、2.4.3 ～ p2、および 2.4.4 以降では、OpenSearch サービスをサポートしています。 オンプレミスのインストールでは、引き続きElasticsearchがサポートされます。

## 統合の強化 {#enhanced-integration-envs}

>[!NOTE]
>
>2020 年 6 月 5 日（PT）より前にプロビジョニングされたプロジェクトには、複数の小規模な統合環境がありました。 テストおよび開発に大規模な統合環境が必要な場合は、拡張統合環境へのアップグレードをリクエストします。 詳しくは、_Adobe Commerce ヘルプセンター [&#128279;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) の Integration Environment リクエスト_ を参照してください。

## 結合オプション {#merge-options}

デフォルトでは、デプロイメントプロセスは `env.php` ファイルのすべての設定を上書きします。ただし、すべての値を上書きせずに、サービス設定の 1 つ以上の値を結合することを選択できます。

「`_merge`」オプションを次のいずれかに設定します。

- `true` – 設定済みのサービス値を環境変数の値とマージ **&#x200B;**&#x200B;マージ）します。
- `false` – 環境変数値を使用して設定されたサービス値を **上書き** します。

## プライベートリポジトリ {#private-repository}

>[!NOTE]
>
>Adobeでは、クラウドインフラストラクチャプロジェクト上のAdobe Commerceにプライベートリポジトリを使用して、拡張機能や機密設定などの専有情報または開発作業をすべて保護することを強くお勧めします。

## Pro セルフサービスの警告 {#pro-self-service-warning}

>[!WARNING]
>
>一部の **Pro プロジェクト** では、`routes.yaml` ファイルのルート設定と `.magento.app.yaml` ファイルの cron 設定を更新するために、サポートチケットが必要です。 Adobe環境で YAML 設定ファイルを更新およびテストしたあと、変更内容をステージング環境にデプロイすることをお勧めします。 再デプロイ後に変更がステージングサイトに適用されず、関連するエラーメッセージがログに存在しない場合は、**必須**&#x200B;[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) すると、試みた設定変更に関する説明が表示されます。 更新された YAML 設定ファイルをチケットに含めます。

## プロサービスサポート {#pro-update-service}

>[!TIP]
>
>Pro プロジェクトの場合、`Staging` および `Production` 環境でのみ [ サービス ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) をインストールまたは更新するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html) する必要があります。
>
>必要なサービスの変更を示し、更新した `.magento.app.yaml` ファイルと `services.yaml` ファイルを含め、PHP バージョンをチケットに記載します。 PHP のバージョン、拡張機能、または環境設定のセルフサービスでの変更については、[ アプリケーション設定 _の ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html)PHP 設定_ を参照してください。
>
>実稼動環境（**Pro のみ**）に対する変更の場合は、少なくとも 48 時間の通知が必要です。 これにより、クラウドインフラストラクチャチームは、リソースをマーシャリングし、安全なアップグレードを実行するのに十分な時間を確保できます。 通知期間は、インフラストラクチャチームがリクエストを承認し、週末を除くアップグレードをスケジュールしたときに開始されます。 例えば、月曜日にサービスアップグレードを完了するには、水曜日までに予定されているアップグレードの確認を受け取る必要があります。 ピーク時の要求期間では、要求の処理により多くの時間がかかる場合があります。

## Pro バックアップ {#pro-backups}

>[!TIP]
>
>ステージング環境および実稼動環境では、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、チケットに記載された日付、時刻、タイムゾーンを示す特定のバックアップを取得する必要があります。
>
>Adobeは、自動バックアップから環境を復元 **ません**。 ステージングまたは実稼動スナップショットを復元する方法を選択する方法については、[ ステージングまたは実稼動からの DB スナップショットの復元 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) を参照してください。

## 再配置警告 {#redeploy-warning}

>[!WARNING]
>
>デプロイメントプロセスは、環境のマージ、プッシュまたは同期化を実行するとき、または [!DNL Commerce] アプリケーションがメンテナンスモードである場合に手動で再デプロイをトリガーするときに開始されます。 実稼動環境の場合、Adobeは、サービスが中断されないように、この作業をピーク外の時間に完了することをお勧めします。

## ルートプレースホルダー {#route-placeholder}

>[!NOTE]
>
>次のルート設定例では、プレースホルダーを含むルートテンプレートを使用します。 `{default}` のプレースホルダーは、サイトに設定されたデフォルトのドメインを表します。 プロジェクトに複数のドメインがある場合は、`{all}` プレースホルダーを使用して、デフォルトドメインとすべてのエイリアスのルーティングを設定します。 [ ルートの設定 ](/help/cloud-guide/routes/routes-yaml.md) を参照してください。

## SCD タイミング {#scd-timing-warning}

>[!WARNING]
>
>カスタムテーマファイルがないなど、デプロイ後のアプリケーションに静的コンテンツファイルに関する問題がある場合は、予想最大実行時間を 900 秒以上に増やします。

## シナリオベースのデプロイメント {#scenarios}

>[!NOTE]
>
>[!DNL ECE-Tools] 2002.1.0 以降では、シナリオベースのデプロイメント機能を使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのビルド、デプロイ、デプロイ後のプロセスをカスタマイズできます。 [ シナリオベースのデプロイメント ](/help/cloud-guide/deploy/scenario-based.md) を参照してください。

## 2 回目のステージング {#second-staging}

>[!NOTE]
>
>一部のプロジェクトでは、より高度な開発ワークフローが必要です。 このニーズをサポートするために、Adobeでは、クラウドインフラストラクチャへのアドオンオプションとして [ 追加のステージング環境 ](/help/cloud-guide/test/second-staging.md) を提供しています。

## サービス指示 {#service-instruction}

`master` ブランチを含む Pro 統合環境とスターター環境でのサービス設定については、以下の手順を使用します。

>[!NOTE]
>
>[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) して、Pro 実稼動環境とステージング環境のサービス設定を変更します。

## サービスの変更 {#service-change-tip}

>[!TIP]
>
>サービスの初期セットアップ後、`services.yaml` および `.magento.app.yaml` の構成ファイルを更新することで、インストールされているサービスのソフトウェア バージョンを変更できます。 サービスのアップグレードまたはダウングレードのガイダンスについては、[ サービスバージョンの変更 ](/help/cloud-guide/services/services-yaml.md#change-service-version) を参照してください。

## スタックしたデプロイメントのヒント {#stuck-deployment-tip}

>[!TIP]
>
>スタックしたデプロイメントのヘルプについては、_Adobe Commerce ヘルプセンターの &lbrace;0[&#128279;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)Commerce デプロイメントのトラブルシューティングツール_ を参照してください。

## ECE-Tools のアップデート {#ece-tools-package}

>[!NOTE]
>
>`ece-tools` パッケージを含まないバージョンのAdobe Commerceをクラウドインフラストラクチャー上で使用する場合は、クラウドプロジェクトに [1 回のアップグレード ](/help/cloud-guide/dev-tools/install-package.md) を行って、非推奨パッケージを削除する必要があります。 現在 `ece-tools` パッケージを使用していて、更新する必要がある場合は、[ECE-Tools パッケージの更新 ](/help/cloud-guide/dev-tools/update-package.md) を参照してください。

## アップグレードのヒント {#upgrade-tip}

>[!TIP]
>
>アップグレードまたはパッチ適用プロセスを開始する前に、統合環境からアクティブなブランチを作成し、新しいブランチをローカルワークステーションにチェックアウトします。 分岐をアップグレードまたはパッチプロセスに割り当てると、進行中の作業への干渉を回避できます。

<!-- Fastly-related snippets begin -->

## 管理者ログイン {#admin-login-step}

1. 管理者に [ ログイン ](/help/get-started/onboarding.md#access-your-admin-panel) します。

## カスタム VCL スニペットの配備の自動化 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>カスタム VCL スニペットを手動でアップロードする代わりに、環境内の `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` ディレクトリにスニペットを追加できます。 Commerce Admin で _VCL を Fastly にアップロード_ をクリックすると、このディレクトリ内のスニペットが自動的にアップロードされます。 Magento 2 ドキュメントの Fastly CDN モジュールの [ 自動カスタム VCL スニペットのデプロイメント ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) を参照してください。

<!-- Fastly-related snippets end -->
