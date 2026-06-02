---
title: ディスク容量の管理
description: コマンドラインインターフェイスを使用してディスク容量を管理する方法を説明します。
feature: Cloud, Storage
exl-id: 1d13dc4e-56eb-4153-a8b1-48d2263ebc4c
TQID: https://experienceleague.adobe.com/645o-d3ZvMtaYOwy0IKwAjSxUyUgkrI36OGLWtZR--g
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 801
ht-degree: 0%

---

# ディスク容量の管理

クラウドプロジェクトの合計ストレージ容量は、[&#x200B; アカウントページ &#x200B;](https://accounts.magento.cloud/user)のAdobe Commerce on cloud infrastructure contractで確認できます。 アカウントの各プロジェクトカードには、_環境_&#x200B;の数、_ストレージ_&#x200B;容量（GB）、_ユーザー_&#x200B;の数が表示されます。 または、次のCloud コマンドを使用することもできます。

```bash
magento-cloud subscription:info | grep storage
```

回答サンプル：

```
| storage              | 51200
```

Proの実稼動環境またはステージング環境がストレージ容量の95%に達するか、それを超えた場合、クラウドインフラストラクチャモニタリングツールは、ストレージ容量の自動増加を通知するサポートアラートをトリガーします。

通知の例：

>[!BEGINSHADEBOX]

_「監視により、クラスター（project-id-environment）上のファイルのストレージが満杯に近づいていることが検出されました。 ディスクの使用状況は現在、残り1 GiB未満の重大な使用状況レベルです。 共有ストレージボリュームは、現在、サービスを稼働させるために60 GiBから70 GiBにアップグレードされています。 実稼動およびステージング ファイルの使用状況を確認して、スペースを空けることができるかどうかを確認してください。&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Adobeでは、このような自動増加を避けるために、ストレージ容量を定期的に監視し、90%未満に維持することをお勧めします。 割り当てが完了すると、Pro ステージングと実稼動のストレージの増加は永続的になり、元に戻すことはできません。

## 統合環境を確認

`magento-cloud` CLIを使用して、統合環境のディスク容量の使用状況を確認できます。

**おおよそのディスク領域の使用状況を確認するには**:

```bash
magento-cloud db:size
```

回答サンプル：

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

すべてのマウントはディスクを共有します。 `magento-cloud` CLIを使用して、マウントのディスク容量の使用状況を確認できます。

**マウント**&#x200B;のディスク領域の使用状況を確認するには、次の手順を実行します。

```bash
magento-cloud mount:size
```

回答サンプル：

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## 専用クラスターの確認

Pro ステージング環境と実稼動環境の場合、`disk free` コマンドを使用して、各環境のディスク容量の使用状況を確認できます。このコマンドは、ファイルシステムで使用されるディスク容量の量を報告します。 リモート環境にログインするには、SSHを使用する必要があります。

```bash
df -h
```

`-h` オプションは、人間が読み取れる形式（KB、MB、またはGB）を使用してレポートを表示します。

次の応答の例では、`/data/exports` マウントはメディアのディスク容量を示し、`/data/mysql/` マウントはデータベースのディスク容量を示します。

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

ディレクトリを指定することで、応答を制限できます。 例：

```bash
df -h var/
```

回答サンプル：

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## ディスク領域の割り当て

2つの[設定ファイル &#x200B;](../environment/overview.md)が、クラウド環境のディスク領域の割り当てを制御します。`.magento.app.yaml` ファイルと`.magento/services.yaml` ファイルです。 各ファイルには、`disk` プロパティが含まれています。このプロパティは、それぞれの設定のディスクサイズ値をMB単位で定義します。 ディスク領域の割り当てを変更できるのは、Pro統合環境とスターター環境のみです。

>[!IMPORTANT]
>
>- Pro実稼動環境およびステージング環境の場合、ディスク領域の割り当てを変更するには、[Adobe Commerce サポートチケット &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=ja#submit-ticket)を送信する必要があります。 Pro実稼動環境とステージング環境のサイズの増加は、一定の間隔でのみ発生する可能性があるため、現在のディスク容量の使用状況に応じて、サポートではディスク容量の割り当てを最低10 GB増やすことをお勧めします。 割り当てが完了すると、Pro ステージングと実稼動環境のストレージの増加を元に戻すことはできません。 ストレージをリソース間で再割り当てまたは再配布することはできません。 ファイルのストレージ容量を増やすには、MySQLに割り当てられているディスク容量を減らします。
>- AWSでホストされているPro実稼動環境とステージング環境には、ディスク容量の増加に適用される[必須の6時間のクールダウン &#x200B;](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVolume.html)があります。 マウント上のディスク容量を増やした後、そのマウント上でディスク容量を再び増やすには、6時間待つ必要があります。

### アプリケーションディスク領域

`.magento.app.yaml` ファイルは、アプリケーションで使用できる[永続ディスク領域](../application/properties.md#disk)を制御します。

**アプリケーションのディスク容量を増やすには**:

1. ローカル開発環境で、`.magento.app.yaml`設定ファイルを開きます。

1. `disk` プロパティに新しい値を設定します（MB単位）。

   ```yaml
   disk: <value-mb>
   ```

1. ファイルに変更を保存します。

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   変更は、更新されたYAML ファイルをリモート環境にプッシュした後に有効になります。

### サービスディスク領域

`.magento/services.yaml` ファイルは、MySQLやRedisなど、各サービスで使用可能なディスク領域を制御します。

**サービスのディスク容量を増やすには**:

1. ローカル開発環境で、`.magento/services.yaml`設定ファイルを開きます。

1. ファイル内のサービスを追加または検索します。 サービスの設定について詳しくは、[を参照してください](../services/services-yaml.md)。

1. ディスクプロパティの新しい値（MB単位）を設定します。

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. ファイルに変更を保存します。

1. コード変更を追加、コミット、プッシュします。

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   変更は、更新されたYAML ファイルをリモート環境にプッシュした後に有効になります。

## ディスク容量の監視

Pro実稼動環境では、New RelicのManaged Alerts for Adobe Commerceアラートポリシーを使用して、ディスク容量やその他のパフォーマンス指標を監視できます。 詳しくは、[管理済みアラートを使用したパフォーマンスの監視](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)を参照してください。 詳細なガイダンスについては、[&#x200B; データベースのパフォーマンスの問題を解決するためのベストプラクティス &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html?lang=ja)を参照してください。

## スペースが残りません

ビルドキャッシュは時間の経過とともに拡張できます。 `No space left on device`という警告が表示された場合は、ビルド キャッシュをクリアして再デプロイしてみてください。

```bash
magento-cloud project:clear-build-cache
```
