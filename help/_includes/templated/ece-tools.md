---
source-git-commit: eff03e0955ae067eb509c7d49eb59f64b3bb1c6a
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 3%

---
# ece-tools

**バージョン**: 2002.2.11

このリファレンスには、`ece-tools` コマンドラインツールを通じて利用できる34のコマンドが含まれています。
最初のリストは、クラウドインフラストラクチャ上のAdobe Commerceで`ece-tools list` コマンドを使用して自動生成されます。

## 一般

この参照は、アプリケーションコードベースから生成されます。 コンテンツを変更するには、_フィードバックを送信してください_ （右上のリンクを見つけてください）。 貢献度ガイドラインについては、[&#x200B; コード貢献度](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)を参照してください。

### グローバルオプション

#### `--help`, `-h`

指定されたコマンドのヘルプを表示します。 コマンドが指定されていない場合は、list コマンドのdisplay help

- 既定：`false`
- 値を受け付けません

#### `--silent`

メッセージを出力しない

- 既定：`false`
- 値を受け付けません

#### `--quiet`, `-q`

エラーのみが表示されます。 他のすべての出力は抑制されます

- 既定：`false`
- 値を受け付けません

#### `--verbose`, `-v|-vv|-vvv`

メッセージの冗長性を上げます。通常の出力は1、詳細な出力は2、デバッグは3

- 既定：`false`
- 値を受け付けません

#### `--version`, `-V`

このアプリケーションのバージョンを表示

- 既定：`false`
- 値を受け付けません

#### `--ansi`

ANSI出力を強制（または – no-ansiを無効にする）

- 値を受け付けません

#### `--no-ansi`

「 – ansi」オプションを無効にする

- 値を受け付けません

#### `--no-interaction`, `-n`

インタラクティブな質問は避けてください

- 既定：`false`
- 値を受け付けません


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

シェル補完の提案を提供する内部コマンド

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--shell`, `-s`

シェルタイプ （&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;）

- 値が必要です

#### `--input`, `-i`

入力トークンの配列（例：COMP_WORDSまたはargv）

- 既定：`[]`
- 値が必要です

#### `--current`, `-c`

カーソルが置かれている「input」配列のインデックス（例：COMP_CWORD）

- 値が必要です

#### `--api-version`, `-a`

完了スクリプトのAPI バージョン

- 値が必要です

#### `--symfony`, `-S`

非推奨

- 値が必要です


## `build`

```bash
ece-tools build
```

アプリケーションを構築します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

シェル完了スクリプトをダンプします

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### 引数

#### `shell`

シェルタイプ （例：「bash」）、環境変数「$SHELL」の値が指定されていない場合は使用されます

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--debug`

完了デバッグログのテール

- 既定：`false`
- 値を受け付けません


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

データベースのバックアップを作成します。

### 引数

#### `databases`

バックアップ用データベース。 使用可能な値：[ メイン見積もり売上]。 引数の値が指定されていない場合、データベースのバックアップは、`MAGENTO_CLOUD_RELATIONSHIP`環境変数または.magento.env.yaml設定ファイルの`stage.deploy.DATABASE_CONFIGURATION` プロパティに格納されている資格情報を使用して作成されます。

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--remove-definers`, `-d`

データベースダンプからの定義の削除

- 既定：`false`
- 値を受け付けません

#### `--dump-directory`, `-a`

ダンプを保存するための代替ディレクトリの使用

- 値が必要です


## `deploy`

```bash
ece-tools deploy
```

アプリケーションをデプロイします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

コマンドのヘルプの表示

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### 引数

#### `command_name`

コマンド名

- 既定：`help`

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--format`

出力形式（txt、xml、json、md）

- 既定：`txt`
- 値が必要です

#### `--raw`

Raw コマンド ヘルプを出力するには

- 既定：`false`
- 値を受け付けません


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

リストコマンド

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### 引数

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

#### `--short`

コマンドの引数の記述をスキップするには

- 既定：`false`
- 値を受け付けません


## `patch`

```bash
ece-tools patch
```

カスタムパッチを適用します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `post-deploy`

```bash
ece-tools post-deploy
```

デプロイ操作の後に実行します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `run`

```bash
ece-tools run <scenario>...
```

シナリオを実行します。

### 引数

#### `scenario`

シナリオ

- 既定：`[]`
- 必須

- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `backup:list`

```bash
ece-tools backup:list
```

バックアップ ファイルのリストを表示します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

重要な設定ファイルを復元します。 バックアップ :listを実行して、バックアップ ファイルのリストを表示します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--force`, `-f`

バックアップの復元中に既存のファイルを上書き

- 既定：`false`
- 値を受け付けません

#### `--file`

特定のファイル回復パス

- 値を受け入れる


## `build:generate`

```bash
ece-tools build:generate
```

ビルドステージに必要なすべてのファイルを生成します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `build:transfer`

```bash
ece-tools build:transfer
```

生成されたファイルをinit ディレクトリに転送します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

指定されたビルド、デプロイ、デプロイ後の変数設定を使用して`.magento.env.yaml` ファイルを作成します。 既存の`.magento.env.yaml` ファイルをすべて上書きします。

### 引数

#### `configuration`

JSON形式での設定

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

指定された設定で既存の`.magento.env.yaml` ファイルを更新します。 ファイルが存在しない場合は、`.magento.env.yaml` ファイルを作成します。

### 引数

#### `configuration`

JSON形式での設定

- 必須

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

`.magento.env.yaml`設定ファイルを検証します

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `config:dump`

```bash
ece-tools config:dumpdump
```

静的コンテンツのデプロイメント用のダンプ設定。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cron:disable`

```bash
ece-tools cron:disable
```

すべてのMagento cron プロセスを無効にし、実行中のすべてのプロセスを終了します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cron:enable`

```bash
ece-tools cron:enable
```

Magento cron プロセスを有効にします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cron:kill`

```bash
ece-tools cron:kill
```

すべてのMagento cron プロセスを終了します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

「実行中」状態で停止したcron ジョブのロックを解除します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--job-code`

ロックを解除するCron ジョブ コード。

- 既定：`[]`
- 複数の値を受け入れる


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

schema.error.yaml ファイルからdist/error-codes.md ファイルを生成します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Gitからデプロイメント用のコンポーザーを更新します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

エンコードされたクラウド設定環境変数を表示します。

### 引数

#### `variable`

表示する環境変数、可能なオプション：サービス、ルート、変数

- 既定：`[]`
- 配列

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

エラーID別のエラーに関する情報または前回のデプロイメントのすべてのエラーに関する情報を表示します。

### 引数

#### `error-code`

エラーコード （コマンドが渡されない場合）最後のデプロイメントからのすべてのエラーに関する情報を表示

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。

#### `--json`, `-j`

JSON形式で結果を取得するために使用します

- 既定：`false`
- 値を受け付けません


## `module:refresh`

```bash
ece-tools module:refresh
```

設定を更新して、新しく追加されたモジュールを有効にします。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `schema:generate`

```bash
ece-tools schema:generate
```

スキーマ *.dist ファイルを生成します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

構成の理想的な状態を検証します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

マスタースレーブ設定を検証します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

ビルド設定でSCDを検証します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

オンデマンド設定のSCDを検証します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

デプロイ設定時にSCDを検証します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

DBを分割する機能と、DBが既に分割されているかどうかを確認します。

### オプション

グローバルオプションについては、[&#x200B; グローバルオプション &#x200B;](#global-options)を参照してください。
