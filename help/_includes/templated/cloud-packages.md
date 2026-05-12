---
source-git-commit: 30d2462df80a252519fcaa90e225c219d4ead2f1
workflow-type: tm+mt
source-wordcount: '3177'
ht-degree: 0%

---
# Adobe Commerce用クラウドパッケージ

<!--
The 'packages' variable contains the 'packages' node of the '_data/codebase/cloud/composer_lock.json' file
-->

<!--
The 'packages-dev' variable contains the 'packages-dev' node of the '_data/codebase/cloud/composer_lock.json' file
-->

<!--
The 'product' variable contains data of the 'magento/magento-cloud-metapackage' package
-->

<!--
The edition variable contains `cloud` value from the _data/names.yml file
-->

Adobe Commerce クラウドインフラストラクチャでは、Composerを使用してPHP パッケージを管理します。

`composer.json` ファイルはパッケージのリストを宣言しますが、`composer.lock` ファイルには、Adobe Commerceのインストールの構築に使用されたパッケージの完全なリスト（各パッケージとその依存関係の完全なバージョン）が格納されます。

次のリファレンスドキュメントは`composer.lock` ファイルから生成され、クラウドインフラストラクチャ 2.4.9-p2のAdobe Commerceに含まれる必要なパッケージについて説明しています。

## 依存関係

`magento/magento-cloud-metapackage 2.4.9-p2`には次の依存関係があります：

```config
aem/rum: ^1.0.4
fastly/magento2: ^1.2.34
magento/ece-tools: ^2002.2.0
magento/module-paypal-on-boarding: ~100.5.0
magento/product-enterprise-edition: >=2.4.9 <2.4.10
```

## サードパーティライセンス

### Apache-2.0、LGPL-2.1-only

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/opensearch-project/opensearch-php.git">opensearch-project/opensearch-php</a>
    </td>
    <td>Library</td>
    <td>OpenSearch用PHP クライアント</td>
  </tr>
  </tbody>
</table>

### Apache-2.0

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/adobe/stock-api-libphp.git">astock/stock-api-libphp</a>
    </td>
    <td>Library</td>
    <td>Adobe Stock API ライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/awslabs/aws-crt-php.git">aws/aws-crt-php</a>
    </td>
    <td>Library</td>
    <td>PHP用AWS Common Runtime</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/aws/aws-sdk-php.git">aws/aws-sdk-php</a>
    </td>
    <td>Library</td>
    <td>PHP用AWS SDK - PHP プロジェクトでのAmazon Web Servicesの使用</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/giggsey/libphonenumber-for-php.git">giggsey/libphonenumber-for-php</a>
    </td>
    <td>Library</td>
    <td>国際電話番号の解析、フォーマット、保存、検証のためのライブラリ、GoogleのlibphonenumberのPHP ポート。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opentelemetry-php/api.git">open-telemetry/api</a>
    </td>
    <td>Library</td>
    <td>OpenTelemetry PHP用のAPI。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opentelemetry-php/context.git">open-telemetry/context</a>
    </td>
    <td>Library</td>
    <td>OpenTelemetry PHPのコンテキスト実装。</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree
    </td>
    <td>メタパッケージ</td>
    <td>Braintree Magento</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/stomp-php/stomp-php.git">stomp-php/stomp-php</a>
    </td>
    <td>Library</td>
    <td>PHPのstomp サポート</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/wikimedia/less.php.git">wikimedia/less.php</a>
    </td>
    <td>Library</td>
    <td>LESS プロセッサーのPHP ポート</td>
  </tr>
  </tbody>
</table>

### BSD-2-Clause

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/Bacon/BaconQrCode.git">bacon/bacon-qr-code</a>
    </td>
    <td>Library</td>
    <td>BaconQrCodeは、PHP用のQR コードジェネレーターです。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/DASPRiD/Enum.git">dasprid/enum</a>
    </td>
    <td>Library</td>
    <td>PHP 7.1 enumの実装</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webimpress/safe-writer.git">webimpress/safe-writer</a>
    </td>
    <td>Library</td>
    <td>レース条件を避けるために、安全にファイルを書き込むためのツール</td>
  </tr>
  </tbody>
