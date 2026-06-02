---
title: Commerceの管理パネルにアクセス
description: Commerce管理パネルにアクセスする方法について説明します。
recommendations: noDisplay, catalog
exl-id: 827417b0-9048-44d8-8c82-07befba476c7
TQID: https://experienceleague.adobe.com/V3BXuCc9aqT5YuyIS8WAZgUdPAYNhQunAgg2i2FCaOs
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080bid: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 361
ht-degree: 0%

---

# Commerceの管理パネルにアクセス

Commerce管理パネルへの管理アクセス権を持つユーザーは、ユーザーの追加、ストアサービスの設定、ストアの設定とカスタマイズ作業の完了などを行うことができます。

新しいプロジェクトの場合、ウェルカムメールを受け取った後の最初の手順は、ライセンス所有者アカウントのパスワードを変更して、プロジェクトへの管理者アクセスを保護することです。 このアカウントのデフォルトのユーザー名は、ライセンス所有者のメールアドレスです。

次のいずれかの方法を使用して、パスワード変更リクエストを送信できます。

- ライセンス所有者のメールアドレスに送信されたウェルカムメールを探し、リンクに従ってパスワードを変更します。

- ストア URLを[[!DNL Cloud Console]](../cloud-guide/project/overview.md)からブラウザーにコピーします。 次に、URLの末尾に`/admin`を追加して、サインインページを開きます。 **パスワードを忘れた場合**&#x200B;をクリックします ライセンス所有者の電子メールアドレスにパスワード変更リクエストを送信するためのリンク。

パスワード変更リクエストを送信したら、電子メールでパスワードリセット通知を確認します。 電子メールが届かない場合は、迷惑メールフォルダーを確認してください。

>[!TIP]
>
>パスワードのリセットに失敗した場合、または管理者パネルにログインできない場合、管理者アクセス権を持つユーザーはSSHを使用してプロジェクトに接続し、`admin:user:create` CLI コマンドを使用して管理者ユーザーを追加できます。 _インストールガイド_&#x200B;の「[管理者アカウントの作成、編集、ロック解除](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html)」を参照してください。

## サイトの健全性を監視

[Site-Wide Analysis Tool](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro)は、Adobe Commerce インストールのセキュリティと操作性を確保するための詳細なシステムインサイトと推奨事項を含む、プロアクティブなセルフサービスツールおよび中央リポジトリです。 24時間365日、リアルタイムのパフォーマンスモニタリング、レポート、アドバイスを提供し、潜在的な問題を特定して、サイトの健全性、安全性、アプリケーション設定をより詳細に可視化できます。 問題解決時間を短縮し、サイトの安定性とパフォーマンスを向上させることができます。 サイト全体の分析ツールには、[管理者パネル ](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel)から直接アクセスできます。
