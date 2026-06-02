---
title: Hooks プロパティ
description: ' [!DNL Commerce]  アプリケーション設定ファイルでhook プロパティを設定する方法の例を参照してください。'
feature: Cloud, Configuration, Build, Deploy
exl-id: 56b7045c-fba5-43f1-b43e-4d438b8e0568
TQID: https://experienceleague.adobe.com/Yc8fn-As9OmhlpQvb-M2gWRs20g5MQz7JtBG4cRQAV0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 318
ht-degree: 0%

---

# Hooks プロパティ

ビルド、デプロイ、デプロイ後のフェーズでシェルコマンドを実行するには、`hooks` セクションを使用します。

- **`build`** - アプリケーションをパッケージ化する前に&#x200B;_コマンドを実行します。_&#x200B;データベースやRedisなどのサービスは、アプリケーションがまだデプロイされていないため利用できません。 カスタム生成されたコンテンツがデプロイメントフェーズに進むように、カスタムコマンド _before_&#x200B;をデフォルトの`php ./vendor/bin/ece-tools` コマンドに追加します。

- **`deploy`** - アプリケーションのパッケージ化とデプロイの後に&#x200B;_コマンドを実行します。_&#x200B;この時点で他のサービスにアクセスできます。 既定の`php ./vendor/bin/ece-tools` コマンドは`app/etc` ディレクトリを正しい場所にコピーするため、カスタムコマンド _after_&#x200B;をデプロイ コマンドに追加して、カスタムコマンドの失敗を防ぐ必要があります。

- **`post_deploy`** - アプリケーションのデプロイ後&#x200B;_コマンドを実行し、_&#x200B;後&#x200B;_コンテナが接続の受け入れを開始します。_`post_deploy` フックはキャッシュをクリアし、キャッシュをプリロード （ウォーミング）します。 [ デプロイ後ステージ ](../environment/variables-post-deploy.md)の`WARM_UP_PAGES`変数を使用して、ページのリストをカスタマイズできます。 これは必須ではありませんが、`SCD_ON_DEMAND`環境変数と連動して動作します。

次の例は、`.magento.app.yaml` ファイルのデフォルト設定を示しています。 `ece-tools` コマンドの&#x200B;_前_&#x200B;の`build`、`deploy`または`post_deploy` セクションにCLI コマンドを追加します。

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

また、`generate`および`transfer` コマンドを使用して、コードのビルドやファイルの移動を具体的に行う際に追加のアクションを実行することで、ビルド フェーズをさらにカスタマイズすることもできます。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` – 最後に失敗したコマンドではなく、最初に失敗したコマンドでフックが失敗します。
- `build:generate` - SCDがビルド フェーズで有効になっている場合、パッチの適用、設定の検証、DIの生成、静的コンテンツの生成を行います。
- `build:transfer` – 生成されたコードと静的コンテンツを最終宛先に転送します。

コマンドはアプリケーション （`/app`） ディレクトリから実行されます。 `cd` コマンドを使用して、ディレクトリを変更できます。 フック内の最後のコマンドが失敗した場合、フックは失敗します。 最初の失敗したコマンドで失敗させるには、フックの先頭に`set -e`を追加します。

**grunt**&#x200B;を使用してSass ファイルをコンパイルするには：

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

ビルド中に発生する静的コンテンツのデプロイメントの前に、`grunt`を使用してSass ファイルをコンパイルします。 `build` コマンドの前に`grunt` コマンドを配置します。

{{scenarios}}
