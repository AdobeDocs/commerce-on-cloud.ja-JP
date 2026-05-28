---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '2710'
ht-degree: 4%

---
# ECE-Tools エラーコード

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## クリティカルエラー

クリティカルエラーは、Commerce on cloud infrastructure プロジェクトの設定に関する問題を示します。この問題は、デプロイメントの失敗、例えば、必要な設定の設定が正しくないか、サポートされていない、または設定が見つからないなどの原因になります。 デプロイする前に、これらのエラーを解決するために設定を更新する必要があります。

### ビルドステージ

| エラーコード | ビルドステップ | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 2 |  | `./app/etc/env.php` ファイルに書き込めません | デプロイメントスクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることはできません。 ファイルシステムの権限を確認します。 |
| 3 |  | 設定が`schema.yaml` ファイルで定義されていません | 設定が`./vendor/magento/ece-tools/config/schema.yaml` ファイルで定義されていません。 設定変数名が正しく定義されていることを確認します。 |
| 4 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 5 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取れません。 ファイルの権限を確認します。 |
| 6 |  | `.schema.yaml` ファイルを読み取れません | `./vendor/magento/ece-tools/config/magento.env.yaml` ファイルを読み取れません。 ファイルのアクセス許可を確認し、再展開します（`magento-cloud environment:redeploy`）。 |
| 7 | refresh-modules | `./app/etc/config.php` ファイルに書き込めません | デプロイメントスクリプトは、`/app/etc/config.php` ファイルに必要な変更を加えることはできません。 ファイルシステムの権限を確認します。 |
| 8 | validate-config | `composer.json` ファイルを読み取れません | `./composer.json` ファイルを読み取れません。 ファイルの権限を確認します。 |
| 9 | validate-config | `composer.json` ファイルに必要なオートロード セクションがありません | 必須の`autoload` セクションが`composer.json` ファイルにありません。 自動ロードセクションをクラウドテンプレートの`composer.json` ファイルと比較し、見つからない設定を追加します。 |
| 10 | validate-config | `.magento.env.yaml` ファイルには、スキーマで宣言されていないオプション、または無効な値またはステージで設定されたオプションが含まれています | `./.magento.env.yaml` ファイルに無効な設定が含まれています。 詳細については、エラーログを確認してください。 |
| 11 | refresh-modules | コマンドが失敗しました：`/bin/magento module:enable --all` | `composer update`をローカルで実行してみてください。 次に、更新された`composer.lock` ファイルをコミットしてプッシュします。 また、詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 12 | apply-patches | パッチを適用できませんでした |  |
| 13 | set-report-dir-nesting-level | ファイル `/pub/errors/local.xml`に書き込めません |  |
| 14 | copy-sample-data | サンプル データ ファイルをコピーできませんでした |  |
| 15 | compile-di | コマンドが失敗しました：`/bin/magento setup:di:compile` | 詳細については、`cloud.log`を確認してください。 `VERBOSE_COMMANDS: '-vvv'`を`.magento.env.yaml`に追加すると、より詳細なコマンド出力が得られます。 |
| 16 | dump-autoload | コマンドが失敗しました：`composer dump-autoload` | `composer dump-autoload` コマンドが失敗しました。 詳細については、`cloud.log`を確認してください。 |
| 17 | ランベーラー | JavaScript バンドルの`Baler`を実行するコマンドが失敗しました | `SCD_USE_BALER`環境変数を確認して、Baler モジュールがJS バンドル用に設定され、有効になっていることを確認します。 Baler モジュールが必要ない場合は、`SCD_USE_BALER: false`を設定します。 |
| 18 | compress-static-content | 必要なユーティリティが見つかりませんでした（タイムアウト、bash） |  |
| 19 | deploy-static-content | コマンド `/bin/magento setup:static-content:deploy`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 20 | compress-static-content | 静的コンテンツ圧縮に失敗しました | 詳細については、`cloud.log`を確認してください。 |
| 21 | backup-data: static-content | 静的コンテンツを`init` ディレクトリにコピーできませんでした | 詳細については、`cloud.log`を確認してください。 |
| 22 | backup-data: writable-dirs | 書き込み可能なディレクトリを`init` ディレクトリにコピーできませんでした | 書き込み可能なディレクトリを`./init` フォルダーにコピーできませんでした。 ファイルシステムの権限を確認します。 |
| 23 |  | ロガーオブジェクトを作成できません |  |
| 24 | backup-data: static-content | `./init/pub/static/` ディレクトリをクリーンアップできませんでした | `./init/pub/static` フォルダーをクリーンアップできませんでした。 ファイルシステムの権限を確認します。 |
| 25 |  | Composer パッケージが見つかりません | GitHub リポジトリからAdobe Commerce アプリケーションのバージョンを直接インストールした場合は、`DEPLOYED_MAGENTO_VERSION_FROM_GIT`環境変数が設定されていることを確認します。 |
| 26 | validate-config | Adobe CommerceおよびMagento Open Source 2.4以降ではサポートされなくなったMagento Braintree モジュール設定を削除します。 | Braintree モジュールのサポートは、Magento 2.4.0以降には含まれなくなりました。 `.magento.app.yaml` ファイルのvariables セクションからCONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL変数を削除します。 Braintreeの支払いサポートについては、代わりにCommerce Marketplaceの公式の拡張機能を使用してください。 |

