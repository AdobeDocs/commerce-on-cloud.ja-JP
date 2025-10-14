---
title: Commerceの管理パネルへのアクセス
description: Commerce管理パネルへのアクセス方法について説明します。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Commerceの管理パネルへのアクセス

Commerce管理パネルへの管理アクセス権を持つユーザーは、ユーザーの追加、ストアサービスの設定、ストアの設定とカスタマイズ作業の完了などを行うことができます。

新規プロジェクトの場合、「ようこそ」のメールを受信した後の最初の手順は、ライセンス所有者アカウントのパスワードを変更して、プロジェクトへの管理者アクセスを保護することです。 このアカウントのデフォルトのユーザー名は、ライセンス所有者のメールアドレスです。

次のいずれかの方法を使用して、パスワード変更リクエストを送信できます。

- ライセンス所有者のメールアドレスに送信されたようこそメールを探し、リンクに従ってパスワードを変更します。

- [[!DNL Cloud Console]](../cloud-guide/project/overview.md) ージからブラウザーにストア URL をコピーします。 次に、URL の末尾に `/admin` を追加して、ログインページを開きます。 **パスワードを忘れた場合？パスワード変更要求をライセンス所有者の電子メールアドレスに送信するためのリンクを** きます。

パスワード変更リクエストを送信したら、パスワードのリセット通知が届いていないかどうかをメールで確認します。 メールが届かない場合は、スパムフォルダーを確認してください。

>[!TIP]
>
>パスワードのリセットに失敗した場合や管理パネルにログインできない場合は、管理者アクセス権を持つユーザーが SSH を使用してプロジェクトに接続し、`admin:user:create` CLI コマンドを使用して管理者ユーザーを追加できます。 [&#x200B; インストール ガイド _の「管理者アカウントを作成、編集、またはロック解除する &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=ja) を参照してください_。

## サイトの正常性の監視

[Site-Wide Analysis Tool](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/site-wide-analysis-tool/intro) は、プロアクティブなセルフサービスツールで、Adobe Commerce インストールのセキュリティと操作性を確保するための詳細なシステムインサイトおよびレコメンデーションが含まれている中央リポジトリです。 24 時間 365 日、パフォーマンスの監視、レポート、アドバイスをリアルタイムで行うことで、潜在的な問題を特定し、サイトの正常性、安全性、アプリケーションの設定をより明確に把握します。 これにより、解決時間が短縮され、サイトの安定性とパフォーマンスが向上します。 [&#x200B; 管理パネル &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel) から直接サイト全体の分析ツールにアクセスできます。
