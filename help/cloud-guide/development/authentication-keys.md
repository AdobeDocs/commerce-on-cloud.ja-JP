---
title: 認証キー
description: クラウドインフラストラクチャー上のAdobe Commerceの開発プロジェクトに認証キーを適用する方法を説明します。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 認証キー

Adobe Commerce リポジトリにアクセスし、クラウドインフラストラクチャプロジェクト上のAdobe Commerceのインストールおよびアップデートコマンドを有効にするには、認証キーが必要です。 Composer 認証資格情報を指定する方法は 2 つあります。

- **認証ファイル** - クラウドインフラストラクチャーのルートディレクトリ上のAdobe CommerceにAdobe Commerce[&#x200B; 認証資格情報 &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html?lang=ja) を含むファイルです。
- **環境変数** – 誤って公開されるのを防ぐために、クラウドインフラストラクチャプロジェクト上のAdobe Commerceに認証キーを設定するための環境変数。

>[!BEGINSHADEBOX]

**セキュリティノート**

Adobeでは、認証資格情報が誤って漏洩されるのを防ぐために、クラウドプロジェクトで [&#x200B; 環境変数 &#x200B;](#composer-auth-environment-variable) 方式を使用することをお勧めします。

ローカル開発ファイル方式は、Cloud Docker for Commerceを認証ツールとして使用する場合に最適ですが、`auth.json` ファイルを公開 Git ベースのリポジトリにアップロードしないように注意してください。 `auth.json` ファイルを [`.gitignore` ファイルに追加でき &#x200B;](../project/file-structure.md#ignoring-files) す。

>[!ENDSHADEBOX]

## 認証ファイル

**`auth.json` ファイルを作成するには**:

1. プロジェクトのルートディレクトリに `auth.json` ファイルがない場合は、作成します。

   - テキスト エディタを使用して、プロジェクト ルート ディレクトリに `auth.json` ファイルを作成します。
   - [sample `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) の内容を新しい `auth.json` ファイルにコピーします。

1. `<public-key>` と `<private-key>` をAdobe Commerceの認証資格情報に置き換えます。

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 変更を保存し、テキストエディターを終了します。

## Composer 認証環境変数

次の方法は、公開 Git ベースのリポジトリ内の機密性の高い認証情報が誤って公開されるのを防ぐための最適な方法です。

**環境変数を使用して認証キーを追加するには**:

1. _[!DNL Cloud Console]_&#x200B;で、プロジェクトナビゲーションの右側にある「設定」アイコンをクリックします。

   ![&#x200B; プロジェクトの設定 &#x200B;](../../assets/icon-configure.png){width="36"}

1. _プロジェクト設定_ リストで、「**[!UICONTROL Variables]**」をクリックします。

1. 「**[!UICONTROL Create variable]**」をクリックします。

1. 「**[!UICONTROL Variable name]**」フィールドに「`env:COMPOSER_AUTH`」と入力します。

1. 「_値_」フィールドに以下を追加し、`<public-key>` と `<private-key>` をAdobe Commerceの認証資格情報に置き換えます。

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. **[!UICONTROL Available during buildtime]** を選択し、**[!UICONTROL Available during runtime]** の選択を解除します。

1. 「**[!UICONTROL Create variable]**」をクリックします。

1. 各環境から `auth.json` ファイルを削除します。