### デプロイステージ

| エラーコード | デプロイステップ | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 101 | pre-deploy: cache | キャッシュ設定が正しくない（ポートまたはホストが見つからない） | キャッシュ設定に必要なパラメーター`server`または`port`がありません。 詳細については、`cloud.log`を確認してください。 |
| 102 |  | `./app/etc/env.php` ファイルに書き込めません | デプロイメントスクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることはできません。 ファイルシステムの権限を確認します。 |
| 103 |  | 設定が`schema.yaml` ファイルで定義されていません | 設定が`./vendor/magento/ece-tools/config/schema.yaml` ファイルで定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 104 |  | `.magento.env.yaml` ファイルを解析できませんでした | 設定が`./vendor/magento/ece-tools/config/schema.yaml` ファイルで定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 105 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取れません。 ファイルの権限を確認します。 |
| 106 |  | `.schema.yaml` ファイルを読み取れません |  |
| 107 | pre-deploy: clean-redis-cache | Redis キャッシュのクリーニングに失敗しました | Redis キャッシュのクリーニングに失敗しました。 Redis キャッシュ設定が正しく、Redis サービスが使用可能であることを確認します。 [Redis サービスのセットアップ ](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html)を参照してください。 |
| 140 | pre-deploy: clean-valkey-cache | Valkey キャッシュのクリーニングに失敗しました | Valkey キャッシュのクリーニングに失敗しました。 Valkey キャッシュ設定が正しく、Valkey サービスが使用可能であることを確認します。 [Valkey サービスの設定](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/valkey.html)を参照してください。 |
| 108 | pre-deploy: set-production-mode | コマンド `/bin/magento maintenance:enable`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 109 | validate-config | データベース設定が正しくない | `DATABASE_CONFIGURATION`環境変数が正しく設定されていることを確認してください。 |
| 110 | validate-config | セッション設定が正しくありません | `SESSION_CONFIGURATION`環境変数が正しく設定されていることを確認してください。 設定には、少なくとも`save` パラメーターを含める必要があります。 |
| 111 | validate-config | 検索設定が正しくない | `SEARCH_CONFIGURATION`環境変数が正しく設定されていることを確認してください。 設定には、少なくとも`engine` パラメーターを含める必要があります。 |
| 112 | validate-config | リソース設定が正しくない | `RESOURCE_CONFIGURATION`環境変数が正しく設定されていることを確認してください。 設定には少なくとも`connection` パラメーターを含める必要があります。 |
| 113 | validate-config:elasticsuite-integrity | ElasticSuiteがインストールされていますが、Elasticsearch サービスを利用できません | `SEARCH_CONFIGURATION`環境変数が正しく設定されていることを確認し、Elasticsearch サービスが使用可能であることを確認します。 |
| 114 | validate-config:elasticsuite-integrity | ElasticSuiteがインストールされますが、別の検索エンジンが使用されます | ElasticSuiteがインストールされていますが、別の検索エンジンが設定されています。 Elasticsearchを有効にするには、`SEARCH_CONFIGURATION`環境変数を更新し、`services.yaml` ファイルでElasticsearch サービス設定を確認します。 |
| 115 |  | データベース クエリの実行に失敗しました |  |
| 116 | install-update: setup | コマンド `/bin/magento setup:install`が失敗しました | 詳細については、`cloud.log`および`install_upgrade.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 117 | install-update: config-import | コマンド `app:config:import`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 118 |  | 必要なユーティリティが見つかりませんでした（タイムアウト、bash） |  |
| 119 | install-update: deploy-static-content | コマンド `/bin/magento setup:static-content:deploy`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 120 | compress-static-content | 静的コンテンツ圧縮に失敗しました | 詳細については、`cloud.log`を確認してください。 |
| 121 | deploy-static-content:generate | デプロイ済みのバージョンを更新できません | `./pub/static/deployed_version.txt` ファイルを更新できません。 ファイルシステムの権限を確認します。 |
| 122 | clean-static-content | 静的コンテンツファイルをクリーンアップできませんでした |  |
| 123 | install-update: split-db | コマンド `/bin/magento setup:db-schema:split`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 124 | clean-view-preprocessed | `var/view_preprocessed` フォルダーをクリーンアップできませんでした | `./var/view_preprocessed` フォルダーをクリーンアップできません。 ファイルシステムの権限を確認します。 |
| 125 | install-update: reset-password | `/var/credentials_email.txt` ファイルを更新できませんでした | `/var/credentials_email.txt` ファイルを更新できませんでした。 ファイルシステムの権限を確認します。 |
| 126 | install-update: update | コマンド `/bin/magento setup:upgrade`が失敗しました | 詳細については、`cloud.log`および`install_upgrade.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 127 | clean-cache | コマンド `/bin/magento cache:flush`が失敗しました | 詳細については、`cloud.log`を確認してください。 コマンド出力の詳細については、`VERBOSE_COMMANDS: '-vvv'` オプションを`.magento.env.yaml` ファイルに追加してください。 |
| 128 | disable-maintenance-mode | コマンド `/bin/magento maintenance:disable`が失敗しました | 詳細については、`cloud.log`を確認してください。 `VERBOSE_COMMANDS: '-vvv'`を`.magento.env.yaml`に追加すると、より詳細なコマンド出力が得られます。 |
| 129 | install-update: reset-password | パスワードリセット テンプレートを読み取れません |  |
| 130 | install-update: cache_type | コマンドが失敗しました：`php ./bin/magento cache:enable` | コマンド `php ./bin/magento cache:enable`は、Adobe Commerceがインストールされたときにのみ実行されますが、デプロイメントの開始時に`./app/etc/env.php` ファイルが存在しないか空でした。 詳細については、`cloud.log`を確認してください。 `VERBOSE_COMMANDS: '-vvv'`を`.magento.env.yaml`に追加すると、より詳細なコマンド出力が得られます。 |
| 131 | install-update | `crypt/key` キー値が`./app/etc/env.php` ファイルまたは`CRYPT_KEY` クラウド環境変数に存在しません | このエラーは、Adobe Commerceのデプロイメントが開始されたときに`./app/etc/env.php` ファイルが存在しない場合、または`crypt/key`値が未定義の場合に発生します。 別の環境からデータベースを移行した場合は、その環境から暗号化キーの値を取得します。 次に、現在の環境の[CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) クラウド環境変数に値を追加します。 [Adobe Commerce暗号化キー](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials)を参照してください。 誤って`./app/etc/env.php` ファイルを削除した場合は、次のコマンドを使用して、以前のデプロイメントから作成されたバックアップファイルから復元します。`./vendor/bin/ece-tools backup:restore` CLI コマンド。」 |
| 132 |  | Elasticsearch サービスに接続できません | 有効なElasticsearch資格情報を確認し、サービスが実行中であることを確認します |
| 137 |  | OpenSearch サービスに接続できません | 有効なOpenSearch資格情報を確認し、サービスが実行されていることを確認します |
| 133 | validate-config | Adobe CommerceまたはMagento Open Source 2.4以降ではサポートされなくなったMagento Braintree モジュール設定を削除します。 | Braintree モジュールのサポートは、Adobe CommerceまたはMagento Open Source 2.4.0以降には含まれなくなりました。 `.magento.app.yaml` ファイルのvariables セクションからCONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL変数を削除します。 Braintree サポートの場合は、代わりにCommerce Marketplaceの公式のBraintree支払い拡張機能を使用してください。 |
| 134 | validate-config | Adobe CommerceおよびMagento Open Source 2.4.0をインストールするには、Elasticsearch サービスが必要です | Elasticsearch サービスのインストール |
| 138 | validate-config | Adobe CommerceおよびMagento Open Source 2.4.4をインストールするには、OpenSearchまたはElasticsearch サービスが必要です | OpenSearch サービスのインストール |
| 135 | validate-config | 検索エンジンは、Adobe CommerceおよびMagento Open Source用のElasticsearch >= 2.4.0に設定する必要があります | `engine` オプションのSEARCH_CONFIGURATION変数を確認してください。 設定されている場合は、オプションを削除するか、値を「elasticsearch」に設定します。 |
| 136 | validate-config | Adobe CommerceおよびMagento Open Source 2.5.0以降、分割データベースが削除されました。 | 分割データベースを使用する場合は、単一のデータベースに戻すか、単一のデータベースに移行するか、別のアプローチを使用する必要があります。 |
| 139 | validate-config | 不正確な検索エンジン | このAdobe Commerce版またはMagento Open Source版は、OpenSearchをサポートしていません。 バージョン 2.3.7-p3、2.4.3-p2以降を使用する |

