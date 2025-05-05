---
user-guide-title: クラウド上の Commerce に関するガイド
user-guide-description: クラウドインフラストラクチャー上で Adobe Commerce アプリケーションを管理する方法について説明します。
product: magento
feature: Cloud
source-git-commit: fd7879e8f3c9e1965cf4aa3d99824e577971529d
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 7%

---


# クラウドインフラストラクチャー上のCommerce {#user-guide}

+ [Commerce](overview.md)
+ アーキテクチャ {#architecture}
   + [クラウドインフラストラクチャ](architecture/cloud-architecture.md)
   + [セキュリティ](architecture/security.md)
   + [技術スタック](architecture/tech-stack.md)
   + [スターターアーキテクチャ](architecture/starter-architecture.md)
   + [スターターワークフロー](architecture/starter-develop-deploy-workflow.md)
   + [Pro アーキテクチャ](architecture/pro-architecture.md)
   + [Pro ワークフロー](architecture/pro-develop-deploy-workflow.md)
   + [拡張されたアーキテクチャ](architecture/scaled-architecture.md)
   + [自動スケーリング](architecture/autoscaling.md)
+ [ はじめに ](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html)
+ リリースノート {#release-notes}
   + [クラウドツールスイート](release-notes/cloud-tools-suite.md)
   + [ECE-Tools パッケージ](release-notes/ece-tools-package.md)
   + [クラウドパッチ](release-notes/cloud-patches.md)
   + [Cloud Docker パッケージ](release-notes/cloud-docker.md)
   + [クラウドコンポーネント](release-notes/cloud-components.md)
   + [クラウドパッケージ](release-notes/cloud-packages.md)
   + [後方互換性のない変更](release-notes/backward-incompatible-changes.md)
   + [リリースノートアーカイブ](release-notes/cloud-release-archive.md)
+ Cloud project {#project}
   + [プロジェクトの概要](project/overview.md)
   + [プロジェクト構造](project/file-structure.md)
   + [ユーザーアクセス](project/user-access.md)
   + [多要素認証](project/multi-factor-authentication.md)
   + [アクティビティストリーム](project/activity-stream.md)
   + [送信メール](project/outgoing-emails.md)
   + [SendGrid メールサービス](project/sendgrid.md)
   + [コンソールブランチ管理](project/console-branches.md)
   + [地域の IP アドレス](project/regional-ip-addresses.md)
+ デベロッパーツール {#dev-tools}
   + [概要](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [CLI の概要](dev-tools/cloud-cli-overview.md)
      + [CLI リファレンス](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [パッケージの概要](dev-tools/package-overview.md)
      + [ECE-Tools を使用するための 1 回限りのアップグレード](dev-tools/install-package.md)
      + [ECE-Tools パッケージの更新](dev-tools/update-package.md)
      + [CLI リファレンス](dev-tools/ece-tools-cli-reference.md)
      + [エラーリファレンス](dev-tools/error-reference.md)
   + Integrations {#integrations}
      + [概要](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [正常性通知](integrations/health-notifications.md)
+ 開発 {#develop}
   + [概要](development/overview.md)
   + [認証キー](development/authentication-keys.md)
   + [CLI ブランチ管理](development/cli-branches.md)
   + [安全な接続](development/secure-connections.md)
   + のデプロイ{#deploy}
      + [デプロイメントプロセス](deploy/process.md)
      + [最適化](deploy/optimization.md)
      + [ベストプラクティス](deploy/best-practices.md)
      + [シナリオベースのデプロイメント](deploy/scenario-based.md)
      + [ダウンタイムなしのデプロイメント](deploy/reduce-downtime.md)
      + [静的コンテンツデプロイメント](deploy/static-content.md)
      + [スマート・ウィザード](deploy/smart-wizards.md)
      + [ステージング環境および実稼動環境にデプロイ](deploy/staging-production.md)
      + [コンポーネント障害からのリカバリ](deploy/recover-failed-deployment.md)
   + テスト {#test}
      + [テストガイダンス](test/guidance.md)
      + [ログ](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [サンプルデータ](test/sample-data.md)
      + [ステージングと実稼動](test/staging-and-production.md)
      + [2 つ目のステージング環境](test/second-staging.md)
   + [PrivateLink サービス](development/privatelink-service.md)
   + [保護ブロック](development/protective-block.md)
   + [環境を復元](development/restore-environment.md)
   + ストレージ {#storage}
      + [ディスク容量の管理](storage/manage-disk-space.md)
      + [プロファイルデータベースクエリ](storage/profile-database-queries.md)
      + [データベースのバックアップ](storage/database-dump.md)
      + [バックアップ管理](storage/snapshots.md)
   + アップグレードとパッチ {#upgrade}
      + [ベストプラクティス](development/best-practices.md)
      + [Commerceのバージョンのアップグレード](development/commerce-version.md)
      + [パッチの適用](development/apply-patches.md)
+ 設定 {#configure}
   + [概要](environment/overview.md)
   + アプリケーション {#app}
      + [アプリケーションのデプロイメントの設定](application/configure-app-yaml.md)
      + [PHP 設定](application/php-settings.md)
      + プロパティ {#properties}
         + [アプリケーションプロパティ](application/properties.md)
         + [クローン](application/crons-property.md)
         + [ファイアウォール（スターターのみ）](application/firewall-property.md)
         + [フック](application/hooks-property.md)
         + [変数](application/variables-property.md)
         + [Web](application/web-property.md)
         + [作業者](application/workers-property.md)
      + [静的ファイルのキャッシュの設定](application/set-cache.md)
   + 環境 {#env}
      + [環境デプロイメントの設定](environment/configure-env-yaml.md)
      + [変数のレベルとオプション](environment/variable-levels.md)
      + 変数の上書き {#stage}
         + [環境変数](environment/variables-intro.md)
         + [ADMIN](environment/variables-admin.md)
         + [クラウド変数](environment/variables-cloud.md)
         + [グローバル](environment/variables-global.md)
         + [ビルド](environment/variables-build.md)
         + [デプロイ](environment/variables-deploy.md)
         + [デプロイ後](environment/variables-post-deploy.md)
      + 通知の設定 {#log}
         + [通知](environment/set-up-notifications.md)
         + [ログハンドラー](environment/log-handlers.md)
   + ルート {#routes}
      + [ルートの設定](routes/routes-yaml.md)
      + [キャッシュ](routes/caching.md)
      + [リダイレクト](routes/redirects.md)
      + [サーバーサイドインクルード](routes/server-side-includes.md)
   + サービス {#service}
      + [サービスの設定](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Fastly サービス {#cdn}
   + [概要](cdn/fastly.md)
   + Fastly のセットアップ {#setup-fastly}
      + [Fastly サービスの設定](cdn/fastly-configuration.md)
      + [キャッシュ設定のカスタマイズ](cdn/fastly-custom-cache-configuration.md)
      + [エラーページとメンテナンスページのカスタマイズ](cdn/fastly-custom-response.md)
   + [Web アプリケーションファイアウォール](cdn/fastly-waf-service.md)
   + [画像の最適化](cdn/fastly-image-optimization.md)
   + VCL を使用したカスタマイズ {#custom-vcl-snippets}
      + [基本を学ぶ](cdn/fastly-vcl-custom-snippets.md)
      + [CMS バックエンドへのリクエストの再ルーティング](cdn/fastly-vcl-wordpress.md)
      + [リファラルスパムをブロック](cdn/fastly-vcl-badreferer.md)
      + [IP許可リスト](cdn/fastly-vcl-allowlist.md)
      + [IPブロックリスト](cdn/fastly-vcl-blocking.md)
      + [Fastly キャッシュのバイパス](cdn/fastly-vcl-bypass-to-origin.md)
   + [Fastly のトラブルシューティング](cdn/fastly-troubleshooting.md)
+ ストア設定 {#configure-store}
   + [概要](store/overview.md)
   + [ベストプラクティス](store/best-practices.md)
   + [カスタムテーマ](store/custom-theme.md)
   + [拡張機能](store/extensions.md)
   + [B2B モジュール](store/b2b-module.md)
   + [複数のサイト](store/multiple-sites.md)
   + [サイトマップ・検索エンジンロボット](store/robots-sitemap.md)
   + [PayPal の支払い方法](store/paypal.md)
   + [設定管理](store/store-settings.md)
+ サイト を起動 {#launch}
   + [概要](launch/overview.md)
   + [Launch チェックリスト](launch/checklist.md)
   + [ローンチ手順](launch/steps.md)
+ サイト の監視 {#monitor}
   + [パフォーマンス](monitor/performance.md)
   + New Relic サービス {#new-relic}
      + [New Relicの概要](monitor/new-relic-service.md)
      + [アカウントとユーザーの管理](monitor/account-management.md)
      + パフォーマンス の調査 {#investigate}
         + [ポリシー、アラート、ワークフロー](monitor/investigate-performance.md)
         + [データ取り込み](monitor/ingest-data.md)
         + [デプロイメントの追跡](monitor/track-deployments.md)
      + [ログ管理](monitor/log-management.md)
