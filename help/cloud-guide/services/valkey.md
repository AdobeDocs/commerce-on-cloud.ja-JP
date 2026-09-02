---
title: Valkey サービスの設定
description: Redisの置き換えやキャッシュバックエンド設定のカスタマイズなど、Adobe Commerce on cloud infrastructureのバックエンドキャッシュソリューションとしてValkeyを設定および最適化する方法について説明します。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Valkey サービスの設定

[Valkey](https://valkey.io)は、クラウド インフラストラクチャ上のAdobe Commerceのオプションのバックエンド キャッシュ ソリューションです。 Valkeyは、Adobe Commerce 2.4.9以降、または2.4.5-p16、2.4.6-p14、2.4.7-p9、および2.4.8-p4以降のパッチリリースでデフォルトのキャッシュ設定を上書きする場合に必要です。

{{service-instruction}}

## Valkeyの設定

RedisをValkeyに置き換えるには、次のファイルを更新します。

- `.magento/services.yaml`
- `.magento.app.yaml`

### サービスの設定

`.magento/services.yaml`で、Redis サービス定義をValkey サービス定義に置き換えます。 `<version>`を、お使いのAdobe Commerce バージョンと現在のCloud テンプレートでサポートされているValkey バージョンに置き換えます。

```yaml
cache:
  type: valkey:<version>
```

**例**

```yaml
cache:
  type: valkey:8.0
```

例のバージョンは普遍的ではありません。 実際のデフォルトバージョンとサポートされているサービスバージョンは、Adobe Commerceのバージョンと現在のCloud テンプレートによって異なります。 現在のプロジェクトテンプレートで指定されたバージョンを使用します。 詳しくは、[&#x200B; サービスの設定](services-yaml.md#service-versions)を参照してください。

>[!WARNING]
>
>サービス IDを変更すると、既存のサービスが削除され、新しいサービスが作成されます。 削除されたサービス内の既存のデータは完全に削除されます。 サービスの名前を変更する前に、環境をバックアップします。

同じサービス IDを保持している場合でも、`type`値を`redis:<version>`から`valkey:<version>`に変更しても、キャッシュ データとセッション データが保持されると仮定しないでください。 移行を新しいキャッシュの作成として扱います。既存のキャッシュとセッションデータは保持される保証はなく、移行が完了するとユーザーはログアウトされます。

### サービス関係の設定

`.magento.app.yaml`で、アプリケーションとValkey サービスとの関係を設定します。

```yaml
relationships:
  valkey: "cache:valkey"
```

関係キー`valkey`は、アプリケーションがサービスにアクセスするために使用する名前です。 値`cache:valkey`は、`.magento/services.yaml`で定義されたサービス IDとサービス タイプを参照しています。

>[!TIP]
>
>Adobe Commerceは、`credis` クライアントライブラリを介してValkeyと通信します。このクライアントライブラリは、デフォルトでプレーン PHP ソケットを介して動作します。 パフォーマンスを向上させるには、`.magento.app.yaml`で`redis` PHP拡張機能を有効にします。 `credis`は、使用可能な場合、コンパイルされた拡張機能を自動的に使用します。
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### 変更をコミットしてデプロイする

設定の変更を追加、コミット、プッシュします。

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

デプロイメントが完了したら、Valkey サービス関係が使用可能であることを確認します。

{{service-change-tip}}

{{valkey-newrelic}}

## Valkey設定のカスタマイズ

キャッシュ、セッション、L2、およびレプリカ接続に関する推奨事項については、_実装プレイブックのベストプラクティスガイド_&#x200B;の「[ValkeyとRedis サービス設定のベストプラクティス &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)」を参照してください。

## サービス関係の確認

デコードされた`MAGENTO_CLOUD_RELATIONSHIPS` オブジェクトを表示するには、設定をデプロイした後、アプリケーション コンテナから次のコマンドを実行します。

SSHを使用してリモートクラウド環境に接続し、次を実行します。

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

このコマンドは、設定されたすべてのサービス関係を表示します。 Valkey接続の詳細を特定するには、valkey関係を見つけます。

**出力例**

次の省略形の例は、`valkey`関係を示しています。 ユニバーサルスキーマではありません。

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

出力は、環境とサービス設定によって異なります。 この例では、ホスト名、ポート、IP アドレス、クラスター名、サービスバージョン、ユーザー名、パスワードをハードコーディングしないでください。 ターゲット環境で`MAGENTO_CLOUD_RELATIONSHIPS`によって返される値を使用します。

`jq`が使用可能な場合は、Valkey関係のみを表示します。

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

サービス関係について詳しくは、[&#x200B; サービスの設定](services-yaml.md)を参照してください。

## Valkey CLIの使用

Valkey関係が`valkey`であると仮定すると、`MAGENTO_CLOUD_RELATIONSHIPS`から返されたホストとポートをValkeyに接続するために使用します。

```terminal
valkey-cli -h <host> -p <port>
```

**例**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## インストール済みのValkey バージョンを取得する

>[!BEGINTABS]

>[!TAB 統合環境]

統合環境で、`valkey`関係によって返されるホストとポートを使用して、次を実行します。

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**応答の例**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

バージョンとビルドの詳細は環境によって異なります。 表示されたサンプルバージョンを必須またはユニバーサルサービスバージョンとして扱わないでください。

>[!TAB Pro ステージングおよび実稼動]

Pro ステージング環境と実稼動環境では、次を実行します。

```terminal
valkey-server -v
```

**応答の例**

```text
Valkey server v=<installed-version> ...
```

バージョンとビルドの詳細は環境によって異なります。 表示されたサンプルバージョンを必須またはユニバーサルサービスバージョンとして扱わないでください。

>[!ENDTABS]

## Valkeyのトラブルシューティング

### キャッシュクリーンエラーは、Valkey設定のキャッシュでRedisを参照します

`cache` サービスがValkeyとして設定されている場合でも、デプロイ前のキャッシュクリーンのエラーで、エラーコード `[107]` （`clean-redis-cache`）と`Connection to Redis` メッセージが表示される可能性があります。 `ece-tools`は、バッキング キャッシュ サービスがRedisであるかValkeyであるかに関係なく、このエラーコードとメッセージをキャッシュ クリーン ステップに使用します。

関係ホストの`Name or service not known`などのDNS エラーが原因で発生した場合、サービス関係が利用可能になる前にデプロイ手順が実行されるか、`.magento.app.yaml`の関係名が`.magento/services.yaml`のサービス IDと一致しません。 [&#x200B; サービス関係の確認](#verify-the-service-relationship)を参照してください。
