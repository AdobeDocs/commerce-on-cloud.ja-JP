---
source-git-commit: fddcfdb97aede07b2cd6ef12bda6d7998f941951
workflow-type: tm+mt
source-wordcount: '13880'
ht-degree: 0%

---
# magento-cloud （Adobe Commerce on cloud infrastructure）

<!-- The template to render with above values -->

**バージョン**: 1.47.0

このリファレンスには、`magento-cloud` コマンドラインツールを通じて利用できる123のコマンドが含まれています。
最初のリストは、クラウドインフラストラクチャ上のAdobe Commerceで`magento-cloud list` コマンドを使用して自動生成されます。

## 一般

この参照は、アプリケーションコードベースから生成されます。 コンテンツを変更するには、[codebase](https://github.com/magento/magento-cloud-cli) リポジトリ内の対応するコマンド実装のソースコードを更新し、変更内容をレビュー用に送信します。 もう1つの方法は、_フィードバックを送信_&#x200B;することです（右上のリンクを見つけてください）。 貢献度ガイドラインについては、[&#x200B; コード貢献度](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)を参照してください。

### グローバルオプション

#### `--help`, `-h`

このヘルプメッセージを表示

- 既定：`false`
- 値を受け付けません

#### `--version`, `-V`

このアプリケーションのバージョンを表示

- 既定：`false`
- 値を受け付けません

#### `--verbose`, `-v|-vv|-vvv`

メッセージの冗長性を高める

- 既定：`false`
- 値を受け付けません

#### `--quiet`, `-q`

必要な出力のみを印刷し、他のメッセージやエラーを抑制します。 これは – インタラクションがないことを意味します。 詳細モードでは無視されます。

- 既定：`false`
- 値を受け付けません

#### `--yes`, `-y`

確認の質問に「はい」と答えます。他の質問のデフォルト値を受け入れます。インタラクションを無効にします

- 既定：`false`
- 値を受け付けません

#### `--no-interaction`

インタラクティブな質問は行わないでください。デフォルト値を使用します。 Magento_CLOUD_CLI_NO_INTERACTION=1を使用することと同等です。

- 既定：`false`
- 値を受け付けません


## `clear-cache`

```bash
magento-cloud cc
```

CLI キャッシュをクリアする

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `console`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

コンソールでプロジェクトを開きます

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--browser`

URLを開くために使用するブラウザー。 「0」を「なし」に設定します。

- 値が必要です

#### `--pipe`

URLをstdoutに出力します。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Magento_CLOUD_VARIABLESなどのエンコードされた文字列のデコード

### 引数

#### `value`

デコードする変数値

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

変数内で表示するプロパティ

- 値が必要です


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

オンラインドキュメントを開く

### 引数

#### `search`

検索語

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--browser`

URLを開くために使用するブラウザー。 「0」を「なし」に設定します。

- 値が必要です

#### `--pipe`

URLをstdoutに出力します。

- 既定：`false`
- 値を受け付けません


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
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

- 既定：`help`

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力形式（txt、json、md）

- 既定：`txt`
- 値が必要です

#### `--raw`

Raw コマンド ヘルプを出力するには

- 既定：`false`
- 値を受け付けません


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Lists コマンド

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

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--raw`

Raw コマンドリストを出力するには

- 既定：`false`
- 値を受け付けません

#### `--format`

出力形式（txt、xml、json、md）

- 既定：`txt`
- 値が必要です

#### `--all`

非表示を含むすべてのコマンドを表示

- 既定：`false`
- 値を受け付けません


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

複数のプロジェクトでコマンドを実行する

### 引数

#### `cmd`

実行するコマンド

- 既定：`[]`
- 必須

- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--projects`, `-p`

プロジェクト IDのリスト。コンマや空白で区切られます

- 値が必要です

#### `--continue`

例外が発生した場合でも、コマンドの実行を続行する

- 既定：`false`
- 値を受け付けません

#### `--sort`

プロジェクトオプションのリストを並べ替えるプロパティ

- 既定：`title`
- 値が必要です

#### `--reverse`

プロジェクトのオプションの順序を反転

- 既定：`false`
- 値を受け付けません


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

アクティビティのキャンセル

### 引数

#### `id`

アクティビティ ID。 デフォルトは、最新のキャンセル可能なアクティビティです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`, `-t`

タイプ別にフィルタリング（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプのワイルドカードとして使用できます。例：&#39;%var%&#39;は、変数関連のアクティビティを選択します。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別に除外（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

単一のアクティビティに関する詳細情報の表示

### 引数

#### `id`

アクティビティ ID。 デフォルトは最新のアクティビティです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--type`, `-t`

タイプ別にフィルタリング（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプのワイルドカードとして使用できます。例：&#39;%var%&#39;は、変数関連のアクティビティを選択します。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別に除外（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--state`

状態でフィルター（デフォルトのアクティビティを選択する場合）：進行中、保留中、完了、またはキャンセル済み。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--result`

結果でフィルタリング（デフォルトのアクティビティを選択する場合）：成功または失敗

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを含める（デフォルトアクティビティを選択する場合）。 —state=in_progress,pendingの略語です

- 既定：`false`
- 値を受け付けません

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境またはプロジェクトのアクティビティのリストを取得

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`, `-t`

タイプ別のアクティビティのフィルタリング値は、コンマ（例：「a、b、c」）や空白で分割できます。 アクティビティ名の最初の部分は省略できます。例えば、「cron」は「environment.cron」アクティビティを選択できます。 %または*文字は、ワイルドカードとして使用できます（例：&#39;%var%&#39;）変数に関連するアクティビティを選択します。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別のアクティビティを除外します。 値はコンマ （例：「a,b,c」）や空白で分割できます。 アクティビティ名の最初の部分は省略できます。例えば、「cron」は「environment.cron」アクティビティを除外できます。 %または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--limit`

表示される結果の数を制限

- 既定：`10`
- 値が必要です

#### `--start`

この日付より前に作成されたアクティビティのみが一覧表示されます

- 値が必要です

#### `--state`

ステータス別にアクティビティをフィルタリング：進行中、保留中、完了、またはキャンセル。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--result`

結果によるアクティビティのフィルタリング：成功または失敗

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを一覧表示

- 既定：`false`
- 値を受け付けません

#### `--all`, `-a`

すべての環境のアクティビティのリスト

- 既定：`false`
- 値を受け付けません

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、created*、description*、progress*、state*、result*、completed、environments、time_build、time_deploy、time_execute、time_wait、type （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

アクティビティのログの表示

### 引数

#### `id`

アクティビティ ID。 デフォルトは最新のアクティビティです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

アクティビティの更新間隔（秒）。 更新を無効にするには、0に設定します。

- 既定：`3`
- 値が必要です

#### `--timestamps`, `-t`

各メッセージの横にタイムスタンプを表示

- 既定：`false`
- 値を受け付けません

#### `--type`

タイプ別にフィルタリング（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプのワイルドカードとして使用できます。例：&#39;%var%&#39;は、変数関連のアクティビティを選択します。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別に除外（デフォルトのアクティビティを選択する場合）。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--state`

状態でフィルター（デフォルトのアクティビティを選択する場合）：進行中、保留中、完了、またはキャンセル済み。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--result`

結果でフィルタリング（デフォルトのアクティビティを選択する場合）：成功または失敗

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを含める（デフォルトアクティビティを選択する場合）。 —state=in_progress,pendingの略語です

- 既定：`false`
- 値を受け付けません

#### `--all`, `-a`

すべての環境で最近のアクティビティを確認する（デフォルトアクティビティを選択する場合）

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

アプリの設定の表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示する設定プロパティ

- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--identity-file`, `-i`

[非推奨のオプション、使用されなくなった]

- 値が必要です


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

プロジェクト内のアプリのリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--pipe`

アプリ名のみのリストを出力

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：名前*、タイプ*、ディスク、パス、サイズ（* = デフォルト列）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

API トークンを使用してMagento Cloudにログインします

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/jp/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--method METHOD] [--max-age MAX-AGE] [--browser BROWSER] [--pipe]
```

ブラウザーを使用してMagento Cloudにログイン

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

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--force`, `-f`

既にログインしている場合でも、再度ログインします

- 既定：`false`
- 値を受け付けません

#### `--method`

特定の認証方法が必要

- 既定：`[]`
- 値が必要です

#### `--max-age`

Web認証セッションの最大経過時間（秒）

- 値が必要です

#### `--browser`

URLを開くために使用するブラウザー。 「0」を「なし」に設定します。

- 値が必要です

#### `--pipe`

URLをstdoutに出力します。

- 既定：`false`
- 値を受け付けません


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

アカウント情報の表示

### 引数

#### `property`

表示するアカウントプロパティ

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--no-auto-login`

自動ログインをスキップします。 ログインしていない場合は何も出力されず、終了コードは0になり、他のエラーがないと仮定されます。

- 既定：`false`
- 値を受け付けません

#### `--property`, `-P`

表示するアカウントプロパティ （代替構文）

- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Magento Cloudからログアウトする

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--all`, `-a`

すべてのローカルセッションからログアウトする

- 既定：`false`
- 値を受け付けません

#### `--other`

他のローカルセッションからログアウトする

- 既定：`false`
- 値を受け付けません


## `autoscaling:get`

```bash
magento-cloud autoscaling [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

環境上のアプリとワーカーの自動スケーリング設定を表示する

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：service*、metric*、direction*、threshold*、duration*、enabled*、instance_count*、cooldown、max_instances、min_instances （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `autoscaling:set`

```bash
magento-cloud autoscaling:set [-s|--service SERVICE] [-m|--metric METRIC] [--enabled ENABLED] [--threshold-up THRESHOLD-UP] [--duration-up DURATION-UP] [--cooldown-up COOLDOWN-UP] [--threshold-down THRESHOLD-DOWN] [--duration-down DURATION-DOWN] [--cooldown-down COOLDOWN-DOWN] [--instances-min INSTANCES-MIN] [--instances-max INSTANCES-MAX] [--dry-run] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境内のアプリまたはワーカーの自動スケーリング設定の設定

```
Configure automatic scaling for apps or workers in an environment.

You can also configure resources statically by running: magento-cloud resources:set
```

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--service`, `-s`

自動拡張を設定するアプリまたはワーカーの名前

- 値が必要です

#### `--metric`, `-m`

自動スケーリングのトリガーに使用する指標の名前

- 値が必要です

#### `--enabled`

指定された指標に基づいて自動拡張を有効にする

- 値が必要です

#### `--threshold-up`

拡張するサービスのしきい値

- 値が必要です

#### `--duration-up`

スケールアップのしきい値に対して評価される指標の期間

- 値が必要です

#### `--cooldown-up`

スケールイベント後にさらにスケールアップを試みる前の待機時間

- 値が必要です

#### `--threshold-down`

サービスをスケールダウンするしきい値

- 値が必要です

#### `--duration-down`

スケールダウンのしきい値に対して評価される指標の期間

- 値が必要です

#### `--cooldown-down`

拡張イベント後にさらにスケールダウンを試みる前の待機時間

- 値が必要です

#### `--instances-min`

スケールダウンされるインスタンスの最小数

- 値が必要です

#### `--instances-max`

スケールアップされるインスタンスの最大数

- 値が必要です

#### `--dry-run`

何も変更せずに行われる変更を表示

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトのBlackfire.io統合の設定

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--server_id`

サーバーID

- 値が必要です

#### `--server_token`

サーバートークン

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトへのSSL証明書の追加

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--cert`

証明書ファイルのパス

- 値が必要です

#### `--key`

証明書の秘密鍵ファイルへのパス

- 値が必要です

#### `--chain`

証明書チェーンファイルへのパス

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

プロジェクトから証明書を削除する

### 引数

#### `id`

証明書ID （または証明書の先頭）

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

証明書の表示

### 引数

#### `id`

証明書ID （または証明書の先頭）

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示する証明書プロパティ

- 値が必要です

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクト証明書の一覧表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--domain`

ドメイン名でフィルタリング（大文字と小文字を区別しない検索）

- 値が必要です

#### `--exclude-domain`

ドメイン名で一致する証明書を除外（大文字と小文字を区別しない検索）

- 値が必要です

#### `--issuer`

イシュアでフィルタリング

- 値が必要です

#### `--only-auto`

自動プロビジョニングされた証明書のみを表示

- 既定：`false`
- 値を受け付けません

#### `--no-auto`

手動で追加された証明書のみを表示

- 既定：`false`
- 値を受け付けません

#### `--ignore-expiry`

期限切れと期限切れでない証明書の両方を表示

- 既定：`false`
- 値を受け付けません

#### `--only-expired`

期限切れの証明書のみを表示

- 既定：`false`
- 値を受け付けません

#### `--no-expired`

期限切れでない証明書のみを表示（デフォルト）

- 既定：`false`
- 値を受け付けません

#### `--pipe-domains`

証明書でカバーされているドメイン名のリストのみを返します

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：created、domains、expires、id、issuer。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

コミットの詳細を表示

### 引数

#### `commit`

コミット SHA。 これは、親コミットの「HEAD」とキャレット（^）またはチルダ（～）接尾辞を受け入れることもできます。

- 既定：`HEAD`

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示するコミットプロパティ。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

リストコミット

### 引数

#### `commit`

開始Git コミット SHA。 これは、親コミットの「HEAD」とキャレット（^）またはチルダ（～）接尾辞を受け入れることもできます。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--limit`

表示するコミットの数。

- 既定：`10`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：作成者、日付、sha、概要。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP]
```

リモートデータベースのローカルダンプの作成

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--schema`

ダンプするスキーマ。 デフォルトのスキーマ（通常は「main」）を使用することを省略します。

- 値が必要です

#### `--file`, `-f`

ダンプのカスタムファイル名

- 値が必要です

#### `--directory`, `-d`

ダンプのカスタムディレクトリ

- 値が必要です

#### `--gzip`, `-z`

gzipを使用してダンプを圧縮する

- 既定：`false`
- 値を受け付けません

#### `--timestamp`, `-t`

ダンプファイル名にタイムスタンプを追加する

- 既定：`false`
- 値を受け付けません

#### `--stdout`, `-o`

ファイルではなくSTDOUTに出力

- 既定：`false`
- 値を受け付けません

#### `--table`

含めるテーブル

- 既定：`[]`
- 値が必要です

#### `--exclude-table`

除外するテーブル

- 既定：`[]`
- 値が必要です

#### `--schema-only`

スキーマのみをダンプ、データなし

- 既定：`false`
- 値を受け付けません

#### `--charset`

ダンプの文字セットエンコーディング

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--] [<query>]
```

リモート・データベースでのSQLの実行

### 引数

#### `query`

実行するSQL文

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--raw`

