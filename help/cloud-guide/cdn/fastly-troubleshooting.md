---
title: Fastlyのトラブルシューティング
description: Adobe CommerceのFastly CDN モジュールとサービスのトラブルシューティングと管理方法について説明します。
feature: Cloud, Configuration, Cache, Services
exl-id: 69954ef9-9ece-411e-934e-814a56542290
TQID: https://experienceleague.adobe.com/2TJ-5byRz5seZ1tpd4FXjZ6JfeaqtKs6ZQlv81Lkr7c
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1911
ht-degree: 0%

---

# Fastlyのトラブルシューティング

以下の情報を使用して、Adobe Commerce on cloud インフラストラクチャプロジェクト環境のMagento 2用Fastly CDN モジュールのトラブルシューティングと管理を行います。 例えば、応答ヘッダーの値とキャッシュ動作を調査して、Fastlyのサービスとパフォーマンスの問題を解決できます。

Pro実稼動環境およびステージング環境では、[New Relic ログ &#x200B;](../monitor/log-management.md)を使用して、Fastly CDNおよびWAF ログデータを表示および分析し、エラーとパフォーマンスの問題をトラブルシューティングできます。

>[!NOTE]
>
>Fastlyの設定と設定について詳しくは、[Fastlyの設定](fastly.md)を参照してください。

## Fastly サービス IDを探す

管理者からFastlyを設定するか、高度なFastly設定とトラブルシューティングのためにFastly API リクエストを送信するには、Fastly サービス IDが必要です。

プロジェクト環境でFastlyが有効になっている場合は、管理者からサービス IDを取得できます。 [Fastly資格情報の取得](fastly-configuration.md#get-fastly-credentials)を参照してください。

開発者と高度なVCL ユーザーは、カスタム VCLを使用して、Fastly変数`req.service_id`を使用してサービス IDを取得できます。 例えば、VCLのカスタムロギングディレクティブに`req.service_id`を追加して、サービス IDの値を取得できます。

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

実稼動環境とステージング環境では、同じVCLを使用できます。 _Fastly ドキュメント_&#x200B;の[`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/)を参照してください。

## サイトのパフォーマンス、パージ、キャッシュの問題

次のリストを使用して、Adobe Commerce on cloud infrastructure環境のFastly サービス設定に関連する問題を特定し、トラブルシューティングします。

- **ストアメニューが表示されないか、動作しません** – ライブサイト URLを使用する代わりに、オリジンサーバーへのリンクまたは一時リンクを直接使用しているか、[cURL コマンド &#x200B;](#check-live-site-through-fastly)で`-H "host:URL"`を使用している可能性があります。 Fastlyをオリジンサーバーにバイパスすると、メインメニューが機能せず、ブラウザー側でキャッシュできる誤ったヘッダーが表示されます。

- **上部ナビゲーションが機能しない** – 上部ナビゲーションは、デフォルトのEdge Fastly VCL スニペットをアップロードするときに有効になるMagento Side Includes （ESI）処理に依存しています。 ナビゲーションが機能しない場合は、[Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly)をアップロードし、サイトを再確認します。

- **Geo-location/GeoIPが機能しません**— デフォルトのMagento Fastly VCL スニペットは、国コードをURLに追加します。 国コードが機能しない場合は、[Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly)をアップロードし、サイトを再確認します。

- **ページがキャッシュされていません** – デフォルトでは、Fastlyは`Set-Cookies` ヘッダーを持つページをキャッシュしません。 Adobe Commerceは、キャッシュ可能なページでもCookieを設定します（TTL > 0）。 デフォルトのMagento Fastly VCLは、キャッシュ可能なページでこれらのCookieを削除します。 ページがキャッシュされていない場合は、[Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly)をアップロードし、サイトを再確認します。

  この問題は、テンプレート内のページブロックがキャッシュ不可とマークされている場合にも発生する可能性があります。 この場合、問題はサードパーティ製のモジュールまたは拡張機能がAdobe Commerce ヘッダーをブロックまたは削除したことが原因である可能性が高くなります。 この問題を解決するには、「[X-Cache contains only MISS, no HIT](#x-cache-contains-only-miss-no-hit)」を参照してください。

- **パージ要求が失敗しています** – パージ要求を送信すると、Fastlyは次のエラーを返します。

  ```text
  The purge request was not processed successfully.
  ```

  この問題は、次のいずれかの問題が原因で発生する可能性があります。

   - Adobe Commerce on cloud infrastructure プロジェクト環境のFastly サービス設定のFastly資格情報が無効です
   - カスタム VCL スニペットの無効なコード

  この問題を解決するには、Adobe Commerce ヘルプセンターの「[&#x200B; クラウド上のFastly キャッシュのパージ中にエラーが発生しました](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-)」を参照してください。

## Fastlyの503 エラー

Fastlyが503個のタイムアウトエラーを返す場合は、エラーログと503個のエラーページを確認して、根本原因を特定します。

>[!NOTE]
>
>一括操作の実行中にタイムアウトが発生した場合は、管理者[&#128279;](fastly-custom-cache-configuration.md#extend-fastly-timeout)のFastly タイムアウトを拡張できます。

503 エラーが発生した場合は、実稼動環境またはステージング環境のエラーログとphp アクセスログを確認して、問題のトラブルシューティングを行います。

**エラーログを確認するには**:

- [エラーログ](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  このログには、アプリケーションまたはPHP エンジンからのエラー（例：`memory_limit`または`max_execution_time exceeded` エラー）が含まれます。 Fastlyに関連するエラーが見つからない場合は、PHP アクセスログを確認してください。

- PHP アクセスログ

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  503 エラーを返したURLのHTTP 200応答のログを検索します。 200件の回答が見つかった場合は、Adobe Commerceがエラーなくページを返したことを意味します。 これは、Fastly サービス設定で設定された`first_byte_timeout`値を超える間隔の後に問題が発生した可能性があることを示しています。

503 エラーが発生すると、Fastlyはエラーとメンテナンスページで理由を返します。 [&#x200B; カスタム応答ページ &#x200B;](fastly-custom-response.md)のコードを追加すると、理由が表示されない場合があります。 デフォルトのエラーページで理由コードを表示するには、カスタムエラーページのHTML コードを削除します。

**Fastly 503 エラーページを確認するには**:

{{admin-login-step}}

1. **Stores** > **Settings** > **Configuration** > **Advanced** > **System**&#x200B;をクリックします。

1. 右側のペインで、**フルページキャッシュ**&#x200B;を展開します。

1. **Fastly Configuration** セクションで、**カスタム合成ページ**&#x200B;を展開します。次の図を参照してください。

   ![&#x200B; カスタム 503 エラーページ &#x200B;](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. 「**HTMLを設定**」をクリックします。

1. カスタムコードを削除します。 後で追加できるように、テキストプログラムに保存することができます。

1. 「**アップロード**」をクリックして、更新をFastlyに送信します。

1. ページの上部にある「**設定を保存**」をクリックします。

1. 503 エラーの原因となったURLを再度開きます。 次の例に示すように、Fastlyは理由を含むエラーページを返します。

   ![Fastly エラー](../../assets/cdn/fastly-503-example.png)

## Fastly アカウントに既に関連付けられているApexとサブドメイン

Adobe Commerce on cloud infrastructure プロジェクトのapex ドメインとサブドメインが、割り当てられたサービス IDを持つ既存のFastly アカウントに既に関連付けられている場合、Fastly設定を更新するまで起動できません。

- 既存のFastly アカウントのapexおよびサブドメイン設定を更新します。 [複数のFastly アカウントと割り当てられたドメイン &#x200B;](fastly.md#multiple-fastly-accounts-and-assigned-domains)を参照してください。

- [Fastly](fastly-configuration.md#enable-fastly-caching)を有効にして設定し、[DNS設定](../launch/checklist.md#update-dns-configuration-with-production-settings)を完了します

## Fastly サービスの検証またはデバッグ

サイト URLをテストし、応答で返されるヘッダー値を調べることで、Adobe Commerce on cloud infrastructure サイトのパフォーマンスまたはキャッシュの問題をトラブルシューティングできます。

### Fastlyでライブサイトをチェックする

Fastly APIを使用して、ライブサイトから返された`Fastly-Magento-VCL-Uploaded`および`X-Cache`応答ヘッダーを確認します。

Fastly API リクエストは、Fastly拡張機能を通じて渡され、オリジンサーバーから応答を取得します。 応答が誤ったヘッダーを返す場合は、[&#x200B; オリジンサーバーを直接テストします](#bypass-fastly-cache-to-check-adobe-commerce-sites)。

**応答ヘッダーを確認するには**:

1. ターミナルで、次の`curl` コマンドを使用して、ライブサイト URLをテストします。

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   静的ルートを設定していないか、ライブサイト上のドメインのDNS設定を完了していない場合は、DNS名前解決を回避する`--resolve` フラグを使用します。

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >`--resolve` オプションでこのコマンドを使用するには、SSL/TLS証明書を介してFastlyでTLSを有効にし、キャッシュノードのIP アドレスを見つける必要があります。

1. 応答で、[&#x200B; ヘッダー](#check-cache-hit-and-miss-response-headers)を確認して、Fastlyが動作していることを確認します。 応答には、次の一意のヘッダーが表示されます。

   ```http
   < Fastly-Magento-VCL-Uploaded: 1.2.222
   < X-Cache: HIT, MISS
   ```

ヘッダーに正しい値がない場合は、次の情報を参照してください。

- [VCL アップロードの確認](#fastly-vcl-has-not-been-uploaded)

- [X-CacheにはMISSのみが含まれ、HITは含まれません](#x-cache-contains-only-miss-no-hit)

### Fastly キャッシュをバイパスしてAdobe Commerce サイトを確認する

Fastly サービスが誤ったヘッダーを返す場合は、Fastly キャッシュをバイパスするリクエストを送信できるVCL スニペットを作成できます。 [Fastly キャッシュのバイパス &#x200B;](fastly-vcl-bypass-to-origin.md)を参照してください。

VCL スニペットを追加したら、cURL コマンドを使用して、指定したIP アドレスからオリジンサーバーにリクエストを送信します。 次に、応答にエラーがないか確認します。

### キャッシュのHITおよびMISS レスポンスヘッダーを確認する

返された応答に次の情報が含まれていることを確認します。

- `X-Magento-Tags` ヘッダーを含む

- `Fastly-Module-Enabled` ヘッダーの値は、`Yes`またはプロジェクト環境にインストールされているFastly for CDN Magento 2 モジュールのバージョン番号のいずれかです

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)が0より大きい

- [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32)設定は`cache`です

cURL コマンド出力の次の抜粋は、`Pragma`、`X-Magento-Tags`、および`Fastly-Module-Enabled` ヘッダーの正しい値を示しています。

```
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>ヒットとミスについて詳しくは、Fastly ドキュメントの「[&#x200B; シールドサービスを使用したキャッシュヒットとミスのヘッダーについて](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services)」を参照してください。

### 応答ヘッダーで見つかったエラーを解決する

この節では、Fastly APIを使用して応答ヘッダーを確認する際に返されるエラーを解決するための推奨事項を示します。

#### Fastly モジュールが有効になっていません

Fastly モジュールが有効になっていない場合（`Fastly-Module-Enabled: no`）またはヘッダーが見つからない場合は、[SSHを使用して](../development/secure-connections.md#connect-to-a-remote-environment)にプロジェクトにログインします。 次に、次のコマンドを実行して、モジュールのステータスを確認します。

```bash
php bin/magento module:status Fastly_Cdn
```

返されたステータスに基づいて、次の手順に従ってFastly設定を更新します。

- `Module does not exist` - モジュールが存在しない場合は、[統合ブランチでFastly CDN Module for Magento 2をインストールして](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md)設定します。 インストールが完了したら、モジュールを有効にして設定します。 [Fastlyの設定](fastly-configuration.md)を参照してください。

- `Module is disabled` - Fastly モジュールが無効になっている場合は、ローカル環境の`integration` ブランチで環境設定を更新して有効にします。 次に、ステージングと実稼動に変更をプッシュします。 [拡張機能の管理](../store/extensions.md#install-an-extension)を参照してください。

  [構成管理](../store/store-settings.md#configure-store)を使用する場合は、実稼動環境またはステージング環境に変更をプッシュする前に、`app/etc/config.php`設定ファイルでFastly CDN モジュールのステータスを確認してください。

  モジュールが`config.php` ファイルで有効になっていない（`Fastly_CDN => 0`）場合は、ファイルを削除し、次のコマンドを実行して、最新の構成設定で`config.php`を更新します。

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCLがアップロードされていない

Fastly VCLがアップロードされていない場合（`Fastly-Magento-VCL-Uploaded`: `false`）、管理者の「*VCL*&#x200B;をアップロード」オプションを使用してアップロードします。 [Fastly VCL スニペットのアップロード &#x200B;](fastly-configuration.md#upload-vcl-to-fastly)を参照してください。

#### X-CacheにはMISSのみが含まれ、HITは含まれません

`X-Cache` ヘッダーに`HIT` （`HIT, HIT`または`HIT, MISS`）が含まれる場合は、Fastlyがキャッシュされたコンテンツを正常に返すことを示します。

`X-Cache` ヘッダーが`MISS, MISS`で、`HIT`が含まれていない場合は、`curl` コマンドを再度実行して、ページが最近キャッシュからパージされていないことを確認します。

同じ結果が得られた場合は、[`curl` コマンド &#x200B;](#check-live-site-through-fastly)を使用し、[応答ヘッダー](#check-cache-hit-and-miss-response-headers)を確認します。

- `Pragma`は`cache`です
- `X-Magento-Tags`が存在します
- `Cache-Control: max-age`が0より大きい

問題が解決しない場合は、別の拡張機能がこれらのヘッダーをリセットしている可能性があります。 ステージング環境で次の手順を繰り返します。すべての拡張機能を無効にし、それぞれの拡張機能を再度有効にして、どの拡張機能がヘッダーをリセットしているかを判断します。 問題の原因となる拡張機能を特定したら、実稼動環境で無効にする必要があります。

**応答ヘッダーをリセットする拡張機能を特定するには：**

{{admin-login-step}}

1. **Stores** > **Settings** > **Configuration** > **Advanced** > **Advanced**&#x200B;に移動します。

1. 右側のパネルの「*モジュール出力を無効にする*」セクションで、すべての拡張機能を見つけて無効にします。

1. 「**設定を保存**」をクリックします。

1. **システム**/**ツール**/**キャッシュ管理**&#x200B;をクリックします。

1. 「**Magento キャッシュをフラッシュ**」をクリックします。

1. Fastly ヘッダーに問題が発生する可能性がある各拡張機能について、次の手順を実行します。

   - 一度に1つの拡張機能を有効にし、設定を保存し、Adobe Commerce キャッシュをフラッシュします。

   - [`curl` コマンド &#x200B;](#check-live-site-through-fastly)を実行して、[応答ヘッダー](#check-cache-hit-and-miss-response-headers)を確認します。

   このプロセスを各拡張機能に対して繰り返します。 Fastly応答ヘッダーが表示されなくなった場合は、Fastlyで問題を引き起こす拡張機能を特定しました。

Fastly ヘッダーをリセットしている拡張機能を特定したら、拡張機能の開発者に連絡して詳細を確認してください。 サードパーティの拡張機能がFastly キャッシュで動作するように修正またはアップデートを提供することはできません。

## Fastly設定のロールバック

カスタム VCL スニペットの更新またはその他のFastly設定の変更により、Adobe Commerce on cloud infrastructure サイトでエラーが発生または返される場合は、Fastly API [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) コマンドを使用して以前のVCL バージョンにロールバックします。 管理者からVCL バージョンをロールバックすることはできません。

**VCL バージョンをロールバックするには**:

1. サービスで使用可能なVCL バージョンのリストを取得するには、次のコマンドを実行します

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. 次のコマンドを実行して、アクティブなVCL バージョンを指定したバージョンに変更します。

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Fastly APIを使用してVCLをレビューおよび管理する方法について詳しくは、[APIを使用したVCLの管理](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)を参照してください。
