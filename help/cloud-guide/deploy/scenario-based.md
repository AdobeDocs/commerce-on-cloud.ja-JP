---
title: シナリオベースの展開
description: カスタム設定ファイルを使用して、Adobe Commerce on cloud infrastructure デプロイメントをカスタマイズする方法について説明します。
feature: Cloud, Configuration, Deploy, Build
exl-id: 44c2a73e-4ea2-49a6-86c1-9fa8cfc8b66e
TQID: https://experienceleague.adobe.com/BttmvnP2iMbN-EAaPR9g2i9mv7fH4REAEwHFTu-2sw0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 874
ht-degree: 0%

---

# シナリオベースの展開

`ece-tools` 2002.1.0以降では、シナリオベースのデプロイメント機能を使用して、デフォルトのデプロイメント動作をカスタマイズできます。
この機能は、設定で**シナリオ**&#x200B;と&#x200B;**ステップ**&#x200B;を使用します。

- **シナリオ設定** – 各デプロイメントフックは&#x200B;*シナリオ*&#x200B;であり、デプロイメントタスクを完了するためのシーケンスと設定パラメーターを記述するXML設定ファイルです。 シナリオは、`.magento.app.yaml` ファイルの`hooks` セクションで設定します。

- **手順の設定** – 各シナリオでは、デプロイメントタスクを完了するために必要な操作をプログラムによって記述する&#x200B;*手順*&#x200B;のシーケンスを使用します。 XML ベースのシナリオ設定ファイルで手順を設定します。

クラウド インフラストラクチャ上のAdobe Commerceでは、`ece-tools` パッケージに[既定のシナリオ ](https://github.com/magento/ece-tools/tree/2002.1/scenario)と[既定の手順](https://github.com/magento/ece-tools/tree/2002.1/src/Step)のセットが用意されています。 カスタム XML設定ファイルを作成してデフォルト設定を上書きまたはカスタマイズすることで、デプロイメントの動作をカスタマイズできます。 また、シナリオと手順を使用して、カスタムモジュールからコードを実行することもできます。

## ビルドフックとデプロイフックを使用したシナリオの追加

Adobe Commerceの構築とデプロイのシナリオを`.magento.app.yaml` ファイルの`hooks` セクションに追加します。 各フックは、各フェーズで実行するシナリオを指定します。 次の例は、デフォルトのシナリオ設定を示しています。

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
>`ece-tools` 2002.1.xのリリースには、新しい[ フック設定](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html)形式があります。 2002.0.x リリース `ece-tools`の従来の形式は、引き続きサポートされます。 ただし、シナリオベースのデプロイメント機能を使用するには、新しい形式に更新する必要があります。

## シナリオステップの確認

フック設定では、各シナリオは、ビルド、デプロイ、デプロイ後のタスクを実行する手順を含むXML ファイルです。 例えば、`scenario/transfer` ファイルには、`compress-static-content`、`clear-init-directory`、`backup-data`の3つの手順が含まれています

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

次の例では、デプロイ設定ファイルを追加してフック設定に追加することで、デフォルトのデプロイシナリオを拡張します。

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

デプロイメント時に、カスタムシナリオは、次のルールに基づいてデフォルトのシナリオベースと結合されます。

- シナリオは、フック定義のシーケンスに基づいて優先順位付けされ、最後のシナリオが最も優先度が高い状態でリストされます。

  この例では、シナリオには次の優先度があります。

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` （既定またはベースライン シナリオ）

- 優先度の高いシナリオのステップは、他のシナリオの同じ名前のステップを上書きします。 設定に新しい手順が追加されます。 例えば（C → B → A）のように、各シナリオが右から左に優先順位付けされる2つ以上のシナリオに対しても同じルールが適用されます。

### デフォルトの手順の削除

`skip` パラメーターを使用して、既定のシナリオからステップを削除します。

例えば、デフォルトのデプロイシナリオで`enable-maintenance-mode`と`set-production-mode`の手順をスキップするには、次の設定を含む設定ファイルを作成します。

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

カスタム設定ファイルを使用するには、デフォルトの`.magento.app.yaml` ファイルを更新します。

> カスタムデプロイシナリオ付きの`.magento.app.yaml`

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

### デフォルトの手順を置き換える

カスタムシナリオは、デフォルトの手順を置き換えて、カスタム実装を提供できます。 これには、カスタムステップの名前としてデフォルトのステップ名を使用します。

例えば、[ デフォルトのデプロイシナリオ ]では、`enable-maintenance-mode`手順によってデフォルトの[EnableMaintenanceMode PHP スクリプト ]が実行されます。

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

この手順を上書きするには、`enable-maintenance-mode` ステップの実行時に別のスクリプトを実行するカスタムシナリオ設定ファイルを作成します。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### ステップの優先順位を変更する

カスタムシナリオでは、デフォルトの手順の優先順位を変更できます。 次の手順では、`enable-maintenance-mode` ステップの優先度が`300`から`10`に変更され、デプロイシナリオの前にステップが実行されます。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

この例では、`enable-maintenance-mode` ステップは、デフォルトのデプロイシナリオの他のすべてのステップよりも優先度が低いため、シナリオの先頭に移動します。

### 例：デプロイシナリオの拡張

次の例では、次の変更を加えて[ デフォルトのデプロイシナリオ ]をカスタマイズします。

- `remove-deploy-failed-flag` ステップをカスタムステップに置き換えます
- デプロイ前の手順で`clean-redis-cache` サブステップをスキップします
- `unlock-cron-jobs` ステップをスキップします
- クリティカルバリデータを無効にするには、`validate-config` ステップをスキップします
- デプロイ前の新しい手順を追加

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

このスクリプトをプロジェクトで使用するには、Adobe Commerce on cloud infrastructure プロジェクトの`.magento.app.yaml` ファイルに次の設定を追加します。

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
>`ece-tools` GitHub リポジトリの[ デフォルトのシナリオ ](https://github.com/magento/ece-tools/tree/2002.1/scenario)と[ デフォルトのステップ設定](https://github.com/magento/ece-tools/tree/2002.1/src/Step)を確認して、プロジェクトのビルド、デプロイ、デプロイ後のタスクに合わせてカスタマイズするシナリオとステップを決定できます。

## カスタムモジュールを追加して`ece-tools`を拡張

`ece-tools` パッケージには、セマンティック バージョン標準に準拠する既定のAPI インターフェイスが用意されています。 すべてのAPI インターフェイスには&#x200B;**@api**&#x200B;注釈が付いています。 カスタムモジュールを作成し、必要に応じてデフォルトコードを変更することで、デフォルトのAPI実装を独自の実装に置き換えることができます。

クラウドインフラストラクチャ上のAdobe Commerceでカスタムモジュールを使用するには、`ece-tools` パッケージの拡張機能リストにモジュールを登録する必要があります。 登録プロセスは、Adobe Commerceでモジュールを登録する際に使用するプロセスと似ています。

**モジュールを`ece-tools` パッケージ**&#x200B;に登録するには：

1. モジュールのルートで`registration.php` ファイルを作成または拡張します。

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. モジュール設定ファイルの`autoload` セクションを更新して、`registration.php` ファイルを含め、`composer.json`にモジュールファイルを自動ロードします。

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

1. モジュールに`config/services.xml` ファイルを追加します。 この設定は、`ece-tools` パッケージから`config/services.xml`を超えてマージされます。

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

依存関係インジェクションについて詳しくは、[Symfony依存関係インジェクション ](https://symfony.com/doc/current/components/dependency_injection.html)を参照してください。

<!-- link definitions -->

[デフォルトデプロイシナリオ]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP スクリプト]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
