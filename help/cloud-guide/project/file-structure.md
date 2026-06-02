---
title: プロジェクト構造
description: Adobe Commerce on cloud infrastructureのファイル構造とプロジェクトテンプレートについて説明します。
exl-id: 364e40e4-a5b3-4d23-b86d-74fc0696ac19
TQID: https://experienceleague.adobe.com/B6fTvmHLFa5THSgLKsjl1smPC8ekPdXB9A-vyqFVwG8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 0%

---

# プロジェクト構造

Adobe Commerce on cloud インフラストラクチャプロジェクトには、資格情報とアプリケーション設定に必要なファイルが含まれています。 これらのファイルは、Adobe Commerceのバージョンに応じてテンプレートとして使用できます。 [`magento/magento-cloud` GitHub リポジトリ &#x200B;](https://github.com/magento/magento-cloud)のAdobe Commerce バージョンに基づくクラウドテンプレートを参照してください。

次の表に、クラウドプロジェクトに含まれるファイルを示します。

| ファイル | 説明 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | `www`をapex ドメインと`php` アプリケーションにリダイレクトしてHTTPを提供する設定ファイル。 [&#x200B; ルートの設定](../routes/routes-yaml.md)を参照してください。 |
| `/.magento/services.yaml` | MySQL インスタンス（MariaDB）、Redis、OpenSearchまたはElasticsearchを定義する設定ファイル。 [&#x200B; サービスの設定](../services/services-yaml.md)を参照してください。 |
| `/app` | `code` フォルダーはカスタムモジュールに使用されます。 `design` フォルダーは[&#x200B; カスタムテーマ &#x200B;](../store/custom-theme.md)に使用されます。 `etc` フォルダーには、アプリケーションの設定ファイルが含まれています。 |
| `/m2-hotfixes` | カスタムパッチに使用されます。 |
| `/update` | サポートモジュールで使用されるサービスフォルダー。 |
| `.gitignore` | 無視するファイルとディレクトリを指定します。 [`.gitignore`参照](#ignoring-files)を参照してください。 |
| `.magento.app.yaml` | アプリケーションを構築するためのプロパティを定義する設定ファイル。 [&#x200B; アプリケーションの設定](../application/configure-app-yaml.md)を参照してください。 |
| `.magento.env.yaml` | ビルド、デプロイ、デプロイ後のフェーズの設定ファイル。 `ece-tools` パッケージには、このファイルのサンプルが含まれています。 [環境の設定](../environment/configure-env-yaml.md)を参照してください。 |
| `composer.json` | アプリケーションを準備するために、Adobe Commerceと設定スクリプトを取得します。 [必要なパッケージ &#x200B;](../development/overview.md#required-packages)を参照してください。 |
| `composer.lock` | 各パッケージのバージョン依存関係を保存します。 [必要なパッケージ &#x200B;](../development/overview.md#required-packages)を参照してください。 |
| `magento-vars.php` | 変数を使用して[複数のストア &#x200B;](../store/multiple-sites.md)とサイトを定義するために使用します。 |

{style="table-layout:auto"}

>[!NOTE]
>
>ローカルの変更をリモート サーバーにプッシュすると、デプロイ スクリプトは`.magento` ディレクトリ内の設定ファイルで定義された値を使用し、その後、スクリプトはディレクトリとその内容を削除します。 ローカル開発環境は影響を受けません。

## アプリケーションのルートディレクトリ

アプリケーションのルートディレクトリの場所は、環境によって異なります。

- **スターターとプロ統合**: `/app`
- **スタータープロダクション**: `/<project-ID>`
- **Pro ステージング**: `/<project-ID>_stg`
- **Pro実稼動**: `/<project-ID>`

### 書き込み可能なディレクトリ

リモート統合環境、ステージング環境および実稼動環境は読み取り専用です。 次のディレクトリは、セキュリティ上の理由から&#x200B;*only*&#x200B;書き込み可能なディレクトリです。

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>実稼動環境とステージング環境では、3 ノードクラスター内の各ノードには、他のノードと共有されていない`/tmp` ディレクトリがあります。

## ファイルを無視

Adobe Commerce on cloud infrastructure プロジェクトリポジトリを持つベース `.gitignore` ファイルがあります。 magento-cloud リポジトリ [&#128279;](https://github.com/magento/magento-cloud/blob/master/.gitignore)の最新の.gitignore ファイルを参照してください。 `.gitignore` リストにあるファイルを追加するには、コミットのステージング時に`-f` （強制）オプションを使用できます。

```bash
git add <path/filename> -f
```

## 基本テンプレートを変更

次の手順を使用して、既存のプロジェクトの構造を変更し、Adobe Commerce on cloud infrastructureの最新の基本テンプレートを反映させることができます。

1. プロジェクトをローカルワークステーションに複製します。

1. `extra` セクションの次の値で`composer.json` ファイルを更新します。

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 基本テンプレート用に設計された`.gitignore` ファイルを追加します。 例えば、バージョン 2.2.6 テンプレートに`.gitignore` ファイルが必要な場合は、2.2.6[&#128279;](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) ファイルの.gitignoreを参照として使用します。

1. Git キャッシュをクリアします。

   ```bash
   git rm -r --cached .
   ```

1. 変更を追加してコミットします。

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
