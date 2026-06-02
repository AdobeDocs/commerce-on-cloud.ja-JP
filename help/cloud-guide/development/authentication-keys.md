---
title: 認証キー
description: Adobe Commerce on cloud infrastructureの開発プロジェクトに認証キーを適用する方法について説明します。
feature: Cloud, Security
topic: Security
exl-id: b5a24fcd-9b43-4ec9-8a0c-52956a74e45e
TQID: https://experienceleague.adobe.com/nYBr0uvw1SZPSQqAU6uHTiitjZ0kcudsLdWagiWRLP8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 318
ht-degree: 0%

---

# 認証キー

Adobe Commerce リポジトリにアクセスし、Adobe Commerce on cloud infrastructure プロジェクトのインストールコマンドとアップデートコマンドを有効にするには、認証キーが必要です。 Composerの認証情報を指定する方法は2つあります。

- **認証ファイル** - Adobe Commerce on cloud infrastructure ルートディレクトリにAdobe Commerce [認証資格情報](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html)を含むファイル。
- **環境変数**:Adobe Commerce on cloud infrastructure プロジェクトで認証キーを設定し、偶発的な露出を防ぐための環境変数。

>[!BEGINSHADEBOX]

**セキュリティノート**

Adobeでは、認証情報が誤って公開されるのを防ぐため、[環境変数](#composer-auth-environment-variable) メソッドをクラウドプロジェクトで使用することをお勧めします。

ローカル開発ファイル方式は、Cloud Docker for Commerceを認証ツールとして使用する場合に最適ですが、`auth.json` ファイルをパブリック Git ベースのリポジトリにアップロードしないように注意してください。 `auth.json` ファイルを[`.gitignore` ファイル ](../project/file-structure.md#ignoring-files)に追加できます。

>[!ENDSHADEBOX]

## 認証ファイル

**`auth.json` ファイルを作成するには**:

1. プロジェクトのルートディレクトリに`auth.json` ファイルがない場合は、ファイルを作成します。

   - テキストエディターを使用して、プロジェクトのルートディレクトリに`auth.json` ファイルを作成します。
   - [ サンプル `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample)の内容を新しい`auth.json` ファイルにコピーします。

1. `<public-key>`と`<private-key>`をAdobe Commerce認証情報に置き換えます。

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

## Composerの認証環境変数

次の方法は、パブリック Git ベースのリポジトリで機密性の高い資格情報が誤って公開されるのを防ぐ最善の方法です。

**環境変数を使用して認証キーを追加するには**:

1. _[!DNL Cloud Console]_で、プロジェクトナビゲーションの右側にある設定アイコンをクリックします。

   ![ プロジェクトの設定](../../assets/icon-configure.png){width="36"}

1. _プロジェクト設定_ リストで、**[!UICONTROL Variables]**&#x200B;をクリックします。

1. **[!UICONTROL Create variable]**&#x200B;をクリックします。

1. 「**[!UICONTROL Variable name]**」フィールドに「`env:COMPOSER_AUTH`」と入力します。

1. _値_ フィールドに次のフィールドを追加し、`<public-key>`と`<private-key>`をAdobe Commerce認証情報に置き換えます。

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

1. **[!UICONTROL Available during buildtime]**&#x200B;を選択し、**[!UICONTROL Available during runtime]**&#x200B;の選択を解除します。

1. **[!UICONTROL Create variable]**&#x200B;をクリックします。

1. 各環境から`auth.json` ファイルを削除します。
