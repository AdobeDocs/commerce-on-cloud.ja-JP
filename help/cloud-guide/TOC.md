---
cloud: Experience Cloud
solution-title: Commerce
user-guide-title: クラウド上の Commerce に関するガイド
user-guide-description: クラウドインフラストラクチャー上で Adobe Commerce アプリケーションを管理する方法について説明します。
product: magento
feature: Cloud
source-git-commit: 5475b65cb9606b200ff6ac3096ed0d0cf3168cf9
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 8%

---


# Commerce on Cloud Infrastructure {#user-guide}

+ [Commerce](overview.md)
+ デザイン {#architecture}
   + [クラウド基盤](architecture/cloud-architecture.md)
   + [セキュリティ](architecture/security.md)
   + [テクノロジースタック](architecture/tech-stack.md)
   + [スターターアーキテクチャ](architecture/starter-architecture.md)
   + [スターターワークフロー](architecture/starter-develop-deploy-workflow.md)
   + [プロアーキテクチャ](architecture/pro-architecture.md)
   + [プロワークフロー](architecture/pro-develop-deploy-workflow.md)
   + [拡張されたアーキテクチャ](architecture/scaled-architecture.md)
   + [自動スケーリング](architecture/autoscaling.md)
+ [基本を学ぶ](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html)
+ リリースノート {#release-notes}
   + [Cloud tools suite](release-notes/cloud-tools-suite.md)
   + [ECE-Tools パッケージ](release-notes/ece-tools-package.md)
   + [クラウドパッチ](release-notes/cloud-patches.md)
   + [Cloud Docker パッケージ](release-notes/cloud-docker.md)
   + [クラウドコンポーネント](release-notes/cloud-components.md)
   + [クラウドパッケージ](release-notes/cloud-packages.md)
   + [後方互換性のない変更](release-notes/backward-incompatible-changes.md)
   + [リリースノートのアーカイブ](release-notes/cloud-release-archive.md)
+ クラウドプロジェクト {#project}
   + [プロジェクト概要](project/overview.md)
   + [プロジェクト構造](project/file-structure.md)
   + [ユーザーアクセス](project/user-access.md)
   + [多要素認証](project/multi-factor-authentication.md)
   + [アクティビティストリーム](project/activity-stream.md)
   + [送信メール](project/outgoing-emails.md)
   + [SendGrid メールサービス](project/sendgrid.md)
   + [コンソールブランチ管理](project/console-branches.md)
   + [地域IP アドレス](project/regional-ip-addresses.md)
+ 開発者用ツール {#dev-tools}
   + [概要](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [CLIの概要](dev-tools/cloud-cli-overview.md)
      + [CLI リファレンス](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [パッケージの概要](dev-tools/package-overview.md)
      + [ECE-Toolsを使用するための1回限りのアップグレード](dev-tools/install-package.md)
      + [ECE-Tools パッケージの更新](dev-tools/update-package.md)
      + [CLI リファレンス](dev-tools/ece-tools-cli-reference.md)
      + [エラー参照](dev-tools/error-reference.md)
   + 連携 {#integrations}
      + [概要](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [ヘルス通知](integrations/health-notifications.md)
+ 開発 {#develop}
   + [概要](development/overview.md)
   + [認証キー](development/authentication-keys.md)
   + [CLI ブランチ管理](development/cli-branches.md)
   + [安全な接続](development/secure-connections.md)
   + デプロイ {#deploy}
      + [デプロイメントプロセス](deploy/process.md)
      + [最適化](deploy/optimization.md)
      + [ベストプラクティス](deploy/best-practices.md)
      + [シナリオベースの展開](deploy/scenario-based.md)
      + [ダウンタイムのゼロ導入](deploy/reduce-downtime.md)
      + [静的コンテンツのデプロイメント](deploy/static-content.md)
      + [スマートウィザード](deploy/smart-wizards.md)
      + [ステージングおよび実稼動へのデプロイ](deploy/staging-production.md)
      + [コンポーネントの障害からの復旧](deploy/recover-failed-deployment.md)
   + テスト {#test}
      + [テストのガイダンス](test/guidance.md)
      + [ログ](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [サンプルデータ](test/sample-data.md)
      + [ステージングと実稼動](test/staging-and-production.md)
      + [2番目のステージング環境](test/second-staging.md)
   + [PrivateLink サービス](development/privatelink-service.md)
   + [保護ブロック](development/protective-block.md)
   + [環境の復元](development/restore-environment.md)
   + 保存 {#storage}
      + [ディスク容量の管理](storage/manage-disk-space.md)
      + [クラウドインフラストラクチャ上のAdobe Commerceのディスク容量の制限を確認する](storage/check-disk-space-limit-on-cloud.md)
      + [プロファイルデータベースクエリ](storage/profile-database-queries.md)
      + [データベースのバックアップ](storage/database-dump.md)
      + [バックアップ管理](storage/snapshots.md)
   + アップグレードとパッチ {#upgrade}
      + [ベストプラクティス](development/best-practices.md)
      + [Commerce バージョンのアップグレード](development/commerce-version.md)
      + [パッチを適用](development/apply-patches.md)
+ 設定 {#configure}
   + [概要](environment/overview.md)
   + アプリケーション {#app}
      + [アプリケーションのデプロイメントの設定](application/configure-app-yaml.md)
      + [PHP設定](application/php-settings.md)
      + プロパティ {#properties}
         + [アプリケーションプロパティ](application/properties.md)
         + [Crons](application/crons-property.md)
         + [ファイアウォール（スターターのみ）](application/firewall-property.md)
         + [フック](application/hooks-property.md)
         + [変数](application/variables-property.md)
         + [Web](application/web-property.md)
         + [作業者](application/workers-property.md)
      + [静的ファイルのキャッシュの設定](application/set-cache.md)
   + 環境 {#env}
      + [環境デプロイメントの設定](environment/configure-env-yaml.md)
      + [変数レベルとオプション](environment/variable-levels.md)
      + 変数を上書き {#stage}
         + [環境変数](environment/variables-intro.md)
         + [管理者](environment/variables-admin.md)
         + [クラウド変数](environment/variables-cloud.md)
         + [グローバル](environment/variables-global.md)
         + [作成する](environment/variables-build.md)
         + [デプロイ](environment/variables-deploy.md)
         + [デプロイ後](environment/variables-post-deploy.md)
      + 通知の設定 {#log}
         + [通知](environment/set-up-notifications.md)
         + [ログハンドラー](environment/log-handlers.md)
   + ルート {#routes}
      + [ルートの設定](routes/routes-yaml.md)
      + [キャッシュ](routes/caching.md)
      + [リダイレクト](routes/redirects.md)
      + [サーバーサイドのインクルード](routes/server-side-includes.md)
   + サービス {#service}
      + [サービスの設定](services/services-yaml.md)
      + [ActiveMQ](services/activemq.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [バルキー](services/valkey.md)
+ Fastly サービス {#cdn}
   + [概要](cdn/fastly.md)
   + Fastlyの設定 {#setup-fastly}
      + [Fastly サービスの設定](cdn/fastly-configuration.md)
      + [キャッシュ設定のカスタマイズ](cdn/fastly-custom-cache-configuration.md)
      + [エラーおよびメンテナンスページのカスタマイズ](cdn/fastly-custom-response.md)
   + [Web アプリケーションファイアウォール](cdn/fastly-waf-service.md)
   + [高度なセキュリティ](cdn/advanced-security.md)
   + [画像の最適化](cdn/fastly-image-optimization.md)
   + VCLによるカスタマイズ {#custom-vcl-snippets}
      + [基本を学ぶ](cdn/fastly-vcl-custom-snippets.md)
      + [CMS バックエンドへのリクエストのルート変更](cdn/fastly-vcl-wordpress.md)
      + [ブロック紹介スパム](cdn/fastly-vcl-badreferer.md)
      + [IP許可リストに加える](cdn/fastly-vcl-allowlist.md)
      + [IPブロックリストに加える](cdn/fastly-vcl-blocking.md)
      + [Fastly キャッシュをバイパス](cdn/fastly-vcl-bypass-to-origin.md)
   + [Fastlyのトラブルシューティング](cdn/fastly-troubleshooting.md)
+ ストア設定 {#configure-store}
   + [概要](store/overview.md)
   + [ベストプラクティス](store/best-practices.md)
   + [カスタムテーマ](store/custom-theme.md)
   + [拡張機能](store/extensions.md)
   + [B2B モジュール](store/b2b-module.md)
   + [複数のサイト](store/multiple-sites.md)
   + [サイトマップと検索エンジンロボット](store/robots-sitemap.md)
   + [PayPalの支払い方法](store/paypal.md)
   + [設定管理](store/store-settings.md)
+ ローンチサイト {#launch}
   + [概要](launch/overview.md)
   + [チェックリストを起動](launch/checklist.md)
   + [ローンチステップ](launch/steps.md)
+ サイトを監視 {#monitor}
   + [パフォーマンス](monitor/performance.md)
   + [運用上の遠隔測定](monitor/operational-telemetry.md)
   + New Relic サービス {#new-relic}
      + [New Relicの概要](monitor/new-relic-service.md)
      + [アカウントとユーザーの管理](monitor/account-management.md)
      + パフォーマンスを調査 {#investigate}
         + [ポリシー、アラート、ワークフロー](monitor/investigate-performance.md)
         + [データ収集](monitor/ingest-data.md)
         + [デプロイメントの追跡](monitor/track-deployments.md)
      + [ログ管理](monitor/log-management.md)
