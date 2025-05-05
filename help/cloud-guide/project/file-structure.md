---
title: プロジェクト構造
description: クラウドインフラストラクチャー上のAdobe Commerceのファイル構造およびプロジェクトテンプレートについて説明します。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# プロジェクト構造

クラウドインフラストラクチャー上のAdobe Commerce プロジェクトには、資格情報とアプリケーション設定に不可欠なファイルが含まれています。 これらのファイルは、Adobe Commerceのバージョンに応じて、テンプレートとしてで使用できます。 [`magento/magento-cloud` GitHub リポジトリ ](https://github.com/magento/magento-cloud) で、Adobe Commerce バージョンに基づくクラウドテンプレートを参照してください。

次の表では、クラウドプロジェクトに含まれるファイルについて説明します。

| ファイル | 説明 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | HTTP を提供するために apex ドメインおよび `php` アプリケーションに `www` ーザーをリダイレクトする設定ファイル。 [ ルートの設定 ](../routes/routes-yaml.md) を参照してください。 |
| `/.magento/services.yaml` | MySQL インスタンス（MariaDB）、Redis、OpenSearch またはElasticsearchを定義する設定ファイルです。 [ サービスの設定 ](../services/services-yaml.md) を参照してください。 |
| `/app` | `code` フォルダーは、カスタムモジュールに使用されます。 `design` フォルダーは [ カスタムテーマ ](../store/custom-theme.md) に使用されます。 `etc` フォルダーには、アプリケーションの設定ファイルが含まれます。 |
| `/m2-hotfixes` | カスタムパッチに使用します。 |
| `/update` | サポートモジュールが使用するサービスフォルダー。 |
| `.gitignore` | 無視するファイルとディレクトリを指定します。 [`.gitignore` リファレンス ](#ignoring-files) を参照してください。 |
| `.magento.app.yaml` | アプリケーションを構築するためのプロパティを定義する設定ファイル。 [ アプリケーションの設定 ](../application/configure-app-yaml.md) を参照してください。 |
| `.magento.env.yaml` | ビルド、デプロイ、デプロイ後のフェーズ用の設定ファイル。 `ece-tools` パッケージには、このファイルのサンプルが含まれています。 [ 環境の設定 ](../environment/configure-env-yaml.md) を参照してください。 |
| `composer.json` | Adobe Commerceと設定スクリプトを取得して、アプリケーションを準備します。 [ 必須パッケージ ](../development/overview.md#required-packages) を参照してください。 |
| `composer.lock` | パッケージごとにバージョンの依存関係を格納します。 [ 必須パッケージ ](../development/overview.md#required-packages) を参照してください。 |
| `magento-vars.php` | 変数を使用して [ 複数のストア ](../store/multiple-sites.md) およびサイトを定義するために使用します。 |

{style="table-layout:auto"}

>[!NOTE]
>
>ローカルで行った変更をリモートサーバーにプッシュすると、デプロイスクリプトは `.magento` ディレクトリ内の設定ファイルで定義された値を使用し、ディレクトリとその内容を削除します。 ローカル開発環境は影響を受けません。

## アプリケーションのルートディレクトリ

アプリケーションのルートディレクトリの場所は、環境によって異なります。

- **Starter と Pro の統合**: `/app`
- **スターター実稼動**: `/<project-ID>`
- **Pro ステージング**: `/<project-ID>_stg`
- **Pro 実稼働**: `/<project-ID>`

### 書き込み可能ディレクトリ

リモート統合環境、ステージング環境、実稼動環境は読み取り専用です。 次のディレクトリは、セキュリティ上の理由から、書き込み可能な *専用* ディレクトリです。

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>実稼動環境とステージング環境では、3 つのノードクラスター内の各ノードには、他のノードと共有されていない `/tmp` ディレクトリがあります。

## ファイルを無視

Adobe Commerce on cloud infrastructure プロジェクトリポジトリを持つベース `.gitignore` ファイルがあります。 magento-cloud リポジトリの最新の [.gitignore ファイルを参照してください ](https://github.com/magento/magento-cloud/blob/master/.gitignore) `.gitignore` リストに含まれるファイルを追加するには、コミットをステージングするときに `-f` （force） オプションを使用します。

```bash
git add <path/filename> -f
```

## 基本テンプレートの変更

次の手順を使用して、既存のプロジェクトの構造を変更し、クラウドインフラストラクチャー上のAdobe Commerceの最新の基本テンプレートを反映させることができます。

1. プロジェクトをローカルワークステーションにクローンします。

1. `extra` セクションの次の値で `composer.json` ファイルを更新します。

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 基本テンプレート用に設計された `.gitignore` ファイルを追加します。 例えば、バージョン 2.2.6 テンプレートの `.gitignore` ファイルが必要な場合は、バージョン 2.2.6[&#128279;](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) ファイルの .gitignore を参照として使用します。

1. Git キャッシュをクリアします。

   ```bash
   git rm -r --cached .
   ```

1. 変更を追加してコミットします。

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
