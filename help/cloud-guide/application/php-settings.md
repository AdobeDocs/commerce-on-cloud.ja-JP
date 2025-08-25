---
title: PHP 設定
description: クラウドインフラストラクチャーにおけるCommerce アプリケーション設定に最適な PHP 設定について説明します。
feature: Cloud, Configuration, Extensions
exl-id: 83094c16-7407-41fa-ba1c-46b206aa160d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# PHP 設定

[ ファイルで実行する ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)PHP のバージョン `.magento.app.yaml` を選択できます。

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>PHP 8.1 以降にアップグレードする場合は、[`runtime: extensions:` ファイルの ](properties.md#runtime) プロパティから JSON を削除し `.magento.app.yaml` 再デプロイします。 JSON 拡張機能は、PHP 8.0 以降、クラウド環境にインストールされています。

## PHP の設定

Adobe Commerceが管理する設定に追加された `php.ini` ファイルを使用して、PHP の設定をカスタマイズできます。

リポジトリで、`php.ini` ファイルをアプリケーションのルート（リポジトリルート）に追加します。

>[!TIP]
>
>PHP の設定を適切に行わないと問題が発生する可能性があるため、これらのオプションを設定するのは上級管理者のみにしてください。

### PHP のメモリ制限を増やす

PHP のメモリ上限を増やすには、`php.ini` ファイルに次の設定を追加します。

```ini
memory_limit = 1G
```

デバッグするには、値を 2G に増やします。

### realpath_cache 構成の最適化

アプリケーションのパフォーマンスを向上させるには、次の `realpath_cache` 設定を行います。

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

これらの設定により、PHP プロセスは、ページが読み込まれるたびにパスを検索する代わりにファイルへのパスをキャッシュすることができます。 PHP ドキュメントの [ パフォーマンスチューニング ](https://www.php.net/manual/en/ini.core.php) を参照してください。

>[!NOTE]
>
>推奨される PHP 設定の一覧については、[ インストールガイド ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) の _必要な PHP 設定_ を参照してください。

### カスタム PHP 設定の確認

`php.ini` の変更をクラウド環境にプッシュしたら、カスタム PHP 設定が環境に追加されたことを確認できます。 例えば、SSH を使用してリモート環境にログインし、PHP の設定情報を表示し、`register_argc_argv` ディレクティブをフィルタリングします。

```bash
php -i | grep register_argc_ar
```

サンプル出力：

```text
register_argc_argv => On => On
```

>[!WARNING]
>
>Cloud Docker for Commerceをローカル開発に使用する場合、Docker 環境でカスタム [ ファイルを使用する方法については、](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container)Docker サービスコンテナ `php.ini` を参照してください。

## 拡張機能の有効化

`runtime:extension` のセクションでは、PHP 拡張機能を有効または無効にすることができます。 また、指定された拡張機能は、Docker PHP コンテナで使用できるようになります。

>[!IMPORTANT]
>
>拡張機能を有効にする前に、PHP バージョンがプロジェクトをホストするオペレーティングシステムと互換性を持つ必要があることを理解しておくことが重要です。 プロジェクト環境では、続行する前に、インフラストラクチャチームによる OS のアップグレードが必要になる場合があります。

ファイル `.magento.app.yaml` 例：

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

SSH を使用して環境にログインし、PHP の拡張機能を一覧表示します。

```bash
php -m
```

特定の PHP 拡張モジュールについて詳しくは、[PHP 拡張モジュールの一覧 ](https://www.php.net/manual/en/extensions.alphabetical.php) を参照してください。

次の表に、Cloud Platform にAdobe Commerceをデプロイする際にサポートされる PHP 拡張機能を示します。

{{$include /help/_includes/templated/php-extensions-cloud.md}}

PHP のモジュール要件は、Adobe Commerceのバージョンに関連付けられています。 [PHP の要件 ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) を参照してください。

### 拡張機能のサポート

Pro プロジェクトの場合、次の拡張機能のインストールには追加のサポートが必要です。

- `ioncube`
- `sourceguardian`

例えば、すべての環境で SourceGuardian 保護スクリプトのみを実行するように PHP を設定するには、`php.ini` ファイルで以下のオプションを設定する必要があります。

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

[SourceGuardian ドキュメントの 3.5 節 ](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf) を参照してください。 _PDFへのリンクです_。

[Adobe Commerce サポートチケットを送信 ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) すると、これらの PHP 拡張機能をすべての実稼動環境および Pro ステージング環境にインストールする際のヘルプが表示されます。 更新した `.magento/services.yaml` ファイル、更新した PHP バージョン `.magento.app.yaml` 含むファイル、および追加の PHP 拡張子を含めます。 実稼動環境に変更を加える場合は、少なくとも 48 時間は通知する必要があります。 クラウドインフラストラクチャチームがプロジェクトを更新するまで、最大 48 時間かかる場合があります。

>[!WARNING]
>
>debug を指定してコンパイルされた PHP はサポートされておらず、Probe は [!DNL XDebug] または [!DNL XHProf] と競合する可能性があります。 プローブを有効にする場合は、これらの拡張機能を無効にします。 Probe は [!DNL Pinba] や IonCube のような PHP 拡張モジュールと競合します。

<!-- Last updated from includes: 2025-04-14 09:39:27 -->