</table>

### BSD-3-Clause

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_File.git">colinmollenhour/cache-backend-file</a>
    </td>
    <td>magento-module</td>
    <td>在庫のZend_Cache_Backend_File バックエンドは、タグによるクリーニングのパフォーマンスが非常に低いため、キャッシュされたアイテム数が増えると使用できなくなります。 このバックエンドでは、多くの変更が行われ、特にタグクリーニングの場合、パフォーマンスが大幅に向上します。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/php-redis-session-abstract.git">colinmollenhour/php-redis-session-abstract</a>
    </td>
    <td>Library</td>
    <td>楽観的ロックを備えたRedis ベースのセッションハンドラー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/duosecurity/duo_universal_php.git">duosecurity/duo_universal_php</a>
    </td>
    <td>Library</td>
    <td>Duo Universal SDKのPHP実装。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/fastly/fastly-magento2.git">fastly/magento2</a>
    </td>
    <td>magento2-module</td>
    <td>Fastly CDN Module for Magento 2.4.x</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/googleapis/php-jwt.git">firebase/php-jwt</a>
    </td>
    <td>Library</td>
    <td>PHPでJSON Web Tokens （JWT）をエンコードおよびデコードするためのシンプルなライブラリ。 現在の仕様に準拠する必要があります。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-code.git">laminas/laminas-code</a>
    </td>
    <td>Library</td>
    <td>PHP Reflection API、静的コードスキャン、コード生成の拡張機能</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-config.git">laminas/laminas-config</a>
    </td>
    <td>Library</td>
    <td>アプリケーション コード内でこの設定データにアクセスするための、ネストされたオブジェクト プロパティ ベースのユーザーインターフェイスを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-escaper.git">laminas/laminas-escaper</a>
    </td>
    <td>Library</td>
    <td>HTML、HTMLの属性、JavaScript、CSS、URLを安全にエスケープできます</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-eventmanager.git">laminas/laminas-eventmanager</a>
    </td>
    <td>Library</td>
    <td>PHP アプリケーション内のイベントのトリガーとリッスン</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-feed.git">laminas/laminas-feed</a>
    </td>
    <td>Library</td>
    <td>は、RSSおよびAtom フィードを作成および使用するための機能を提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-filter.git">laminas/laminas-filter</a>
    </td>
    <td>Library</td>
    <td>データとファイルをプログラムでフィルタリングおよび正規化する</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-http.git">laminas/laminas-http</a>
    </td>
    <td>Library</td>
    <td>HTTP （Hyper-Text Transfer Protocol）リクエストを実行するための簡単なインターフェイスを提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-i18n.git">laminas/laminas-i18n</a>
    </td>
    <td>Library</td>
    <td>アプリケーションの翻訳を提供し、国際化された値をフィルタリングおよび検証します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-json.git">laminas/laminas-json</a>
    </td>
    <td>Library</td>
    <td>ネイティブ PHPをJSONにシリアル化し、JSONをネイティブ PHPにデコードするための便利なメソッドを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-loader.git">laminas/laminas-loader</a>
    </td>
    <td>Library</td>
    <td>自動ロードとプラグインのロード戦略</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-modulemanager.git">laminas/laminas-modulemanager</a>
    </td>
    <td>Library</td>
    <td>laminas-mvc アプリケーション向けモジュラーアプリケーションシステム</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-permissions-acl.git">laminas/laminas-permissions-acl</a>
    </td>
    <td>Library</td>
    <td>権限管理のための軽量で柔軟なアクセス制御リスト（ACL）実装を提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-recaptcha.git">laminas/laminas-recaptcha</a>
    </td>
    <td>Library</td>
    <td>ReCaptcha web サービスのOOP ラッパー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-servicemanager.git">laminas/laminas-servicemanager</a>
    </td>
    <td>Library</td>
    <td>工場駆動依存性注入コンテナ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-session.git">laminas/laminas-session</a>
    </td>
    <td>Library</td>
    <td>PHP セッションとストレージへのオブジェクト指向インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-stdlib.git">laminas/laminas-stdlib</a>
    </td>
    <td>Library</td>
    <td>SPL拡張機能、配列ユーティリティ、エラーハンドラーなど</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-text.git">laminas/laminas-text</a>
    </td>
    <td>Library</td>
    <td>FIGletとテキストベースのテーブルの作成</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-translator.git">laminas/laminas-translator</a>
    </td>
    <td>Library</td>
    <td>laminas-i18nのTranslator コンポーネントのインターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-uri.git">laminas/laminas-uri</a>
    </td>
    <td>Library</td>
    <td>» Uniform Resource Identifiers （URI）の操作と検証を支援するコンポーネント</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-validator.git">laminas/laminas-validator</a>
    </td>
    <td>Library</td>
    <td>幅広いドメインの検証クラスと、複雑な検証条件を作成するチェーンバリデータの機能</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-view.git">laminas/laminas-view</a>
    </td>
    <td>Library</td>
    <td>複数のビューレイヤー、ヘルパーなどをサポートする柔軟なビューレイヤー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/marc-mabe/php-enum.git">marc-mabe/php-enum</a>
    </td>
    <td>Library</td>
    <td>ネイティブ PHPによる列挙のシンプルかつ迅速な実装</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/nikic/PHP-Parser.git">nikic/php-parser</a>
    </td>
    <td>Library</td>
    <td>PHPで書かれたPHP パーサー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-db/phpdb.git">php-db/phpdb</a>
    </td>
    <td>Library</td>
    <td>データベース抽象化層、SQL抽象化、結果セット抽象化、RowDataGatewayおよびTableDataGatewayの実装</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpfui/recaptcha.git">phpfui/recaptcha</a>
    </td>
    <td>Library</td>
    <td>PHP 8.4以降のreCAPTCHA Google用クライアントライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tedious/JShrink.git">tedivm/jshrink</a>
    </td>
    <td>Library</td>
    <td>PHPで構築されたJavascript ミニファイア</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port.git">tubalmartin/cssmin</a>
    </td>
    <td>Library</td>
    <td>YUI CSS コンプレッサーのPHP ポート</td>
  </tr>
  </tbody>
