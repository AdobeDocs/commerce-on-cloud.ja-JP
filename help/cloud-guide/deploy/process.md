---
title: デプロイメントプロセス
description: クラウドインフラプロジェクトでのAdobe Commerceのデプロイメントの仕組みを説明します。
feature: Cloud, Build, Deploy, SCD
exl-id: 76806381-0ecc-4d76-974a-f203d3bf44da
TQID: https://experienceleague.adobe.com/mSJOsLfNVGbkSNSrUzJgszxsqc07c-4KFhrJxm5I72U
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 413
ht-degree: 0%

---

# デプロイメントプロセス

デプロイメントプロセスは、環境の結合、プッシュ、同期を実行するか、[手動の再デプロイメント &#x200B;](../dev-tools/cloud-cli-overview.md#redeploy-the-environment)をトリガーするときに開始します。 デプロイメントプロセスには時間がかかりますが、開発とテストを行っているか、ライブサイトを使用しているかによって、デプロイメントを最適化する方法があります。 最も注目すべきことは、[静的コンテンツのデプロイメント &#x200B;](static-content.md)を制御できます。

デプロイメントプロセスには、ビルド、デプロイ、デプロイ後の3つのフェーズがあります。 各段階では、限られたリソースで特定のアクションを実行します。

## ![&#x200B; ビルド フェーズ &#x200B;](../../assets/status-build.png) ビルド フェーズ

_ビルド_ フェーズでは、設定ファイルで定義されたサービスのコンテナをアセンブリし、`composer.lock` ファイルに基づいて依存関係をインストールし、`.magento.app.yaml` ファイルで定義されたビルド フックを実行します。 サービスへの接続やデータベースへのアクセス機能がなければ、ビルド フェーズは環境に制限されたリソースに依存します。

## ![&#x200B; デプロイ フェーズ &#x200B;](../../assets/status-deploy.png) デプロイ フェーズ

_デプロイ_ フェーズでは、着信要求を一時的に保留し、サイトを[&#x200B; メンテナンスモード &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=ja)に移行します。 デプロイフェーズでは、新しいコンテナを使用し、ファイルシステムをマウントした後、ネットワーク接続を開き、`.magento.app.yaml` ファイルの`relationships` セクションで定義されたサービスをアクティブ化し、`.magento.app.yaml` ファイルで定義されたデプロイフックを実行します。 `.magento.app.yaml` ファイルで定義されているディレクトリを除き、すべてが&#x200B;_読み取り専用_&#x200B;です。 デフォルトでは、[`mounts` プロパティ &#x200B;](../application/properties.md#mounts)には次のディレクトリが含まれています。

- `app/etc` - `env.php`および`config.php`設定ファイルが含まれています
- `pub/media` – 製品やカテゴリなど、すべてのメディアデータが含まれます
- `pub/static` – 生成された静的ファイルが含まれています
- `var` – 実行時に作成された一時ファイルが含まれます

他のすべてのディレクトリには、読み取り専用の権限があります。 新しいサイトは、メンテナンスモードから移行し、受信リクエストの一時的な保留を解除すると、デプロイフェーズの終了時にアクティブになります。

デプロイ フェーズでは、`app/etc/config.php`および`app/etc/env.php`のデプロイメント設定ファイルのコピーがBAK拡張機能とともに保存されます。 これらのファイルの復元について詳しくは、[&#x200B; ストア設定](../store/store-settings.md#restore-configuration-files)を参照してください。

## ![&#x200B; デプロイ後フェーズ &#x200B;](../../assets/status-post-deploy.png) デプロイ後フェーズ

_デプロイ後_ フェーズでは、`.magento.app.yaml` ファイルで定義されたデプロイ後のフックが実行されます。 このフェーズで任意のアクションを実行すると、サイトのパフォーマンスに影響を与える可能性があります。ただし、[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages)環境変数を使用してキャッシュにデータを入力できます。

## ![状態の検証](../../assets/status-verify.png)設定の検証

[&#x200B; スマートウィザード &#x200B;](smart-wizards.md)を実行することで、プロジェクトの状態に最適な設定をテストできます。

>[!NOTE]
>
>`ece-tools` 2002.1.0以降では、シナリオベースのデプロイメント機能を使用して、Adobe Commerce on cloud infrastructure プロジェクトのビルド、デプロイ、デプロイ後のプロセスをカスタマイズできます。 [&#x200B; シナリオベースのデプロイメント &#x200B;](scenario-based.md)を参照してください。
