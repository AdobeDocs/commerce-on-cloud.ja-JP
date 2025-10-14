---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Fastly のカスタム VCL を削除するためのファイルを含める

## カスタム VCL スニペットの削除

1. 管理者に [&#x200B; ログイン &#x200B;](/help/get-started/onboarding.md#access-your-admin-panel) します。

1. **ストア**/**設定**/**設定**/**詳細**/**システム** をクリックします。

1. **フルページキャッシュ**/**Fastly 設定**/**カスタム VCL スニペット** の順に展開します。

   ![&#x200B; カスタム VCL スニペットの管理 &#x200B;](/help/assets/cdn/fastly-manage-snippets.png)

1. _アクション_ 列で、削除するスニペットの横にあるごみ箱アイコンをクリックします。

1. 次のモーダルウィンドウで、「**DELETE」をクリックし** 新しいバージョンをアクティベートします。

>[!WARNING]
>
>「_カスタム VCL スニペット_ UI オプションには、Adobe Commerce管理を通じて追加されたスニペットのみが表示されます。 Fastly API を使用してスニペットを追加する場合は、API を使用して [&#x200B; スニペットを管理 &#x200B;](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api) します。