</table>

### BSD-3-Clause-Modification

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_Redis.git">colinmollenhour/cache-backend-redis</a>
    </td>
    <td>magento-module</td>
    <td>タグを完全にサポートするRedisを使用したZend_Cache バックエンド。</td>
  </tr>
  </tbody>
</table>

### LGPL-2.1以降

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/ezyang/htmlpurifier.git">ezyang/htmlpurifier</a>
    </td>
    <td>Library</td>
    <td>PHPで記述された標準準拠のHTML フィルター</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-amqplib/php-amqplib.git">php-amqplib/php-amqplib</a>
    </td>
    <td>Library</td>
    <td>以前はvidelalvaro/php-amqplibでした。  このライブラリは、AMQP プロトコルの純粋なPHP実装です。 RabbitMQに対してテストされています。</td>
  </tr>
  </tbody>
</table>

### MIT

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/braintree/braintree_php.git">braintree/braintree_php</a>
    </td>
    <td>Library</td>
    <td>Braintree PHP クライアントライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/math.git"> レンガ/計算</a>
    </td>
    <td>Library</td>
    <td>任意精度演算ライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/phonenumber.git"> レンガ/音声番号</a>
    </td>
    <td>Library</td>
    <td>電話番号ライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/varexporter.git">brick/varexporter</a>
    </td>
    <td>Library</td>
    <td>var_export （）に代わる強力な方法。__set_state （）なしでクロージャやオブジェクトを書き出すことができます。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon-doctrine-types.git">炭水化物/炭素ドクトリンの種類</a>
    </td>
    <td>Library</td>
    <td>ドクトリンで炭素を使用する種類</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ChristianRiesen/base32.git">christian-riesen/base32</a>
    </td>
    <td>Library</td>
    <td>RFC 4648に準拠したBase32 エンコーダ/デコーダ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/credis.git">colinmollenhour/credis</a>
    </td>
    <td>Library</td>
    <td>Credisは、Redis キー値ストアの軽量インターフェイスで、パフォーマンスを向上させるために利用可能な場合にphpredis ライブラリをラップします。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/ca-bundle.git"> コンポーザー/ca バンドル </a>
    </td>
    <td>Library</td>
    <td>システム CA バンドルへのパスを見つけることができ、Mozilla CA バンドルへのフォールバックが含まれます。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/class-map-generator.git">composer/class-map-generator</a>
    </td>
    <td>Library</td>
    <td>PHP コードをスキャンしてクラスマップを生成するユーティリティ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/composer.git"> コンポーザー/コンポーザー</a>
    </td>
    <td>Library</td>
    <td>Composerは、PHP プロジェクトの依存関係を宣言、管理、インストールするのに役立ちます。 これにより、あらゆる場所に適切なスタックを構築できます。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/metadata-minifier.git">composer/metadata-minifier</a>
    </td>
    <td>Library</td>
    <td>メタデータの縮小と拡張を扱う小規模なユーティリティライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/pcre.git"> コンポーザー/pcre</a>
    </td>
    <td>Library</td>
    <td>タイプ セーフのpreg_*置換を提供するPCRE ラッピングライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/semver.git"> コンポーザー/サーバー</a>
    </td>
    <td>Library</td>
    <td>ユーティリティ、バージョン制約の解析および検証を提供するSemver ライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/spdx-licenses.git"> コンポーザー/spdx-licenses</a>
    </td>
    <td>Library</td>
    <td>SPDX ライセンスのリストと検証ライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/xdebug-handler.git">composer/xdebug-handler</a>
    </td>
    <td>Library</td>
    <td>Xdebugを使用せずにプロセスを再起動します。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/doctrine/lexer.git">doctrine/lexer</a>
    </td>
    <td>Library</td>
    <td>トップダウン、再帰的な降順パーサーで使用できるPHP Doctrine Lexer パーサーライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/egulias/EmailValidator.git">egulias/email-validator</a>
    </td>
    <td>Library</td>
    <td>複数のRFCに対して電子メールを検証するためのライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/elastic/elastic-transport-php.git">弾性/輸送</a>
    </td>
    <td>Library</td>
    <td>Elastic製品用のHTTP トランスポート PHP ライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/elastic/elasticsearch-php.git">elasticsearch/elasticsearch</a>
    </td>
    <td>Library</td>
    <td>ELASTICSEARCH用PHP クライアント</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/endroid/qr-code.git">endroid/qr-code</a>
    </td>
    <td>Library</td>
    <td>Endroid QR コード</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/guzzlestreams.git">ezimuel/guzzlestreams</a>
    </td>
    <td>Library</td>
    <td>elasticsearch-phpで使用するguzzle/streams （abandoned）のフォーク</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/ringphp.git">ezimuel/ringphp</a>
    </td>
    <td>Library</td>
    <td>elasticsearch-phpで使用するguzzle/RingPHPのフォーク（放棄）</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/FriendsOfPHP/proxy-manager-lts.git">friendsofphp/proxy-manager-lts</a>
    </td>
    <td>Library</td>
    <td>ocramius/proxy-managerに幅広いPHP バージョンのサポートを追加</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/giggsey/Locale.git">giggsey/locale</a>
    </td>
    <td>Library</td>
    <td>libphonenumber-for-phpで必要なロケール関数</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/bzikarsky/gelf-php.git">graylog2/gelf-php</a>
    </td>
    <td>Library</td>
    <td>GELF互換バックエンド（Graylog2など）にログメッセージを送信するためのphp実装。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/guzzle.git">guzzlehttp/guzzle</a>
    </td>
    <td>Library</td>
    <td>GuzzleはPHP HTTP クライアントライブラリです</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/promises.git">guzzlehttp/promises</a>
    </td>
    <td>Library</td>
    <td>ガズル・プロミス・ライブラリー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/psr7.git">guzzlehttp/psr7</a>
    </td>
    <td>Library</td>
    <td>PSR-7 メッセージの実装。共通のユーティリティメソッドも提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/collections.git">照光/コレクション </a>
    </td>
    <td>Library</td>
    <td>Illuminate コレクション パッケージ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/conditionable.git">照光/条件付き</a>
    </td>
    <td>Library</td>
    <td>Illuminate Conditionable パッケージ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/config.git">照らす/設定</a>
    </td>
    <td>Library</td>
    <td>Illuminate Config パッケージ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/contracts.git">照明/契約</a>
    </td>
    <td>Library</td>
    <td>Illuminate Contracts パッケージ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/macroable.git">照光/マクロ可能</a>
    </td>
    <td>Library</td>
    <td>Illuminate Macroable パッケージ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jsonrainbow/json-schema.git">justinrainbow/json-schema</a>
    </td>
    <td>Library</td>
    <td>Json スキーマを検証するライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem.git"> リーグ/フライシステム </a>
    </td>
    <td>Library</td>
    <td>PHPのファイルストレージの抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-aws-s3-v3.git">league/flysystem-aws-s3-v3</a>
    </td>
    <td>Library</td>
    <td>Flysystem用AWS S3 ファイルシステムアダプタ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-local.git">league/flysystem-local</a>
    </td>
    <td>Library</td>
    <td>Flysystem用のローカル・ファイルシステム・アダプタ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/mime-type-detection.git">league/mime-type-detection</a>
    </td>
    <td>Library</td>
    <td>FlysystemのMime タイプ検出</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/monolog.git">monolog/monolog</a>
    </td>
    <td>Library</td>
    <td>ログをファイル、ソケット、受信トレイ、データベース、各種web サービスに送信します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jmespath/jmespath.php.git">mtdowling/jmespath.php</a>
    </td>
    <td>Library</td>
    <td>JSON ドキュメントから要素を抽出する方法を宣言的に指定します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon.git"> ネスボット/カーボン </a>
    </td>
    <td>Library</td>
    <td>281の異なる言語をサポートするDateTimeのAPI拡張機能。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/constant_time_encoding.git">paragonie/constant_time_encoding</a>
    </td>
    <td>Library</td>
    <td>RFC 4648 エンコーディングの定時実装（Base-64、Base-32、Base-16）</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/random_compat.git">paragonie/random_compat</a>
    </td>
    <td>Library</td>
    <td>PHP 5.x polyfill for random_bytes （） and random_int （） from PHP 7</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/emogrifier.git">pelago/emogrifier</a>
    </td>
    <td>Library</td>
    <td>HTML コードでCSS スタイルをインラインスタイル属性に変換します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/discovery.git">php-http/discovery</a>
    </td>
    <td>composer-plugin</td>
    <td>PSR-7、PSR-17、PSR-18およびHTTPlug実装を検索してインストールします</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/httplug.git">php-http/httplug</a>
    </td>
    <td>Library</td>
    <td>HTTPlug:PHP用のHTTP クライアント抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/promise.git">php-http/promise</a>
    </td>
    <td>Library</td>
    <td>非同期HTTP リクエストに使用されるPromise</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpgt/CssXPath.git">phpgt/cssxpath</a>
    </td>
    <td>Library</td>
    <td>CSS セレクターをXPath クエリに変換します。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpgt/Dom.git">phpgt/dom</a>
    </td>
    <td>Library</td>
    <td>最新のDOM API:</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/PropFunc.git">phpgt/propfunc</a>
    </td>
    <td>Library</td>
    <td>プロパティアクセサー関数とミューテーター関数。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/mcrypt_compat.git">phpseclib/mcrypt_compat</a>
    </td>
    <td>Library</td>
    <td>mcrypt拡張機能のためのPHP 5.x-8.x polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/phpseclib.git">phpseclib/phpseclib</a>
    </td>
    <td>Library</td>
    <td>PHP Secure Communications Library - RSA、AES、SSH2、SFTP、X.509などの純粋なPHP実装。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/predis/predis.git">predis/predis</a>
    </td>
    <td>Library</td>
    <td>PHP用の柔軟で機能的なRedis/Valkey クライアント。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/cache.git">psr/cache</a>
    </td>
    <td>Library</td>
    <td>ライブラリをキャッシュするための共通インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/clock.git">psr/clock</a>
    </td>
    <td>Library</td>
    <td>時計を読むための一般的なインターフェイス。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/container.git">psr/container</a>
    </td>
    <td>Library</td>
    <td>共通コンテナインターフェイス（PHP FIG PSR-11）</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/event-dispatcher.git">psr/event-dispatcher</a>
    </td>
    <td>Library</td>
    <td>イベント処理用の標準インターフェイス。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-client.git">psr/http-client</a>
    </td>
    <td>Library</td>
    <td>HTTP クライアント用の共通インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-factory.git">psr/http-factory</a>
    </td>
    <td>Library</td>
    <td>PSR-17: PSR-7 HTTP メッセージ ファクトリの共通インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-message.git">psr/http-message</a>
    </td>
    <td>Library</td>
    <td>HTTP メッセージの共通インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/log.git">psr/log</a>
    </td>
    <td>Library</td>
    <td>ライブラリをログ記録するための共通インターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/simple-cache.git">psr/simple-cache</a>
    </td>
    <td>Library</td>
    <td>シンプルなキャッシュのための一般的なインターフェイス</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ralouphie/getallheaders.git">ralouphie/getallheaders</a>
    </td>
    <td>Library</td>
    <td>getallheaders用のpolyfill。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/collection.git"> ラムジー/コレクション </a>
    </td>
    <td>Library</td>
    <td>コレクションを表現および操作するためのPHP ライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/uuid.git"> ラムジー/uuid</a>
    </td>
    <td>Library</td>
    <td>ユニバーサル固有識別子（UUID）を生成して使用するためのPHP ライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/reactphp/promise.git">react/promise</a>
    </td>
    <td>Library</td>
    <td>CommonJS Promises/A for PHPの軽量な実装</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/PHP-CSS-Parser.git">sabberworm/php-css-parser</a>
    </td>
    <td>Library</td>
    <td>PHPで書かれたCSS ファイルのパーサー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/jsonlint.git">seld/jsonlint</a>
    </td>
    <td>Library</td>
    <td>JSON Linter</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/phar-utils.git">seld/phar-utils</a>
    </td>
    <td>Library</td>
    <td>PHAR ファイル形式ユーティリティ（PHPが起動したときに使用）</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/signal-handler.git">seld/signal-handler</a>
    </td>
    <td>Library</td>
    <td>簡単なクロスプラットフォーム開発のために、シグナルがサポートされていない場合にサイレントで失敗するシンプルなunix シグナルハンドラー</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/aes-key-wrap.git">spomky-labs/aes-key-wrap</a>
    </td>
    <td>Library</td>
    <td>AES Key Wrap for PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/otphp.git">spomky-labs/otphp</a>
    </td>
    <td>Library</td>
    <td>RFC 4226 （HOTP アルゴリズム）およびRFC 6238 （TOTP アルゴリズム）に準拠し、Google Authenticatorと互換性のある1回限りのパスワードを生成するためのPHP ライブラリ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/pki-framework.git">spomky-labs/pki-framework</a>
    </td>
    <td>Library</td>
    <td>公開鍵インフラストラクチャを管理するためのPHP フレームワーク。 これは、X.509公開鍵証明書、属性証明書、証明要求、および証明書パス検証で構成されます。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/cache.git">symfony/cache</a>
    </td>
    <td>Library</td>
    <td>拡張されたPSR-6、PSR-16 （およびタグ）実装を提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/cache-contracts.git">symfony/cache-contracts</a>
    </td>
    <td>Library</td>
    <td>キャッシュに関連する一般的な抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/clock.git">symfony/clock</a>
    </td>
    <td>Library</td>
    <td>システムクロックからアプリケーションを切り離す</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/config.git">symfony/config</a>
    </td>
    <td>Library</td>
    <td>あらゆる種類の設定値を検索、読み込み、結合、自動入力、検証できます</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/console.git">symfony/console</a>
    </td>
    <td>Library</td>
    <td>美しくテスト可能なコマンドラインインターフェイスの作成を容易にする</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/css-selector.git">symfony/css-selector</a>
    </td>
    <td>Library</td>
    <td>CSS セレクターをXPath式に変換します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/dependency-injection.git">symfony/dependency-injection</a>
    </td>
    <td>Library</td>
    <td>アプリケーションでのオブジェクトの構築方法を標準化および一元化できます</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/deprecation-contracts.git">symfony/deprecation-contracts</a>
    </td>
    <td>Library</td>
    <td>トリガーの非推奨化に関する一般的な関数と規則</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/error-handler.git">symfony/error-handler</a>
    </td>
    <td>Library</td>
    <td>エラーを管理し、PHP コードのデバッグを容易にするツールを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher.git">symfony/event-dispatcher</a>
    </td>
    <td>Library</td>
    <td>イベントをディスパッチしてリッスンすることで、アプリケーションコンポーネントが互いに通信できるようにするツールを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher-contracts.git">symfony/event-dispatcher-contracts</a>
    </td>
    <td>Library</td>
    <td>イベントのディスパッチに関連する一般的な抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/filesystem.git">symfony/filesystem</a>
    </td>
    <td>Library</td>
    <td>ファイルシステムの基本的なユーティリティを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/finder.git">symfony/finder</a>
    </td>
    <td>Library</td>
    <td>直感的な流暢なインターフェイスを介してファイルとディレクトリを検索します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client-contracts.git">symfony/http-client-contracts</a>
    </td>
    <td>Library</td>
    <td>HTTP クライアントに関連する汎用的な抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-foundation.git">symfony/http-foundation</a>
    </td>
    <td>Library</td>
    <td>HTTP仕様のオブジェクト指向レイヤーを定義します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-kernel.git">symfony/http-kernel</a>
    </td>
    <td>Library</td>
    <td>リクエストをレスポンスに変換するための構造化プロセスを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/intl.git">symfony/intl</a>
    </td>
    <td>Library</td>
    <td>ICU ライブラリのローカライゼーションデータへのアクセスを提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/mailer.git">symfony/mailer</a>
    </td>
    <td>Library</td>
    <td>メール送信を支援</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/mime.git">symfony/mime</a>
    </td>
    <td>Library</td>
    <td>MIME メッセージの操作を許可します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-ctype.git">symfony/polyfill-ctype</a>
    </td>
    <td>Library</td>
    <td>Ctype関数のSymfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-grapheme.git">symfony/polyfill-intl-grapheme</a>
    </td>
    <td>Library</td>
    <td>インテルのgrapheme_*関数のためのSymfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-idn.git">symfony/polyfill-intl-idn</a>
    </td>
    <td>Library</td>
    <td>インテルのidn_to_ascii関数およびidn_to_utf8関数のSymfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-normalizer.git">symfony/polyfill-intl-normalizer</a>
    </td>
    <td>Library</td>
    <td>InlのNormalizer クラスおよび関連関数のSymfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-mbstring.git">symfony/polyfill-mbstring</a>
    </td>
    <td>Library</td>
    <td>Mbstring拡張機能のSymfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php73.git">symfony/polyfill-php73</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 7.3+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php80.git">symfony/polyfill-php80</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.0+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php81.git">symfony/polyfill-php81</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.1+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php82.git">symfony/polyfill-php82</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.2+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php83.git">symfony/polyfill-php83</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.3+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php84.git">symfony/polyfill-php84</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.4+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php85.git">symfony/polyfill-php85</a>
    </td>
    <td>Library</td>
    <td>Symfony polyfill backporting some PHP 8.5+ features to lower PHP versions</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/process.git">symfony/process</a>
    </td>
    <td>Library</td>
    <td>サブプロセス内でコマンドを実行</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/proxy-manager-bridge.git">symfony/proxy-manager-bridge</a>
    </td>
    <td>シンフォニーブリッジ</td>
    <td>様々なSymfony コンポーネントを使用したProxyManagerの統合を提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/serializer.git">symfony/serializer</a>
    </td>
    <td>Library</td>
    <td>オブジェクトグラフを含むデータ構造を、配列構造またはXMLやJSONなどの他の形式にシリアル化およびシリアル化解除します。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/service-contracts.git">symfony/service-contracts</a>
    </td>
    <td>Library</td>
    <td>ライティングサービスに関連する一般的な抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/string.git">symfony/string</a>
    </td>
    <td>Library</td>
    <td>文字列にオブジェクト指向APIを提供し、バイト、UTF-8 コードポイント、およびgrapheme クラスターを統一された方法で処理します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation.git">symfony/translation</a>
    </td>
    <td>Library</td>
    <td>アプリケーションを国際化するツールを提供</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation-contracts.git">symfony/translation-contracts</a>
    </td>
    <td>Library</td>
    <td>翻訳に関連する一般的な抽象化</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-dumper.git">symfony/var-dumper</a>
    </td>
    <td>Library</td>
    <td>任意のPHP変数を実行するためのメカニズムを提供します</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-exporter.git">symfony/var-exporter</a>
    </td>
    <td>Library</td>
    <td>シリアル化可能なPHP データ構造をプレーン PHP コードに書き出すことができます</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/yaml.git">symfony/yaml</a>
    </td>
    <td>Library</td>
    <td>YAML ファイルの読み込みとダンプ</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thecodingmachine/safe.git">thecodingmachine/safe</a>
    </td>
    <td>Library</td>
    <td>エラー時にFALSEを返す代わりに例外をスローするPHP コア関数</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/web-token/jwt-framework.git">web-token/jwt-framework</a>
    </td>
    <td>symfony-bundle</td>
    <td>PHPおよびSymfony バンドル用のJSON オブジェクト署名および暗号化ライブラリ。</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webonyx/graphql-php.git">webonyx/graphql-php</a>
    </td>
    <td>Library</td>
    <td>GraphQL参照実装のPHP ポート</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/zordius/lightncandy.git">zordius/lightncandy</a>
    </td>
    <td>Library</td>
    <td>ハンドルバー（http://handlebarsjs.com/）とmustache （http://mustache.github.io/）の非常に高速なPHP実装。</td>
  </tr>
  </tbody>
</table>

### OSL-3.0、AFL-3.0

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-customer-balance
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-card
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-card-account
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-wrapping
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-graph-ql
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-reward
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  </tbody>
</table>

### OSL-3.0

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### PHP

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/2tvenom/CBOREncode.git">2tvenom/cborencode</a>
    </td>
    <td>Library</td>
    <td>PHP用CBOR エンコーダー</td>
  </tr>
  </tbody>
</table>

### 専有

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### 専有

<table>
  <thead>
    <tr>
      <th>名前</th>
      <th>タイプ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      aem/rum
    </td>
    <td>magento2-module</td>
    <td>該当なし</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-core
    </td>
    <td>magento2-module</td>
    <td>Gene Commerce for PayPalによるMagento Braintree 2.2.0 モジュールからのフォーク。</td>
  </tr>
  </tbody>
</table>
