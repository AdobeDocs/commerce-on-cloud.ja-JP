---
title: クラウドインフラプロジェクト
description: Adobe Commerce on cloud infrastructure [!DNL Cloud Console] の概要と、アカウント設定にアクセスする方法について説明します。
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 8eed04c7-6469-45a4-aa89-dc594c977264
TQID: https://experienceleague.adobe.com/I7odylklFhMfZP0R43bY6LXyXXT6Pk5-vFsGsbSrqOk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: c32adafa-ed01-4b31-997e-2413013911b0id: cc250cf1-34eb-4863-80d0-d170d45ea067id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1022
ht-degree: 0%

---

# クラウドインフラプロジェクト

Adobe Commerce on cloud infrastructure プロジェクトには、[!DNL Commerce] アプリケーションをデプロイするためのGit ブランチ、関連する環境、およびスクリプトのすべてのコードが含まれています。 環境には、データベース、web サーバー、およびキャッシュ サーバーを含む、[!DNL Commerce] アプリケーションをサポートするサービスが含まれています。

Adobeには、プロジェクトのあらゆる側面を完全に管理するための[!DNL Cloud Console]および開発者ツールが用意されています。 アカウント所有者は、すべての環境に完全にアクセスできます。

## [!DNL Cloud Console]

[!DNL Cloud Console]には、使いやすいフォーマットでCommerce コードをビルド、管理、デプロイするためのインタラクティブなメソッドが用意されています。 [ [!DNL Cloud Console]](https://console.adobecommerce.com)にログインして、プロジェクト リストを表示します。 管理者として、または特定の環境タイプに対してアクセスする権限を持つプロジェクトのみを表示できます。 Adobe ソリューションパートナーの場合、サポートしているクライアントに複数のプロジェクトが表示される場合があります。

>[!TIP]
>
>プロジェクトが表示されない場合は、プロジェクトに関連付けられている[ アカウントオーナーまたはプロジェクト管理者](../project/user-access.md)に連絡してアクセスをリクエストする必要があります。 初めてのユーザーについては、_入門_ ガイドの[ オンボーディング トピック ](../../get-started/onboarding.md#cloud-console)を参照してください。

_すべてのプロジェクト_ ビューには、アクセス権限を持つすべてのプロジェクトが一覧表示されます。 **[!UICONTROL Show filters]**&#x200B;をクリックして、プロジェクト リストをタイプ、地域、またはプランでフィルタリングできます。

![ プロジェクトリスト ](../../assets/ui-allprojects-list.png)

### プロジェクト概要

_すべてのプロジェクト_ リストからプロジェクトを選択すると、プロジェクトの概要が開きます。 プロジェクト概要には、環境セレクターと設定ボタンを含むプロジェクトナビゲーションバーが常に表示されます。

![ プロジェクトナビゲーション ](../../assets/project-nav.png)

環境を選択していない限り、プロジェクト概要には、プレビュー領域にプロジェクトの詳細の概要が表示されます。

- プロジェクト名
- 地域、プロジェクト ID
- 計画、割り当て済みストレージ、環境、ユーザー
- **[!UICONTROL Set a custom domain]** ボタンを含むストアフロント URL

メインプロジェクトの概要では、次の操作を行います。

- 環境ビューには、![ アクティブなブランチ ](../../assets/icon-active.png){width="32"} （アクティブ）および![非アクティブなブランチ ](../../assets/icon-inactive.png){width="32"} （非アクティブ）環境のリストまたはツリービューが表示されます。
- [ アクティビティストリーム ](activity-stream.md)には、プロジェクトの実行中、保留中、および最近のアクティビティが表示されます。
<!-- - Apps & Services—Shows a topology of service containers -->

**Starter** プロジェクトの場合、`master` （実稼動環境）から始まる分岐の階層があります。 作成したブランチはすべて、`master` ブランチの子として表示されます。 Adobeでは、`staging` ブランチを作成してから、開発用に`integration` ブランチを作成することをお勧めします。 [ スターターアーキテクチャ ](../architecture/starter-architecture.md)を参照してください。

**Pro**&#x200B;の場合、`production`から`staging`から`integration`までの分岐の階層があります。 ![専用アイコン ](../../assets/icon-dedicated.png){width="32"} アイコンは、ブランチが専用環境にデプロイされていることを示します。 作成した分岐はすべて、`integration` ブランチの子として表示されます。 [Pro アーキテクチャ ](../architecture/pro-architecture.md)を参照してください。

![Pro環境リスト ](../../assets/pro-environments.png)

### 環境概要

プロジェクトナビゲーションバーから環境を選択すると、概要とナビゲーションバーが変更され、選択した環境にフォーカスされます。 ナビゲーションバーには、ブランチコントロール（ブランチ、結合、同期）と設定ボタンが含まれます。

![環境が選択されました](../../assets/environment-selected.png)

環境の概要には、環境の詳細の概要がプレビュー領域に表示されます。

- 環境名、タイプ
- 地域、プロジェクト ID
- バックアップを含む最後のアクティビティの日時
- HTTP アクセスと検索エンジンのステータス
- 環境に割り当てられたマシン名
- 環境ステータス（アクティブまたは非アクティブ）
- **[!UICONTROL Set a custom domain]** ボタンを含むストアフロント URL

メイン環境の概要では、次の操作を行います。

- [ アクティビティストリーム ](activity-stream.md)は、メイン環境の概要を構成し、選択した環境の実行中、保留中、および最近のアクティビティを表示します。
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- 「[ バックアップ」タブ ](../storage/snapshots.md#create-a-manual-backup)には、保存されたバックアップのリスト、バックアップアクションの履歴、およびバックアップボタンが表示されます。

### ストアフロントへのアクセス

各アクティブ環境にはストアフロントがあります。 上部のナビゲーションから環境を選択し、環境の概要でURLをクリックします。 また、アクティビティリストの右側には&#x200B;**[!UICONTROL URLs]** リストがあります。

Web アクセス URLには、次のものが含まれる場合があります。

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **一意のID** = 7個のランダムな英数字
- **プロジェクト ID** = 13文字のプロジェクト ID
- **地域** = AWSまたはAzureの地域名、[地域IP アドレス ](regional-ip-addresses.md)を参照

Pro実稼動環境とステージング環境には、次のリンクを使用してアクセスできる3つのノードが含まれています。

- ロードバランサーのURL:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- 3台の冗長なサーバーのいずれかに直接アクセスします。

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  本番URLは、コンテンツ配信ネットワーク（CDN）で使用されます。

## 設定

プロジェクトナビゲーションの右側にある「![ プロジェクトを設定」アイコン ](../../assets/icon-configure.png){width="36"} （設定）アイコンをクリックして、_設定_ パネルを開きます。

### プロジェクト設定

**[!UICONTROL Project Settings]**&#x200B;は、ユーザーや変数などを管理するために、プロジェクトレベルのコントロールのメニューを拡張します。

| オプション | 説明 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| 一般 | スケジュールのバックアップまたはメンテナンスで使用するタイムゾーンを管理します。 |
| アクセス | プロジェクトおよび環境タイプへの[ ユーザーアクセス ](user-access.md)を管理します。 |
| 証明書 | プロジェクトに関連付けられているSSL証明書のリストを表示します。 |
| キーをデプロイ | プロジェクトコードリポジトリに公開鍵を追加して表示します。 |
| ドメイン | プロジェクトにドメイン名を追加します。 [ ドメインの管理](../cdn/fastly-custom-cache-configuration.md#manage-domains)を参照してください。 |
| 連携 | ヘルス通知やwebhookなど、[統合](../integrations/overview.md)を追加および管理します。 |
| 変数 | すべての環境でビルド時および実行時に使用できる[ プロジェクトレベルの変数](../environment/variable-levels.md)を追加します。 |

{style="table-layout:auto"}

### 環境設定

「**[!UICONTROL Environments]**」をクリックし、リストから特定の環境を選択して、サイト設定や環境変数などを管理するコントロールを選択します。

| オプション | 説明 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 一般 | 表示名、環境タイプ、親環境を設定します。<br>異なる環境設定を切り替えます： |
|           | **送信メールを有効にする**: SMTP プロトコルを使用して、環境から[送信メール ](outgoing-emails.md)を送信します。 |
|           | **検索エンジンから非表示**：検索エンジンのインデクサーとweb クローラーをサイトからブロックします。 |
|           | **HTTP アクセス制御**: ログインとIP アドレスのアクセス制御を使用して、[!DNL Cloud Console]のセキュリティ設定を有効にします。 |
|           | ステータスは`active`または`inactive`です。 あなたの仕事の多くは活動的な環境で行われています。 環境を非アクティブ化または削除できます。 |
| 変数 | 実行時に利用可能な[環境レベルの変数](../environment/variable-levels.md)を表示、作成、管理します。 |
| ドメイン | [設定済みのルート ](../routes/routes-yaml.md)のリストを表示します。 |

{style="table-layout:auto"}

>[!WARNING]
>
>**プロステージング環境と実稼動環境の保護にHTTP アクセス制御方式を使用しないでください**。 これにより、Fastlyのキャッシュが破損します。 代わりに、Fastly CDN for Adobe Commerceで利用できる[ ブロック ](../cdn/fastly-vcl-blocking.md)機能を使用してアクセスをブロックするか、[Fastly Basic Auth](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md)を使用してアクセス制御を実装します。

## FastlyとNew Relicの認証情報

プロジェクトには、[Fastly](../cdn/fastly.md)と[New Relic](../monitor/new-relic-service.md)が含まれます。 プロジェクトの詳細には、プロジェクト計画の情報と、これらの統合の重要なライセンスとトークンが表示されます。 ライセンス所有者のみが、資格情報とサービスに最初にアクセスできます。 必要に応じて、これらの資格情報を技術リソースや開発者リソースに提供します。

- [Fastly](https://www.fastly.com/)は、Adobe Commerceのクラウドインフラストラクチャプロジェクト向けに、コンテンツ配信（CDN）、画像最適化、セキュリティサービス（DDoSおよびWAF）を提供します。 [Fastly資格情報の取得](../cdn/fastly-configuration.md#get-fastly-credentials)を参照してください。

- [New Relic](../monitor/new-relic-service.md)は、ステージング環境と実稼動環境に関するアプリケーションの指標とパフォーマンス情報を提供します。

[Cloud CLI](../dev-tools/cloud-cli-overview.md)を使用して、統合トークン、IDなどを確認します。

```bash
magento-cloud subscription:info services
```