生の非表形式出力を生成する

- 既定：`false`
- 値を受け付けません

#### `--schema`

使用するスキーマ。 デフォルトのスキーマ（通常は「main」）を使用することを省略します。 スキーマを使用しないように空の文字列を渡します。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

プロジェクトに新しいドメインを追加します。 このオプションは、Cloud Pro プランプロジェクトでは使用できません。

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--cert`

カスタム証明書ファイルへのパス

- 値が必要です

#### `--key`

カスタム証明書の秘密鍵へのパス

- 値が必要です

#### `--chain`

カスタム証明書のチェーンファイルへのパス

- 既定：`[]`
- 値が必要です

#### `--attach`

環境のルートで、これが置き換える実稼動ドメイン。 実稼動以外の環境ドメインに必要です。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

プロジェクトからドメインを削除します。 このオプションは、Cloud Pro プランプロジェクトでは使用できません。

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

ドメインの詳細情報を表示します。 このオプションは、Cloud Pro プランプロジェクトでは使用できません。

### 引数

#### `name`

ドメイン名

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示するドメインプロパティ

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

すべてのドメインのリストを取得します。 このオプションは、Cloud Pro プランプロジェクトでは使用できません。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：name*、ssl*、created_at*、registered_name、replacement_for、type、updated_at （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

ドメインを更新します。 このオプションは、Cloud Pro プランプロジェクトでは使用できません。

### 引数

#### `name`

ドメイン名

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--cert`

