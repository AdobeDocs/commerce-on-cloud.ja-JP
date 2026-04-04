---
source-git-commit: c8effbdb82060e2a2cbcbdef577fed7249a76799
workflow-type: tm+mt
source-wordcount: '1313'
ht-degree: 0%

---
# クラウドスニペット

## Elasticsearchの警告 {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11以降は、Adobe Commerce オンクラウドインフラストラクチャではサポートされていません。 Adobe Commerce バージョン 2.3.7-p3、2.4.3-p2、および2.4.4以降では、OpenSearch サービスがサポートされています。 オンプレミスのインストールは、引き続きElasticsearchをサポートします。

## 拡張統合 {#enhanced-integration-envs}

>[!NOTE]
>
>2020年6月5日より前にプロビジョニングされたプロジェクトには、複数の小さい統合環境がありました。 テストと開発に大規模な統合環境が必要な場合は、拡張統合環境へのアップグレードをリクエストしてください。 詳しくは、_Adobe Commerce ヘルプセンター_&#x200B;の[Integration Environment リクエスト ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html)の記事を参照してください。

## 結合オプション {#merge-options}

デフォルトでは、デプロイメントプロセスは`env.php` ファイル内のすべての設定を上書きしますが、すべての値を上書きすることなく、サービス設定の1つ以上の値を結合することを選択できます。

`_merge` オプションを次のいずれかに設定します。

- `true`—**設定されたサービス値を環境変数の値と結合**。
- `false`—**設定されたサービス値を環境変数の値で**&#x200B;上書きします。

## プライベートリポジトリ {#private-repository}

>[!NOTE]
>
>Adobeでは、拡張機能や機密性の高い設定など、独自の情報や開発作業を保護するために、Adobe Commerce on cloud infrastructure プロジェクトのプライベートリポジトリを使用することを強くお勧めします。

## プロセルフサービスの警告 {#pro-self-service-warning}

>[!WARNING]
>
>一部の&#x200B;**Pro プロジェクト**&#x200B;では、`routes.yaml` ファイルのルート設定と`.magento.app.yaml` ファイルのcron設定を更新するために、Adobe サポートの支援が必要です。 Adobeでは、まず統合環境でYAML設定の変更をすべて行って検証し、その後ステージング環境にデプロイすることをお勧めします。
>
>
>変更が再デプロイ後にステージングサイトに反映されず、ログに関連するエラーメッセージが表示されない場合は、**Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を**&#x200B;送信する必要があります。[ チケットで、試した設定の変更を明確に説明し、更新されたYAML設定ファイルをチケットに添付します。

## プロサービスのサポート {#pro-update-service}

>[!BEGINSHADEBOX]

- Pro プロジェクトの場合、`Staging`および`Production`環境でのみ[ サービス ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html)をインストールまたは更新するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。

- 必要なサービス変更を示し、更新された`.magento.app.yaml`および`services.yaml` ファイルを含め、チケットにPHP バージョンを明記します。 PHPのバージョン、拡張機能、環境設定に対するセルフサービスの変更については、_アプリケーション設定_&#x200B;の[PHP設定](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html)を参照してください。

  >[!IMPORTANT]
  >
  >新しいチケットフォームで「環境」フィールドを選択する場合は、Adobeの環境命名を使用します。 例えば、その環境&#x200B;**Dev**&#x200B;を内部的に呼び出す場合でも、「ステージング」を選択します。 説明に内部名を記載できますが、「環境」フィールド自体にはAdobeの命名規則を使用する必要があります。

- 実稼動環境（**Proのみ**）への変更の場合、48時間以上の通知が必要です。 これにより、クラウドインフラチームはリソースを集め、安全なアップグレードを行うのに十分な時間を確保できます。 通知期間は、インフラチームがリクエストを確認し、週末を除くアップグレードをスケジュールした時点から開始されます。 例えば、月曜日にサービスのアップグレードを完了させるには、水曜日までにスケジュールされたアップグレードの確認を受け取る必要があります。 需要のピーク時には、リクエストの処理により多くの時間がかかる場合があります。

  >[!NOTE]
  >
  >すべての通信で明確さと一貫性を確保するために、スケジュールされたすべてのメンテナンスウィンドウをUTC形式で提供する必要があります。 ステージング環境ではサービスのアップグレードをスケジュールできません。ほとんどの場合、ステージングのアップグレードはリクエストと同じ日に実行されます。
  >
  >RabbitMQ アップグレードをリクエストする場合は、アップグレード完了後に環境を再デプロイして、メッセージキューが再初期化されるようにしてください。

