---
title: ストアの設定管理
description: クラウドインフラストラクチャー上のすべてのAdobe Commerceでストア設定を管理および同期する方法について説明します。
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# ストアの設定管理

ストアのデフォルトの設定は、適切なモジュールの `config.xml` に格納されます。 Commerce Admin または CLI `bin/magento config:set` コマンドで設定を変更すると、変更内容がコアデータベース（特に `core_config_data` テーブル）に反映されます。 これらの設定は、`config.xml` ファイルに保存されているデフォルトの設定を上書きします。

ストア設定は、管理 **ストア**/**設定**/**設定** セクションの設定を参照し、設定のタイプに基づいてデプロイメント設定ファイルに格納されます。

- `app/etc/config.php` - ストア、web サイト、モジュールまたは拡張機能、静的ファイル最適化、静的コンテンツのデプロイメントに関連するシステム値の設定。 [Configuration Guide _の_ config.php リファレンス ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) を参照してください。
- `app/etc/env.php` - ソース管理に保管する必要があるシステム固有の上書きと機密設定の値 _NOT_。 [Configuration Guide _の_ env.php リファレンス ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) を参照してください。

>[!NOTE]
>
>クラウドインフラストラクチャー上のAdobe Commerceでは、実稼動モードとメンテナンスモードのみをサポートしているので、管理者から **詳細**/**開発者** セクションにアクセスできません。 構成管理タスクを完了するには、[ 環境管理者権限 ](../project/user-access.md) が必要です。 [ 環境変数 ](../environment/configure-env-yaml.md) を使用して、追加設定を指定できます。

構成管理では、パイプラインデプロイメントを使用して、ダウンタイムを最小限に抑えながら、環境全体で一貫性のあるストア設定をデプロイする方法を提供します。 クラウドインフラストラクチャー上のAdobe Commerce プロジェクトには、ビルドサーバー、ビルドおよびデプロイスクリプト、デプロイメント環境が含まれ、これらは [ パイプラインのデプロイメント方法 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) を念頭に置いて設計されています。

## 構成オーバーライド スキーム

すべてのシステム設定は、次のオーバーライドスキームに従ってビルドおよびデプロイフェーズで設定されます。

1. 環境変数が存在する場合、カスタム設定を使用し、デフォルト設定を無視します。
1. 環境変数が存在しない場合は、[`.magento.app.yaml` ファイルの `MAGENTO_CLOUD_RELATIONSHIPS` の名前と値のペアから設定を使用します ](../application/configure-app-yaml.md)。 デフォルトの設定を無視します。
1. 環境変数が存在せず、`MAGENTO_CLOUD_RELATIONSHIPS` に名前と値のペアが含まれていない場合は、カスタマイズした設定をすべて削除し、デフォルト設定の値を使用します。

要約すると、環境変数は他のすべての値を上書きします。

>[!TIP]
>
>パイプラインデプロイメントのオーバーライドスキームについて詳しくは、[&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) 設定ガイド _設定の管理_ を参照してください。

同じ設定が複数の場所で設定されている場合、アプリケーションは次の設定階層に基づいて、環境に適用する値を決定します。

| 優先度 | 設定 <br> メソッド | 説明 |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br> 環境変数 | [!DNL Cloud Console] の環境設定の「_変数_」タブから追加された値。 ここで、機密性の高い設定や環境固有の設定の値を指定します。 ここで指定した設定は、管理者から編集できません。 [ 環境設定変数 ](../project/overview.md#configure-environment) を参照してください。 |
| 2 | `.magento.app.yaml` | `.magento.app.yaml` ファイルの `variables` セクションに追加された値。 ここで値を指定し、すべての環境で一貫した設定を行います。 **`.magento.app.yaml` ファイル内で機密性の高い値を指定しないでください。[ アプリケーション設定 ](../application/configure-app-yaml.md) を参照してくださ**。 |
| 3 | `app/etc/env.php` | ここに格納された環境固有の設定値は、`app:config:dump` コマンドを使用して追加します。 環境変数または CLI を使用して、システム固有の機密性の高い値を設定します。 [ 機密データ ](#sensitive-data) を参照してください。 `env.php` ファイルはソース管理に含まれていま **ん**。 |
| 4 | `app/etc/config.php` | ここに格納された値は、`app:config:dump` コマンドを使用して追加されます。 共有設定値が `config.php` に追加されます。 管理者から、または CLI を使用して共有構成を設定します。 `config.php` ファイルはソース管理に含まれます。 |
| 5 | データベース | ここに格納された値は、管理者の設定によって追加されます。 上記の方法のいずれかを使用して設定された設定は、ロック（グレー表示）され、管理者から編集できません。 |
| 6 | `config.xml` | 多くの設定では、モジュールの `config.xml` ファイルにデフォルト値が設定されています。 上記のいずれかの方法で設定された値がAdobe Commerceで見つからない場合、設定されていれば、デフォルト値にフォールバックします。 |

{style="table-layout:auto"}

## 設定ダンプ

次の `ece-tools` コマンドを使用して、現在のすべてのストア設定を含む `config.php` ファイルを生成できます。

```bash
./vendor/bin/ece-tools config:dump
```

`app/etc/config.php` ファイルに「ダンプ」されたデータは _ロック済み_ になり、Commerce管理者の対応するフィールドは **読み取り専用** になります。 `config.php` ファイルには、設定した設定のみが含まれます。 デフォルト値はロックされません。 また、更新した値のみをロックすると、特に Fastly の場合、読み取り専用設定が原因で、ステージング環境と実稼動環境で使用されるすべての拡張機能が破損しなくなります。

>[!WARNING]
>
>`ece-tools config:dump` コマンドは、B2B などのモジュールの詳細な設定を取得しません。 包括的な設定ダンプが必要な場合は、`app:config:dump` コマンドを使用します。ただし、このコマンドは、設定値を読み取り専用状態でロックします。

### 機密データ

`bin/magento app:config:dump` コマンドを使用すると、機密性の高い設定がすべて `app/etc/env.php` ファイルに書き出されます。 CLI コマンドを使用して機密性の高い値を設定できます：`bin/magento config:sensitive:set`。 [2&rbrace;Commerce PHP Extensions](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) ガイドの &lbrace;Sensitive and environment-specific settings _を参照して、コンフィギュレーション設定を機密またはシステム固有として指定する方法を確認してください。_

_設定ガイド_ の [ 機密またはシステム固有の設定 ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) のリストを参照してください。

### SCD パフォーマンス

ストアのサイズによっては、デプロイする静的コンテンツファイルが多数ある場合があります。 通常、静的コンテンツのデプロイは、アプリケーションがメンテナンスモードのデプロイ段階でおこなわれます。 最も最適な設定は、ビルドフェーズで静的コンテンツを生成することです。 [ デプロイ方法の選択 ](../deploy/static-content.md) を参照してください。

設定のダンプ後に設定管理を有効にした場合は、SCD_*変数をデプロイステージからビルドステージに移動し、ビルドフェーズ中に静的コンテンツの生成を適切に有効にする必要があります。 [ 環境変数 ](../environment/configure-env-yaml.md#environment-variables) を参照してください。

**設定管理前**:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

**構成管理を有効にした後**:

SCD_*変数をビルドステージに移動します。

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```

>[!NOTE]
>
>静的ファイルをデプロイする前に、ビルドフェーズとデプロイフェーズで、GZIP を使用して静的コンテンツを圧縮します。 静的ファイルを圧縮すると、サーバーの負荷が軽減され、サイトのパフォーマンスが向上します。 ファイル圧縮のカスタマイズまたは無効化については、[ ビルドオプション ](../environment/variables-build.md) を参照してください。

## 設定を管理する手順

以下に、このプロセスの概要を示します。

![ スターター設定管理の概要 ](../../assets/starter/configuration-management-flow.png)

**ストアを設定し、設定ファイルを生成するには**:

1. いずれかの環境で、管理者のストアのすべての設定を完了します。

   - スターター：アクティブな開発ブランチ
   - Pro：統合環境のアクティブなブランチ

   この環境からステージング環境および実稼動環境にデータベースをダンプする予定がない限り、これらの設定には実際の製品は含まれません。 通常、開発データベースには、完全なストアデータは含まれていません。

1. ローカルワークステーションで、をプロジェクトディレクトリに変更します。

1. リモート・データベースのローカル・ダンプを作成します。

   ```bash
   magento-cloud db:dump
   ```

1. コードの変更を追加、コミット、プッシュして、リモート環境を更新します。

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

デプロイメントが完了したら、更新された環境の管理者にログインして、設定を確認します。 必要に応じて、追加の設定をステージング環境と実稼動環境に引き続き結合します。

### 設定を更新

管理者を通じて環境を変更し、コマンドを再度実行すると、新しい設定が `config.php` ファイルのコードに追加されます。

>[!WARNING]
>
>ステージング環境と実稼動環境では `config.php` ファイルを手動で編集できますが、**推奨しません**。 ファイルは、すべての環境ですべての設定の一貫性を維持するのに役立ちます。 再構築のために `config.php` ファイルを削除しないでください。 ファイルを削除すると、ビルドプロセスおよびデプロイプロセスに必要な特定の設定を削除できます。

### 設定ファイルの復元

デプロイメントプロセス中に元の `app/etc/env.php` ファイルと `app/etc/config.php` ファイルのコピーが作成され、同じフォルダーに保存されます。 以下に、同じ `app/etc` フォルダー内の BAK （バックアップファイル）と PHP （元のファイル）を示します。

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

古い設定では `app/etc/config.local.php` ファイルを使用していました。 [ 古い設定の移行 ](#migrate-older-configurations) を参照してください。

**設定ファイルを復元するには**:

1. ローカルワークステーションで、SSH を使用してリモートプロジェクトおよび環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. バックアップファイルの場所と可用性を確認します。

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   応答の例：

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. バックアップファイルを復元します。

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### 古い設定を移行

Cloud infrastructure 2.2 以降でAdobe Commerceにアップグレードする場合は、`config.local.php` ファイルから新しい `config.php` ファイルに設定を移行することをお勧めします。 管理者の設定がファイルの内容と一致する場合は、手順に従って `config.php` ファイルを生成して追加します。

異なる場合は、`config.local.php` ファイルのコンテンツを新しい `config.php` ファイルに追加できます。

1. 指示に従って `config.php` ファイルを生成します。

1. `config.php` ファイルを開き、最後の行を削除します。

1. `config.local.php` ファイルを開き、内容をコピーします。

1. 内容を `config.php` ファイルに貼り付け、保存して、Git への追加を完了します。

1. 環境全体にデプロイします。

この移行は 1 回だけ完了します。 移行後、`config.php` ファイルを使用します。

### ロケールを変更

[SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) を有効にしている場合は、複雑な設定のインポートおよびエクスポートプロセスに従わずに _ストアロケールを変更できます_。 管理者を使用してロケールを更新できます。

別のロケールをステージング環境または実稼動環境に追加するには、統合ブランチで `SCD_ON_DEMAND` を有効にし、新しいロケール情報を含む更新された `config.php` ファイルを生成して、設定ファイルをターゲット環境にコピーします。

>[!WARNING]
>
>このプロセス **上書き** は、ストアの設定です。環境に同じストアが含まれている場合にのみ、次の操作を行います。

1. 統合環境で、[`.magento.env.yaml` ファイル ](../environment/configure-env-yaml.md) を使用して `SCD_ON_DEMAND` 変数を有効にします。

1. 管理者を使用して、必要なロケールを追加します。

1. SSH を使用してリモート環境にログインし、すべてのロケールを含む `/app/etc/config.php` ファイルを生成します。

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. 新しい設定ファイルをリモート統合環境からローカル環境ディレクトリにコピーします。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. コードの変更を追加、コミット、プッシュして、リモート環境を更新します。
