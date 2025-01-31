---
title: ECE-Tools パッケージのエラーメッセージ
description: クラウドインフラストラクチャのビルド、デプロイ、デプロイ後のプロセスで、Adobe Commerce中に発生する可能性のあるエラーコードとメッセージのリストをご覧ください。
recommendations: noDisplay
role: Developer
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '2763'
ht-degree: 4%

---

# ECE ツールのエラーメッセージ

このエラーメッセージリファレンスでは、クラウドインフラストラクチャー上のAdobe Commerceのビルド、デプロイおよびデプロイ後のプロセスで発生する可能性のあるエラーのトラブルシューティングについて説明します。

デプロイ中に発生する重大なエラーメッセージや警告エラーメッセージはすべて、`var/log/cloud.log` ファイルと `/var/log/cloud.error.log` ファイルの両方に書き込まれます。 クラウドエラーログファイルには、最新のデプロイメントのエラーのみが含まれます。 空のファイルは、エラーのないデプロイメントが成功したことを示します。

`cloud.error.log` ファイルでは、解析しやすいように、各エントリが JSON 文字列としてフォーマットされています。

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

エラーメッセージは、デプロイメントステージ（ビルド、デプロイ、デプロイ後）の 1 つで分類されます。 各セクションには、関連するエラーのリストと、各エラーに関する次の情報が表示されます。

- **エラーコード**:Adobe Commerceによって割り当てられたエラーメッセージの識別情報
- **ステージ**：エラーがビルド、デプロイ、またはデプロイ後のステージで発生したかどうかを示します
- **手順**：エラーを返す可能性のあるデプロイメントシナリオの手順を示します。 _ステップ_ 列が空白の場合、エラーは一般的なエラーであり、複数のステップや前処理の操作中に返される可能性があります。 ビルド、デプロイ、デプロイ後の手順について詳しくは、[ シナリオベースのデプロイメント ](../deploy/scenario-based.md) を参照してください。
- **提案**：エラーのトラブルシューティングと解決のガイダンスを提供します。
- **タイトル（エラーの説明）**：エラーの原因を要約した説明
- **種類**：エラーが重大エラーか警告かを示します

<!-- Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository. -->

## 重大なエラー

重大なエラーは、クラウドインフラストラクチャプロジェクト上のCommerce設定の問題によって、デプロイメントエラーが発生する問題（例えば、設定が正しくない、サポートされていない、必要な設定が見つからないなど）を示します。 デプロイする前に、設定を更新してこれらのエラーを解決する必要があります。

### ステージの作成