- **アップグレードのスケジュールを設定するための2部構成のハンドシェイクプロセス**

  スムーズで調和のとれたアップグレードプロセスを実現するために、Adobe Commerce サポートでは、すべての実稼動環境のアップグレードに対して2部構成のハンドシェイクプロセスを実行します。

   1. **お客様確認**:Adobe サポートは、お客様がアップグレードの希望日時を確認することを最初にリクエストします。 このステップにより、スケジュールが顧客のビジネスニーズとメンテナンスウィンドウに合致していることを確認します。
   2. **スケジュール設定と最終確認**：お客様がタイミングを確認すると、Adobe サポートはインフラストラクチャチームにリクエストを送信し、その後、リクエストを確認し、スケジュールされたアップグレードウィンドウの最終確認を行います。

アップグレードは、インフラチームが最終確認を行うまでスケジュールされたとは見なされません。 お客様には、遅延を回避し、適切な通知を行うために、アップグレード期間の少なくとも48時間前に迅速に対応することが推奨されます。

>[!ENDSHADEBOX]

## プロバックアップ {#pro-backups}

>[!TIP]
>
>Pro ステージング環境および実稼動環境では、チケット内の日付、時刻、タイムゾーンを記録したバックアップを取得するには、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信する必要があります。
>
>Adobeは、自動バックアップから任意の環境を&#x200B;**not**&#x200B;復元します。 ステージングまたは実稼動スナップショットを復元する方法を選択する方法については、[ ステージングまたは実稼動からのDB スナップショットの復元](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html)を参照してください。

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
>[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信して、Pro実稼動環境とステージング環境のサービス構成を変更します。

## サービスの変更 {#service-change-tip}

>[!TIP]
>
>最初のサービス設定の後、`services.yaml`および`.magento.app.yaml`設定ファイルを更新することで、インストール済みサービスのソフトウェアバージョンを変更できます。 サービスのアップグレードまたはダウングレードに関するガイダンスについては、[ サービスバージョンの変更](/help/cloud-guide/services/services-yaml.md#change-service-version)を参照してください。

## 展開のヒントをスタック {#stuck-deployment-tip}

>[!TIP]
>
>デプロイメントが停止している場合は、_Adobe Commerce ヘルプセンター_&#x200B;の[Commerce デプロイメントのトラブルシューティング ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)を参照してください。

## ECE-Toolsへのアップデート {#ece-tools-package}

>[!NOTE]
>
>`ece-tools` パッケージを含まないクラウドインフラストラクチャでAdobe Commerceのバージョンを使用する場合は、非推奨パッケージを削除するために、クラウドプロジェクトに[1回アップグレード ](/help/cloud-guide/dev-tools/install-package.md)を実行する必要があります。 現在`ece-tools` パッケージを使用しており、それを更新する必要がある場合は、[ECE-Tools パッケージの更新](/help/cloud-guide/dev-tools/update-package.md)を参照してください。

## アップグレードのヒント {#upgrade-tip}

>[!TIP]
>
>アップグレードまたはパッチ適用プロセスを開始する前に、統合環境からアクティブなブランチを作成し、新しいブランチをローカル ワークステーションにチェックアウトします。 アップグレードまたはパッチプロセスにブランチを割り当てることで、進行中の作業への干渉を回避できます。

<!-- Fastly-related snippets begin -->

## 管理者ログイン {#admin-login-step}

1. [管理者に](/help/get-started/onboarding.md#access-your-admin-panel) ログインします。

## カスタム VCL スニペットのデプロイメントの自動化 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>カスタム VCL スニペットを手動でアップロードする代わりに、環境内の`$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` ディレクトリにスニペットを追加できます。 このディレクトリのスニペットは、Commerce管理者で&#x200B;_VCLをFastly_&#x200B;にアップロードをクリックすると、自動的にアップロードされます。 Magento 2用Fastly CDN モジュールの[自動カスタム VCL スニペットのデプロイメント ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)を参照してください。

<!-- Fastly-related snippets end -->