### デプロイ後のステージ

| エラーコード | 配信後の手順 | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 201 | is-deploy-failed | デプロイ ステージに失敗しました |  |
| 202 |  | `./app/etc/env.php` ファイルは書き込み可能ではありません | デプロイメントスクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることはできません。 ファイルシステムの権限を確認します。 |
| 203 |  | 設定が`schema.yaml` ファイルで定義されていません | 設定が`./vendor/magento/ece-tools/config/schema.yaml` ファイルで定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 204 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 205 |  | `.magento.env.yaml` ファイルを読み取れません | ファイルの権限を確認します。 |
| 206 |  | `.schema.yaml` ファイルを読み取れません |  |
| 207 | ウォームアップ | 一部のウォームアップページをプリロードできませんでした |  |
| 208 | 最初のバイトまでの時間 | 最初のバイトまでの時間をテストできませんでした（TTFB） |  |
| 227 | clean-cache | コマンド `/bin/magento cache:flush`が失敗しました | 詳細については、`cloud.log`を確認してください。 `VERBOSE_COMMANDS: '-vvv'`を`.magento.env.yaml`に追加すると、より詳細なコマンド出力が得られます。 |

### 一般

| エラーコード | 一般的な手順 | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 243 |  | 設定が`schema.yaml` ファイルで定義されていません | 設定変数名が正しく、定義されていることを確認します。 |
| 244 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 245 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取れません。 ファイルの権限を確認します。 |
| 246 |  | `.schema.yaml` ファイルを読み取れません |  |
| 247 |  | イベント用のモジュールを生成できません | 詳細については、`cloud.log`を確認してください。 |
| 248 |  | イベント用のモジュールを有効にできません | 詳細については、`cloud.log`を確認してください。 |
| 249 |  | AdobeCommerceWebhookPlugins モジュールを生成できませんでした | 詳細については、`cloud.log`を確認してください。 |
| 250 |  | AdobeCommerceWebhookPlugins モジュールを有効にできませんでした | 詳細については、`cloud.log`を確認してください。 |

