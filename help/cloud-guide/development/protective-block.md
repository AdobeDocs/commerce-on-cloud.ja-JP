---
title: 保護ブロック
description: Adobe Commerce on cloud infrastructureの保護ブロック機能と、既知のセキュリティ脆弱性からサイトを保護する仕組みについて説明します。
feature: Cloud, Configuration, Security
topic: Security
exl-id: 4a470e75-0b42-4ab7-b3dc-9f50b63bea14
TQID: https://experienceleague.adobe.com/E0lyCu6cFEaHR0KoauRFmQi9QThzoGDmNetnzYv3hVg
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
source-wordcount: 311
ht-degree: 0%

---

# 保護ブロック

Adobe Commerce オンクラウドインフラストラクチャには、特定の状況下で、セキュリティ上の脆弱性を抱えたweb サイトへのアクセスを制限する保護ブロッキング機能があります。 この部分的なブロック方法は、既知のセキュリティ脆弱性の悪用を防ぎます。 古いソフトウェアにはエクスプロイトが含まれていることが多いため、これらのサイトへのアクセスを部分的にブロックすることで、これらのエクスプロイトから保護することが重要です。

## 保護ブロックの仕組み

Adobe Commerceでは、クラウドインフラストラクチャに一般的にデプロイされているオープンソースのソフトウェアの既知のセキュリティ脆弱性の署名データベースを管理します。 セキュリティチェックは、オープンソースプロジェクトの既知の脆弱性のみを分析します。書き込んだカスタマイズを調べることはできません。

セキュリティスキャンが実行されます。

- 新しいコードをGitにプッシュすると
- 新しい脆弱性がデータベースに追加された場合

アプリケーションで重大な脆弱性が検出された場合、Git プッシュは拒否されます。

実行されるブロックには2つのタイプがあります。

1. **ブロックを完了** – 開発web サイト用。 `git push`に付随するエラーメッセージには、脆弱性に関する詳細情報が記載されています。

1. **部分ブロック** – 実稼動web サイト用。このサイトは、ほぼオンライン状態を維持できます。 脆弱性の性質によっては、クエリ文字列、Cookie、その他のヘッダーなどのリクエストの一部がGET リクエストから削除される場合があります。 ログイン、フォーム送信、製品チェックアウトなど、その他のすべてのリクエストは完全にブロックされる場合があります。

ブロック解除は、セキュリティリスクの解決時に自動化されます。 このブロックは、脆弱性を削除するセキュリティアップグレードを適用した直後に削除されます。

## 保護ブロックからオプトアウト

保護ブロックは、Adobe Commerceをクラウドインフラストラクチャにデプロイするソフトウェアの既知の脆弱性からユーザーを保護するためのものです。

ただし、次の項目を[`.magento.app.yaml`](../application/configure-app-yaml.md)に追加すると、保護ブロックをオプトアウトできます。

```yaml
   preflight:
      enabled: false
```

次のように、特定のチェックを明示的にオプトアウトできます。

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
