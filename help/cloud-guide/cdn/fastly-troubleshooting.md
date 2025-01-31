---
title: Fastly のトラブルシューティング
description: Adobe Commerce用の Fastly CDN モジュールおよびサービスのトラブルシューティングと管理の方法について説明します。
feature: Cloud, Configuration, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Fastly のトラブルシューティング

以下の情報を使用して、クラウドインフラストラクチャプロジェクト環境のAdobe CommerceのMagento 2 用 Fastly CDN モジュールのトラブルシューティングと管理を行います。 例えば、応答ヘッダー値とキャッシュ動作を調査して、Fastly のサービスとパフォーマンスの問題を解決できます。

プロの実稼動環境およびステージング環境では、[New Relic ログ ](../monitor/log-management.md) を使用して、Fastly CDN およびWAF ログデータを表示および分析し、エラーとパフォーマンスの問題のトラブルシューティングを行うことができます。

>[!NOTE]
>
>Fastly のセットアップと設定について詳しくは、[Fastly のセットアップ ](fastly.md) を参照してください。

## Fastly サービス ID を見つけます

管理者から Fastly を設定したり、高度な Fastly 設定とトラブルシューティングのために Fastly API リクエストを送信したりするには、Fastly サービス ID が必要です。

プロジェクト環境で Fastly が有効になっている場合は、管理者からサービス ID を取得できます。 [Fastly 資格情報の取得 ](fastly-configuration.md#get-fastly-credentials) を参照してください。

開発者と高度な VCL ユーザーは、カスタム VCL を使用して、Fastly 変数 `req.service_id` を使用してサービス ID を取得できます。 例えば、VCL のカスタムログディレクティブに `req.service_id` を追加して、サービス ID 値を取得できます。

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

実稼動環境とステージング環境に同じ VCL を使用できます。 _Fastly ドキュメント_ の [`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/) を参照してください。

## サイトのパフォーマンス、パージ、キャッシュの問題

以下のリストを使用して、クラウドインフラストラクチャー上のAdobe Commerceの Fastly サービス設定に関する問題を特定し、トラブルシューティングを行います。

- **ストアメニューが表示されない、または機能しない** - ライブサイト URL を使用する代わりに、元のサーバーへのリンクまたは一時リンクを直接使用している可能性 `-H "host:URL"` あります。または、[cURL コマンドで使用した可能性もあります ](#check-live-site-through-fastly)。 Fastly をオリジンサーバーにバイパスすると、メインメニューが機能せず、ブラウザー側でのキャッシュを許可する誤ったヘッダーが表示されます。

- **トップナビゲーションが機能しない** - トップナビゲーションは、Edge サイドインクルード （ESI）処理に依存しており、デフォルトのMagento Fastly VCL スニペットをアップロードする際に有効になります。 ナビゲーションが機能しない場合は [Fastly VCL をアップロード ](fastly-configuration.md#upload-vcl-to-fastly) し、サイトを再確認します。

- **Geo-location/GeoIP が機能しない** - デフォルトMagentoの Fastly VCL スニペットは、国コードを URL に付加します。 国コードが機能しない場合は、[Fastly VCL をアップロード ](fastly-configuration.md#upload-vcl-to-fastly) してサイトを再確認します。

- **ページはキャッシュされません** - デフォルトでは、Fastly は `Set-Cookies` ヘッダーを持つページをキャッシュしません。 Adobe Commerceは、キャッシュ可能なページ（TTL > 0）でも cookie を設定します。 デフォルトのMagentoである Fastly VCL では、キャッシュ可能なページにこれらの Cookie が削除されます。 ページがキャッシュされない場合は、[Fastly VCL をアップロード ](fastly-configuration.md#upload-vcl-to-fastly) して、サイトを再確認します。

  この問題は、テンプレートのページブロックがキャッシュ不可とマークされている場合にも発生する可能性があります。 その場合、問題はサードパーティのモジュールまたは拡張機能がAdobe Commerce ヘッダーをブロックまたは削除したことが原因で発生する可能性が高くなります。 この問題を解決するには、[X-Cache contains only MISS, no HIT](#x-cache-contains-only-miss-no-hit) を参照してください。

- **パージリクエストが失敗します** - パージリクエストを送信すると、Fastly で次のエラーが返されます。

  ```text
  The purge request was not processed successfully.
  ```

  この問題は、次のいずれかの問題が原因で発生する可能性があります。

   - クラウドインフラストラクチャプロジェクト環境のAdobe Commerce用 Fastly サービス設定の Fastly 資格情報が無効です
   - カスタム VCL スニペットのコードが無効です

  この問題を解決するには、Adobe Commerce ヘルプセンターの [Fastly Cache on Cloud のパージ時のエラー ](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) を参照してください。

## Fastly の 503 エラー

Fastly が 503 タイムアウトエラーを返す場合は、エラーログと 503 エラーページを確認して、根本原因を特定します。

>[!NOTE]
>
>一括操作の実行時にタイムアウトが発生した場合は、[Admin の Fastly タイムアウトを拡張する ](fastly-custom-cache-configuration.md#extend-fastly-timeout) ことができます。

503 エラーが発生した場合は、実稼動環境またはステージング環境のエラーログと PHP アクセスログを確認して、問題のトラブルシューティングを行ってください。

**エラーログを確認するには**:

- [エラーログ](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  このログには、アプリケーションまたは PHP エンジンからのエラー（`memory_limit` エラーや `max_execution_time exceeded` エラーなど）が含まれます。 Fastly 関連のエラーが見つからない場合は、PHP アクセスログを確認します。

- PHP アクセスログ

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  503 エラーを返した URL のログで HTTP 200 応答を検索します。 200 の応答が見つかった場合は、Adobe Commerceがエラーなくページを返したことを意味します。 これは、間隔が Fastly サービス設定で設定された `first_byte_timeout` 値を超えた後に、問題が発生した可能性があることを示します。

503 エラーが発生した場合、Fastly はエラーとメンテナンスページで理由を返します。 [ カスタム応答ページ ](fastly-custom-response.md) のコードを追加した場合、理由を確認できないことがあります。 デフォルトのエラーページで理由コードを確認するには、カスタムエラーページのHTMLコードを削除します。

**Fastly 503 エラーページを確認するには**:

{{admin-login-step}}

1. **ストア**/**設定**/**設定**/**詳細**/**システム** をクリックします。

1. 右側のペインで、「**フルページキャッシュ**」を展開します。

1. 「**Fastly 設定**」セクションで、次の図に示すように、「**カスタム合成ページ**」を展開します。

   ![ カスタム 503 エラーページ ](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. **HTMLを設定** をクリックします。

1. カスタムコードを削除します。 後で追加し直すために、テキストプログラムで保存することができます。

1. **アップロード** をクリックして、Fastly にアップデートを送信します。

1. ページ上部にある「**設定を保存**」をクリックします。

1. 503 エラーの原因となった URL を再度開きます。 Fastly は、次の例に示す理由を含むエラーページを返します。

   ![Fastly エラー ](../../assets/cdn/fastly-503-example.png)

## Apex とサブドメインは既に Fastly アカウントに関連付けられています

クラウドインフラストラクチャプロジェクトのAdobe Commerceの apex ドメインとサブドメインが、割り当てられたサービス ID を持つ既存の Fastly アカウントに既に関連付けられている場合、Fastly 設定を更新するまで起動できません。

- 既存の Fastly アカウントの apex とサブドメインの設定を更新します。 [ 複数の Fastly アカウントと割り当てられたドメイン ](fastly.md#multiple-fastly-accounts-and-assigned-domains) を参照してください。

- [Fastly を有効にして設定 ](fastly-configuration.md#enable-fastly-caching) し、[DNS 設定を完了します ](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Fastly サービスの検証またはデバッグ

サイト URL をテストし、応答で返されるヘッダー値を調べることで、クラウドインフラストラクチャサイト上のAdobe Commerceのパフォーマンスやキャッシュの問題をトラブルシューティングできます。

### Fastly でのライブサイトの確認

Fastly API を使用して、ライブサイトから返された `Fastly-Magento-VCL-Uploaded` および `X-Cache` 応答ヘッダーを確認します。

Fastly API リクエストは、Fastly 拡張機能を通じて渡され、オリジンサーバーから応答を取得します。 応答から誤ったヘッダーが返された場合は、[ オリジンサーバーを直接 ](#bypass-fastly-cache-to-check-adobe-commerce-sites) テストします。

**応答ヘッダーを確認するには**:

1. ターミナルでは、次の `curl` コマンドを使用してライブサイト URL をテストします。

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   静的ルートを設定していない場合や、ライブサイト上のドメインの DNS 設定が完了した場合は、`--resolve` フラグを使用して DNS の名前解決をバイパスします。

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >`--resolve` オプションでこのコマンドを使用するには、SSL/TLS 証明書を介して Fastly で TLS を有効にし、キャッシュノードの IP アドレスを見つける必要があります。

1. 応答で [headers](#check-cache-hit-and-miss-response-headers) を検証し、Fastly が機能していることを確認します。 応答に次の一意のヘッダーが表示されます。

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

ヘッダーに正しい値がない場合は、次の情報を参照してください。

- [VCL アップロードのチェック](#fastly-vcl-has-not-been-uploaded)

- [X-Cache には MISS のみが含まれ、ヒットは含まれない](#x-cache-contains-only-miss-no-hit)

### Fastly キャッシュをバイパスしてAdobe Commerce サイトをチェック

Fastly サービスが誤ったヘッダーを返す場合、Fastly キャッシュをバイパスするリクエストを送信できる VCL スニペットを作成できます。 [Fastly キャッシュのバイパス ](fastly-vcl-bypass-to-origin.md) を参照してください。

VCL スニペットを追加した後、cURL コマンドを使用して、指定した IP アドレスから発信元サーバーに要求を送信します。 次に、応答にエラーがないか確認します。

### キャッシュヒットおよびミス応答ヘッダーを確認します

返された応答に次の情報が含まれていることを確認します。

- `X-Magento-Tags` ヘッダーを含む

- `Fastly-Module-Enabled` ヘッダーの値は、`Yes` またはプロジェクト環境にインストールされている Fastly for CDN Magento 2 モジュールのバージョン番号です

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) が 0 より大きい

- [ プラグマ ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) 設定は `cache`

cURL コマンド出力から抜粋した以下のコードは、`Pragma`、`X-Magento-Tags`、`Fastly-Module-Enabled` の各ヘッダーの正しい値を示しています。

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
>ヒットとミスについて詳しくは、Fastly ドキュメントの [ シールドサービスにおけるキャッシュの HIT ヘッダーと MISS ヘッダーについて ](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) を参照してください。

### 応答ヘッダーで見つかったエラーの解決

この節では、Fastly API を使用して応答ヘッダーを確認する際に返されるエラーを解決するための推奨事項を説明します。

#### Fastly モジュールが有効になっていません

Fastly モジュールが有効になっていない（`Fastly-Module-Enabled: no`）場合や、ヘッダーがない場合は、[SSH を使用してログイン ](../development/secure-connections.md#connect-to-a-remote-environment) プロジェクトにログインします。 次に、次のコマンドを実行して、モジュールのステータスを確認します。

```bash
php bin/magento module:status Fastly_Cdn
```

返されたステータスに基づいて、次の手順を使用して Fastly 設定を更新します。

- `Module does not exist` - モジュールが存在しない場合 [ インストールと設定 ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md)、統合ブランチのMagento 2 用 Fastly CDN モジュール。 インストールが完了したら、モジュールを有効にして設定します。 [Fastly の設定 ](fastly-configuration.md) を参照してください。

- `Module is disabled` - Fastly モジュールが無効な場合は、ローカル環境の `integration` ブランチで環境設定を更新して有効にします。 次に、変更をステージング環境および実稼動環境にプッシュします。 詳しくは、[ 拡張機能の管理 ](../store/extensions.md#install-an-extension) を参照してください。

  [ 設定管理 ](../store/store-settings.md#configure-store) を使用している場合は、変更を実稼動環境またはステージング環境にプッシュする前に、`app/etc/config.php` 設定ファイルで Fastly CDN モジュールのステータスを確認します。

  `config.php` ファイルでモジュールが有効（`Fastly_CDN => 0`）になっていない場合は、ファイルを削除し、次のコマンドを実行して `config.php` を最新の設定に更新します。

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCL はアップロードされていません

Fastly VCL がアップロードされていない場合（`Fastly-Magento-VCL-Uploaded`:`false`）、Admin の *Upload VCL* オプションを使用してアップロードします。 [Fastly VCL スニペットのアップロード ](fastly-configuration.md#upload-vcl-to-fastly) を参照してください。

#### X-Cache には MISS のみが含まれ、ヒットは含まれない

`X-Cache` ヘッダーに `HIT` （`HIT, HIT` または `HIT, MISS`）が含まれている場合、Fastly がキャッシュされたコンテンツを正常に返すことを示します。

`X-Cache` ヘッダーが `MISS, MISS` で、`HIT` が含まれていない場合は、`curl` コマンドを再度実行し、ページが最近キャッシュからパージされていないことを確認してください。

同じ結果が得られる場合は、[`curl` のコマンドを使用し ](#check-live-site-through-fastly)[response ヘッダー ](#check-cache-hit-and-miss-response-headers) を確認します。

- `Pragma` is `cache`
- `X-Magento-Tags` が存在する
- `Cache-Control: max-age` が 0 より大きい

問題が解決しない場合は、別の拡張機能がこれらのヘッダーをリセットしている可能性があります。 ステージング環境で次の手順を繰り返し、すべての拡張機能を無効にし、各拡張機能を再度有効にして、ヘッダーをリセットしている拡張機能を特定します。 問題の原因となっている拡張機能を特定したら、実稼動環境でその拡張機能を無効にする必要があります。

**応答ヘッダーをリセットする拡張機能を識別するには：**

{{admin-login-step}}

1. **Stores**/**Settings**/**Configuration**/**Advanced**/**Advanced** に移動します。

1. 右側のパネルの *モジュール出力の無効化* セクションで、すべての拡張機能を見つけて無効にします。

1. 「**設定を保存**」をクリックします。

1. **システム**/**ツール**/**キャッシュ管理** をクリックします。

1. **Magentoキャッシュをフラッシュ** をクリックします。

1. Fastly ヘッダーで問題を引き起こす可能性がある拡張機能ごとに、次の手順を実行します。

   - 一度に 1 つの拡張機能を有効にし、設定を保存して、Adobe Commerce キャッシュをフラッシュします。

   - [`curl` のコマンドを実行して ](#check-live-site-through-fastly)[ 応答ヘッダー ](#check-cache-hit-and-miss-response-headers) を確認します。

   各拡張機能に対して、このプロセスを繰り返します。 Fastly 応答ヘッダーが表示されなくなった場合は、Fastly で問題を引き起こしている拡張機能を特定しました。

Fastly ヘッダーをリセットしている拡張機能を特定したら、拡張機能の開発者に問い合わせてください。 サードパーティの拡張機能を Fastly キャッシュで機能させるための修正や更新を提供することはできません。

## Fastly 設定をロールバックする

カスタム VCL スニペットの更新やその他の Fastly 設定の変更により、クラウドインフラストラクチャサイト上のAdobe Commerceでエラーが発生したりエラーが返されたりする場合は、Fastly API [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) コマンドを使用して、以前の VCL バージョンにロールバックします。 管理者から VCL バージョンをロールバックすることはできません。

**VCL バージョンをロールバックするには**:

1. サービスで使用可能な VCL バージョンのリストを取得するには、次のコマンドを実行します

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. 次のコマンドを実行して、アクティブな VCL バージョンを指定のバージョンに変更します。

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Fastly API を使用した VCL のレビューと管理について詳しくは、[API を使用した VCL の管理 ](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api) を参照してください。
