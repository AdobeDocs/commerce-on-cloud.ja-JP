---
title: 開発の準備
description: 資格情報を収集し、クラウドインフラストラクチャプロジェクト上のCommerceで使用する開発ワークスペースを設定するために使用できるツールについて説明します。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# 開発の準備

Commerceを初めて使用する場合でも、クラウドインフラストラクチャに移行する既存のCommerce オーナーでも、これらの手順を使用して Cloud プロジェクトの開発ワークスペースを準備します。 これらの手順の一部を既に完了している場合、または既存のAdobe Commerce開発者環境がある場合は、期待される結果について次の点を確認し、次の手順に進みます。 一部の設定とワークフローは、一般的なオンプレミスのインストールとは異なります。

## 資格情報

ワークスペースを設定する前に、次のキーとアカウントのアクセス権を収集します。

- **認証キー（Composer キー）**

  認証キーは 32 文字の認証トークンで、Adobe Commerce Composer リポジトリ（`repo.magento.com`）や、GitHub などのアプリケーション開発に必要な他の Git サービスへの安全なアクセスを提供します。 アカウントには複数の認証キーを設定できます。 ワークスペースを設定する場合は、まずコードリポジトリに固有のキーを 1 つ使用します。 キーがない場合は、プロジェクトの所有者に問い合わせるか、自分で [ 認証キー ](../cloud-guide/development/authentication-keys.md) を作成します。

- **クラウドプロジェクトアカウント**

  プロジェクトオーナーに、クラウドインフラストラクチャプロジェクトのAdobe Commerceへの招待を受ける必要があります。 電子メールによる招待を受け取ったら、リンクをクリックし、画面の指示に従ってアカウントを作成します。 [ オンボーディング ](onboarding.md) を参照してください。

- **Adobe Commerce暗号化キー**

  既存のシステムのみをインポートする場合は、データベースのアクセスとデータを保護するために使用する暗号化キーを取得します。 このキーについて詳しくは、[ 暗号化キーに関する問題の解決 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html?lang=ja) を参照してください。

## デベロッパーツール

- **Cloud CLI のインストール**

  `magento-cloud` CLI をインストールして、クラウド環境を管理し、自動化タスクを実行できるようにします。 インストール手順については、[Cloud CLI](../cloud-guide/dev-tools/cloud-cli-overview.md) を参照してください。

- **ローカル開発およびテスト用の Docker のインストール**

  必要に応じて、Docker 環境を使用して、Commerce on cloud infrastructure `integration` 環境をローカル開発用にエミュレートします。 必須のコンポーネントには、Adobe Commerce v2 テンプレート、Docker Compose および `ece-tools` パッケージの 3 つがあります。

   - [Docker アーキテクチャと一般的なコマンド](../cloud-guide/dev-tools/cloud-docker.md)
   - [Docker 開発環境の起動 ](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [ECE-Tools パッケージ](../cloud-guide/dev-tools/package-overview.md)

- **Git ベースのサービスの統合**

  オプションで、GitHub や GitLab などの Git ベースのホスティングサービスと、クラウドインフラストラクチャー上のAdobe Commerceを統合します。 [ 統合 ](../cloud-guide/integrations/overview.md) を参照してください。

## プロジェクトコード

リモート環境とやり取りするには、安全な接続が不可欠です。 新規プロジェクトの場合は、[ にログイン  [!DNL Cloud Console]](https://console.adobecommerce.com) し、「**[!UICONTROL No SSH key]**」をクリックします。 このアイコンはコマンドフィールドの右側にあり、プロジェクトに SSH キーが含まれていない場合に表示されます。 [ 安全な接続 ](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account) を参照してください。

**コードベースをローカルワークステーションにクローンするには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com) で「**[!UICONTROL code]**」をクリックし、「**[!UICONTROL Git]**」タブを選択します。

   ![ コードのクローンを作成 ](../assets/ui-git-code.png){width="450"}

1. 提供された `git clone ...` コマンドをコピーします。

1. ターミナルでを作成し、作業ディレクトリに変更します。

1. 貼り付けて、`git clone ...` コマンドを実行します。

>[!TIP]
>
>Adobeは、特定のバージョンのAdobe Commerceのパッケージ手順を含むテンプレートリポジトリを使用して、初期プロジェクト環境をプロビジョニングします。 [ プロジェクトファイル構造 ](../cloud-guide/project/file-structure.md) のトピックを確認し、重要なプロジェクトファイルとクラウドテンプレートについて詳しく学びます。
