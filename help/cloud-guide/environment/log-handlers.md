---
title: ログハンドラー
description: クラウドインフラストラクチャー上でAdobe Commerceのログハンドラーを設定する方法について説明します。
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# ログハンドラー

ログハンドラーを設定して、メッセージをリモートログサーバーに送信できます。 ログハンドラーは、ログをSlackやメールにプッシュする方法と同様に、ログの作成とデプロイを他のシステムにプッシュします。 ハードウェア関連のメッセージをログに記録するのに最適な _syslog_ ハンドラ、またはソフトウェア アプリケーションからのメッセージをログに記録するのに最適な Graylog Extended Log Format （GELF） ハンドラを有効にできます。

次の例では、`.magento.env.yaml` ファイルに設定を追加して、これらのハンドラーの両方を設定します。 最小ログレベル（`min_level`）の値については、[ ログレベル ](#log-levels) を参照してください。

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## ログレベル

ログレベルは、通知メッセージの詳細レベルを決定します。 次のログレベルカテゴリには、その下のすべてのログレベルが含まれます。 例えば、`debug` レベルにはすべてのレベルからのログが含まれるのに対して、`alert` レベルにはアラートと緊急事態のみが表示されます。

- **debug** – 詳細なデバッグ情報
- **info** - ユーザー・ログインや SQL ログなどの興味深いイベント
- **注意** – 通常の、重要なイベント
- **警告** – 非推奨の API の使用や API の不適切な使用など、エラー以外の例外的な発生
- **error** – 即時アクションを必要としない実行時エラー
- **クリティカル** - アプリケーション・コンポーネントが使用できない、または予期しない例外など、クリティカルな状態
- **アラート**:SMS アラートをトリガーする即時処理（Web サイトがダウンしている、データベースが使用できないなど）
- **emergency**：システムが使用できない
