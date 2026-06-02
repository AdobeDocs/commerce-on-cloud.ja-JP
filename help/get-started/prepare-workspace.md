---
title: 開発のための準備
description: Commerce on cloud infrastructure プロジェクトで使用する開発ワークスペースをセットアップするために使用できるツールについて、資格情報を収集し、学習します。
recommendations: noDisplay, catalog
exl-id: adb7f74f-8007-4f23-bc07-46b0f7d0ebd9
TQID: https://experienceleague.adobe.com/al-kKS6wmNpCQt5tDRMIO2L10UEShSC2kOf66eThcfw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 498
ht-degree: 0%

---

# 開発のための準備

Commerceを初めて使用する場合でも、クラウド インフラストラクチャに移行する既存のCommerce オーナーの場合でも、次の手順を使用して、Cloud プロジェクト用の開発ワークスペースを準備します。 これらの手順の一部をすでに完了している場合、または既存のAdobe Commerce開発環境がある場合は、次の手順を確認して期待される結果を確認し、次の手順に進みます。 一部の構成とワークフローは、一般的なオンプレミス インストールとは異なります。

## 資格情報

ワークスペースを設定する前に、次のキーとアカウントアクセスを収集します。

- **認証キー（コンポーザーのキー）**

  認証キーは、Adobe Commerce Composer リポジトリ （`repo.magento.com`）およびGitHubなどのアプリケーション開発に必要なその他のGit サービスへの安全なアクセスを提供する32文字の認証トークンです。 アカウントには複数の認証キーを設定できます。 ワークスペース設定の場合は、コードリポジトリ用の1つの特定のキーから始めます。 キーがない場合は、プロジェクト所有者に連絡するか、自分で[認証キー](../cloud-guide/development/authentication-keys.md)を作成してください。

- **クラウドプロジェクトアカウント**

  プロジェクトオーナーは、Adobe Commerce on cloud インフラストラクチャプロジェクトに招待します。 招待メールを受け取ったら、リンクをクリックし、プロンプトに従ってアカウントを作成します。 [&#x200B; オンボーディング &#x200B;](onboarding.md)を参照してください。

- **Adobe Commerce暗号化キー**

  既存のシステムのみを読み込む場合は、データベースのアクセスとデータを保護するために使用される暗号化キーを取得します。 このキーについて詳しくは、[暗号化キーに関する問題の解決](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)を参照してください

## 開発者用ツール

- **Cloud CLIのインストール**

  クラウド環境を管理し、自動化タスクを実行できるように、`magento-cloud` CLIをインストールします。 インストール手順については、[Cloud CLI](../cloud-guide/dev-tools/cloud-cli-overview.md)を参照してください。

- **ローカル開発とテスト用にDockerをインストール**

  必要に応じて、Docker環境を使用して、ローカル開発用にCommerce on cloud infrastructure `integration`環境をエミュレートします。 3つの必須コンポーネント（Adobe Commerce v2 テンプレート、Docker Compose、および`ece-tools` パッケージ）があります。

   - [Docker アーキテクチャと共通コマンド](../cloud-guide/dev-tools/cloud-docker.md)
   - [Docker開発環境の起動](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [ECE-Tools パッケージ](../cloud-guide/dev-tools/package-overview.md)

- **Git ベースのサービスの統合**

  GitHubやGitLabなどのGit ベースのホスティングサービスを、Adobe Commerce on cloud インフラストラクチャと任意で統合できます。 [統合](../cloud-guide/integrations/overview.md)を参照してください。

## プロジェクトコード

リモート環境とのやり取りには、安全な接続が不可欠です。 新しいプロジェクトの場合は、[に [!DNL Cloud Console]](https://console.adobecommerce.com)にログインし、**[!UICONTROL No SSH key]**&#x200B;をクリックします。 このアイコンはコマンドフィールドの右側にあり、プロジェクトにSSH キーが含まれていない場合に表示されます。 [&#x200B; セキュア接続](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account)を参照してください。

**コードベースをローカル ワークステーションに複製するには**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)で「**[!UICONTROL code]**」をクリックし、「**[!UICONTROL Git]**」タブを選択します。

   ![&#x200B; コードを複製](../assets/ui-git-code.png){width="450"}

1. 指定された`git clone ...` コマンドをコピーします。

1. ターミナルで、作業ディレクトリを作成して変更します。

1. `git clone ...` コマンドを貼り付けて実行します。

>[!TIP]
>
>Adobeでは、Adobe Commerceの特定のバージョンのパッケージ手順を含むテンプレートリポジトリを使用して、初期プロジェクト環境をプロビジョニングします。 [&#x200B; プロジェクトファイル構造](../cloud-guide/project/file-structure.md)のトピックを確認し、重要なプロジェクトファイルとクラウドテンプレートについて詳しく説明します。
