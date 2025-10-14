---
title: Hooks プロパティ
description: アプリケーション設定ファイルの hooks プロパティを設定する方法の例  [!DNL Commerce]  参照してください。
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Hooks プロパティ

`hooks` のセクションを使用して、ビルド、デプロイ、およびデプロイ後のフェーズでシェルコマンドを実行します。

- **`build`** - アプリケーションをパッケージ化する _前_ コマンドを実行します。 アプリケーションがまだデプロイされていないので、データベースや Redis などのサービスは使用できません。 カスタム生成コンテンツがデプロイメントフェーズに継続するように、デフォルトの `php ./vendor/bin/ece-tools` コマンド _前_ を追加します。

- **`deploy`** - アプリケーションのパッケージ化および配置のコマンド _後_ を実行します。 この時点で他のサービスにアクセスできます。 デフォルトの `php ./vendor/bin/ece-tools` コマンドは `app/etc` ディレクトリを正しい場所にコピーするので、カスタムコマンドが失敗しないように、deploy コマンドの後にカスタムコマンド _after_ を追加する必要があります。

- **`post_deploy`** - アプリケーションの配備 _後_ コマンドを実行し、_後_ コンテナが接続の受け入れを開始します。 `post_deploy` フックは、キャッシュをクリアし、キャッシュをプリロード（ウォームアップ）します。 [&#x200B; デプロイ後のステージ &#x200B;](../environment/variables-post-deploy.md) の `WARM_UP_PAGES` 変数を使用して、ページのリストをカスタマイズできます。 必須ではありませんが、これは `SCD_ON_DEMAND` 環境変数と並行して動作します。

次の例は、`.magento.app.yaml` ファイルのデフォルトの設定を示しています。 `ece-tools` のコマンドの `build`、`deploy`、または `post_deploy` のセクション _前_ に CLI コマンドを追加します。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

また、コードを特別にビルドしたりファイルを移動したりする際に、`generate` および `transfer` コマンドを使用して追加のアクションを実行することにより、ビルドフェーズをさらにカスタマイズすることもできます。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` – 最後の失敗したコマンドではなく、最初の失敗したコマンドでフックを失敗させます。
- `build:generate` - SCD がビルド・フェーズで使用可能な場合に、パッチの適用、構成の検証、DI の生成、静的コンテンツの生成を行います。
- `build:transfer` – 生成されたコードと静的コンテンツを最終的な宛先に転送します。

コマンドは、アプリケーション（`/app`）ディレクトリから実行されます。 `cd` コマンドを使用して、ディレクトリを変更できます。 フックは、最終的なコマンドが失敗すると失敗します。 最初に失敗したコマンドでそれらを失敗させるには、フックの先頭に `set -e` を追加します。

**grunt を使用して Sass ファイルをコンパイルするには**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

ビルド時に静的コンテンツのデプロイメントを行う前に、`grunt` を使用して Sass ファイルをコンパイルします。 `grunt` コマンドを `build` コマンドの前に配置します。

{{scenarios}}
