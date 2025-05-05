---
title: 作業者
description: アプリケーション設定ファイルでワーカープロパティを設定する方法  [!DNL Commerce]  説明します。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# 労働者財産

ワーカーは、実行中の Nginx インスタンスを使用せずに、web インスタンスとは別に実行するように定義できます。ただし、ワーカーは、[!DNL Commerce] アプリケーションで使用されているのと同じネットワークストレージを使用します。 ルーターがワーカーに公開リクエストを送信できないので、ワーカーインスタンスに（Node.js または Go を使用して） web サーバーを設定する必要はありません。 これにより、ワーカーインスタンスがバックグラウンドタスクや、デプロイメントをブロックするリスクのあるタスクを継続的に実行する場合に最適になります。

## ワーカーの設定

ワーカーは、Pro ステージング環境および実稼動環境でのみ使用できます。 Pro 統合およびスターター環境は、[CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) 変数の使用を選択できます。

ステージング環境または実稼動環境でワーカーを設定するには、[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket) して、次の情報を含めます。

- プロジェクト ID
- 環境 ID
- 作業者名
- 開始コマンド

ワーカーごとに 1 つのプロセスを設定できます。 `.magento.app.yaml` ファイル内の基本的で一般的なワーカー設定は、次のようになります。

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

この例では、リソース割り当ての小さい（サイズ S） レベルで `queue` という名前の単一のワーカーを定義し、起動時に `php ./bin/magento` コマンドを実行します。 その後、ワーカー `queue` はワーカープロセスとして各ノードで実行されます。 コマンドが終了すると、自動的に再起動されます。

## コマンドと上書き

`commands.start` キーは、ワーカーアプリケーションでコマンドを起動するために必要です。 任意の有効なシェルコマンドを使用できますが、アプリケーションの言語を使用するのが理想的です。 `start` キーで指定されたコマンドが終了すると、自動的に再起動します。

>[!IMPORTANT]
>
>`deploy` および `post_deploy` フックと `crons` コマンドは、web コンテナでのみ実行され、ワーカーインスタンスでは実行されません。

### 継承

`size`、`relationships`、`access`、`disk`、`mount` および `variables` プロパティの定義は、明示的に上書きされない限り、ワーカーに継承されます。

次のプロパティは、[ トップレベルの設定 ](properties.md) をオーバーライドする場合に最も一般的に使用されます。

- `size`：単一のバックグラウンド・プロセスに割り当てるリソースを減らす
- `variables` - アプリケーションに別の方法で実行するよう指示します

### タイミングとキュー

各ワーカーは別のワーカーの後ろにキューを作成しますが、次の設定では、PHP コード内の 8 秒のスリープに関係なく、`var/time.txt` ファイルにタイムスタンプで一貫性のある 2 秒の分離が生成されます。

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