## 警告エラー

警告エラーは、Commerce on cloud infrastructure プロジェクトの設定に関する問題を示します。例えば、サイトの運用に影響を与える可能性のあるオプション機能について、誤った設定、非推奨、サポートされていない設定、設定が欠落している設定などです。 警告はデプロイメントの失敗の原因にはなりませんが、警告メッセージを確認し、設定を更新して解決する必要があります。

### ビルドステージ

| エラーコード | ビルドステップ | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 1001 | validate-config | ファイル app/etc/config.phpが存在しません |  |
| 1002 | validate-config | ./build_options.ini ファイルはサポートされなくなりました |  |
| 1003 | validate-config | 共有設定ファイルにモジュールセクションがありません |  |
| 1004 | validate-config | 設定は、このバージョンのMagentoと互換性がありません |  |
| 1005 | validate-config | SCD オプションが無視されました |  |
| 1006 | validate-config | 設定された状態は理想的ではありません |  |
| 1007 | ランベーラー | Baler JS バンドリングは使用できません |  |

### デプロイステージ

| エラーコード | デプロイステップ | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 2001 | プレデプロイ :cache | キャッシュは、使用できないRedis サービス用に設定されています。 設定は無視されます。 |  |
| 2032 | プレデプロイ :cache | キャッシュは、利用できないValkey サービス用に設定されています。 設定は無視されます。 |  |
| 2002 | validate-config | 設定された状態は理想的ではありません |  |
| 2003 | validate-config | エラーレポートのディレクトリネストレベルの値が設定されていません |  |
| 2004 | validate-config | ./pub/errors/local.xml ファイルの設定が無効です。 |  |
| 2005 | validate-config | 管理者データは、初期インストール時にのみ管理者ユーザーを作成するために使用されます。 管理者データに対する変更は、アップグレードプロセス中は無視されます。 | 最初のインストール後、設定から管理者データを削除できます。 |
| 2006 | validate-config | 管理者メールが設定されていないため、管理者ユーザーが作成されませんでした | インストール後、管理者ユーザーを手動で作成できます。sshを使用して環境に接続します。 次に、`bin/magento admin:user:create` コマンドを実行します。 |
| 2007 | validate-config | php バージョンを推奨バージョンに更新する |  |
| 2008 | validate-config | Solr サポートは、Adobe CommerceおよびMagento Open Source 2.1で廃止されました。 |  |
| 2009 | validate-config | Solrは、Adobe CommerceおよびMagento Open Source 2.2以降ではサポートされなくなりました。 |  |
| 2010 | validate-config | Elasticsearch サービスはインフラストラクチャ層にインストールされますが、検索エンジンとしては使用されません。 | リソース使用を最適化するために、インフラストラクチャ層からElasticsearchサービスを削除することを検討してください。 |
| 2011 | validate-config | インフラストラクチャ レイヤー上のElasticsearch サービス バージョンは、Adobe Commerce アプリケーションで使用されている現在のバージョンのelasticsearch/elasticsearch モジュールと互換性がありません。 |  |
| 2012 | validate-config | 現在の設定は、このバージョンのAdobe Commerceと互換性がありません |  |
| 2013 | validate-config | デプロイプロセスがビルド段階で実行されなかったため、SCD オプションは無視されました |  |
| 2014 | validate-config | この設定には、非推奨の変数または値が含まれています |  |
| 2015 | validate-config | 環境設定が無効です |  |
| 2016 | validate-config | JSON タイプ設定をデコードできません |  |
| 2017 | validate-config | 現在の設定は、このバージョンのAdobe Commerceと互換性がありません |  |
| 2018 | validate-config | 一部のサービスはEOLを通過しました |  |
| 2019 | validate-config | MySQL検索設定オプションは非推奨です | 代わりにElasticsearchを使用してください。 |
| 2029 | validate-config | Split DatabaseはAdobe CommerceおよびMagento Open Source 2.4.2で廃止され、2.5で削除されます。 | 分割データベースを使用する場合は、単一のデータベースに戻したり、単一のデータベースに移行したり、別のアプローチを使用したりする計画を開始する必要があります。 |
| 2020 | install-update | Adobe Commerceのインストールは完了しましたが、`app/etc/env.php`設定ファイルが見つからないか、空でした。 | 必要なデータは、環境設定と.magento.env.yaml ファイルから復元されます。 |
| 2021 | install-update:db-connection | カスタム接続を使用したスプリットデータベースの場合 |  |
| 2022 | install-update:db-connection | スレーブ接続と互換性のないデータベース設定に変更しました。 |  |
| 2023 | install-update:split-db | 分割データベースの有効化はスキップされます。 |  |
| 2024 | install-update:split-db | SPLIT_DB変数には、スプリット接続タイプの設定がありません。 |  |
| 2025 | install-update:split-db | スレーブ接続が設定されていません。 |  |
| 2026 | プレデプロイ :restore-writable-dirs | ビルド フェーズで生成されたデータの一部をマウントされたディレクトリに復元できませんでした | 詳細については、`cloud.log`を確認してください。 |
| 2027 | validate-config:mage-mode-variable | MAGE_MODE環境変数のモード値はサポートされていません | MAGE_MODE環境変数を削除するか、その値を「production」に変更します。 Adobe Commerce on cloud infrastructureでは、「実稼動」モードのみがサポートされます。 |
| 2028 | remote-storage | リモートストレージを有効にできませんでした。 | リモートストレージの資格情報を確認します。 |
| 2030 | validate-config | ElasticsearchとOpenSearch サービスは、両方ともインフラストラクチャ レイヤーにインストールされます。 Adobe CommerceおよびMagento Open Source 2.4.4以降では、デフォルトでOpenSearchが使用されます | リソース使用を最適化するために、インフラストラクチャ層からElasticsearchまたはOpenSearch サービスを削除することを検討してください。 |

### デプロイ後のステージ

| エラーコード | 配信後の手順 | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 3001 | validate-config | Adobe Commerceでデバッグログを有効にする | ディスク容量を節約するには、実稼動環境のデバッグログを有効にしないでください。 |
| 3002 | ウォームアップ | ストア URLを取得できません |  |
| 3003 | ウォームアップ | ストア URLを取得できません |  |
| 3004 | バックアップ | バックアップファイルを作成できません |  |

### 一般

| エラーコード | 一般的な手順 | エラーの説明（タイトル） | 推奨アクション |
| - | - | - | - |
| 4001 |  | システム プロセッサ数を取得できません： |  |
