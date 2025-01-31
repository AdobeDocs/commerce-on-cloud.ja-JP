---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud （クラウドインフラストラクチャー上のAdobe Commerce）

<!-- The template to render with above values -->

**バージョン**:1.46.1

このリファレンスには、`magento-cloud` のコマンド ライン ツールで使用できる 119 個のコマンドが含まれています。
最初のリストは、クラウドインフラストラクチャ上のAdobe Commerceで `magento-cloud list` コマンドを使用して自動生成されます。

## 一般

この参照は、アプリケーションコードベースから生成されます。 コンテンツを変更するには、[codebase](https://github.com/magento/magento-cloud-cli) リポジトリ内の対応するコマンド実装のソースコードを更新し、変更点をレビュー用に送信します。 もう 1 つの方法は、_フィードバックを提供_ （右上のリンクを見つけます）です。 投稿のガイドラインについては、[ コードの投稿 ](https://developer.adobe.com/commerce/contributor/guides/code-contributions/) を参照してください。

### グローバルオプション

#### `--help`, `-h`

このヘルプ メッセージを表示する

- デフォルト：`false`
- 値を受け入れません

#### `--verbose`, `-v|-vv|-vvv`

メッセージの冗長性の向上

- デフォルト：`false`
- 値を受け入れません

#### `--version`, `-V`

このアプリケーションのバージョンを表示

- デフォルト：`false`
- 値を受け入れません

#### `--yes`, `-y`

確認の質問には「はい」と答え、他の質問ではデフォルト値をそのまま使用し、インタラクションを無効にします。

- デフォルト：`false`
- 値を受け入れません

#### `--no-interaction`

インタラクティブな質問はしないでください。デフォルト値を使用します。 次のMAGENTO変数を使用した場合と同じ：environment_CLOUD_CLI_NO_INTERACTION=1

- デフォルト：`false`
- 値を受け入れません


## `clear-cache`

```bash
magento-cloud magento-cloud cc
```

CLI のキャッシュをクリアする

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `decode`

```bash
magento-cloud magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Decode_CLOUD_VARIABLES などのエンコードされたMAGENTOをデコードします

### 引数

#### `value`

デコードする変数値

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

変数内に表示するプロパティ

- 値が必要です


## `docs`

```bash
magento-cloud magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

オンラインドキュメントを開く

### 引数

#### `search`

検索語句

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--browser`

URL を開くために使用するブラウザー。 何も設定しない場合は 0 を設定します。

- 値が必要です

#### `--pipe`

URL を stdout に出力します。

- デフォルト：`false`
- 値を受け入れません


## `help`

```bash
magento-cloud magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

コマンドのヘルプを表示します

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### 引数

#### `command_name`

コマンド名

- デフォルト：`help`

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式（txt、json、md）

- デフォルト：`txt`
- 値が必要です

#### `--raw`

生のコマンド ヘルプを出力するには

- デフォルト：`false`
- 値を受け入れません


## `list`

```bash
magento-cloud magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

コマンドを一覧表示します

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### 引数

#### `command`

実行するコマンド

- 必須


#### `namespace`

名前空間名

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--raw`

生のコマンド リストを出力するには

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式（txt、xml、json、md）

- デフォルト：`txt`
- 値が必要です

#### `--all`

非表示のコマンドも含めて、すべてのコマンドを表示する

- デフォルト：`false`
- 値を受け入れません


## `multi`

```bash
magento-cloud magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

複数のプロジェクトでのコマンドの実行

### 引数

#### `cmd`

実行するコマンド

- デフォルト：`[]`
- 必須

- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--projects`, `-p`

コンマや空白で区切られたプロジェクト ID のリスト

- 値が必要です

#### `--continue`

例外が発生した場合でも、コマンドの実行を続行する

- デフォルト：`false`
- 値を受け入れません

#### `--sort`

プロジェクトオプションのリストを並べ替えるプロパティ

- デフォルト：`title`
- 値が必要です

#### `--reverse`

プロジェクト オプションの順序を逆にする

- デフォルト：`false`
- 値を受け入れません


## `web`

```bash
magento-cloud magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Web UI でプロジェクトを開きます

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--browser`

URL を開くために使用するブラウザー。 何も設定しない場合は 0 を設定します。

- 値が必要です

#### `--pipe`

URL を stdout に出力します。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `activity:cancel`

```bash
magento-cloud magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

アクティビティのキャンセル

### 引数

#### `id`

アクティビティ ID。 デフォルトでは、キャンセル可能な最新のアクティビティに設定されています。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--type`, `-t`

タイプでフィルターします（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプのワイルドカードとして使用できます（例：&#39;%var%&#39;）。変数関連のアクティビティを選択します。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプで除外（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `activity:get`

```bash
magento-cloud magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

単一のアクティビティの詳細情報の表示

### 引数

#### `id`

アクティビティ ID。 デフォルトは最新のアクティビティです。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--type`, `-t`

タイプでフィルターします（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプのワイルドカードとして使用できます（例：&#39;%var%&#39;）。変数関連のアクティビティを選択します。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプで除外（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--state`

状態でフィルター（デフォルトのアクティビティを選択する場合）:in_progress、pending、complete または cancelled。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--result`

結果でフィルター（デフォルトアクティビティを選択する場合）：成功または失敗

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを含める（デフォルトアクティビティを選択する場合）。 —state=in_progress,pending の略記法です。

- デフォルト：`false`
- 値を受け入れません

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `activity:list`

```bash
magento-cloud magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境またはプロジェクトのアクティビティのリストを取得する

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--type`, `-t`

タイプ別にアクティビティをフィルター値は、コンマ（「a,b,c」など）および空白で分割できます。 アクティビティ名の最初の部分は省略できます。例えば、「cron」は「environment.cron」アクティビティを選択できます。 % または*文字はワイルドカードとして使用できます。例：&#39;%var%&#39;。変数関連のアクティビティを選択します。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別のアクティビティを除外します。 値は、コンマ（「a,b,c」など）や空白で分割できます。 アクティビティ名の最初の部分は省略できます。例えば、「cron」は「environment.cron」アクティビティを除外できます。 % または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--limit`

表示する結果の数を制限する

- デフォルト：`10`
- 値が必要です

#### `--start`

この日付より前に作成されたアクティビティのみがリストされます

- 値が必要です

#### `--state`

アクティビティを状態（in_progress、pending、complete または cancelled）でフィルタリングします。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--result`

結果（成功または失敗）でアクティビティをフィルター

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみをリスト

- デフォルト：`false`
- 値を受け入れません

#### `--all`, `-a`

すべての環境のアクティビティのリスト

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、created*、description*、progress*、state*、result*、completed、environments、type （* = default columns）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `activity:log`

```bash
magento-cloud magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

アクティビティのログの表示

### 引数

#### `id`

アクティビティ ID。 デフォルトは最新のアクティビティです。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

アクティビティの更新間隔（秒）。 0 に設定すると、更新が無効になります。

- デフォルト：`3`
- 値が必要です

#### `--timestamps`, `-t`

各メッセージの横にタイムスタンプを表示します

- デフォルト：`false`
- 値を受け入れません

#### `--type`

タイプでフィルターします（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプのワイルドカードとして使用できます（例：&#39;%var%&#39;）。変数関連のアクティビティを選択します。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプで除外（デフォルトアクティビティを選択する場合）。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--state`

状態でフィルター（デフォルトのアクティビティを選択する場合）:in_progress、pending、complete または cancelled。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--result`

結果でフィルター（デフォルトアクティビティを選択する場合）：成功または失敗

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを含める（デフォルトアクティビティを選択する場合）。 —state=in_progress,pending の略記法です。

- デフォルト：`false`
- 値を受け入れません

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `app:config-get`

```bash
magento-cloud magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

アプリの設定を表示します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示する設定プロパティ

- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--identity-file`, `-i`

[ 非推奨オプション、使用されなくなりました ]

- 値が必要です


## `app:list`

```bash
magento-cloud magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

プロジェクト内のアプリのリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--pipe`

アプリ名のリストのみを出力

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：name*、type*、disk、path、size （* = デフォルトの列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `auth:api-token-login`

```bash
magento-cloud magento-cloud auth:api-token-login
```

API トークンを使用してMagentoクラウドにログインします

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `auth:browser-login`

```bash
magento-cloud magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

ブラウザーでMagentoクラウドにログインします。

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--force`, `-f`

既にログインしている場合も、再度ログインします

- デフォルト：`false`
- 値を受け入れません

#### `--browser`

URL を開くために使用するブラウザー。 何も設定しない場合は 0 を設定します。

- 値が必要です

#### `--pipe`

URL を stdout に出力します。

- デフォルト：`false`
- 値を受け入れません


## `auth:info`

```bash
magento-cloud magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

アカウント情報の表示

### 引数

#### `property`

表示するアカウントプロパティ

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--no-auto-login`

自動ログインをスキップします。 ログインしていない場合は何も出力されず、その他のエラーがないと仮定すると終了コードは 0 になります。

- デフォルト：`false`
- 値を受け入れません

#### `--property`, `-P`

表示するアカウントプロパティ （代替構文）

- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `auth:logout`

```bash
magento-cloud magento-cloud logout [-a|--all] [--other]
```

Magentoクラウドからログアウトします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--all`, `-a`

すべてのローカルセッションからログアウトする

- デフォルト：`false`
- 値を受け入れません

#### `--other`

他のローカルセッションからのログアウト

- デフォルト：`false`
- 値を受け入れません


## `blackfire:setup`

```bash
magento-cloud magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトのBlackfire.io 統合の設定

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--server_id`

サーバー ID

- 値が必要です

#### `--server_token`

サーバートークン

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `certificate:add`

```bash
magento-cloud magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトに SSL 証明書を追加する

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--cert`

証明書ファイルへのパス

- 値が必要です

#### `--key`

証明書の秘密キーファイルのパス

- 値が必要です

#### `--chain`

証明書チェーン ファイルへのパス

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `certificate:delete`

```bash
magento-cloud magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

プロジェクトから証明書を削除する

### 引数

#### `id`

証明書 ID （またはその開始）

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `certificate:get`

```bash
magento-cloud magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

証明書の表示

### 引数

#### `id`

証明書 ID （またはその開始）

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示する証明書プロパティ

- 値が必要です

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `certificate:list`

```bash
magento-cloud magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクト証明書のリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--domain`

ドメイン名でフィルター（大文字と小文字を区別しない検索）

- 値が必要です

#### `--exclude-domain`

証明書を除外、ドメイン名で一致（大文字と小文字を区別しない検索）

- 値が必要です

#### `--issuer`

発行者でフィルター

- 値が必要です

#### `--only-auto`

自動プロビジョニングされた証明書のみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--no-auto`

手動で追加された証明書のみを表示する

- デフォルト：`false`
- 値を受け入れません

#### `--ignore-expiry`

期限切れの証明書と期限切れでない証明書の両方を表示する

- デフォルト：`false`
- 値を受け入れません

#### `--only-expired`

期限切れの証明書のみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--no-expired`

期限切れでない証明書のみを表示（デフォルト）

- デフォルト：`false`
- 値を受け入れません

#### `--pipe-domains`

証明書でカバーされるドメイン名のリストのみを返す

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：作成済み、ドメイン、有効期限、ID、発行者。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `commit:get`

```bash
magento-cloud magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

コミットの詳細を表示

### 引数

#### `commit`

コミット SHA です。 親コミットの場合は「HEAD」やキャレット（^）またはチルダ（～）を使用できます。

- デフォルト：`HEAD`

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示するコミットプロパティ。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `commit:list`

```bash
magento-cloud magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

コミットのリスト

### 引数

#### `commit`

開始時の Git コミット SHA。 親コミットの場合は「HEAD」やキャレット（^）またはチルダ（～）を使用できます。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--limit`

表示するコミットの数。

- デフォルト：`10`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：author、date、sha、summary。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `db:dump`

```bash
magento-cloud magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

リモートデータベースのローカルダンプを作成します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--schema`

ダンプするスキーマ。 デフォルトのスキーマ（通常は「main」）を使用する場合は省略します。

- 値が必要です

#### `--file`, `-f`

ダンプのカスタムファイル名

- 値が必要です

#### `--directory`, `-d`

ダンプのカスタムディレクトリ

- 値が必要です

#### `--gzip`, `-z`

gzip を使用してダンプを圧縮します

- デフォルト：`false`
- 値を受け入れません

#### `--timestamp`, `-t`

ダンプファイル名にタイムスタンプを追加します

- デフォルト：`false`
- 値を受け入れません

#### `--stdout`, `-o`

ファイルではなく STDOUT に出力

- デフォルト：`false`
- 値を受け入れません

#### `--table`

含めるテーブル

- デフォルト：`[]`
- 値が必要です

#### `--exclude-table`

除外するテーブル

- デフォルト：`[]`
- 値が必要です

#### `--schema-only`

スキーマのみをダンプし、データはダンプしない

- デフォルト：`false`
- 値を受け入れません

#### `--charset`

ダンプの文字セットエンコーディング

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `db:size`

```bash
magento-cloud magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

データベースのディスク使用量の見積もり

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示します。

- デフォルト：`false`
- 値を受け入れません

#### `--cleanup`, `-C`

テーブルをクリーンアップできるかどうかを確認し、推奨事項を表示します（InnoDb のみ）。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：max、percent_used、used。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `db:sql`

```bash
magento-cloud magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

リモート・データベースで SQL を実行する

### 引数

#### `query`

実行する SQL 文

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--raw`

生の非表形式出力を生成する

- デフォルト：`false`
- 値を受け入れません

#### `--schema`

使用するスキーマ。 デフォルトのスキーマ（通常は「main」）を使用する場合は省略します。 スキーマを使用しない場合、空の文字列を渡します。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `domain:add`

```bash
magento-cloud magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

新しいドメインをプロジェクトに追加する

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--cert`

このドメインの証明書ファイルへのパス

- 値が必要です

#### `--key`

指定された証明書の秘密鍵ファイルへのパス。

- 値が必要です

#### `--chain`

指定された証明書の証明書チェーンファイルまたはファイルへのパス

- デフォルト：`[]`
- 値が必要です

#### `--attach`

環境のルートでこのドメインが置き換える実稼動ドメイン。 実稼動以外の環境ドメインに必要です。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `domain:delete`

```bash
magento-cloud magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

プロジェクトからドメインを削除する

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `domain:get`

```bash
magento-cloud magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

ドメインの詳細情報を表示する

### 引数

#### `name`

ドメイン名

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示するドメインプロパティ

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `domain:list`

```bash
magento-cloud magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

すべてのドメインのリストの取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：name*、ssl*、created_at*、registered_name、replacement_for、type、updated_at （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `domain:update`

```bash
magento-cloud magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

ドメインの更新

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--cert`

このドメインの証明書ファイルへのパス

- 値が必要です

#### `--key`

指定された証明書の秘密鍵ファイルへのパス。

- 値が必要です

#### `--chain`

指定された証明書の証明書チェーンファイルまたはファイルへのパス

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:activate`

```bash
magento-cloud magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

環境のアクティブ化

### 引数

#### `environment`

アクティブ化する環境

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--parent`

アクティブ化する前に新しい環境の親を設定

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:branch`

```bash
magento-cloud magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

環境のブランチ

### 引数

#### `id`

新しい環境の ID （ブランチ名）


#### `parent`

新しい環境の親

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--title`

新しい環境のタイトル

- 値が必要です

#### `--type`

新しい環境のタイプ

- 値が必要です

#### `--no-clone-parent`

親環境のデータのクローンを作成しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:checkout`

```bash
magento-cloud magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

環境のチェックアウト

### 引数

#### `id`

チェックアウトする環境の ID。 例：「sprint2」

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `environment:delete`

```bash
magento-cloud magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

1 つ以上の環境を削除

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### 引数

#### `environment`

削除する環境。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--delete-branch`

非アクティブな環境の Git ブランチを確認なしで削除

- デフォルト：`false`
- 値を受け入れません

#### `--no-delete-branch`

Git ブランチ（非アクティブな環境）を削除しないでください

- デフォルト：`false`
- 値を受け入れません

#### `--type`

タイプのすべての環境を削除（選択されたその他のものに追加）値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--only-type`, `-t`

特定のタイプの削除環境のみ。値は、コンマ（「a,b,c」など）および空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--exclude`

削除しない環境。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`

値を削除しない環境タイプは、コンマ（「a,b,c」など）および空白（またはその両方）で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--inactive`

非アクティブな環境をすべて削除（選択されたその他のに追加）

- デフォルト：`false`
- 値を受け入れません

#### `--merged`

すべての結合環境を削除（選択されたその他のに追加）

- デフォルト：`false`
- 値を受け入れません

#### `--allow-delete-parent`

子を持つ環境の削除を許可

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:http-access`

```bash
magento-cloud magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境の HTTP アクセス設定の更新

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--access`

「permission:address」形式でのアクセス制限。 すべてのアドレスをクリアするには 0 を使用します。

- デフォルト：`[]`
- 値が必要です

#### `--auth`

「username:password」の形式の HTTP 基本認証資格情報。 0 を使用して、すべての資格情報をクリアします。

- デフォルト：`[]`
- 値が必要です

#### `--enabled`

アクセス制御を有効にするかどうか：1 を有効にする、0 を無効にする

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:info`

```bash
magento-cloud magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

環境のプロパティの読み取りまたは設定

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:init`

```bash
magento-cloud magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

公開 Git リポジトリからの環境の初期化

### 引数

#### `url`

Git リポジトリへの URL

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--profile`

プロファイルの名前

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:list`

```bash
magento-cloud magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

環境のリストの取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--no-inactive`, `-I`

非アクティブな環境は表示しない

- デフォルト：`false`
- 値を受け入れません

#### `--pipe`

環境 ID の簡単なリストを出力します。

- デフォルト：`false`
- 値を受け入れません

#### `--refresh`

リストを更新するかどうか。

- デフォルト：`1`
- 値が必要です

#### `--sort`

並べ替えの基準にするプロパティ

- デフォルト：`title`
- 値が必要です

#### `--reverse`

逆順（降順）で並べ替え

- デフォルト：`false`
- 値を受け入れません

#### `--type`

環境タイプでリストをフィルタリングします。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、title*、status*、type*、created、machine_name、updated （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `environment:logs`

```bash
magento-cloud magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

環境のログの読み取り

### 引数

#### `type`

ログのタイプ （例：「アクセス」、「エラー」）

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--lines`

表示するライン数

- デフォルト：`100`
- 値が必要です

#### `--tail`

ログを継続的にテール

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `environment:merge`

```bash
magento-cloud magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

環境の結合

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### 引数

#### `environment`

結合する環境

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:pause`

```bash
magento-cloud magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境の一時停止

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:push`

```bash
magento-cloud magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

環境へのコードのプッシュ

### 引数

#### `source`

ソース参照：ブランチ名またはコミットハッシュ

- デフォルト：`HEAD`

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--target`

ターゲットのブランチ名。 デフォルトは現在のブランチです。

- 値が必要です

#### `--force`, `-f`

早送り以外の更新を許可

- デフォルト：`false`
- 値を受け入れません

#### `--force-with-lease`

リモートトラッキングブランチが最新の場合、非早送り更新を許可します

- デフォルト：`false`
- 値を受け入れません

#### `--set-upstream`, `-u`

ターゲット環境をソースブランチのアップストリームとして設定します。 これにより、ターゲットプロジェクトがローカルリポジトリのリモートとして設定されます。

- デフォルト：`false`
- 値を受け入れません

#### `--activate`

プッシュする前に環境をアクティベート

- デフォルト：`false`
- 値を受け入れません

#### `--parent`

新しい環境の親を設定します（– activate と共にのみ使用）

- 値が必要です

#### `--type`

環境のタイプを設定します（—activate でのみ使用）。

- 値が必要です

#### `--no-clone-parent`

親ブランチのデータのクローンを作成しない（—activate でのみ使用）

- デフォルト：`false`
- 値を受け入れません

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `environment:redeploy`

```bash
magento-cloud magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境の再デプロイ

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:relationships`

```bash
magento-cloud magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

環境の関係を示す

### 引数

#### `environment`

環境

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示する関係プロパティ

- 値が必要です

#### `--refresh`

関係を更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `environment:resume`

```bash
magento-cloud magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

一時停止した環境の再開

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:scp`

```bash
magento-cloud magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

scp を使用した環境との間でのファイルのコピー

### 引数

#### `files`

コピーするファイル。 remote: プレフィックスを使用してリモートの場所を定義します。

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--recursive`, `-r`

ディレクトリ全体を再帰的にコピー

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `environment:ssh`

```bash
magento-cloud magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

現在の環境への SSH

### 引数

#### `cmd`

環境で実行するコマンド。

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--pipe`

SSH URL のみを出力します。

- デフォルト：`false`
- 値を受け入れません

#### `--all`

すべての SSH URL を出力します（各アプリ用）。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `environment:synchronize`

```bash
magento-cloud magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

環境のコードやデータをその親から同期

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### 引数

#### `synchronize`

同期するもの：「コード」、「データ」またはその両方

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--rebase`

結合の代わりにリベースによるコードの同期

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `environment:url`

```bash
magento-cloud magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境のパブリック URL の取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--primary`, `-1`

プライマリルートの URL のみを返す

- デフォルト：`false`
- 値を受け入れません

#### `--browser`

URL を開くために使用するブラウザー。 何も設定しない場合は 0 を設定します。

- 値が必要です

#### `--pipe`

URL を stdout に出力します。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `environment:xdebug`

```bash
magento-cloud magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

環境で Xdebug へのトンネルを開きます。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--port`

ローカルポート

- デフォルト：`9000`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `integration:activity:get`

```bash
magento-cloud magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

単一の統合アクティビティの詳細情報の表示

### 引数

#### `integration`

統合 ID。 リストから選択する場合は、空白のままにします。


#### `activity`

アクティビティ ID。 デフォルトは最新の統合アクティビティです。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

[ 非推奨（廃止予定）のオプション、未使用 ]

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `integration:activity:list`

```bash
magento-cloud magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

統合のアクティビティのリストの取得

### 引数

#### `id`

統合 ID。 リストから選択する場合は、空白のままにします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--type`

タイプでアクティビティをフィルタリングします。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別のアクティビティを除外します。 値は、コンマ（「a,b,c」など）や空白で分割できます。 % または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--limit`

表示する結果の数を制限する

- デフォルト：`10`
- 値が必要です

#### `--start`

この日付より前に作成されたアクティビティのみがリストされます

- 値が必要です

#### `--state`

状態でアクティビティをフィルタリングします。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--result`

結果でアクティビティをフィルタリング

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみをリスト

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、created*、description*、type*、state*、result*、completed （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

[ 非推奨（廃止予定）のオプション、未使用 ]

- 値が必要です


## `integration:activity:log`

```bash
magento-cloud magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

統合アクティビティのログを表示

### 引数

#### `integration`

統合 ID。 リストから選択する場合は、空白のままにします。


#### `activity`

アクティビティ ID。 デフォルトは最新の統合アクティビティです。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--timestamps`, `-t`

各メッセージの横にタイムスタンプを表示します

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

[ 非推奨（廃止予定）のオプション、未使用 ]

- 値が必要です


## `integration:add`

```bash
magento-cloud magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトへの統合の追加

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--type`

統合タイプ （「bitbucket」、「bitbucket_server」、「github」、「gitlab」、「webhook」、「health.email」、「health.pagerduty」、「health.slack」、「health.webhook」、「httplog」、「script」、「newrelic」、「splunk」、「sumologic」、「syslog」）

- 値が必要です

#### `--base-url`

サーバーインストールのベース URL

- 値が必要です

#### `--bitbucket-url`

Bitbucket サーバーのインストールのベース URL

- 値が必要です

#### `--username`

Bitbucket サーバーのユーザー名

- 値が必要です

#### `--token`

統合の認証またはアクセストークン

- 値が必要です

#### `--key`

Bitbucket OAuth コンシューマーキー

- 値が必要です

#### `--secret`

Bitbucket OAuth コンシューマーシークレット

- 値が必要です

#### `--license-key`

New Relic ログのライセンスキー

- 値が必要です

#### `--server-project`

プロジェクト（例：「名前空間/リポジトリ」）

- 値が必要です

#### `--repository`

追跡するリポジトリ （例：「所有者/リポジトリ」）

- 値が必要です

#### `--build-merge-requests`

GitLab：結合リクエストを環境として作成

- デフォルト：`true`
- 値が必要です

#### `--build-pull-requests`

すべてのプルリクエストを環境として作成

- デフォルト：`true`
- 値が必要です

#### `--build-draft-pull-requests`

下書きプルリクエストの作成

- デフォルト：`true`
- 値が必要です

#### `--build-pull-requests-post-merge`

結合後の状態に基づいてプルリクエストを作成する

- デフォルト：`false`
- 値が必要です

#### `--build-wip-merge-requests`

GitLab:WIP 結合リクエストの作成

- デフォルト：`true`
- 値が必要です

#### `--merge-requests-clone-parent-data`

GitLab：結合リクエストのデータを複製

- デフォルト：`true`
- 値が必要です

#### `--pull-requests-clone-parent-data`

プルリクエスト用に親環境のデータをクローンします

- デフォルト：`true`
- 値が必要です

#### `--resync-pull-requests`

ビルドごとにプルリクエスト環境データを再同期

- デフォルト：`false`
- 値が必要です

#### `--fetch-branches`

リモートから（非アクティブな環境として）すべてのブランチを取得する

- デフォルト：`true`
- 値が必要です

#### `--prune-branches`

リモートに存在しないブランチを削除

- デフォルト：`true`
- 値が必要です

#### `--resources-init`

新しいサービスの初期化時に使用するリソース （&#39;minimum&#39;、&#39;default&#39;、&#39;manual&#39;、&#39;parent&#39;）

- 値が必要です

#### `--url`

統合の URL または API エンドポイント

- 値が必要です

#### `--shared-key`

Webhook:JWS 共有秘密鍵

- 値が必要です

#### `--file`

アップロードするスクリプトを含むローカルファイルの名前

- 値が必要です

#### `--events`

操作するイベントのリスト（environment.push など）

- デフォルト：`*`
- 値が必要です

#### `--states`

操作する状態のリスト （保留中、処理中、完了など）

- デフォルト：`complete`
- 値が必要です

#### `--environments`

含める環境 ID

- デフォルト：`*`
- 値が必要です

#### `--excluded-environments`

除外する環境 ID

- デフォルト：`[]`
- 値が必要です

#### `--from-address`

[ オプション ] アラートメールのカスタム送信元アドレス

- 値が必要です

#### `--recipients`

受信者の E メールアドレス

- デフォルト：`[]`
- 値が必要です

#### `--channel`

Slackチャンネル

- 値が必要です

#### `--routing-key`

PagerDuty ルーティング キー

- 値が必要です

#### `--category`

Sumo ロジック カテゴリ。フィルタリングに使用されます

- 値が必要です

#### `--index`

Splunk インデックス

- 値が必要です

#### `--sourcetype`

Splunk イベントソースタイプ

- 値が必要です

#### `--protocol`

Syslog トランスポートプロトコル （「tcp」、「udp」、「tls」）

- デフォルト：`tls`
- 値が必要です

#### `--syslog-host`

Syslog リレー/コレクタ・ホスト

- 値が必要です

#### `--syslog-port`

Syslog リレー/コレクタ・ポート

- 値が必要です

#### `--facility`

Syslog 機能

- デフォルト：`1`
- 値が必要です

#### `--message-format`

Syslog メッセージ形式（「rfc3164」または「rfc5424」）

- デフォルト：`rfc5424`
- 値が必要です

#### `--auth-mode`

認証モード （&#39;prefix&#39;または&#39;structured_data&#39;）

- デフォルト：`prefix`
- 値が必要です

#### `--auth-token`

認証トークン

- 値が必要です

#### `--verify-tls`

HTTPS 証明書の検証を有効にするかどうか（推奨）

- デフォルト：`true`
- 値が必要です

#### `--header`

POSTリクエストで使用する HTTP ヘッダー。 名前と値はコロン（:）で区切ります。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `integration:delete`

```bash
magento-cloud magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

プロジェクトからの統合の削除

### 引数

#### `id`

統合 ID。 リストから選択する場合は、空白のままにします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `integration:get`

```bash
magento-cloud magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

統合の詳細の表示

### 引数

#### `id`

統合 ID。 リストから選択する場合は、空白のままにします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示する統合プロパティ

- 値を受け入れる

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `integration:list`

```bash
magento-cloud magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクト統合のリストの表示

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id、summary、type。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `integration:update`

```bash
magento-cloud magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

統合の更新

### 引数

#### `id`

更新する統合の ID

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--type`

統合タイプ （「bitbucket」、「bitbucket_server」、「github」、「gitlab」、「webhook」、「health.email」、「health.pagerduty」、「health.slack」、「health.webhook」、「httplog」、「script」、「newrelic」、「splunk」、「sumologic」、「syslog」）

- 値が必要です

#### `--base-url`

サーバーインストールのベース URL

- 値が必要です

#### `--bitbucket-url`

Bitbucket サーバーのインストールのベース URL

- 値が必要です

#### `--username`

Bitbucket サーバーのユーザー名

- 値が必要です

#### `--token`

統合の認証またはアクセストークン

- 値が必要です

#### `--key`

Bitbucket OAuth コンシューマーキー

- 値が必要です

#### `--secret`

Bitbucket OAuth コンシューマーシークレット

- 値が必要です

#### `--license-key`

New Relic ログのライセンスキー

- 値が必要です

#### `--server-project`

プロジェクト（例：「名前空間/リポジトリ」）

- 値が必要です

#### `--repository`

追跡するリポジトリ （例：「所有者/リポジトリ」）

- 値が必要です

#### `--build-merge-requests`

GitLab：結合リクエストを環境として作成

- デフォルト：`true`
- 値が必要です

#### `--build-pull-requests`

すべてのプルリクエストを環境として作成

- デフォルト：`true`
- 値が必要です

#### `--build-draft-pull-requests`

下書きプルリクエストの作成

- デフォルト：`true`
- 値が必要です

#### `--build-pull-requests-post-merge`

結合後の状態に基づいてプルリクエストを作成する

- デフォルト：`false`
- 値が必要です

#### `--build-wip-merge-requests`

GitLab:WIP 結合リクエストの作成

- デフォルト：`true`
- 値が必要です

#### `--merge-requests-clone-parent-data`

GitLab：結合リクエストのデータを複製

- デフォルト：`true`
- 値が必要です

#### `--pull-requests-clone-parent-data`

プルリクエスト用に親環境のデータをクローンします

- デフォルト：`true`
- 値が必要です

#### `--resync-pull-requests`

ビルドごとにプルリクエスト環境データを再同期

- デフォルト：`false`
- 値が必要です

#### `--fetch-branches`

リモートから（非アクティブな環境として）すべてのブランチを取得する

- デフォルト：`true`
- 値が必要です

#### `--prune-branches`

リモートに存在しないブランチを削除

- デフォルト：`true`
- 値が必要です

#### `--resources-init`

新しいサービスの初期化時に使用するリソース （&#39;minimum&#39;、&#39;default&#39;、&#39;manual&#39;、&#39;parent&#39;）

- 値が必要です

#### `--url`

統合の URL または API エンドポイント

- 値が必要です

#### `--shared-key`

Webhook:JWS 共有秘密鍵

- 値が必要です

#### `--file`

アップロードするスクリプトを含むローカルファイルの名前

- 値が必要です

#### `--events`

操作するイベントのリスト（environment.push など）

- デフォルト：`*`
- 値が必要です

#### `--states`

操作する状態のリスト （保留中、処理中、完了など）

- デフォルト：`complete`
- 値が必要です

#### `--environments`

含める環境 ID

- デフォルト：`*`
- 値が必要です

#### `--excluded-environments`

除外する環境 ID

- デフォルト：`[]`
- 値が必要です

#### `--from-address`

[ オプション ] アラートメールのカスタム送信元アドレス

- 値が必要です

#### `--recipients`

受信者の E メールアドレス

- デフォルト：`[]`
- 値が必要です

#### `--channel`

Slackチャンネル

- 値が必要です

#### `--routing-key`

PagerDuty ルーティング キー

- 値が必要です

#### `--category`

Sumo ロジック カテゴリ。フィルタリングに使用されます

- 値が必要です

#### `--index`

Splunk インデックス

- 値が必要です

#### `--sourcetype`

Splunk イベントソースタイプ

- 値が必要です

#### `--protocol`

Syslog トランスポートプロトコル （「tcp」、「udp」、「tls」）

- デフォルト：`tls`
- 値が必要です

#### `--syslog-host`

Syslog リレー/コレクタ・ホスト

- 値が必要です

#### `--syslog-port`

Syslog リレー/コレクタ・ポート

- 値が必要です

#### `--facility`

Syslog 機能

- デフォルト：`1`
- 値が必要です

#### `--message-format`

Syslog メッセージ形式（「rfc3164」または「rfc5424」）

- デフォルト：`rfc5424`
- 値が必要です

#### `--auth-mode`

認証モード （&#39;prefix&#39;または&#39;structured_data&#39;）

- デフォルト：`prefix`
- 値が必要です

#### `--auth-token`

認証トークン

- 値が必要です

#### `--verify-tls`

HTTPS 証明書の検証を有効にするかどうか（推奨）

- デフォルト：`true`
- 値が必要です

#### `--header`

POSTリクエストで使用する HTTP ヘッダー。 名前と値はコロン（:）で区切ります。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `integration:validate`

```bash
magento-cloud magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

既存の統合の検証

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### 引数

#### `id`

統合 ID。 リストから選択する場合は、空白のままにします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `local:build`

```bash
magento-cloud magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

現在のプロジェクトをローカルにビルド

### 引数

#### `app`

構築するアプリケーションを指定

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--abslinks`, `-a`

絶対リンクを使用

- デフォルト：`false`
- 値を受け入れません

#### `--source`, `-s`

ソースディレクトリ。 デフォルトは現在のプロジェクトルートです。

- 値が必要です

#### `--destination`, `-d`

各アプリの web ルートがシンボリックリンクされる宛先。 デフォルト：_www

- 値が必要です

#### `--copy`, `-c`

ソースからのシンボリックリンクの代わりに、ビルドディレクトリにコピー

- デフォルト：`false`
- 値を受け入れません

#### `--clone`

Git を使用して、現在のHEADをビルドディレクトリにクローンします

- デフォルト：`false`
- 値を受け入れません

#### `--run-deploy-hooks`

デプロイおよび post_deploy フックの実行

- デフォルト：`false`
- 値を受け入れません

#### `--no-clean`

古いビルドを削除しない

- デフォルト：`false`
- 値を受け入れません

#### `--no-archive`

ビルドアーカイブを作成または使用しない

- デフォルト：`false`
- 値を受け入れません

#### `--no-backup`

以前のビルドをバックアップしない

- デフォルト：`false`
- 値を受け入れません

#### `--no-cache`

キャッシュを無効にする

- デフォルト：`false`
- 値を受け入れません

#### `--no-build-hooks`

ビルド後のフックを実行しない

- デフォルト：`false`
- 値を受け入れません

#### `--no-deps`

ビルドの依存関係をローカルにインストールしない

- デフォルト：`false`
- 値を受け入れません

#### `--working-copy`

Drush：単にバージョンをダウンロードするのではなく、git を使用して各 Drupal モジュールのリポジトリを複製します

- デフォルト：`false`
- 値を受け入れません

#### `--concurrency`

プッシュ：同時に処理される同時プロジェクトの数を設定します

- デフォルト：`4`
- 値が必要です

#### `--lock`

Drush：ロックファイルを作成または更新します（Drush バージョン 7 以降でのみ利用可能）

- デフォルト：`false`
- 値を受け入れません


## `local:dir`

```bash
magento-cloud magento-cloud dir [<subdir>]
```

ローカルプロジェクトルートの検索

### 引数

#### `subdir`

検索するサブディレクトリ （「local」、「web」または「shared」）

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `metrics:all`

```bash
magento-cloud magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA環境のCPU、ディスク、メモリの指標を表示します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- デフォルト：`false`
- 値を受け入れません

#### `--range`, `-r`

時間範囲。 終了時間（– to）まで、この期間の指標が読み込まれます。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最小 5m、最大 8 時間以上（プロジェクトによって異なる）、デフォルトは 10m。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 デフォルトは、範囲の除算です。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最低 1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは now です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データ ポイントのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルタリングします。% または*文字はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（—service が指定されていない場合）。 バージョンは必須ではありません。 % または*はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、cpu_percent*、mem_percent*、disk_percent*、tmp_disk_percent*、cpu_limit、cpu_used、disk_limit、inodes_limit、inodes_percent、inodes_used、mem_limit、mem_used、tmp_disk_limit、tmp_disk_used、tmp_inodes_limit、tmp_inodes_percent、tmp_inodes_used、type （* =デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `metrics:cpu`

```bash
magento-cloud magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA環境のCPUの使用状況を表示

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--range`, `-r`

時間範囲。 終了時間（– to）まで、この期間の指標が読み込まれます。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最小 5m、最大 8 時間以上（プロジェクトによって異なる）、デフォルトは 10m。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 デフォルトは、範囲の除算です。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最低 1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは now です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データ ポイントのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルタリングします。% または*文字はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（—service が指定されていない場合）。 バージョンは必須ではありません。 % または*はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、used*、limit*、%*、type （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `metrics:disk-usage`

```bash
magento-cloud magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

環境のディスク使用量を表示

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- デフォルト：`false`
- 値を受け入れません

#### `--range`, `-r`

時間範囲。 終了時間（– to）まで、この期間の指標が読み込まれます。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最小 5m、最大 8 時間以上（プロジェクトによって異なる）、デフォルトは 10m。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 デフォルトは、範囲の除算です。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最低 1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは now です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データ ポイントのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルタリングします。% または*文字はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（—service が指定されていない場合）。 バージョンは必須ではありません。 % または*はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--tmp`

一時的なディスク使用量をレポートします（列：timestamp、service、tmp_used、tmp_limit、tmp_percent、tmp_ipercent）

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、used*、limit*、percent*、ipercent*、ilimit、iused、tmp_ilimit、tmp_ipercent、tmp_iused、tmp_limit、tmp_used、type （* = デフォルトの列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `metrics:memory`

```bash
magento-cloud magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA環境のメモリ使用量を表示します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- デフォルト：`false`
- 値を受け入れません

#### `--range`, `-r`

時間範囲。 終了時間（– to）まで、この期間の指標が読み込まれます。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最小 5m、最大 8 時間以上（プロジェクトによって異なる）、デフォルトは 10m。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 デフォルトは、範囲の除算です。 単位として、時間（h）、分（m）、秒（s）を指定できます。 最低 1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは now です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データ ポイントのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルタリングします。% または*文字はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（—service が指定されていない場合）。 バージョンは必須ではありません。 % または*はワイルドカードとして使用できます。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、used*、limit*、%*、type （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `mount:download`

```bash
magento-cloud magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

rsync を使用したマウントからのファイルのダウンロード

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--all`, `-a`

すべてのマウントからダウンロード

- デフォルト：`false`
- 値を受け入れません

#### `--mount`, `-m`

マウント（アプリ相対パスとして）

- 値が必要です

#### `--target`

ファイルのダウンロード先のディレクトリ。 —all を使用する場合は、マウント・パスが追加される

- 値が必要です

#### `--source-path`

—all を使用する場合は、マウントのソース・パス（マウント・パスではなく）をターゲットのサブディレクトリとして使用する

- デフォルト：`false`
- 値を受け入れません

#### `--delete`

ターゲットディレクトリ内の不要なファイルを削除するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--exclude`

ダウンロードから除外するファイル （パターン）

- デフォルト：`[]`
- 値が必要です

#### `--include`

除外しないファイル （パターン）

- デフォルト：`[]`
- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `mount:list`

```bash
magento-cloud magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

マウントのリストの取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--paths`

マウント・パスのみを出力する（1 行につき 1 つ）

- デフォルト：`false`
- 値を受け入れません

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：定義、パス。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `mount:size`

```bash
magento-cloud magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

マウントのディスク使用量の確認

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- デフォルト：`false`
- 値を受け入れません

#### `--refresh`

キャッシュを更新

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：available、max、mounts、percent_used、size、used。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `mount:upload`

```bash
magento-cloud magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

rsync を使用したマウントへのファイルのアップロード

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--source`

アップロードするファイルを含むディレクトリ

- 値が必要です

#### `--mount`, `-m`

マウント（アプリ相対パスとして）

- 値が必要です

#### `--delete`

マウント内の不要なファイルを削除するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--exclude`

アップロードから除外するファイル （パターン）

- デフォルト：`[]`
- 値が必要です

#### `--include`

除外しないファイル （パターン）

- デフォルト：`[]`
- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `operation:list`

```bash
magento-cloud magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

環境のBETA リストのランタイム操作

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--full`

表示するコマンドの長さを制限しないでください。 デフォルトの上限は 24 行です。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：service*、name*、start*、role、stop （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `operation:run`

```bash
magento-cloud magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA環境で操作を実行します

### 引数

#### `operation`

操作名

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

セカンダリ名

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `project:clear-build-cache`

```bash
magento-cloud magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

プロジェクトのビルド キャッシュをクリアする

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `project:get`

```bash
magento-cloud magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

プロジェクトのローカルクローン

### 引数

#### `project`

プロジェクト ID


#### `directory`

クローン先のディレクトリ。 デフォルトはプロジェクトタイトルです

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--environment`, `-e`

複製する環境 ID。 デフォルトは、プロジェクトのデフォルトまたは最初に使用可能な環境です

- 値が必要です

#### `--depth`

シャロークローンの作成：履歴のコミット数を制限します

- 値が必要です

#### `--build`

クローン後のプロジェクトのビルド

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `project:info`

```bash
magento-cloud magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

プロジェクトのプロパティの読み取りまたは設定

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `project:list`

```bash
magento-cloud magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

すべてのアクティブなプロジェクトのリストの取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--pipe`

プロジェクト ID の簡単なリストを出力します。 ページネーションを無効にします。

- デフォルト：`false`
- 値を受け入れません

#### `--region`

地域でフィルター（完全一致）

- 値が必要です

#### `--title`

タイトルでフィルター（大文字と小文字を区別しない検索）

- 値が必要です

#### `--my`

所有しているプロジェクトのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--refresh`

リストを更新するかどうか

- デフォルト：`1`
- 値が必要です

#### `--sort`

並べ替えの基準にするプロパティ

- デフォルト：`title`
- 値が必要です

#### `--reverse`

逆順（降順）で並べ替え

- デフォルト：`false`
- 値を受け入れません

#### `--page`

ページ番号。 これにより、設定や – count に関わらず、ページネーションが有効になります。 —pipe が指定されている場合は無視されます。

- 値が必要です

#### `--count`, `-c`

ページごとに表示するプロジェクトの数。 ページネーションを無効にするには 0 を使用します。 —page が指定されている場合は無視されます。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`

表示する列。 使用可能な列：id*、title*、region*、created_at、organization_id、organization_label、organization_name、status （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `project:set-remote`

```bash
magento-cloud magento-cloud set-remote [<project>]
```

現在の Git リポジトリのリモートプロジェクトを設定します

### 引数

#### `project`

プロジェクト ID

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `repo:cat`

```bash
magento-cloud magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

プロジェクトリポジトリ内のファイルを読み取る

### 引数

#### `path`

ファイルへのパス

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--commit`, `-c`

コミット SHA です。 親コミットの場合は「HEAD」やキャレット（^）またはチルダ（～）を使用できます。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `repo:ls`

```bash
magento-cloud magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

プロジェクトリポジトリ内のファイルのリスト

### 引数

#### `path`

サブディレクトリへのパス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--directories`, `-d`

ディレクトリのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--files`, `-f`

ファイルのみを表示

- デフォルト：`false`
- 値を受け入れません

#### `--git-style`

「git ls-tree」に類似したスタイル出力

- デフォルト：`false`
- 値を受け入れません

#### `--commit`, `-c`

コミット SHA です。 親コミットの場合は「HEAD」やキャレット（^）またはチルダ（～）を使用できます。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `repo:read`

```bash
magento-cloud magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

プロジェクトリポジトリ内のディレクトリまたはファイルを読み取る

### 引数

#### `path`

ディレクトリまたはファイルへのパス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--commit`, `-c`

コミット SHA です。 親コミットの場合は「HEAD」やキャレット（^）またはチルダ（～）を使用できます。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `route:get`

```bash
magento-cloud magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

工順に関する詳細情報の表示

### 引数

#### `route`

ルートの元の URL

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--id`

選択するルート ID

- 値が必要です

#### `--primary`, `-1`

プライマリ ルートの選択

- デフォルト：`false`
- 値を受け入れません

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--refresh`

ルートのキャッシュをバイパス

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

[ 非推奨オプション、使用されなくなりました ]

- 値が必要です

#### `--identity-file`, `-i`

[ 非推奨オプション、使用されなくなりました ]

- 値が必要です


## `route:list`

```bash
magento-cloud magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

環境のすべてのルートのリスト

### 引数

#### `environment`

環境 ID

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

ルートのキャッシュをバイパス

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：route*、type*、to*、url （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `self:install`

```bash
magento-cloud magento-cloud self:install [--shell-type SHELL-TYPE]
```

CLI 構成ファイルのインストールまたは更新

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--shell-type`

オートコンプリートのシェルタイプ （bash または zsh）

- 値が必要です


## `self:update`

```bash
magento-cloud magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

CLI を最新バージョンに更新します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--no-major`

マイナーバージョンまたはパッチバージョン間のみを更新

- デフォルト：`false`
- 値を受け入れません

#### `--unstable`

新しい不安定なバージョンへの更新（可能な場合）

- デフォルト：`false`
- 値を受け入れません

#### `--manifest`

マニフェストファイルの場所の上書き

- 値が必要です

#### `--current-version`

現在のバージョンを上書き

- 値が必要です

#### `--timeout`

バージョン確認のタイムアウト

- デフォルト：`30`
- 値が必要です


## `service:list`

```bash
magento-cloud magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

プロジェクト内のサービスのリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--pipe`

サービス名のリストのみを出力

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：ディスク、名前、サイズ、タイプ。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `service:mongo:dump`

```bash
magento-cloud magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB からのデータのバイナリアーカイブダンプの作成

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--collection`, `-c`

ダンプするコレクション

- 値が必要です

#### `--gzip`, `-z`

gzip を使用してダンプを圧縮します

- デフォルト：`false`
- 値を受け入れません

#### `--stdout`, `-o`

ファイルではなく STDOUT に出力

- デフォルト：`false`
- 値を受け入れません

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:export`

```bash
magento-cloud magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB からのデータのエクスポート

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--collection`, `-c`

エクスポートするコレクション

- 値が必要です

#### `--jsonArray`

単一の JSON 配列としてのデータのエクスポート

- デフォルト：`false`
- 値を受け入れません

#### `--type`

書き出しタイプ（例：「csv」）

- 値が必要です

#### `--fields`, `-f`

エクスポートするフィールド

- デフォルト：`[]`
- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:restore`

```bash
magento-cloud magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

データのバイナリアーカイブダンプを MongoDB に復元します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--collection`, `-c`

復元するコレクション

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:shell`

```bash
magento-cloud magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB シェルの使用

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--eval`

JavaScript フラグメントをシェルに渡す

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:redis-cli`

```bash
magento-cloud magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Redis CLI へのアクセス

### 引数

#### `args`

Redis コマンドに追加する引数

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `snapshot:create`

```bash
magento-cloud magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

環境のスナップショットの作成

### 引数

#### `environment`

環境

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--live`

ライブバックアップ：環境を停止しないでください。 設定すると、バックアップ中も環境が実行中で、接続を開いたままになります。 これにより、整合性のない状態でデータをバックアップするリスクのあるダウンタイムが削減されます。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `snapshot:delete`

```bash
magento-cloud magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

環境スナップショットの削除

### 引数

#### `id`

スナップショットの ID。 非インタラクティブモードでは必須です。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `snapshot:get`

```bash
magento-cloud magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

環境スナップショットの表示

### 引数

#### `id`

スナップショットの ID。 デフォルトは最新の値です。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示するプロパティ。

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です


## `snapshot:list`

```bash
magento-cloud magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境の使用可能なスナップショットのリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `snapshot:restore`

```bash
magento-cloud magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

環境スナップショットの復元

### 引数

#### `snapshot`

スナップショットの名前。 デフォルトは最新の値

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--target`

復元先の環境。 デフォルトは、スナップショットの現在の環境です

- 値が必要です

#### `--branch-from`

—target がまだ存在しない場合、新しい環境の親を指定します

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `source-operation:list`

```bash
magento-cloud magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

環境のソース操作のリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--full`

表示するコマンドの長さを制限しないでください。 デフォルトの上限は 24 行です。

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：アプリ、コマンド、操作。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `source-operation:run`

```bash
magento-cloud magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

ソース操作の実行

### 引数

#### `operation`

操作名

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--variable`

操作中に設定する変数を type:name=value の形式で指定します。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `ssh-cert:load`

```bash
magento-cloud magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

SSH 証明書の生成

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh-only`

必要な場合にのみ証明書を更新します（SSH 設定は書かないでください）

- デフォルト：`false`
- 値を受け入れません

#### `--new`

証明書を強制的に更新する

- デフォルト：`false`
- 値を受け入れません

#### `--new-key`

[ 非推奨 ] 代わりに – new を使用してください。

- デフォルト：`false`
- 値を受け入れません


## `ssh-key:add`

```bash
magento-cloud magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

新しい SSH キーの追加

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 引数

#### `path`

既存の SSH 公開鍵へのパス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--name`

キーを識別する名前

- 値が必要です


## `ssh-key:delete`

```bash
magento-cloud magento-cloud ssh-key:delete [<id>]
```

SSH キーの削除

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 引数

#### `id`

削除する SSH キーの ID

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `ssh-key:list`

```bash
magento-cloud magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

アカウントの SSH キーのリストを取得する

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、title*、path*、fingerprint （* =デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `subscription:info`

```bash
magento-cloud magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

購読プロパティの読み取りまたは変更

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--id`, `-s`

サブスクリプション ID

- 値が必要です

#### `--date-fmt`

日付形式（PHP 日付書式文字列）

- デフォルト：`c`
- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `tunnel:close`

```bash
magento-cloud magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH トンネルを閉じる

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--all`, `-a`

すべてのトンネルを閉じる

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `tunnel:info`

```bash
magento-cloud magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH トンネルの関係情報の表示

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

表示する関係プロパティ

- 値が必要です

#### `--encode`, `-c`

base64 でエンコードされた JSON としての出力

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `tunnel:list`

```bash
magento-cloud magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

SSH トンネルのリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--all`, `-a`

すべてのトンネルを表示

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：port*、project*、environment*、app*、relationship*、url （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません


## `tunnel:open`

```bash
magento-cloud magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

アプリの関係に SSH トンネルを開く

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--gateway-ports`, `-g`

リモート・ホストがローカル転送ポートに接続できるようにする

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `tunnel:single`

```bash
magento-cloud magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

アプリの関係に対して 1 つの SSH トンネルを開く

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--port`

ローカルポート

- 値が必要です

#### `--gateway-ports`, `-g`

リモート・ホストがローカル転送ポートに接続できるようにする

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービスの関係

- 値が必要です

#### `--identity-file`, `-i`

使用する SSH ID （秘密鍵）

- 値が必要です


## `user:add`

```bash
magento-cloud magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

プロジェクトにユーザーを追加する

### 引数

#### `email`

ユーザーの E メールアドレス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--role`, `-r`

ユーザーのプロジェクトの役割（「管理者」または「閲覧者」）または環境の種類の役割（「ステージング：投稿者」や「実稼動：閲覧者」など）。 環境タイプからユーザーを削除するには、役割を「なし」に設定します。 % または*文字を環境タイプのワイルドカードとして使用できます（例：「%:viewer」）。これにより、ユーザーにすべてのタイプで「viewer」の役割を付与できます。 役割は短縮できます（例：「production:v」）。

- デフォルト：`[]`
- 値が必要です

#### `--force-invite`

既に送信されている招待状も送信する

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `user:delete`

```bash
magento-cloud magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

プロジェクトからユーザーを削除する

### 引数

#### `email`

ユーザーの E メールアドレス

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `user:get`

```bash
magento-cloud magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

ユーザーの役割の表示

### 引数

#### `email`

ユーザーの E メールアドレス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--level`, `-l`

役割レベル （「プロジェクト」または「環境」）

- 値が必要です

#### `--pipe`

（変更を加えた後で）ロールを stdout に出力します

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません

#### `--role`, `-r`

[ 非推奨：ユーザーの役割を変更するには user:update を使用してください ]

- 値が必要です


## `user:list`

```bash
magento-cloud magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクトユーザーのリスト

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：email*、name*、role*、id*、granted_at、updated_at （* = デフォルト列）。 「+」文字は、デフォルトの列のプレースホルダーとして使用できます。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です


## `user:update`

```bash
magento-cloud magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

プロジェクトのユーザーの役割を更新

### 引数

#### `email`

ユーザーの E メールアドレス

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--role`, `-r`

ユーザーのプロジェクトの役割（「管理者」または「閲覧者」）または環境の種類の役割（「ステージング：投稿者」や「実稼動：閲覧者」など）。 環境タイプからユーザーを削除するには、役割を「なし」に設定します。 % または*文字を環境タイプのワイルドカードとして使用できます（例：「%:viewer」）。これにより、ユーザーにすべてのタイプで「viewer」の役割を付与できます。 役割は短縮できます（例：「production:v」）。

- デフォルト：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `variable:create`

```bash
magento-cloud magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

変数の作成

### 引数

#### `name`

変数名

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--update`, `-u`

変数が既に存在する場合は、更新します

- デフォルト：`false`
- 値を受け入れません

#### `--level`, `-l`

変数を設定するレベル （「プロジェクト」または「環境」）

- 値が必要です

#### `--name`

変数名

- 値が必要です

#### `--value`

変数の値

- 値が必要です

#### `--json`

変数値が JSON 形式かどうか

- デフォルト：`false`
- 値が必要です

#### `--sensitive`

変数値が大文字と小文字を区別するかどうか

- デフォルト：`false`
- 値が必要です

#### `--prefix`

変数名のプレフィックス。型を決定できます（例：&#39;env&#39;）。 名前にプレフィックスがまだ含まれていない場合にのみ適用されます。 （例：「none」または「env:」）

- デフォルト：`none`
- 値が必要です

#### `--enabled`

環境で変数を有効にする必要があるかどうか

- デフォルト：`true`
- 値が必要です

#### `--inheritable`

変数が子環境によって継承可能かどうか

- デフォルト：`true`
- 値が必要です

#### `--visible-build`

ビルド時に変数を表示するかどうか

- 値が必要です

#### `--visible-runtime`

実行時に変数を表示するかどうか

- デフォルト：`true`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `variable:delete`

```bash
magento-cloud magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

変数の削除

### 引数

#### `name`

変数名

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--level`, `-l`

変数レベル （「プロジェクト」、「環境」、「p」または「e」）

- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `variable:get`

```bash
magento-cloud magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

変数の表示

### 引数

#### `name`

変数の名前

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--property`, `-P`

単一の変数プロパティの表示

- 値が必要です

#### `--level`, `-l`

変数レベル （「プロジェクト」、「環境」、「p」または「e」）

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--pipe`

[ 非推奨オプション ] 変数値のみを出力します

- デフォルト：`false`
- 値を受け入れません


## `variable:list`

```bash
magento-cloud magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

リスト変数

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--level`, `-l`

変数レベル （「プロジェクト」、「環境」、「p」または「e」）

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：is_enabled、level、name、value。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `variable:update`

```bash
magento-cloud magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

変数の更新

### 引数

#### `name`

変数名

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--allow-no-change`

変更がない場合は成功（ゼロ終了コード）を返します

- デフォルト：`false`
- 値を受け入れません

#### `--level`, `-l`

変数レベル （「プロジェクト」、「環境」、「p」または「e」）

- 値が必要です

#### `--value`

変数の値

- 値が必要です

#### `--json`

変数値が JSON 形式かどうか

- デフォルト：`false`
- 値が必要です

#### `--sensitive`

変数値が大文字と小文字を区別するかどうか

- デフォルト：`false`
- 値が必要です

#### `--enabled`

環境で変数を有効にする必要があるかどうか

- デフォルト：`true`
- 値が必要です

#### `--inheritable`

変数が子環境によって継承可能かどうか

- デフォルト：`true`
- 値が必要です

#### `--visible-build`

ビルド時に変数を表示するかどうか

- 値が必要です

#### `--visible-runtime`

実行時に変数を表示するかどうか

- デフォルト：`true`
- 値が必要です

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待たない

- デフォルト：`false`
- 値を受け入れません

#### `--wait`

操作が完了するまで待ちます（デフォルト）

- デフォルト：`false`
- 値を受け入れません


## `worker:list`

```bash
magento-cloud magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

デプロイされたすべてのワーカーのリストを取得

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- デフォルト：`false`
- 値を受け入れません

#### `--pipe`

ワーカー名のリストのみを出力

- デフォルト：`false`
- 値を受け入れません

#### `--project`, `-p`

プロジェクト ID または URL

- 値が必要です

#### `--environment`, `-e`

環境 ID。 「。」を使用します プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力形式：table、csv、tsv、plain

- デフォルト：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：コマンド、名前、タイプ。 % または*はワイルドカードとして使用できます。 値は、コンマ（「a,b,c」など）や空白で分割できます。

- デフォルト：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- デフォルト：`false`
- 値を受け入れません