カスタム証明書ファイルへのパス

- 値が必要です

#### `--key`

カスタム証明書の秘密鍵へのパス

- 値が必要です

#### `--chain`

カスタム証明書のチェーンファイルへのパス

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

環境のアクティブ化

### 引数

#### `environment`

アクティブ化する環境

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--parent`

アクティブ化する前に新しい環境の親を設定

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [--no-checkout] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

環境の分岐

### 引数

#### `id`

新しい環境のID （ブランチ名）


#### `parent`

新しい環境の親

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--title`

新環境のタイトル

- 値が必要です

#### `--type`

新規環境のタイプ

- 値が必要です

#### `--no-clone-parent`

親環境のデータを複製しないでください

- 既定：`false`
- 値を受け付けません

#### `--no-checkout`

ブランチをローカルでチェックアウトしない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:checkout`

```bash
magento-cloud checkout [<id>]
```

環境のチェックアウト

### 引数

#### `id`

チェックアウトする環境のID。 例：「sprint2」

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--status STATUS] [--only-status ONLY-STATUS] [--exclude-status EXCLUDE-STATUS] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

1つ以上の環境の削除

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### 引数

#### `environment`

削除する環境。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--delete-branch`

確認せずに、非アクティブな環境のGit ブランチを削除する

- 既定：`false`
- 値を受け付けません

#### `--no-delete-branch`

Git ブランチ（非アクティブ環境）は削除しないでください

- 既定：`false`
- 値を受け付けません

#### `--type`

タイプのすべての環境を削除（選択した他の環境に追加）値は、コンマ（例：「a、b、c」）または空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--only-type`, `-t`

特定のタイプの削除環境のみ値はコンマ（例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--exclude`

削除しない環境。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`

値を削除しない環境タイプは、コンマ（「a、b、c」など）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--inactive`

すべての非アクティブな環境を削除（選択した他の環境に追加）

- 既定：`false`
- 値を受け付けません

#### `--status`

ステータスのすべての環境を削除（選択した他の環境に追加）値はコンマ（例：「a、b、c」）または空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--only-status`

特定のステータスの環境のみを削除値は、コンマ（例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--exclude-status`

値を削除しない環境ステータスは、コンマ（「a、b、c」など）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--merged`

結合されたすべての環境を削除（選択した他の環境に追加）

- 既定：`false`
- 値を受け付けません

#### `--allow-delete-parent`

子を持つ環境の削除を許可

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:deploy`

```bash
magento-cloud deploy [-s|--strategy STRATEGY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境のステージングされた変更をデプロイする

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--strategy`, `-s`

デプロイメント戦略、ストップスタート（デフォルト、シャットダウンで再起動）またはローリング（ダウンタイムなし）

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:deploy:type`

```bash
magento-cloud environment:deploy:type [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<type>]
```

環境デプロイメントタイプの表示または設定

```
Choose automatic (the default) if you want your changes to be deployed immediately as they are made.
Choose manual to have changes staged until you trigger a deployment (including changes to code, variables, domains and settings).
```

### 引数

#### `type`

環境デプロイメントタイプ：自動または手動。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--pipe`

デプロイメントタイプをstdoutに出力する

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境のHTTP アクセス設定の更新

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--access`

「permission:address」形式のアクセス制限。 すべてのアドレスをクリアするには0を使用します。

- 既定：`[]`
- 値が必要です

#### `--auth`

「username:password」形式のHTTP Basic認証情報。 すべての資格情報を消去するには、0を使用します。

- 既定：`[]`
- 値が必要です

#### `--enabled`

アクセス制御を有効にするかどうか：1を有効にする、0を無効にする

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

環境のプロパティの読み取りまたは設定

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

パブリック Git リポジトリからの環境の初期化

### 引数

#### `url`

Git リポジトリへのURL

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--profile`

プロファイルの名前

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--status STATUS] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

環境のリストを取得

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--no-inactive`, `-I`

非アクティブな環境を表示しない

- 既定：`false`
- 値を受け付けません

#### `--status`

ステータス（アクティブ、非アクティブ、ダーティ、一時停止、削除）で環境をフィルタリングします。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--pipe`

環境IDの簡単なリストを出力します。

- 既定：`false`
- 値を受け付けません

#### `--refresh`

リストを更新するかどうか。

- 既定：`1`
- 値が必要です

#### `--sort`

並べ替えるプロパティ

- 既定：`title`
- 値が必要です

#### `--reverse`

逆順（降順）で並べ替え

- 既定：`false`
- 値を受け付けません

#### `--type`

環境タイプでリストをフィルタリングします。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、title*、status*、type*、created、machine_name、updated （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

環境のログの読み取り

### 引数

#### `type`

ログの種類（例：「アクセス」または「エラー」）

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--lines`

表示する行数

- 既定：`100`
- 値が必要です

#### `--tail`

ログを継続的にテールする

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

環境の結合

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### 引数

#### `environment`

統合する環境

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境を一時停止

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<source>]
```

環境へのコードのプッシュ

### 引数

#### `source`

Git ソース参照（例：ブランチ名やコミットハッシュ）。