| エラーコード | ビルドステップ | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 2 |  | `./app/etc/env.php` ファイルに書き込めません | 配置スクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることができません。 ファイルシステムの権限を確認します。 |
| 3 |  | `schema.yaml` ファイルで設定が定義されていません | `./vendor/magento/ece-tools/config/schema.yaml` ファイルで設定が定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 4 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 5 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取ることができません。 ファイルの権限を確認します。 |
| 6 |  | `.schema.yaml` ファイルを読み取れません | `./vendor/magento/ece-tools/config/magento.env.yaml` ファイルを読み取ることができません。 ファイルの権限を確認し、（`magento-cloud environment:redeploy`）を再デプロイします。 |
| 7 | refresh-modules | `./app/etc/config.php` ファイルに書き込めません | 配置スクリプトは、`/app/etc/config.php` ファイルに必要な変更を加えることができません。 ファイルシステムの権限を確認します。 |
| 8 | validate-config | `composer.json` ファイルを読み取れません | `./composer.json` ファイルを読み取ることができません。 ファイルの権限を確認します。 |
| 9 | validate-config | `composer.json` ファイルに必要な自動ロード セクションがありません | 必要な `autoload` セクションが `composer.json` ファイルにありません。 自動ロードセクションをクラウドテンプレート内の `composer.json` ファイルと比較し、不足している設定を追加します。 |
| 10 | validate-config | `.magento.env.yaml` ファイルに、スキーマで宣言されていないオプションが含まれているか、無効な値またはステージで構成されたオプションが含まれています | `./.magento.env.yaml` ファイルに無効な構成が含まれています。 詳細については、エラーログを確認してください。 |
| 11 | refresh-modules | コマンドが失敗しました：`/bin/magento module:enable --all` | `composer update` をローカルで実行してみてください。 次に、更新された `composer.lock` ファイルをコミットしてプッシュします。 また、詳しくは `cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 12 | apply-patches | パッチを適用できませんでした |  |
| 13 | set-report-dir-nesting-level | ファイル `/pub/errors/local.xml` に書き込めません |  |
| 14 | copy-sample-data | サンプルデータファイルをコピーできませんでした |  |
| 15 | compile-di | コマンドが失敗しました：`/bin/magento setup:di:compile` | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` に `VERBOSE_COMMANDS: '-vvv'` を追加してください。 |
| 16 | dump-autoload | コマンドが失敗しました：`composer dump-autoload` | `composer dump-autoload` コマンドが失敗しました。 詳しくは、`cloud.log` を確認してください。 |
| 17 | ランベーラー | Javascript バンドル用に `Baler` を実行するコマンドが失敗しました | `SCD_USE_BALER` 環境変数をチェックして、Baler モジュールが設定され、JS のバンドルが有効になっていることを確認します。 Baler モジュールが必要ない場合は、`SCD_USE_BALER: false` を設定します。 |
| 18 | compress-static-content | 必要なユーティリティが見つかりませんでした（タイムアウト、bash） |  |
| 19 | deploy-static-content | コマンド `/bin/magento setup:static-content:deploy` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 20 | compress-static-content | 静的コンテンツの圧縮に失敗しました | 詳しくは、`cloud.log` を確認してください。 |
| 21 | backup-data: static-content | 静的コンテンツを `init` ディレクトリにコピーできませんでした | 詳しくは、`cloud.log` を確認してください。 |
| 22 | backup-data: writable-dirs | 一部の書き込み可能ディレクトリを `init` ディレクトリにコピーできませんでした | 書き込み可能なディレクトリを `./init` フォルダーにコピーできませんでした。 ファイルシステムの権限を確認します。 |
| 23 |  | ロガーオブジェクトを作成できません |  |
| 24 | backup-data: static-content | `./init/pub/static/` ディレクトリをクリーンアップできませんでした | フォルダーを削除でき `./init/pub/static` せんでした。 ファイルシステムの権限を確認します。 |
| 25 |  | Composer パッケージが見つかりません | GitHub リポジトリから直接Adobe Commerce アプリケーションバージョンをインストールした場合は、`DEPLOYED_MAGENTO_VERSION_FROM_GIT` 環境変数が設定されていることを確認します。 |
| 26 | validate-config | Adobe CommerceおよびMagento Open Source 2.4 以降でサポートされなくなったMagentoBraintreeモジュール設定を削除します。 | Braintreeモジュールのサポートは、Magento 2.4.0 以降には含まれなくなりました。 `.magento.app.yaml` ファイルの variables セクションから CONFIG__STORES__DEFAULT__PAYMENT__VARIABLES__CHANNELBRAINTREEを削除します。 Braintree支払いサポートについては、代わりにCommerce Marketplaceの公式の内線番号を使用してください。 |

### ステージのデプロイ

