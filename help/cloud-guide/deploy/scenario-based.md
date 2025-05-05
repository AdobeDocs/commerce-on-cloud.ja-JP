---
title: シナリオベースのデプロイメント
description: カスタム設定ファイルを使用して、クラウドインフラストラクチャデプロイメント上でAdobe Commerceをカスタマイズする方法について説明します。
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# シナリオベースのデプロイメント

`ece-tools` 2002.1.0 以降では、シナリオベースのデプロイメント機能を使用して、デフォルトのデプロイメント動作をカスタマイズできます。
この機能は、設定で **シナリオ** と **手順** を使用します。

- **シナリオ設定** – 各デプロイメントフックは *シナリオ* です。これは、デプロイメントタスクを完了するためのシーケンスと設定パラメーターを記述する XML 設定ファイルです。 シナリオは、`.magento.app.yaml` ファイルの `hooks` セクションで設定します。

- **ステップ設定** – 各シナリオでは、デプロイメントタスクの完了に必要な操作をプログラムで説明する一連の *ステップ* を使用します。 この手順は XML ベースのシナリオ設定ファイルで設定します。

クラウドインフラストラクチャー上のAdobe Commerceでは、`ece-tools` パッケージに一連の [ デフォルトのシナリオ ](https://github.com/magento/ece-tools/tree/2002.1/scenario) と [ デフォルトの手順 ](https://github.com/magento/ece-tools/tree/2002.1/src/Step) が用意されています。 デフォルトの設定をオーバーライドまたはカスタマイズするカスタム XML 設定ファイルを作成することで、デプロイメントの動作をカスタマイズできます。 シナリオと手順を使用して、カスタムモジュールからコードを実行することもできます。

## ビルドフックとデプロイフックを使用したシナリオの追加

Adobe Commerceを構築およびデプロイするためのシナリオを `.magento.app.yaml` ファイルの `hooks` セクションに追加します。 各フックは、各フェーズで実行するシナリオを指定します。 次の例に、デフォルトのシナリオ設定を示します。

> `magento.app.yaml` フック

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>`ece-tools` 2002.1.x のリリースでは、新しい [ フック設定 ](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html?lang=ja) 形式が追加されました。 `ece-tools` 2002.0.x リリースのレガシー形式は、引き続きサポートされます。 ただし、シナリオベースのデプロイメント機能を使用するには、新しい形式に更新する必要があります。

## シナリオ手順のレビュー

フック設定では、各シナリオは、ビルド、デプロイまたはデプロイ後のタスクを実行する手順を含む XML ファイルです。 例えば、`scenario/transfer` ファイルには、`compress-static-content`、`clear-init-directory`、`backup-data` の 3 つのステップが含まれています

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## デフォルトシナリオの拡張

次の例では、フック設定にデプロイ設定ファイルを追加することで、デフォルトのデプロイシナリオを拡張しています。

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

デプロイメント時に、カスタムシナリオは次のルールに基づいてデフォルトのシナリオと結合されます。

- シナリオの優先順位は、フック定義内のシーケンスに基づいて設定され、リストされた最後のシナリオが最も優先順位が高くなります。

  この例では、シナリオの優先度は次のとおりです。

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` （デフォルトまたはベースラインシナリオ）

- 最も優先度が高いシナリオのステップは、他のシナリオの同じ名前のステップを上書きします。 新しい手順が設定に追加されます。 例えば（C → B → A）のように、各シナリオが右から左に優先順位付けされる 2 つ以上のシナリオにも同じルールが適用されます。

### デフォルトの手順を削除

ステップをデフォルトのシナリオから削除するには、`skip` パラメーターを使用します。

例えば、デフォルトのデプロイシナリオの `enable-maintenance-mode` と `set-production-mode` の手順をスキップするには、次の設定を含む設定ファイルを作成します。

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

カスタム設定ファイルを使用するには、デフォルトの `.magento.app.yaml` ファイルを更新します。

> カスタム `.magento.app.yaml` プロイメントシナリオを使用したデプロイ

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### デフォルトの手順を置換

カスタムシナリオは、デフォルトの手順を置き換えて、カスタムの実装を提供できます。 それには、デフォルトのステップ名をカスタムステップの名前として使用します。

例えば、[ デフォルトのデプロイシナリオ ] では、`enable-maintenance-mode` の手順でデフォルトの [EnableMaintenanceMode PHP スクリプト ] が実行されます。

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

この手順を上書きするには、カスタムシナリオ設定ファイルを作成して、`enable-maintenance-mode` の手順の実行時に別のスクリプトを実行します。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### ステップの優先度の変更

カスタムシナリオで、デフォルトのステップの優先度を変更できます。 次の手順では、`enable-maintenance-mode` の手順の優先度を `300` から `10` に変更して、デプロイシナリオの前半で手順が実行されるようにします。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

この例では、デフォルトのデプロイシナリオの他のすべてのステップよりも優先度が低いので、`enable-maintenance-mode` のステップがシナリオの最初に移動します。

### 例：デプロイシナリオの拡張

次の例では、次の変更を加えて [ デフォルトのデプロイシナリオ ] をカスタマイズしています。

- `remove-deploy-failed-flag` 手順をカスタム手順に置き換えます
- デプロイ前手順の `clean-redis-cache` サブステップをスキップします
- `unlock-cron-jobs` の手順をスキップします
- `validate-config` の手順をスキップして重要なバリデーターを無効にします
- 新しいプレデプロイ手順を追加します

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

プロジェクトでこのスクリプトを使用するには、クラウドインフラストラクチャプロジェクト上のAdobe Commerceの `.magento.app.yaml` ファイルに次の設定を追加します。

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>`ece-tools` GitHub リポジトリで [ デフォルトのシナリオ ](https://github.com/magento/ece-tools/tree/2002.1/scenario) と [ デフォルトのステップ設定 ](https://github.com/magento/ece-tools/tree/2002.1/src/Step) を確認して、プロジェクトのビルド、デプロイ、デプロイ後のタスクに合わせてカスタマイズするシナリオと手順を決定できます。

## `ece-tools` を拡張するカスタムモジュールの追加

`ece-tools` パッケージは、セマンティックバージョン標準に準拠したデフォルトの API インターフェイスを提供します。 すべての API インターフェイスは、**@api** 注釈でマークされます。 カスタムモジュールを作成し、必要に応じてデフォルトコードを変更することで、デフォルトの API 実装を独自の API 実装に置き換えることができます。

クラウドインフラストラクチャ上のAdobe Commerceでカスタムモジュールを使用するには、`ece-tools` パッケージの拡張機能リストにモジュールを登録する必要があります。 登録プロセスは、Adobe Commerceへのモジュール登録に使用するプロセスと似ています。

**モジュールを `ece-tools` パッケージに登録するには**:

1. モジュールのルートに `registration.php` ファイルを作成または拡張します。

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. モジュール設定ファイルの `autoload` セクションを更新して、`composer.json` でモジュールファイルを自動ロードする `registration.php` ファイルを含めます。

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. `config/services.xml` ファイルをモジュールに追加します。 この設定は、パッケージから `config/services.xml` で結合 `ece-tools` れます。

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

依存関係の挿入について詳しくは、[Symfony 依存関係の挿入 ](https://symfony.com/doc/current/components/dependency_injection.html) を参照してください。

<!-- link definitions -->

[デフォルトのデプロイシナリオ]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP スクリプト]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
