---
title: 作業者
description: ' [!DNL Commerce]  アプリケーション設定ファイルでworkers プロパティを設定する方法を説明します。'
feature: Cloud, Configuration
exl-id: 62d9dfaf-6265-4016-8d68-26362cf6a63a
TQID: https://experienceleague.adobe.com/sLfoGU5aolWVm6p-jHMC6VkF-DgNGdt7Wk40oALTj0o
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 357
ht-degree: 0%

---

# Workers プロパティ

実行中のNginx インスタンスを使用せずに、web インスタンスから独立して実行するワーカーを定義できます。ただし、ワーカーは、[!DNL Commerce] アプリケーションで使用されているのと同じネットワークストレージを使用します。 ルーターは公開リクエストをワーカーに転送できないため、（Node.jsまたはGoを使用して）ワーカーインスタンスにweb サーバーを設定する必要はありません。 これにより、ワーカーインスタンスは、バックグラウンドタスクや、デプロイメントをブロックするリスクがあるタスクを継続的に実行するのに最適になります。

## ワーカーの設定

ワーカーは、Pro ステージング環境と実稼動環境でのみ使用できます。 プロ統合環境とスターター環境では、[CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner)変数を選択して使用できます。

Pro ステージングまたは実稼動環境でワーカーを設定するには、[Adobe Commerce サポートチケットを送信し](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)次の情報を含めます。

- プロジェクト ID
- 環境ID
- ワーカー名
- 開始コマンド

ワーカーごとに1つのプロセスを設定できます。 `.magento.app.yaml` ファイルの基本的な共通ワーカー設定は、次のようになります。

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

この例では、リソース割り当ての小さい（サイズ S）レベルの`queue`という名前の単一のワーカーを定義し、起動時に`php ./bin/magento` コマンドを実行します。 ワーカー`queue`は、ワーカープロセスとして各ノードで実行されます。 コマンドが終了すると、自動的に再起動されます。

## コマンドとオーバーライド

ワーカーアプリケーションでコマンドを起動するには、`commands.start` キーが必要です。 任意の有効なシェルコマンドを使用できますが、アプリケーションの言語を使用することをお勧めします。 `start` キーで指定されたコマンドが終了すると、自動的に再起動します。

>[!IMPORTANT]
>
>`deploy`および`post_deploy` フックと`crons` コマンドは、ワーカーインスタンスではなく、web コンテナでのみ実行されます。

### 継承

`size`、`relationships`、`access`、`disk`および`mount`、および`variables` プロパティの定義は、明示的に上書きされない限り、ワーカーによって継承されます。

次のプロパティは、[&#x200B; トップレベル設定](properties.md)を上書きするために最もよく使用されます。

- `size` – 単一のバックグラウンド プロセスに割り当てるリソースの数を減らします
- `variables` - アプリケーションの実行方法を変更するように指示します

### タイミングとキュー

各ワーカーは別のワーカーの後ろにキューに入れますが、次の設定では、PHP コード内の8秒のスリープに関係なく、`var/time.txt` ファイルのタイムスタンプで2秒の分離が一貫して生成されます。

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
