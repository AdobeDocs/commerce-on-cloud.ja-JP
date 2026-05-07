---
title: PHP設定
description: クラウドインフラストラクチャでのCommerce アプリケーション設定に最適なPHP設定について説明します。
feature: Cloud, Configuration, Extensions
exl-id: 83094c16-7407-41fa-ba1c-46b206aa160d
source-git-commit: eff03e0955ae067eb509c7d49eb59f64b3bb1c6a
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 0%

---

# PHP設定

`.magento.app.yaml` ファイルで実行する[ バージョンのPHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)を選択できます。

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>PHP 8.1以降にアップグレードする場合は、`.magento.app.yaml` ファイルの[`runtime: extensions:` プロパティ ](properties.md#runtime)からJSONを削除して再デプロイします。 JSON拡張機能は、PHP 8.0以降のクラウド環境にインストールされます。

## PHPの設定

Adobe Commerceが管理する設定に追加する`php.ini` ファイルを使用して、環境のPHP設定をカスタマイズできます。

リポジトリで、`php.ini` ファイルをアプリケーションのルート（リポジトリルート）に追加します。

>[!TIP]
>
>PHP設定を適切に設定しないと問題が発生する可能性があるため、これらのオプションを設定するのは上級の管理者のみです。

### PHPのメモリ制限を増やす

PHPのメモリ制限を増やすには、次の設定を`php.ini` ファイルに追加します。

```ini
memory_limit = 1G
```

デバッグの場合は、値を2Gに増やします。

### realpath_cache設定の最適化

アプリケーションのパフォーマンスを向上させるために、次の`realpath_cache`設定を設定します。

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

これらの設定を使用すると、PHP プロセスはページ読み込みごとにパスを検索するのではなく、ファイルにパスをキャッシュできます。 PHP ドキュメントの[ パフォーマンスチューニング ](https://www.php.net/manual/en/ini.core.php)を参照してください。

>[!NOTE]
>
>推奨されるPHP設定のリストについては、_インストールガイド_&#x200B;の[必須PHP設定](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html)を参照してください。

### カスタム PHP設定の確認

`php.ini`の変更をCloud環境にプッシュした後、カスタム PHP設定が環境に追加されたことを確認できます。 例えば、SSHを使用してリモート環境にログインし、PHP設定情報を表示し、`register_argc_argv` ディレクティブのフィルターを実行します。

```bash
php -i | grep register_argc_ar
```

出力サンプル：

```text
register_argc_argv => On => On
```

>[!WARNING]
>
>ローカル開発にCloud Docker for Commerceを使用する場合は、Docker環境でカスタム `php.ini` ファイルを使用する方法について、[Docker サービスコンテナ ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#fpm-container)を参照してください。

## 拡張機能を有効にする

`runtime:extension` セクションでPHP拡張機能を有効または無効にできます。 また、指定された拡張機能は、Docker PHP コンテナで使用できるようになります。

>[!IMPORTANT]
>
>拡張機能を有効にする前に、PHP バージョンがプロジェクトをホストするオペレーティングシステムと互換性がある必要があることを理解することが重要です。 プロジェクト環境では、続行する前にインフラストラクチャチームによるOSのアップグレードが必要になる場合があります。

`.magento.app.yaml` ファイルの例：

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

SSHを使用して環境にログインし、PHP拡張機能を一覧表示します。

```bash
php -m
```

特定のPHP拡張機能について詳しくは、[PHP拡張機能リスト ](https://www.php.net/manual/en/extensions.alphabetical.php)を参照してください。

次の表は、Adobe CommerceをCloud Platformにデプロイする際にサポートされるPHP拡張機能を示しています。

{{$include /help/_includes/templated/php-extensions-cloud.md}}

PHP モジュールの要件は、Adobe Commerce バージョンに関連付けられています。 [PHPの要件](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html)を参照してください。

### 拡張機能サポート

Pro プロジェクトの場合、次の拡張機能をインストールするには追加のサポートが必要です。

- `ioncube`
- `sourceguardian`

例えば、すべての環境でSourceGuardianで保護されたスクリプトのみを実行するようにPHPを設定するには、`php.ini` ファイルで次のオプションを設定する必要があります。

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

SourceGuardian ドキュメント ](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf)の[ セクション 3.5を参照してください。 _これはPDF_&#x200B;へのリンクです。

すべての実稼動環境およびPro ステージング環境でこれらのPHP拡張機能をインストールする方法については、[Adobe Commerce サポートチケット ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)を送信してください。 更新した`.magento/services.yaml` ファイル、更新したPHP バージョンの`.magento.app.yaml` ファイル、および追加のPHP拡張機能を含めます。 実稼働環境への変更の場合は、48時間以上の通知が必要です。 クラウドインフラチームがプロジェクトを更新するのに最大48時間かかります。

>[!WARNING]
>
>デバッグでコンパイルされたPHPはサポートされておらず、プローブが[!DNL XDebug]または[!DNL XHProf]と競合する可能性があります。 プローブを有効にする場合は、これらの拡張機能を無効にします。 このプローブは、[!DNL Pinba]やIonCubeなどの一部のPHP拡張機能と競合しています。

<!-- Last updated from includes: 2026-04-24 14:50:02 -->