| エラーコード | デプロイステップ | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 101 | 事前デプロイ：キャッシュ | キャッシュの構成が正しくありません（ポートまたはホストがありません） | キャッシュ構成に必須パラメーター `server` または `port` がありません。 詳しくは、`cloud.log` を確認してください。 |
| 102 |  | `./app/etc/env.php` ファイルに書き込めません | 配置スクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることができません。 ファイルシステムの権限を確認します。 |
| 103 |  | `schema.yaml` ファイルで設定が定義されていません | `./vendor/magento/ece-tools/config/schema.yaml` ファイルで設定が定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 104 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./vendor/magento/ece-tools/config/schema.yaml` ファイルで設定が定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 105 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取ることができません。 ファイルの権限を確認します。 |
| 106 |  | `.schema.yaml` ファイルを読み取れません |  |
| 107 | pre-deploy: clean-redis-cache | Redis キャッシュをクリーンアップできませんでした | Redis キャッシュをクリーンアップできませんでした。 Redis のキャッシュ構成が正しく、Redis サービスが利用可能であることを確認します。 [Redis サービスの設定 ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/redis.html) を参照してください。 |
| 108 | pre-deploy: set-production-mode | コマンド `/bin/magento maintenance:enable` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 109 | validate-config | データベース設定が正しくありません | `DATABASE_CONFIGURATION` 環境変数が正しく設定されていることを確認します。 |
| 110 | validate-config | セッション設定が正しくありません | `SESSION_CONFIGURATION` 環境変数が正しく設定されていることを確認します。 設定には、少なくとも `save` パラメーターを含める必要があります。 |
| 111 | validate-config | 検索設定が正しくありません | `SEARCH_CONFIGURATION` 環境変数が正しく設定されていることを確認します。 設定には、少なくとも `engine` パラメーターを含める必要があります。 |
| 112 | validate-config | リソース設定が正しくありません | `RESOURCE_CONFIGURATION` 環境変数が正しく設定されていることを確認します。 設定には、少なくとも `connection` パラメーターを含める必要があります。 |
| 113 | validate-config:elasticsuite-integrity | ElasticSuite がインストールされていますが、Elasticsearchサービスを利用できません | `SEARCH_CONFIGURATION`Elasticsearch変数が正しく設定されていること、および環境サービスが使用可能であることを確認します。 |
| 114 | validate-config:elasticsuite-integrity | ElasticSuite はインストールされていますが、別の検索エンジンが使用されています | ElasticSuite はインストールされていますが、別の検索エンジンが設定されています。 `SEARCH_CONFIGURATION` 環境変数を更新してElasticsearchを有効にし、`services.yaml` ファイルでElasticsearchサービスの設定を確認します。 |
| 115 |  | データベースクエリの実行に失敗しました |  |
| 116 | install-update: setup | コマンド `/bin/magento setup:install` が失敗しました | 詳細については、`cloud.log` および `install_upgrade.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 117 | install-update:config-import | コマンド `app:config:import` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 118 |  | 必要なユーティリティが見つかりませんでした（タイムアウト、bash） |  |
| 119 | install-update: deploy-static-content | コマンド `/bin/magento setup:static-content:deploy` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 120 | compress-static-content | 静的コンテンツの圧縮に失敗しました | 詳しくは、`cloud.log` を確認してください。 |
| 121 | deploy-static-content:generate | デプロイされたバージョンを更新できません | `./pub/static/deployed_version.txt` ファイルを更新できません。 ファイルシステムの権限を確認します。 |
| 122 | clean-static-content | 静的コンテンツファイルをクリーンアップできませんでした |  |
| 123 | install-update:split-db | コマンド `/bin/magento setup:db-schema:split` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 124 | clean-view-preprocessed | `var/view_preprocessed` フォルダーをクリーンアップできませんでした | `./var/view_preprocessed` フォルダーをクリーンアップできません。 ファイルシステムの権限を確認します。 |
| 125 | install-update: reset-password | `/var/credentials_email.txt` ファイルを更新できませんでした | `/var/credentials_email.txt` ファイルを更新できませんでした。 ファイルシステムの権限を確認します。 |
| 126 | install-update:update | コマンド `/bin/magento setup:upgrade` が失敗しました | 詳細については、`cloud.log` および `install_upgrade.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 127 | キャッシュのクリーンアップ | コマンド `/bin/magento cache:flush` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` ファイルに `VERBOSE_COMMANDS: '-vvv'` オプションを追加してください。 |
| 128 | disable-maintenance-mode | コマンド `/bin/magento maintenance:disable` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` に `VERBOSE_COMMANDS: '-vvv'` を追加してください。 |
| 129 | install-update: reset-password | パスワードリセットテンプレートを読み取れません |  |
| 130 | install-update:cache_type | コマンドが失敗しました：`php ./bin/magento cache:enable` | コマンド `php ./bin/magento cache:enable` は、Adobe Commerceがインストールされてい `./app/etc/env.php` が、デプロイメントの開始時にファイルが存在しないか空の場合にのみ実行されます。 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` に `VERBOSE_COMMANDS: '-vvv'` を追加してください。 |
| 131 | install-update | `crypt/key` キーの値が `./app/etc/env.php` ファイルまたは `CRYPT_KEY` クラウド環境変数に存在しません | このエラーは、Adobe Commerceのデプロイメント開始時に `./app/etc/env.php` ファイルが存在しない場合、または `crypt/key` の値が定義されていない場合に発生します。 別の環境からデータベースを移行した場合は、その環境から暗号化キーの値を取得します。 次に、現在の環境のクラウド環境変数 [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy.html#crypt_key) に値を追加します。 [Adobe Commerce暗号化キー ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/develop/overview.html#gather-credentials) を参照してください。 `./app/etc/env.php` ファイルを誤って削除した場合は、次のコマンドを使用して、以前のデプロイメントで作成されたバックアップファイルから復元します。`./vendor/bin/ece-tools backup:restore` CLI コマンド」 |
| 132 |  | Elasticsearchサービスに接続できません | 有効なElasticsearch資格情報を確認し、サービスが実行中であることを確認します |
| 137 |  | OpenSearch サービスに接続できません | 有効な OpenSearch 認証情報を確認し、サービスが実行中であることを確認します |
| 133 | validate-config | Adobe CommerceまたはMagento Open Source 2.4 以降でサポートされなくなったMagentoBraintreeモジュール設定を削除します。 | Braintreeモジュールのサポートは、Adobe CommerceまたはMagento Open Source 2.4.0 以降には含まれなくなりました。 `.magento.app.yaml` ファイルの variables セクションから CONFIG__STORES__DEFAULT__PAYMENT__VARIABLES__CHANNELBRAINTREEを削除します。 Braintreeのサポートについては、Commerce Marketplaceの公式のBraintree支払い拡張機能を使用します。 |
| 134 | validate-config | Adobe CommerceおよびMagento Open Source 2.4.0 では、Elasticsearchサービスをインストールする必要があります | Elasticsearchサービスのインストール |
| 138 | validate-config | Adobe CommerceおよびMagento Open Source 2.4.4 では、OpenSearch またはElasticsearchサービスをインストールする必要があります | OpenSearch サービスのインストール |
| 135 | validate-config | 検索エンジンは、Adobe CommerceのElasticsearchおよびMagento Open Source >= 2.4.0 に設定する必要があります | `engine` オプションの SEARCH_CONFIGURATION 変数を確認します。 設定されている場合は、オプションを削除するか、値を「elasticsearch」に設定します。 |
| 136 | validate-config | Split Database は、Adobe CommerceおよびMagento Open Source 2.5.0 以降で削除されました。 | 分割データベースを使用している場合は、単一のデータベースに戻すか、単一のデータベースに移行するか、別の方法を使用する必要があります。 |
| 139 | validate-config | 検索エンジンが正しくありません | このAdobe CommerceまたはMagento Open Sourceのバージョンでは、OpenSearch がサポートされていません。 バージョン 2.3.7-p3、2.4.3-p2 以降を使用する必要があります |

### デプロイ後のステージ

| エラーコード | デプロイ後手順 | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 201 | is-deploy-failed | ステージのデプロイに失敗しました |  |
| 202 |  | `./app/etc/env.php` ファイルは書き込み可能ではありません | 配置スクリプトは、`/app/etc/env.php` ファイルに必要な変更を加えることができません。 ファイルシステムの権限を確認します。 |
| 203 |  | `schema.yaml` ファイルで設定が定義されていません | `./vendor/magento/ece-tools/config/schema.yaml` ファイルで設定が定義されていません。 設定変数名が正しく、定義されていることを確認します。 |
| 204 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 205 |  | `.magento.env.yaml` ファイルを読み取れません | ファイルの権限を確認します。 |
| 206 |  | `.schema.yaml` ファイルを読み取れません |  |
| 207 | ウォームアップ | いくつかのウォームアップページをプリロードできませんでした |  |
| 208 | 最初のバイトまでの時間 | 最初のバイト （TTFB）への時間のテストに失敗しました |  |
| 227 | キャッシュのクリーンアップ | コマンド `/bin/magento cache:flush` が失敗しました | 詳しくは、`cloud.log` を確認してください。 コマンド出力の詳細については、`.magento.env.yaml` に `VERBOSE_COMMANDS: '-vvv'` を追加してください。 |

### 一般

| エラーコード | 一般的な手順 | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 243 |  | `schema.yaml` ファイルで設定が定義されていません | 設定変数名が正しく、定義されていることを確認します。 |
| 244 |  | `.magento.env.yaml` ファイルを解析できませんでした | `./.magento.env.yaml` ファイル形式が無効です。 YAML パーサーを使用して構文を確認し、エラーを修正します。 |
| 245 |  | `.magento.env.yaml` ファイルを読み取れません | `./.magento.env.yaml` ファイルを読み取ることができません。 ファイルの権限を確認します。 |
| 246 |  | `.schema.yaml` ファイルを読み取れません |  |
| 247 |  | イベント用のモジュールを生成できません | 詳しくは、`cloud.log` を確認してください。 |
| 248 |  | イベント用のモジュールを有効にできません | 詳しくは、`cloud.log` を確認してください。 |
| 249 |  | AdobeCommerceWebhookPlugins モジュールを生成できませんでした | 詳しくは、`cloud.log` を確認してください。 |
| 250 |  | AdobeCommerceWebhookPlugins モジュールを有効にできませんでした | 詳しくは、`cloud.log` を確認してください。 |

## 警告エラー

警告エラーは、クラウドインフラストラクチャプロジェクト上のCommerceの設定に関する問題（誤った、非推奨、サポートされていない、オプション機能の設定が見つからないなど、サイトの操作に影響を与える可能性がある）を示します。 警告によって配置の失敗が発生することはありませんが、警告メッセージを確認し、それらを解決するために構成を更新する必要があります。

### ステージの作成

| エラーコード | ビルドステップ | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 1001 | validate-config | ファイル app/etc/config.phpは存在しません |  |
| 1002 | validate-config | を使用します。/build_options.ini ファイルはサポートされなくなりました |  |
| 1003 | validate-config | 共有構成ファイルにモジュール セクションがありません |  |
| 1004 | validate-config | この設定は、このバージョンのMagentoと互換性がありません |  |
| 1005 | validate-config | 無視された SCD オプション |  |
| 1006 | validate-config | 設定された状態は理想的ではありません |  |
| 1007 | ランベーラー | Baler JS のバンドルは使用できません |  |

### ステージのデプロイ

| エラーコード | デプロイステップ | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 2001 | pre-deploy:cache | キャッシュは、使用できない Redis サービス用に設定されています。 設定は無視されます。 |  |
| 2002 | validate-config | 設定された状態は理想的ではありません |  |
| 2003 | validate-config | エラーレポート用のディレクトリのネスティングレベル値が設定されていません |  |
| 2004 | validate-config | での設定が無効です。/pub/errors/local.xml ファイル。 |  |
| 2005 | validate-config | 管理データは、初期インストール時にのみ管理者ユーザーを作成するために使用されます。 アップグレードプロセス中、管理者データに対する変更は無視されます。 | 最初のインストール後、管理者データを設定から削除できます。 |
| 2006 | validate-config | 管理者の電子メールが設定されていなかったので、管理者ユーザーは作成されませんでした | インストール後、管理者ユーザーを手動で作成できます。ssh を使用して環境に接続します。 次に、`bin/magento admin:user:create` コマンドを実行します。 |
| 2007 | validate-config | php のバージョンを推奨バージョンに更新する |  |
| 2008 | validate-config | Solr のサポートは、Adobe CommerceおよびMagento Open Source 2.1 で非推奨（廃止予定）となりました。 |  |
| 2009 | validate-config | Solr は、Adobe CommerceおよびMagento Open Source 2.2 以降ではサポートされなくなりました。 |  |
| 2010 | validate-config | Elasticsearchサービスは、インフラストラクチャレイヤーにインストールされますが、検索エンジンとしては使用されません。 | リソースの使用を最適化するには、インフラストラクチャレイヤーからElasticsearchサービスを削除することを検討してください。 |
| 2011 | validate-config | インフラストラクチャレイヤーのElasticsearchサービスバージョンは、Adobe Commerce アプリケーションで使用されている現在のバージョンの elasticsearch/elasticsearch モジュールと互換性がありません。 |  |
| 2012 | validate-config | 現在の設定は、このバージョンのAdobe Commerceと互換性がありません |  |
| 2013 | validate-config | デプロイプロセスがビルドフェーズで実行されなかったので、SCD オプションは無視されました |  |
| 2014 | validate-config | 設定に、非推奨の変数または値が含まれています |  |
| 2015 | validate-config | 環境設定が無効です |  |
| 2016 | validate-config | JSON タイプの設定をデコードできません |  |
| 2017 | validate-config | 現在の設定は、このバージョンのAdobe Commerceと互換性がありません |  |
| 2018 | validate-config | 一部のサービスは提供終了（EOL）になりました |  |
| 2019 | validate-config | MySQL 検索設定オプションは非推奨です | 代わりに、Elasticsearchを使用します。 |
| 2029 | validate-config | Split Database は、Adobe CommerceおよびMagento Open Source 2.4.2 で非推奨となり、2.5 で削除される予定です。 | 分割データベースを使用している場合は、単一のデータベースに戻す、単一のデータベースに移行する、または別の方法を使用する計画を開始する必要があります。 |
| 2020 | install-update | Adobe Commerceのインストールは完了しましたが、`app/etc/env.php` 設定ファイルが見つからないか、空でした。 | 必須データは、環境設定および.magento.env.yaml ファイルから復元されます。 |
| 2021 | install-update:db-connection | カスタム接続を使用する分割データベースの場合 |  |
| 2022 | install-update:db-connection | スレーブ接続と互換性のないデータベース構成に変更しました。 |  |
| 2023 | install-update:split-db | 分割データベースの有効化はスキップされます。 |  |
| 2024 | install-update:split-db | SPLIT_DB 変数に分割接続タイプの構成がありません。 |  |
| 2025 | install-update:split-db | スレーブ接続が設定されていません。 |  |
| 2026 | pre-deploy:restore-writable-dirs | ビルド フェーズで生成された一部のデータをマウントされたディレクトリに復元できませんでした | 詳しくは、`cloud.log` を確認してください。 |
| 2027 | validate-config:mage-mode-variable | MAGE_MODE 環境変数のモード値はサポートされていません | MAGE_MODE 環境変数を削除するか、その値を「production」に変更します。 クラウドインフラストラクチャー上のAdobe Commerceでは、「実稼動」モードのみをサポートします。 |
| 2028 | remote-storage | リモート記憶域を有効にできませんでした。 | リモートストレージの資格情報を確認します。 |
| 2030 | validate-config | Elasticsearchサービスと OpenSearch サービスは、どちらもインフラストラクチャレイヤーにインストールされます。 Adobe CommerceおよびMagento Open Source 2.4.4 以降では、デフォルトで OpenSearch を使用します | リソースの使用状況を最適化するには、インフラストラクチャレイヤーからElasticsearchサービスまたは OpenSearch サービスを削除することを検討してください。 |

### デプロイ後のステージ

| エラーコード | デプロイ後手順 | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 3001 | validate-config | Adobe Commerceではデバッグログが有効になっています | ディスク容量を節約するには、実稼動環境のデバッグログを有効にしないでください。 |
| 3002 | ウォームアップ | ストア URL を取得できません |  |
| 3003 | ウォームアップ | ストア URL を取得できません |  |
| 3004 | バックアップ | バックアップ ファイルを作成できません |  |

### 一般

| エラーコード | 一般的な手順 | エラーの説明（タイトル） | 推奨されるアクション |
| - | - | - | - |
| 4001 |  | システム プロセッサ数を取得できません： |  |
