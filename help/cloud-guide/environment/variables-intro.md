---
title: 環境変数
description: Adobe Commerce on cloud infrastructureに固有の環境変数の一覧を参照してください。
feature: Cloud, Build, Configuration, Deploy
exl-id: 38b2cdc2-1a98-48bd-90b2-13ef179da26f
TQID: 'https://experienceleague.adobe.com/qRdv72nxgkwRjRz0lXqs33rSmZKc3akq2W0pJK4CM7k'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: bcd8874c-7b93-5596-bdaa-22660e84df14
    internal-label: Deploy
subfeature_v2:
  - id: adedf3b3-e153-47a3-ae73-b5d65067b544
    internal-label: Build system
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%
---
# 環境変数

Adobe Commerce on cloud infrastructureでは、環境変数を割り当てて設定オプションを上書きできます。 `ece-tools` パッケージは、[ クラウド変数](variables-cloud.md)、[!DNL Cloud Console]で設定された変数、`.magento.env.yaml`設定ファイルの値に基づいて、`env.php` ファイルの値を設定します。

`.magento.env.yaml` ファイルの環境変数は、既存のCommerce設定を上書きしてCloud環境をカスタマイズします。 デフォルト値が`Not Set`の場合、`ece-tools` パッケージは&#x200B;**NO** アクションを実行し、[!DNL Commerce]のデフォルトまたは`MAGENTO_CLOUD_RELATIONSHIPS`設定の値を使用します。 デフォルト値が設定されている場合、`ece-tools` パッケージはそのデフォルト値を設定するように動作します。

環境変数の種類は次のとおりです。

- [ADMIN](variables-admin.md)：変数がプロジェクト管理者の変数を上書きする
- [MAGENTO_CLOUD](variables-cloud.md) – クラウドインフラストラクチャ固有の変数
- `.magento.env.yaml` ファイルで使用される変数：
  - [ グローバル ](variables-global.md)：変数がビルド、デプロイ、デプロイ後のステージに影響する
  - [ ビルド ](variables-build.md) – 変数はビルドアクションを制御します
  - [ デプロイ ](variables-deploy.md) – 変数がデプロイアクションを制御します
  - [ デプロイ後](variables-post-deploy.md) – 変数はデプロイ後のアクションを制御します

変数は&#x200B;_階層_&#x200B;です。つまり、変数が上書きされない場合、親環境から継承されます。

[管理者変数](variables-admin.md)は、[!DNL Cloud Console]またはAdobe Commerce CLIを使用して設定できます。 [`.magento.env.yaml`](configure-env-yaml.md) ファイルから他の環境変数を管理して、Pro ステージングと実稼動を含むあらゆる環境でビルドとデプロイのアクションを管理できます。サポートチケットは必要ありません。

>[!TIP]
>
>YAML ファイルでは大文字と小文字が区別され、タブは使用できません。 `.magento.env.yaml` ファイル全体で一貫したインデントを使用するように注意してください。そうしないと、設定が期待どおりに動作しない可能性があります。 このドキュメントとサンプルファイルの例では、_2空間_ インデントを使用しています。 [ece-tools validate コマンド ](configure-env-yaml.md#validate-configuration-file)を使用して、設定を確認します。
