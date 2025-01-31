---
title: 環境変数
description: クラウドインフラストラクチャー上のAdobe Commerceに固有の環境変数のリストを参照してください。
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 環境変数

クラウドインフラストラクチャー上のAdobe Commerceを使用すると、設定オプションを上書きする環境変数を割り当てることができます。 `ece-tools` パッケージは、[ クラウド変数 ](variables-cloud.md)、[!DNL Cloud Console] で設定された変数、`.magento.env.yaml` 設定ファイルの値に基づいて `env.php` ファイルの値を設定します。

`.magento.env.yaml` ファイルの環境変数は、既存の Cloud Configuration を上書きしてCommerce環境をカスタマイズします。 デフォルト値が `Not Set` の場合、`ece-tools` パッケージは **NO** アクションを実行し、[!DNL Commerce] デフォルトまたは `MAGENTO_CLOUD_RELATIONSHIPS` 設定の値を使用します。 デフォルト値が設定されている場合、`ece-tools` パッケージはそのデフォルト値を設定します。

環境変数のタイプには、次のものが含まれます。

- [ADMIN](variables-admin.md) – 変数は、プロジェクトの ADMIN 変数を上書きします。
- [CLOUD_CLOUD](variables-cloud.md) - MAGENTOインフラストラクチャに固有の変数
- `.magento.env.yaml` ファイルで使用される変数：
   - [ グローバル ](variables-global.md) – 変数は、構築、デプロイおよびデプロイ後のステージに影響します
   - [ ビルド ](variables-build.md) – 変数はビルドアクションを制御します
   - [Deploy](variables-deploy.md) – 変数はデプロイアクションを制御します。
   - [ デプロイ後 ](variables-post-deploy.md) – 変数は、デプロイ後のアクションを制御します

変数は _階層_ です。つまり、上書きされない変数は、親環境から継承されます。

[ 管理変数 ](variables-admin.md) は、[!DNL Cloud Console] から、またはAdobe Commerce CLI を使用して設定できます。 サポートチケットを必要とせずに、[`.magento.env.yaml`](configure-env-yaml.md) ファイルから他の環境変数を管理して、ステージング版および実稼動版を含むすべての環境でビルドおよびデプロイアクションを管理できます。

>[!TIP]
>
>YAML ファイルでは大文字と小文字が区別され、タブは使用できません。 `.magento.env.yaml` ファイル全体で一貫性のあるインデントを使用するように注意してください。使用しないと、設定が期待どおりに動作しない場合があります。 このドキュメントとサンプルファイルの例では、_2 スペース_ インデントを使用しています。 [ece-tools validate コマンド ](configure-env-yaml.md#validate-configuration-file) を使用して、設定を確認します。
