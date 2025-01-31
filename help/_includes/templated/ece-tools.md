---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 0%

---
# ece-tools

**バージョン**:2002.2.0

このリファレンスには、`ece-tools` のコマンド ライン ツールで使用できる 34 のコマンドが含まれています。
最初のリストは、クラウドインフラストラクチャ上のAdobe Commerceで `ece-tools list` コマンドを使用して自動生成されます。

## 一般

この参照は、アプリケーションコードベースから生成されます。 コンテンツを変更するには、_フィードバックを送信_ （右上のリンクを参照）。 投稿のガイドラインについては、[ コードの投稿 ](https://developer.adobe.com/commerce/contributor/guides/code-contributions/) を参照してください。

### グローバルオプション

#### `--help`, `-h`

指定されたコマンドのヘルプを表示します。 コマンドが指定されていない場合は、list コマンドの表示ヘルプが表示されます

- デフォルト：`false`
- 値を受け入れません

#### `--quiet`, `-q`

メッセージを出力しない

- デフォルト：`false`
- 値を受け入れません

#### `--verbose`, `-v|-vv|-vvv`

メッセージの冗長さを増やします。通常の出力の場合は 1、詳細な出力の場合は 2、デバッグの場合は 3 です。

- デフォルト：`false`
- 値を受け入れません

#### `--version`, `-V`

このアプリケーションのバージョンを表示

- デフォルト：`false`
- 値を受け入れません

#### `--ansi`

ANSI 出力を強制（または無効化 – no-ansi）

- 値を受け入れません

#### `--no-ansi`

「– ansi」オプションを否定します

- デフォルト：`false`
- 値を受け入れません

#### `--no-interaction`, `-n`

対話型の質問をしない

- デフォルト：`false`
- 値を受け入れません


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

シェル補完の候補を提供する内部コマンド

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--shell`, `-s`

シェル型（「bash」、「fish」、「zsh」）

- 値が必要です

#### `--input`, `-i`

入力トークンの配列（例：COMP_WORDS または argv）

- デフォルト：`[]`
- 値が必要です

#### `--current`, `-c`

カーソルがある「入力」配列のインデックス （例：COMP_CWORD）

- 値が必要です

#### `--api-version`, `-a`

完了スクリプトの API バージョン

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

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

シェル完了スクリプトをダンプ

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

シェルの型（例：&#39;&#39;bash&#39;&#39;）は、&#39;&#39;$SHELL&#39;&#39;環境変数の値が指定されていない場合に使用されます

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--debug`

完了デバッグログのテール

- デフォルト：`false`
- 値を受け入れません


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

データベースのバックアップを作成します。

### 引数

#### `databases`

バックアップするデータベース。 使用可能な値：[ メインの見積もり売上 ]。 引数の値が指定されていない場合、データベースバックアップは、`MAGENTO_CLOUD_RELATIONSHIP` 環境変数または.magento.env.yaml 設定ファイルの `stage.deploy.DATABASE_CONFIGURATION` プロパティ（あるいはその両方）に保存されている資格情報を使用して作成されます。

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--remove-definers`, `-d`

データベース ダンプから定義を削除する

- デフォルト：`false`
- 値を受け入れません

#### `--dump-directory`, `-a`

ダンプの保存に代替ディレクトリを使用

- 値が必要です


## `deploy`

```bash
ece-tools deploy
```

アプリケーションをデプロイします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

コマンドのヘルプを表示する

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

- デフォルト：`help`

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--format`

出力形式（txt、xml、json、md）

- デフォルト：`txt`
- 値が必要です

#### `--raw`

生のコマンド ヘルプを出力するには

- デフォルト：`false`
- 値を受け入れません


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

コマンドのリスト

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

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--raw`

生のコマンド リストを出力するには

- デフォルト：`false`
- 値を受け入れません

#### `--format`

出力形式（txt、xml、json、md）

- デフォルト：`txt`
- 値が必要です

#### `--short`

説明コマンドの引数をスキップするには

- デフォルト：`false`
- 値を受け入れません


## `patch`

```bash
ece-tools patch
```

カスタムパッチを適用します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `post-deploy`

```bash
ece-tools post-deploy
```

デプロイ後の操作を実行します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `run`

```bash
ece-tools run <scenario>...
```

シナリオを実行します。

### 引数

#### `scenario`

シナリオ

- デフォルト：`[]`
- 必須

- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `backup:list`

```bash
ece-tools backup:list
```

バックアップ ファイルの一覧を表示します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

重要な設定ファイルを復元します。 backup:list を実行して、バックアップ ファイルのリストを表示します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--force`, `-f`

バックアップの復元中に既存のファイルを上書き

- デフォルト：`false`
- 値を受け入れません

#### `--file`

特定のファイル回復パス

- 値を受け入れる


## `build:generate`

```bash
ece-tools build:generate
```

ビルドステージに必要なすべてのファイルを生成します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `build:transfer`

```bash
ece-tools build:transfer
```

生成されたファイルを init ディレクトリに転送します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

指定されたビルド、デプロイ、およびデプロイ後の変数設定で `.magento.env.yaml` ファイルを作成します。 既存の `.magento,.env.yaml` ファイルを上書きします。

### 引数

#### `configuration`

JSON 形式の設定

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

既存の `.magento.env.yaml` ファイルを指定された設定で更新します。 ファイル `.magento.env.yaml` 存在しない場合は作成します。

### 引数

#### `configuration`

JSON 形式の設定

- 必須

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

設定ファイル `.magento.env.yaml` 検証します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `config:dump`

```bash
ece-tools config:dumpdump
```

静的コンテンツのデプロイメントのダンプ設定です。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cron:disable`

```bash
ece-tools cron:disable
```

すべてのMagento cron プロセスを無効にして、実行中のすべてのプロセスを終了します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cron:enable`

```bash
ece-tools cron:enable
```

MagentoCron プロセスを有効にします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cron:kill`

```bash
ece-tools cron:kill
```

すべてのMagento cron プロセスを終了します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

「実行中」状態のままになる cron ジョブのロックを解除します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--job-code`

ロックを解除する Cron ジョブコード。

- デフォルト：`[]`
- 複数の値を使用できます


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

schema.error.yaml ファイルからdist/error-codes.md ファイルを生成します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Git からデプロイメント用に Composer を更新しました。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

エンコードされたクラウド設定環境変数を表示します。

### 引数

#### `variable`

表示する環境変数、可能なオプション：サービス、ルート、変数

- デフォルト：`[]`
- 配列

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

エラー ID 別のエラーに関する情報、または前回のデプロイメントのすべてのエラーに関する情報を表示します。

### 引数

#### `error-code`

エラーコード（渡されなかった場合） コマンドは、前回のデプロイメントからのすべてのエラーに関する情報を表示します

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。

#### `--json`, `-j`

JSON 形式で結果を取得するために使用されます

- デフォルト：`false`
- 値を受け入れません


## `module:refresh`

```bash
ece-tools module:refresh
```

設定を更新して、新しく追加されたモジュールを有効にします。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `schema:generate`

```bash
ece-tools schema:generate
```

スキーマ *.dist ファイルを生成します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

設定の理想的な状態を確認します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

マスター/スレーブ構成を確認します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

ビルド設定時に SCD を検証します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

SCD オンデマンド構成を検証します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

デプロイ時の SCD 設定を検証します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

DB を分割する機能と、DB が既に分割されているかどうかを確認します。

### オプション

グローバルオプションについては、[ グローバルオプション ](#global-options) を参照してください。
