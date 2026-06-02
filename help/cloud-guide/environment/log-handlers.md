---
title: ログハンドラー
description: クラウドインフラストラクチャ上のAdobe Commerceのログハンドラーを設定する方法について説明します。
feature: Cloud, Logs, Configuration
role: Developer
exl-id: 0d7fb653-468b-432c-9830-582b0fed8512
TQID: https://experienceleague.adobe.com/4dowk2oMMCROVmEc8muHE7CzaZ-T3SaQi4sANVnMeWQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 235
ht-degree: 0%

---

# ログハンドラー

ログハンドラーを設定して、メッセージをリモートログサーバーに送信できます。 ログハンドラーは、ログをSlackやメールにプッシュする方法と同様に、ビルドとデプロイのログを他のシステムにプッシュします。 ハードウェアに関連するメッセージのログ記録に最適な&#x200B;_syslog_ ハンドラー、またはソフトウェアアプリケーションからのメッセージのログ記録に最適なGraylog Extended Log Format （GELF） ハンドラーを有効にできます。

次の例では、設定を`.magento.env.yaml` ファイルに追加して、これらのハンドラーの両方を設定します。 ログ レベル （`min_level`）の最小値については、[ ログ レベル ](#log-levels)を参照してください。

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

ログレベルは、通知メッセージの詳細レベルを決定します。 次のログレベルのカテゴリには、その下のすべてのログレベルが含まれます。 例えば、`debug` レベルには各レベルのログが含まれますが、`alert` レベルにはアラートと緊急事態のみが表示されます。

- **debug** – 詳細なデバッグ情報
- **info** - ユーザーログインやSQL ログなどの興味深いイベント
- **notice** – 通常ですが、重要なイベント
- **警告** – 非推奨のAPIの使用やAPIの使用不足など、エラーではない例外的な発生
- **エラー** – すぐにアクションを実行する必要のない実行時エラー
- **critical** – 使用できないアプリケーションコンポーネントや予期しない例外などの重大な条件
- **アラート** - Web サイトがダウンしているか、データベースが利用できないなど、すぐにアクションを実行する必要があり、SMS アラートをトリガーする
- **emergency** – システムは使用できません