- 既定：`HEAD`

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--target`

ターゲットブランチ名。 デフォルトは現在のブランチです。

- 値が必要です

#### `--force`, `-f`

非フォワードアップデートを許可

- 既定：`false`
- 値を受け付けません

#### `--force-with-lease`

リモートトラッキングブランチが最新の場合、早送り以外の更新を許可

- 既定：`false`
- 値を受け付けません

#### `--set-upstream`, `-u`

ターゲット環境をソースブランチのアップストリームとして設定します。 これにより、ターゲットプロジェクトがローカルリポジトリのリモートとして設定されます。

- 既定：`false`
- 値を受け付けません

#### `--activate`

環境をアクティブ化します。 一時停止した環境が再開されます。 これにより、変更がプッシュされなくても、環境がアクティブになります。

- 既定：`false`
- 値を受け付けません

#### `--parent`

環境の親を設定します（ – activateでのみ使用）

- 値が必要です

#### `--type`

環境タイプを設定します（ – activateでのみ使用）

- 値が必要です

#### `--no-clone-parent`

親ブランチのデータを複製しないでください（ – activateでのみ使用）

- 既定：`false`
- 値を受け付けません

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

環境の再デプロイ

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<environment>]
```

環境の関係を示す

### 引数

#### `environment`

環境

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示する関係プロパティ

- 値が必要です

#### `--refresh`

関係を更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

一時停止した環境の再開

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<files>]...
```

scpを使用した環境との間でのファイルのコピー

### 引数

#### `files`

コピーするファイル： remote：接頭辞を使用して、リモートの場所を定義します。

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--recursive`, `-r`

ディレクトリ全体を再帰的にコピーする

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-o|--option OPTION] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<cmd>]...
```

現在の環境へのSSH

### 引数

#### `cmd`

環境上で実行するコマンド。

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--pipe`

SSH URLのみを出力します。

- 既定：`false`
- 値を受け付けません

#### `--all`

すべてのSSH URLを出力します（アプリごとに）。

- 既定：`false`
- 値を受け付けません

#### `--option`, `-o`

SSHに追加オプションを渡す

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

環境のコードやデータを親から同期する

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child.

Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### 引数

#### `synchronize`

同期するもの：「コード」、「データ」またはその両方

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--rebase`

結合ではなくリベースによるコードの同期

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境のパブリック URLを取得する

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--primary`, `-1`

プライマリルートのURLのみを返します

- 既定：`false`
- 値を受け付けません

#### `--browser`

URLを開くために使用するブラウザー。 「0」を「なし」に設定します。

- 値が必要です

#### `--pipe`

URLをstdoutに出力します。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

