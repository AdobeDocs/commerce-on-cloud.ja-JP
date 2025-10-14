---
title: デプロイメントプロセス
description: クラウドインフラストラクチャプロジェクトでのAdobe Commerceのデプロイメントの仕組みについて説明します。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# デプロイメントプロセス

デプロイメントプロセスは、環境の結合、プッシュ、同期を実行するか、トリガー[&#x200B; 手動の再デプロイメント &#x200B;](../dev-tools/cloud-cli-overview.md#redeploy-the-environment) を実行すると開始されます。 デプロイメントプロセスには時間がかかりますが、開発とテストを行っているか、ライブサイトを使用しているかによって、デプロイメントを最適化する方法があります。 特に、[&#x200B; 静的コンテンツのデプロイメント &#x200B;](static-content.md) を制御できます。

デプロイメントプロセスには、ビルド、デプロイ、ポストデプロイの 3 つの異なるフェーズがあります。 各フェーズでは、限られたリソースで特定のアクションが実行されます。

## ![&#x200B; ビルドフェーズ &#x200B;](../../assets/status-build.png) ビルドフェーズ

_ビルド_ フェーズでは、設定ファイルで定義されているサービスのコンテナを組み立て、`composer.lock` ファイルに基づいて依存関係をインストールし、`.magento.app.yaml` ファイルで定義されているビルドフックを実行します。 サービスに接続したりデータベースにアクセスしたりできない場合、ビルドフェーズは環境に限定されたリソースに応じて異なります。

## ![&#x200B; デプロイフェーズ &#x200B;](../../assets/status-deploy.png) デプロイフェーズ

_デプロイ_ フェーズでは、受信リクエストを一時的に保持し、サイトを [&#x200B; メンテナンスモード &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=ja) に移行します。 デプロイフェーズでは、新しいコンテナを使用し、ファイルシステムをマウントした後、ネットワーク接続を開き、`.magento.app.yaml` ファイルの `relationships` セクションで定義されているサービスをアクティベートし、`.magento.app.yaml` ファイルで定義されているデプロイフックを実行します。 `.magento.app.yaml` ファイルで定義されているディレクトリを除き、すべてが _読み取り専用_ です。 デフォルトでは、[`mounts` プロパティには &#x200B;](../application/properties.md#mounts) 次のディレクトリが含まれています。

- `app/etc` - `env.php` と `config.php` の構成ファイルが含まれます
- `pub/media` – 製品やカテゴリなど、すべてのメディア・データが含まれます
- `pub/static` – 生成された静的ファイルが含まれます
- `var` – 実行時に作成された一時ファイルが含まれます

その他のすべてのディレクトリには、読み取り専用の権限があります。 新しいサイトは、メンテナンスモードを終了し、受信リクエストの一時的な保留を解除すると、デプロイフェーズの最後にアクティブになります。

デプロイフェーズでは、`app/etc/config.php` と `app/etc/env.php` のデプロイメント設定ファイルのコピーが BAK 拡張子で保存されます。 これらのファイルの復元については、[&#x200B; ストア設定 &#x200B;](../store/store-settings.md#restore-configuration-files) を参照してください。

## ![&#x200B; デプロイ後フェーズ &#x200B;](../../assets/status-post-deploy.png) デプロイ後フェーズ

_デプロイ後_ フェーズでは、`.magento.app.yaml` ファイルで定義されたデプロイ後のフックを実行します。 このフェーズで任意のアクションを実行すると、サイトのパフォーマンスに影響を与える可能性があります。ただし、[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) 環境変数を使用してキャッシュを設定することができます。

## ![&#x200B; 状態の検証 &#x200B;](../../assets/status-verify.png) 設定の検証

[&#x200B; スマートウィザード &#x200B;](smart-wizards.md) を実行して、プロジェクトの状態に合った最適な設定をテストできます。

>[!NOTE]
>
>`ece-tools` 2002.1.0 以降では、シナリオベースのデプロイメント機能を使用して、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのビルド、デプロイ、デプロイ後のプロセスをカスタマイズできます。 [&#x200B; シナリオベースのデプロイメント &#x200B;](scenario-based.md) を参照してください。
