---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# クラウドスニペット

## Elasticsearchの警告 {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7以降は、Adobe Commerce オンクラウドインフラストラクチャではサポートされていません。 Adobe Commerce 2.4.4以降では、OpenSearch サービスをサポートしています。

## 拡張統合 {#enhanced-integration-envs}

>[!NOTE]
>
>2020年6月5日より前にプロビジョニングされたプロジェクトには、複数の小さい統合環境がありました。 テストと開発に大規模な統合環境が必要な場合は、拡張統合環境へのアップグレードをリクエストしてください。 詳しくは、_Adobe Commerce ヘルプセンター_&#x200B;の[Integration Environment リクエスト ](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27242)の記事を参照してください。

## 結合オプション {#merge-options}

デフォルトでは、デプロイメントプロセスは`env.php` ファイル内のすべての設定を上書きしますが、すべての値を上書きすることなく、サービス設定の1つ以上の値を結合することを選択できます。

`_merge` オプションを次のいずれかに設定します。

- `true`—**設定されたサービス値を環境変数の値と結合**。
- `false`—**設定されたサービス値を環境変数の値で**&#x200B;上書きします。

## プライベートリポジトリ {#private-repository}

>[!NOTE]
>
>Adobeでは、拡張機能や機密性の高い設定など、独自の情報や開発作業を保護するために、Adobe Commerce on cloud infrastructure プロジェクトのプライベートリポジトリを使用することをお勧めします。

## プロセルフサービスの警告 {#pro-self-service-warning}