環境上のXdebugへのトンネルを開きます

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--port`

ローカルポート

- 既定：`9000`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

単一の統合アクティビティに関する詳細情報の表示

### 引数

#### `integration`

統合ID。 リストから選択するには、空白のままにします。


#### `activity`

アクティビティ ID。 デフォルトは、最新の統合アクティビティです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

[非推奨オプション、未使用]

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `integration:activity:list`

```bash
magento-cloud integration:activities [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

統合のアクティビティのリストを取得

### 引数

#### `id`

統合ID。 リストから選択するには、空白のままにします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`

タイプ別にアクティビティをフィルタリングします。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--exclude-type`, `-x`

タイプ別のアクティビティを除外します。 値はコンマ （例：「a,b,c」）や空白で分割できます。 %または*文字は、タイプを除外するためのワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--limit`

表示される結果の数を制限

- 既定：`10`
- 値が必要です

#### `--start`

この日付より前に作成されたアクティビティのみが一覧表示されます

- 値が必要です

#### `--state`

ステータス別にアクティビティをフィルタリングします。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--result`

結果によるアクティビティのフィルタリング

- 値が必要です

#### `--incomplete`, `-i`

不完全なアクティビティのみを一覧表示

- 既定：`false`
- 値を受け付けません

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、created*、description*、type*、state*、result*、completed、progress、time_build、time_deploy、time_execute、time_wait （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

[非推奨オプション、未使用]

- 値が必要です


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

統合アクティビティのログの表示

### 引数

#### `integration`

統合ID。 リストから選択するには、空白のままにします。


#### `activity`

アクティビティ ID。 デフォルトは、最新の統合アクティビティです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--timestamps`, `-t`

各メッセージの横にタイムスタンプを表示

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

[非推奨オプション、未使用]

- 値が必要です


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

プロジェクトへの統合の追加

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`

統合タイプ （&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;, &#39;otlplog&#39;）

- 値が必要です

#### `--base-url`

サーバーインストールのベース URL

- 値が必要です

#### `--bitbucket-url`

Bitbucket Server インストールのベース URL

- 値が必要です

#### `--username`

Bitbucket Server ユーザー名

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

New Relic Logsのライセンスキー

- 値が必要です

#### `--server-project`

プロジェクト（例：「namespace/repo」）

- 値が必要です

#### `--repository`

追跡するリポジトリ （例：「所有者/リポジトリ」）

- 値が必要です

#### `--build-merge-requests`

GitLab：マージリクエストを環境としてビルドする

- 既定：`true`
- 値が必要です

#### `--build-pull-requests`

あらゆるプルリクエストを

- 既定：`true`
- 値が必要です

#### `--build-draft-pull-requests`

ドラフトプルリクエストの作成

- 既定：`true`
- 値が必要です

#### `--build-pull-requests-post-merge`

結合後の状態に基づいてプルリクエストを作成します

- 既定：`false`
- 値が必要です

#### `--build-wip-merge-requests`

GitLab: WIP結合リクエストの作成

- 既定：`true`
- 値が必要です

#### `--merge-requests-clone-parent-data`

GitLab：結合リクエスト用のデータのクローン

- 既定：`true`
- 値が必要です

#### `--pull-requests-clone-parent-data`

プルリクエスト用に親環境のデータを複製します

- 既定：`true`
- 値が必要です

#### `--resync-pull-requests`

ビルドごとにプルリクエスト環境データを再同期

- 既定：`false`
- 値が必要です

#### `--fetch-branches`

（非アクティブな環境として）リモートからすべてのブランチを取得する

- 既定：`true`
- 値が必要です

#### `--prune-branches`

リモートに存在しないブランチを削除

- 既定：`true`
- 値が必要です

#### `--resources-init`

新しいサービスを初期化する際に使用するリソース（「minimum」、「default」、「manual」、「parent」）

- 値が必要です

#### `--url`

統合のURLまたはAPI エンドポイント

- 値が必要です

#### `--shared-key`

Webhook:JWS共有秘密鍵

- 値が必要です

#### `--file`

アップロードするスクリプトを含むローカルファイルの名前

- 値が必要です

#### `--events`

アクションを実行するイベントのリスト（例：environment.push）

- 既定：`*`
- 値が必要です

#### `--states`

行動する状態のリスト（保留中、進行中、完了など）

- 既定：`complete`
- 値が必要です

#### `--environments`

含める環境ID

- 既定：`*`
- 値が必要です

#### `--excluded-environments`

除外する環境ID

- 既定：`[]`
- 値が必要です

#### `--from-address`

[ オプション ] アラートメールのカスタム送信元アドレス

- 値が必要です

#### `--recipients`

受信者のメールアドレス

- 既定：`[]`
- 値が必要です

#### `--channel`

Slackチャンネル

- 値が必要です

#### `--routing-key`

PagerDuty ルーティングキー

- 値が必要です

#### `--category`

フィルタリングに使用されるSumo Logic カテゴリ

- 値が必要です

#### `--index`

Splunk インデックス

- 値が必要です

#### `--sourcetype`

Splunk イベントソースタイプ

- 値が必要です

#### `--protocol`

Syslog トランスポートプロトコル （&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;）

- 既定：`tls`
- 値が必要です

#### `--syslog-host`

Syslog リレー/コレクターホスト

- 値が必要です

#### `--syslog-port`

Syslog リレー/コレクターポート

- 値が必要です

#### `--facility`

Syslog ファシリティ

- 既定：`1`
- 値が必要です

#### `--message-format`

Syslog メッセージ形式（&#39;rfc3164&#39;または&#39;rfc5424&#39;）

- 既定：`rfc5424`
- 値が必要です

#### `--auth-mode`

認証モード （&#39;prefix&#39;または&#39;structured_data&#39;）

- 既定：`prefix`
- 値が必要です

#### `--auth-token`

認証トークン

- 値が必要です

#### `--verify-tls`

HTTPS証明書検証を有効にするかどうか（推奨）

- 既定：`true`
- 値が必要です

#### `--header`

POST リクエストで使用するHTTP ヘッダー。 名前と値はコロン（:）で区切ります。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

プロジェクトからの統合の削除

### 引数

#### `id`

統合ID。 リストから選択するには、空白のままにします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

統合の詳細の表示

### 引数

#### `id`

統合ID。 リストから選択するには、空白のままにします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示する統合プロパティ

- 値を受け入れる

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `integration:list`

```bash
magento-cloud integrations [-t|--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクト統合のリストを表示する

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`, `-t`

タイプ別にフィルタリング

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id、概要、タイプ。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

統合の更新

### 引数

#### `id`

更新する統合のID

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--type`

統合タイプ （&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;, &#39;otlplog&#39;）

- 値が必要です

#### `--base-url`

サーバーインストールのベース URL

- 値が必要です

#### `--bitbucket-url`

Bitbucket Server インストールのベース URL

- 値が必要です

#### `--username`

Bitbucket Server ユーザー名

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

New Relic Logsのライセンスキー

- 値が必要です

#### `--server-project`

プロジェクト（例：「namespace/repo」）

- 値が必要です

#### `--repository`

追跡するリポジトリ （例：「所有者/リポジトリ」）

- 値が必要です

#### `--build-merge-requests`

GitLab：マージリクエストを環境としてビルドする

- 既定：`true`
- 値が必要です

#### `--build-pull-requests`

あらゆるプルリクエストを

- 既定：`true`
- 値が必要です

#### `--build-draft-pull-requests`

ドラフトプルリクエストの作成

- 既定：`true`
- 値が必要です

#### `--build-pull-requests-post-merge`

結合後の状態に基づいてプルリクエストを作成します

- 既定：`false`
- 値が必要です

#### `--build-wip-merge-requests`

GitLab: WIP結合リクエストの作成

- 既定：`true`
- 値が必要です

#### `--merge-requests-clone-parent-data`

GitLab：結合リクエスト用のデータのクローン

- 既定：`true`
- 値が必要です

#### `--pull-requests-clone-parent-data`

プルリクエスト用に親環境のデータを複製します

- 既定：`true`
- 値が必要です

#### `--resync-pull-requests`

ビルドごとにプルリクエスト環境データを再同期

- 既定：`false`
- 値が必要です

#### `--fetch-branches`

（非アクティブな環境として）リモートからすべてのブランチを取得する

- 既定：`true`
- 値が必要です

#### `--prune-branches`

リモートに存在しないブランチを削除

- 既定：`true`
- 値が必要です

#### `--resources-init`

新しいサービスを初期化する際に使用するリソース（「minimum」、「default」、「manual」、「parent」）

- 値が必要です

#### `--url`

統合のURLまたはAPI エンドポイント

- 値が必要です

#### `--shared-key`

Webhook:JWS共有秘密鍵

- 値が必要です

#### `--file`

アップロードするスクリプトを含むローカルファイルの名前

- 値が必要です

#### `--events`

アクションを実行するイベントのリスト（例：environment.push）

- 既定：`*`
- 値が必要です

#### `--states`

行動する状態のリスト（保留中、進行中、完了など）

- 既定：`complete`
- 値が必要です

#### `--environments`

含める環境ID

- 既定：`*`
- 値が必要です

#### `--excluded-environments`

除外する環境ID

- 既定：`[]`
- 値が必要です

#### `--from-address`

[ オプション ] アラートメールのカスタム送信元アドレス

- 値が必要です

#### `--recipients`

受信者のメールアドレス

- 既定：`[]`
- 値が必要です

#### `--channel`

Slackチャンネル

- 値が必要です

#### `--routing-key`

PagerDuty ルーティングキー

- 値が必要です

#### `--category`

フィルタリングに使用されるSumo Logic カテゴリ

- 値が必要です

#### `--index`

Splunk インデックス

- 値が必要です

#### `--sourcetype`

Splunk イベントソースタイプ

- 値が必要です

#### `--protocol`

Syslog トランスポートプロトコル （&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;）

- 既定：`tls`
- 値が必要です

#### `--syslog-host`

Syslog リレー/コレクターホスト

- 値が必要です

#### `--syslog-port`

Syslog リレー/コレクターポート

- 値が必要です

#### `--facility`

Syslog ファシリティ

- 既定：`1`
- 値が必要です

#### `--message-format`

Syslog メッセージ形式（&#39;rfc3164&#39;または&#39;rfc5424&#39;）

- 既定：`rfc5424`
- 値が必要です

#### `--auth-mode`

認証モード （&#39;prefix&#39;または&#39;structured_data&#39;）

- 既定：`prefix`
- 値が必要です

#### `--auth-token`

認証トークン

- 値が必要です

#### `--verify-tls`

HTTPS証明書検証を有効にするかどうか（推奨）

- 既定：`true`
- 値が必要です

#### `--header`

POST リクエストで使用するHTTP ヘッダー。 名前と値はコロン（:）で区切ります。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
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

統合ID。 リストから選択するには、空白のままにします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

現在のプロジェクトをローカルにビルドする

### 引数

#### `app`

ビルドするアプリケーションを指定

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--abslinks`, `-a`

絶対リンクを使用

- 既定：`false`
- 値を受け付けません

#### `--source`, `-s`

ソースディレクトリ。 デフォルトは現在のプロジェクトルートです。

- 値が必要です

#### `--destination`, `-d`

各アプリのweb ルートがシンボリックリンクされる宛先。 デフォルト：_www

- 値が必要です

#### `--copy`, `-c`

ソースからシンボリックリンクするのではなく、ビルドディレクトリにコピーします

- 既定：`false`
- 値を受け付けません

#### `--clone`

Gitを使用して、現在のHEADをビルドディレクトリに複製します

- 既定：`false`
- 値を受け付けません

#### `--run-deploy-hooks`

デプロイおよび/またはpost_deploy フックの実行

- 既定：`false`
- 値を受け付けません

#### `--no-clean`

古いビルドを削除しない

- 既定：`false`
- 値を受け付けません

#### `--no-archive`

ビルドアーカイブを作成または使用しない

- 既定：`false`
- 値を受け付けません

#### `--no-backup`

前のビルドをバックアップしない

- 既定：`false`
- 値を受け付けません

#### `--no-cache`

キャッシュを無効にする

- 既定：`false`
- 値を受け付けません

#### `--no-build-hooks`

ビルド後のフックを実行しない

- 既定：`false`
- 値を受け付けません

#### `--no-deps`

ビルド依存関係をローカルにインストールしない

- 既定：`false`
- 値を受け付けません

#### `--working-copy`

Drush: gitを使用して、単にバージョンをダウンロードするのではなく、各Drupal モジュールのリポジトリを複製します

- 既定：`false`
- 値を受け付けません

#### `--concurrency`

ドラッシュ：同時に処理される同時プロジェクトの数を設定します

- 既定：`4`
- 値が必要です

#### `--lock`

Drush: ロックファイルを作成または更新します（Drush バージョン 7+でのみ利用可能）

- 既定：`false`
- 値を受け付けません


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

ローカルプロジェクトのルートの検索

### 引数

#### `subdir`

検索するサブディレクトリ（「local」、「web」、「shared」）

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

環境のCPU、ディスク、およびメモリの指標を表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- 既定：`false`
- 値を受け付けません

#### `--range`, `-r`

時間範囲。 指標は、終了時間（ – to）まで、この期間に読み込まれます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最小5m、最大8h以上（プロジェクトによって異なります）、デフォルトは10mです。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 初期設定では、範囲の除算が行われます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最低1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは今です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データポイントのみを表示

- 既定：`false`
- 値を受け付けません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルター%または*文字をワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（ – serviceが指定されていない場合）。 バージョンは必要ありません。 %または*の文字は、ワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、cpu_percent*、mem_percent*、disk_percent*、tmp_disk_percent*、cpu_limit、cpu_used、disk_limit、disk_used、inodes_limit、inodes_percent、inodes_used、mem_limit、mem_used、tmp_disk_limit、tmp_disk_used、tmp_inodes_limit、tmp_inodes_percent、tmp_inodes_used、type （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

CPUでの環境の使用状況の表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--range`, `-r`

時間範囲。 指標は、終了時間（ – to）まで、この期間に読み込まれます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最小5m、最大8h以上（プロジェクトによって異なります）、デフォルトは10mです。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 初期設定では、範囲の除算が行われます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最低1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは今です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データポイントのみを表示

- 既定：`false`
- 値を受け付けません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルター%または*文字をワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（ – serviceが指定されていない場合）。 バージョンは必要ありません。 %または*の文字は、ワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、used*、limit*、%*、type （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

環境のディスク使用状況を表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- 既定：`false`
- 値を受け付けません

#### `--range`, `-r`

時間範囲。 指標は、終了時間（ – to）まで、この期間に読み込まれます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最小5m、最大8h以上（プロジェクトによって異なります）、デフォルトは10mです。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 初期設定では、範囲の除算が行われます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最低1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは今です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データポイントのみを表示

- 既定：`false`
- 値を受け付けません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルター%または*文字をワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（ – serviceが指定されていない場合）。 バージョンは必要ありません。 %または*の文字は、ワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--tmp`

一時的なディスク使用状況をレポートします（列：timestamp, service, tmp_used, tmp_limit, tmp_percent, tmp_ipercent）

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*, service*, used*, limit*,%*, ipercent*, tmp_percent*, ilimit, iused, tmp_ilimit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, type （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

環境のメモリ使用量を表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--bytes`, `-B`

サイズをバイト単位で表示

- 既定：`false`
- 値を受け付けません

#### `--range`, `-r`

時間範囲。 指標は、終了時間（ – to）まで、この期間に読み込まれます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最小5m、最大8h以上（プロジェクトによって異なります）、デフォルトは10mです。

- 値が必要です

#### `--interval`, `-i`

時間間隔。 初期設定では、範囲の除算が行われます。 単位は、時間（h）、分（m）、秒（s）のいずれかを指定できます。 最低1m。

- 値が必要です

#### `--to`

終了時間。 デフォルトは今です。

- 値が必要です

#### `--latest`, `-1`

最新の単一データポイントのみを表示

- 既定：`false`
- 値を受け付けません

#### `--service`, `-s`

サービス名またはアプリケーション名でフィルター%または*文字をワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--type`

サービスタイプでフィルタリングします（ – serviceが指定されていない場合）。 バージョンは必要ありません。 %または*の文字は、ワイルドカードとして使用できます。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：timestamp*、service*、used*、limit*、%*、type （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

rsyncを使用したマウントからのファイルのダウンロード

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--all`, `-a`

すべてのマウントからダウンロード

- 既定：`false`
- 値を受け付けません

#### `--mount`, `-m`

マウント （アプリの相対パスとして）

- 値が必要です

#### `--target`

ファイルのダウンロード先ディレクトリ。 —allを使用すると、マウントパスが追加されます

- 値が必要です

#### `--source-path`

—allが使用されている場合は、マウントのソースパス（マウントパスではなく）をターゲットのサブディレクトリとして使用します

- 既定：`false`
- 値を受け付けません

#### `--delete`

ターゲットディレクトリ内の不要なファイルを削除するかどうか

- 既定：`false`
- 値を受け付けません

#### `--exclude`

ダウンロードから除外するファイル（パターン）

- 既定：`[]`
- 値が必要です

#### `--include`

除外しないファイル（パターン）

- 既定：`[]`
- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

マウントのリストを取得

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--paths`

マウントパスのみを出力します（1行につき1つ）

- 既定：`false`
- 値を受け付けません

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：定義、パス。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

rsyncを使用したマウントへのファイルのアップロード

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--source`

アップロードするファイルを含むディレクトリ

- 値が必要です

#### `--mount`, `-m`

マウント （アプリの相対パスとして）

- 値が必要です

#### `--delete`

マウント内の不要なファイルを削除するかどうか

- 既定：`false`
- 値を受け付けません

#### `--exclude`

アップロードから除外するファイル（パターン）

- 既定：`[]`
- 値が必要です

#### `--include`

除外しないファイル（パターン）

- 既定：`[]`
- 値が必要です

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--instance`, `-I`

インスタンス ID

- 値が必要です


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

環境でのランタイム操作のリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--full`

表示するコマンドの長さを制限しないでください。 デフォルトの制限は24行です。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：service*、name*、start*、role、stop （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

環境に対する操作の実行

### 引数

#### `operation`

操作名

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--worker`

ワーカー名

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

プロジェクトのビルドキャッシュをクリアする

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [--] [<project>] [<directory>]
```

プロジェクトをローカルに複製する

### 引数

#### `project`

プロジェクト ID


#### `directory`

複製するディレクトリ。 デフォルトはプロジェクトタイトルです

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--environment`, `-e`

複製する環境ID。 デフォルトは、プロジェクトのデフォルト、または最初に使用可能な環境です

- 値が必要です

#### `--depth`

浅いクローンを作成：履歴のコミット数を制限します

- 値が必要です

#### `--build`

複製後にプロジェクトを作成する

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

プロジェクトのプロパティの読み取りまたは設定

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

アクティブなすべてのプロジェクトのリストを取得

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--pipe`

プロジェクト IDの簡単なリストを出力します。 ページネーションを無効にします。

- 既定：`false`
- 値を受け付けません

#### `--region`

地域でフィルター（完全一致）

- 値が必要です

#### `--title`

タイトルでフィルター（大文字と小文字を区別しない検索）

- 値が必要です

#### `--my`

所有しているプロジェクトのみを表示

- 既定：`false`
- 値を受け付けません

#### `--refresh`

リストを更新するかどうか

- 既定：`1`
- 値が必要です

#### `--sort`

並べ替えるプロパティ

- 既定：`title`
- 値が必要です

#### `--reverse`

逆順（降順）で並べ替え

- 既定：`false`
- 値を受け付けません

#### `--page`

ページ番号： これにより、設定または – countにもかかわらず、ページ分割が可能になります。 —pipeが指定されている場合は無視されます。

- 値が必要です

#### `--count`, `-c`

ページごとに表示するプロジェクトの数。 ページネーションを無効にするには、0を使用します。 —pageが指定されている場合は無視されます。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`

表示する列。 使用可能な列：id*、title*、region*、created_at、organization_id、organization_label、organization_name、organization_type、status （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

現在のGit リポジトリのリモートプロジェクトの設定

### 引数

#### `project`

プロジェクト ID

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

プロジェクトリポジトリ内のファイルの読み取り

### 引数

#### `path`

ファイルへのパス

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--commit`, `-c`

コミット SHA。 これは、親コミットの「HEAD」とキャレット（^）またはチルダ（～）接尾辞を受け入れることもできます。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

プロジェクトリポジトリ内のファイルのリスト

### 引数

#### `path`

サブディレクトリへのパス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--directories`, `-d`

ディレクトリのみを表示

- 既定：`false`
- 値を受け付けません

#### `--files`, `-f`

ファイルのみを表示

- 既定：`false`
- 値を受け付けません

#### `--git-style`

「git ls-tree」と同様のスタイル出力

- 既定：`false`
- 値を受け付けません

#### `--commit`, `-c`

コミット SHA。 これは、親コミットの「HEAD」とキャレット（^）またはチルダ（～）接尾辞を受け入れることもできます。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

プロジェクトリポジトリ内のディレクトリまたはファイルの読み取り

### 引数

#### `path`

ディレクトリまたはファイルへのパス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--commit`, `-c`

コミット SHA。 これは、親コミットの「HEAD」とキャレット（^）またはチルダ（～）接尾辞を受け入れることもできます。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `resources:build:get`

```bash
magento-cloud build-resources:get [-p|--project PROJECT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

プロジェクトのビルド リソースの表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：cpu、メモリ。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

ルートに関する詳細情報の表示

### 引数

#### `route`

ルートのオリジナル URL

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--id`

選択するルート ID

- 値が必要です

#### `--primary`, `-1`

プライマリルートの選択

- 既定：`false`
- 値を受け付けません

#### `--property`, `-P`

表示するプロパティ

- 値が必要です

#### `--refresh`

ルートのキャッシュをバイパスする

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

[非推奨のオプション、使用されなくなった]

- 値が必要です

#### `--identity-file`, `-i`

[非推奨のオプション、使用されなくなった]

- 値が必要です


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

環境のすべてのルートのリスト

### 引数

#### `environment`

環境ID

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

ルートのキャッシュをバイパスする

- 既定：`false`
- 値を受け付けません

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：route*、type*、to*、url （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

CLI設定ファイルのインストールまたは更新

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--shell-type`

オートコンプリート用のシェルタイプ （bashまたはzsh）

- 値が必要です


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

CLIを最新バージョンに更新します

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--no-major`

マイナーバージョンとパッチバージョンの間でのみ更新

- 既定：`false`
- 値を受け付けません

#### `--unstable`

新しい不安定なバージョンが利用可能な場合は、更新します

- 既定：`false`
- 値を受け付けません

#### `--manifest`

マニフェストファイルの場所を上書きする

- 値が必要です

#### `--current-version`

現在のバージョンを上書き

- 値が必要です

#### `--timeout`

バージョン チェックのタイムアウト

- 既定：`30`
- 値が必要です


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

プロジェクト内のサービスのリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--pipe`

サービス名のリストのみを出力

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：ディスク、名前、サイズ、タイプ。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDBからデータのバイナリアーカイブダンプを作成する

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--collection`, `-c`

ダンプするコレクション

- 値が必要です

#### `--gzip`, `-z`

gzipを使用してダンプを圧縮する

- 既定：`false`
- 値を受け付けません

#### `--stdout`, `-o`

ファイルではなくSTDOUTに出力

- 既定：`false`
- 値を受け付けません

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDBからのデータのエクスポート

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--collection`, `-c`

書き出すコレクション

- 値が必要です

#### `--jsonArray`

データを単一のJSON配列としてエクスポートする

- 既定：`false`
- 値を受け付けません

#### `--type`

書き出しタイプ（例：「csv」）

- 値が必要です

#### `--fields`, `-f`

エクスポートするフィールド

- 既定：`[]`
- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDBへのデータのバイナリアーカイブダンプの復元

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--collection`, `-c`

復元するコレクション

- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB シェルの使用

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--eval`

JavaScript フラグメントをシェルに渡す

- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]...
```

Redis CLIへのアクセス

### 引数

#### `args`

redis-cli コマンドに追加する引数

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `service:valkey-cli`

```bash
magento-cloud valkey [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]...
```

Valkey CLIへのアクセス

### 引数

#### `args`

valkey-cli コマンドに追加する引数

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

環境のスナップショットを作成する

### 引数

#### `environment`

環境

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--live`

ライブスナップショット：環境を停止しないでください。 設定すると、環境は実行中のままになり、スナップショット中に接続に対してオープンになります。 これにより、データが一貫性のない状態でバックアップされるリスクが生じ、ダウンタイムが削減されます。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

環境スナップショットの削除

### 引数

#### `id`

スナップショットのID。 非インタラクティブモードでは必須です。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

環境スナップショットの表示

### 引数

#### `id`

スナップショットのID。 デフォルトは最新のものです。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示するプロパティ。

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

環境の使用可能なスナップショットのリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [--no-code] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

環境スナップショットの復元

### 引数

#### `snapshot`

スナップショットのID。 デフォルトは最新のものです

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--target`

復元する環境。 デフォルトは、スナップショットの現在の環境です

- 値が必要です

#### `--branch-from`

—targetがまだ存在しない場合、これは新しい環境の親を指定します

- 値が必要です

#### `--no-code`

コードを復元せず、データのみを復元します。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

環境のソース操作のリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--full`

表示するコマンドの長さを制限しないでください。 デフォルトの制限は24行です。

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：アプリ、コマンド、操作。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

ソース操作の実行

### 引数

#### `operation`

操作名

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--variable`

操作中に設定する変数（形式type:name=value）

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

SSH証明書の生成

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

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh-only`

必要に応じて証明書のみを更新します（SSH設定を書き込まないでください）

- 既定：`false`
- 値を受け付けません

#### `--new`

証明書を強制的に更新する

- 既定：`false`
- 値を受け付けません

#### `--new-key`

新しいキーペアを強制的に生成する

- 既定：`false`
- 値を受け付けません


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

新しいSSH キーを追加

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 引数

#### `path`

既存のSSH公開鍵へのパス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--name`

キーを識別する名前

- 値が必要です


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
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

削除するSSH キーのID

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

アカウント内のSSH キーのリストを取得する

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：id*、title*、path*、フィンガープリント （* = デフォルト列）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

購読プロパティの読み取りまたは変更

### 引数

#### `property`

プロパティの名前


#### `value`

プロパティに新しい値を設定します

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--id`, `-s`

サブスクリプション ID

- 値が必要です

#### `--date-fmt`

日付形式（PHP日付形式文字列として）

- 既定：`c`
- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH トンネルを閉じる

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--all`, `-a`

すべてのトンネルを閉じる

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH トンネルの関係情報の表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

表示する関係プロパティ

- 値が必要です

#### `--encode`, `-c`

base64 エンコードされたJSONとして出力

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

SSH トンネルのリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--all`, `-a`

すべてのトンネルを表示

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：port*、project*、environment*、app*、relationship*、url （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

アプリの関係に対するSSH トンネルを開く

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

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--gateway-ports`, `-g`

リモート ホストがローカル転送ポートに接続することを許可する

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP]
```

アプリ リレーションシップへの単一のSSH トンネルを開く

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--port`

ローカルポート

- 値が必要です

#### `--gateway-ports`, `-g`

リモート ホストがローカル転送ポートに接続することを許可する

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--app`, `-A`

リモートアプリケーション名

- 値が必要です

#### `--relationship`, `-r`

使用するサービス関係

- 値が必要です


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

プロジェクトへのユーザーの追加

### 引数

#### `email`

ユーザーのメールアドレス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--role`, `-r`

ユーザーのプロジェクトの役割（「管理者」または「閲覧者」）または環境タイプの役割（「ステージング :contributor」または「実稼動:viewer」など）。 環境タイプからユーザーを削除するには、役割を「なし」に設定します。 %または*文字は、環境タイプのワイルドカードとして使用できます。例：&#39;%:viewer&#39;を使用すると、すべてのタイプでユーザーに「閲覧者」の役割を与えることができます。 役割は省略できます（例：「production:v」）。

- 既定：`[]`
- 値が必要です

#### `--force-invite`

招待状が既に送信されている場合でも送信する

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

プロジェクトからユーザーを削除する

### 引数

#### `email`

ユーザーのメールアドレス

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

ユーザーの役割の表示

### 引数

#### `email`

ユーザーのメールアドレス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--level`, `-l`

役割レベル （「プロジェクト」または「環境」）

- 値が必要です

#### `--pipe`

ロールをstdoutに出力します（変更を加えた後）

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません

#### `--role`, `-r`

[非推奨：ユーザー:updateを使用してユーザーの役割]を変更します

- 値が必要です


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

プロジェクトユーザーのリスト

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：email*、name*、role*、id*、granted_at、updated_at （* = default columns）。 「+」という文字は、デフォルトの列のプレースホルダーとして使用できます。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

プロジェクトのユーザー役割の更新

### 引数

#### `email`

ユーザーのメールアドレス

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--role`, `-r`

ユーザーのプロジェクトの役割（「管理者」または「閲覧者」）または環境タイプの役割（「ステージング :contributor」または「実稼動:viewer」など）。 環境タイプからユーザーを削除するには、役割を「なし」に設定します。 %または*文字は、環境タイプのワイルドカードとして使用できます。例：&#39;%:viewer&#39;を使用すると、すべてのタイプでユーザーに「閲覧者」の役割を与えることができます。 役割は省略できます（例：「production:v」）。

- 既定：`[]`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

変数の作成

### 引数

#### `name`

変数名

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--update`, `-u`

変数が既に存在する場合は更新します

- 既定：`false`
- 値を受け付けません

#### `--level`, `-l`

変数（「プロジェクト」または「環境」）を設定するレベル

- 値が必要です

#### `--name`

変数名

- 値が必要です

#### `--value`

変数の値

- 値が必要です

#### `--json`

変数値がJSON形式かどうか

- 既定：`false`
- 値が必要です

#### `--sensitive`

変数値が機密性があるかどうか

- 既定：`false`
- 値が必要です

#### `--prefix`

変数名のプレフィックス。例えば、「env」のように、そのタイプを決定できます。 名前にまだプレフィックスが含まれていない場合にのみ適用されます。 （例：「none」または「env:」）

- 既定：`none`
- 値が必要です

#### `--enabled`

変数を環境上で有効にする必要があるかどうか

- 既定：`true`
- 値が必要です

#### `--inheritable`

変数が子環境で継承できるかどうか

- 既定：`true`
- 値が必要です

#### `--visible-build`

変数をビルド時に表示するかどうか

- 値が必要です

#### `--visible-runtime`

変数を実行時に表示するかどうか

- 既定：`true`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

変数の削除

### 引数

#### `name`

変数名

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--level`, `-l`

変数レベル （「project」、「environment」、「p」、「e」）

- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

変数の表示

### 引数

#### `name`

変数の名前

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--property`, `-P`

単一の変数プロパティの表示

- 値が必要です

#### `--level`, `-l`

変数レベル （「project」、「environment」、「p」、「e」）

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--pipe`

[非推奨オプション ]変数値のみを出力

- 既定：`false`
- 値を受け付けません


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

リスト変数

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--level`, `-l`

変数レベル （「project」、「environment」、「p」、「e」）

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：is_enabled、level、name、value。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

変数の更新

### 引数

#### `name`

変数名

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--allow-no-change`

変更が提供されない場合は、成功（終了コードがゼロ）を返します

- 既定：`false`
- 値を受け付けません

#### `--level`, `-l`

変数レベル （「project」、「environment」、「p」、「e」）

- 値が必要です

#### `--value`

変数の値

- 値が必要です

#### `--json`

変数値がJSON形式かどうか

- 既定：`false`
- 値が必要です

#### `--sensitive`

変数値が機密性があるかどうか

- 既定：`false`
- 値が必要です

#### `--enabled`

変数を環境上で有効にする必要があるかどうか

- 既定：`true`
- 値が必要です

#### `--inheritable`

変数が子環境で継承できるかどうか

- 既定：`true`
- 値が必要です

#### `--visible-build`

変数をビルド時に表示するかどうか

- 値が必要です

#### `--visible-runtime`

変数を実行時に表示するかどうか

- 既定：`true`
- 値が必要です

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--no-wait`, `-W`

操作が完了するのを待ってはいけません

- 既定：`false`
- 値を受け付けません

#### `--wait`

操作が完了するのを待ちます（デフォルト）

- 既定：`false`
- 値を受け付けません


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

デプロイされたすべてのワーカーのリストを取得

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--refresh`

キャッシュを更新するかどうか

- 既定：`false`
- 値を受け付けません

#### `--pipe`

ワーカー名のみのリストを出力

- 既定：`false`
- 値を受け付けません

#### `--project`, `-p`

プロジェクト IDまたはURL

- 値が必要です

#### `--environment`, `-e`

環境ID。 「。」を使用 プロジェクトのデフォルト環境を選択します。

- 値が必要です

#### `--format`

出力フォーマット：table、csv、tsv、またはplain

- 既定：`table`
- 値が必要です

#### `--columns`, `-c`

表示する列。 使用可能な列：コマンド、名前、タイプ。 %または*の文字は、ワイルドカードとして使用できます。 値はコンマ （例：「a,b,c」）や空白で分割できます。

- 既定：`[]`
- 値が必要です

#### `--no-header`

テーブルヘッダーを出力しない

- 既定：`false`
- 値を受け付けません
