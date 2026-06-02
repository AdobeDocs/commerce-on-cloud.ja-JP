---
title: 環境の復元
description: クラウドインフラストラクチャプロジェクトからAdobe Commerce アプリケーションをアンインストールし、環境を安定した状態に復元する方法を説明します。
role: Developer
topic: Development
exl-id: 78e4c535-8c4d-4185-9a56-d25cb04db991
TQID: https://experienceleague.adobe.com/fYo3B1zwc8a9UAFL3Eo8dOwjG7XQXrhUiJHYqNLTbeQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 493
ht-degree: 0%

---

# 環境の復元

統合環境で問題が発生し、[有効なバックアップ ](../storage/snapshots.md)がない場合、または環境を空白のスレートにリセットする場合は、次のいずれかの方法で環境を復元またはリセットできます。

- Git ブランチのコードをリセットまたは元に戻す
- [!DNL Commerce] アプリケーションのアンインストール
- 再展開の強制
- データベースを手動でリセット

{{stuck-deployment-tip}}

## Git ブランチをリセットする

Git ブランチをリセットすると、コードは以前の安定した状態に戻ります。

**ブランチをリセットするには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. Git コミット履歴を確認します。 `--oneline`を使用して、1行に省略されたコミットを表示します。

   ```bash
   git log --oneline
   ```

   回答サンプル：

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. コードの最後の既知の安定した状態を表すコミットハッシュを選択します。

   ブランチを初期化された状態にリセットするには、ブランチを作成した最初のコミットを見つけます。 `--reverse`を使用して、履歴を時系列の逆順で表示できます。

1. 「ハードリセット」オプションを使用して、ブランチをリセットします。 このコマンドを使用すると、選択したコミット以降のすべての変更が破棄されるので注意してください。

   ```bash
   git reset --hard <commit>
   ```

1. 変更をプッシュして、Adobe Commerceを再インストールする再デプロイメントをトリガーします。

   ```bash
   git push --force <origin> <branch>
   ```

## Commerceのアンインストール

[!DNL Commerce] アプリケーションをアンインストールすると、データベースを復元し、デプロイメント設定を削除して、`var/` サブディレクトリをクリアすることで、環境が元の状態に戻ります。 このガイダンスでは、Git ブランチを以前の安定した状態にリセットします。 最近のバックアップがない場合でも、SSHを使用してリモート環境にアクセスできる場合は、次の手順に従って環境を復元します。

- 構成管理の無効化
- Adobe Commerceのアンインストール
- Git ブランチをリセットする

Adobe Commerce ソフトウェアをアンインストールすると、データベースが削除および復元され、デプロイメント設定が削除され、`var/` サブディレクトリがクリアされます。 次回のデプロイメント時に以前の構成設定を自動的に適用しないように、[構成管理](../store/store-settings.md)を無効にすることが重要です。 `app/etc/` ディレクトリに`config.php` ファイルが含まれていないことを確認してください。

**Adobe Commerce ソフトウェアをアンインストールするには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. 設定ファイルを削除します。
   - Adobe Commerce 2.2以降：

     ```bash
     rm app/etc/config.php
     ```

   - Adobe Commerce 2.1の場合

     ```bash
     rm app/etc/config.local.php
     ```

1. Adobe Commerce アプリケーションをアンインストールします。

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. Adobe Commerceが正常にアンインストールされたことを確認します。

   アンインストールが正常に完了したことを確認するには、次のメッセージが表示されます。

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. `var/` サブディレクトリをクリアします。

   ```bash
   rm -rf var/*
   ```

1. ログアウトします。

>[!TIP]
>
>オプションとして、ビルドキャッシュをクリーンアップすることをお勧めします。
>
>```bash
>magento-cloud project:clear-build-cache
>```

## 再展開の強制

Adobe Commerceをアンインストールしようとし、デプロイメントが失敗し続ける場合は、手動で再デプロイメントを強制してみてください。

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## データベースのリセット

Adobe Commerceのアンインストールを試みたが、コマンドが失敗した場合、またはコマンドが完了しなかった場合は、データベースを手動でリセットできます。

**データベースをリセットするには**:

1. ローカル ワークステーションで、プロジェクト ディレクトリに移動します。

1. SSHを使用してリモート環境にログインします。

   ```bash
   magento-cloud ssh
   ```

1. データベースに接続します。

   ```bash
   mysql -h database.internal
   ```

1. `main` データベースをドロップします。

   ```shell
   drop database main;
   ```

1. 空の`main` データベースを作成します。

   ```shell
   create database main;
   ```

1. 次の設定ファイルを削除します。

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. ログアウトして、再展開をトリガーします。

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