>[!WARNING]
>
>一部の&#x200B;**Pro プロジェクト**&#x200B;では、`routes.yaml` ファイルのルート設定と`.magento.app.yaml` ファイルのcron設定を更新するために、Adobe サポートの支援が必要です。 Adobeでは、まず統合環境でYAML設定の変更をすべて行って検証し、その後ステージング環境にデプロイすることをお勧めします。
>
>
>再展開後に変更がステージングサイトに反映されず、ログに関連するエラーメッセージがない場合は、**Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)を**[送信する必要があります。 チケットで、試した設定の変更を明確に説明し、更新されたYAML設定ファイルをチケットに添付します。

## プロバックアップ {#pro-backups}

>[!TIP]
>
>Pro ステージング環境および実稼動環境で特定のバックアップを取得するには、[ チケットの日付、時刻、タイムゾーンを記載したAdobe Commerce サポートチケット ](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)を送信します。
>
>Adobeは、自動バックアップから任意の環境を&#x200B;**not**&#x200B;復元します。 ステージングまたは実稼動スナップショットを復元する方法を選択する方法については、[ ステージングまたは実稼動からのDB スナップショットの復元](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production)を参照してください。

## 再展開の警告 {#redeploy-warning}

>[!WARNING]
>
>デプロイメントプロセスは、環境の結合、プッシュ、同期を実行する場合、または[!DNL Commerce] アプリケーションがメンテナンスモードになっている手動の再デプロイメントをトリガーする場合に開始します。 実稼動環境の場合、サービスの中断を避けるために、Adobeではオフピーク時にこの作業を行うことをお勧めします。

## ルートプレースホルダー {#route-placeholder}

>[!NOTE]
>
>次のルート設定例では、ルートテンプレートとプレースホルダーを使用しています。 `{default}` プレースホルダーは、サイトに設定された既定のドメインを表します。 プロジェクトに複数のドメインがある場合は、`{all}` プレースホルダーを使用して、デフォルトドメインとすべてのエイリアスのルーティングを設定します。 [ ルートの設定](/help/cloud-guide/routes/routes-yaml.md)を参照してください。

## SCD タイミング {#scd-timing-warning}

>[!WARNING]
>
>カスタムテーマファイルが見つからないなど、デプロイメント後にアプリケーションの静的コンテンツファイルに問題が発生した場合は、想定される最大実行時間を900秒以上に増やします。

## シナリオベースの展開 {#scenarios}

>[!NOTE]
>
>[!DNL ECE-Tools] 2002.1.0以降では、シナリオベースのデプロイメント機能を使用して、Adobe Commerce on cloud infrastructure プロジェクトのビルド、デプロイ、デプロイ後のプロセスをカスタマイズできます。 [ シナリオベースのデプロイメント ](/help/cloud-guide/deploy/scenario-based.md)を参照してください。

## 2番目のステージ {#second-staging}

>[!NOTE]
>
>一部のプロジェクトでは、より洗練された開発ワークフローが求められます。 このニーズをサポートするために、Adobeでは、クラウドインフラストラクチャにアドオンオプションとして[追加のステージング環境](/help/cloud-guide/test/second-staging.md)を提供しています。

## サービス命令 {#service-instruction}

`master` ブランチを含むPro統合環境およびスターター環境でのサービス設定については、次の手順を使用します。

>[!NOTE]
>
>Pro実稼動環境とステージング環境のサービス構成を変更するには、[Adobe Commerce サポートチケットを送信](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)してください。 スケジュール要件と顧客の可用性に関するガイダンスについては、_サービスの設定_&#x200B;の[Pro サービス サポート ](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support)を参照してください。

## サービスの変更 {#service-change-tip}

>[!TIP]
>
>最初のサービス設定の後、`services.yaml`および`.magento.app.yaml`設定ファイルを更新することで、インストール済みサービスのソフトウェアバージョンを変更できます。 サービスのアップグレードまたはダウングレードに関するガイダンスについては、[ サービスバージョンの変更](/help/cloud-guide/services/services-yaml.md#change-service-version)を参照してください。 このセルフサービス方式は、Pro ステージング環境または実稼動環境には適用されません。_Configure services_&#x200B;の[Pro サービスのサポート ](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support)を参照してください。

## 展開のヒントをスタック {#stuck-deployment-tip}

>[!TIP]
>
>デプロイメントが停止している場合は、_Adobe Commerce ヘルプセンター_&#x200B;の[Commerce デプロイメントのトラブルシューティング ](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-29640)を参照してください。

## ECE-Toolsへのアップデート {#ece-tools-package}

>[!NOTE]
>
>`ece-tools` パッケージを含まないクラウドインフラストラクチャ上のAdobe Commerceのバージョンで非推奨パッケージを削除するには、クラウドプロジェクトに[1回アップグレード ](/help/cloud-guide/dev-tools/install-package.md)を実行する必要があります。 現在`ece-tools` パッケージを使用しており、それを更新する必要がある場合は、[ECE-Tools パッケージの更新](/help/cloud-guide/dev-tools/update-package.md)を参照してください。

## アップグレードのヒント {#upgrade-tip}

>[!TIP]
>
>アップグレードまたはパッチ適用プロセスを開始する前に、統合環境からアクティブなブランチを作成し、新しいブランチをローカル ワークステーションにチェックアウトします。 アップグレードまたはパッチプロセスにブランチを割り当てることで、進行中の作業への干渉を回避できます。

## New Relicのバルキー {#valkey-newrelic}

>[!NOTE]
>
>New Relicは、Valkeyへの移行後もRedisを表示する可能性があります。
>
>New Relicは、環境がValkeyに移行された後も、引き続きキャッシュサービスをRedisと呼ぶことが予想されます。
>
>ValkeyはRedisのオープンソースのフォークであり、一部のツールや統合機能は、明確なValkey ラベルではなくRedis命名を使用してサービスを識別し続けています。 この動作は、Redisがまだインストールされていることを必ずしも示しません。

<!-- Fastly-related snippets begin -->

## 管理者ログイン {#admin-login-step}

1. [管理者に](/help/get-started/onboarding.md#access-your-admin-panel) ログインします。

## カスタム VCL スニペットのデプロイメントの自動化 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>カスタム VCL スニペットを手動でアップロードする代わりに、環境内の`$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` ディレクトリにスニペットを追加できます。 このディレクトリのスニペットは、Commerce管理者で&#x200B;_VCLをFastly_&#x200B;にアップロードをクリックすると、自動的にアップロードされます。 Magento 2用Fastly CDN モジュールの[自動カスタム VCL スニペットのデプロイメント ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)を参照してください。

<!-- Fastly-related snippets end -->
