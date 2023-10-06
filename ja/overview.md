## Security > Network Firewall > 概要

Network Firewallは、NHN Cloudで使用するインフラ資産を安全に保護するために提供するネットワークセキュリティサービスです。
NHN Cloudに特化したアクセス制御を適用することができ、別途ファイアウォール製品を使用しなくても簡単にファイアウォール機能を使用できます。

## 主な機能
* 効率的にネットワーク通信ポリシーを管理できます。
    * Stateful方式により、1つのポリシーでトラフィックを制御します。
* Hub - Spoke構造で外部攻撃から安全にインスタンスを保護できます。
    * VPC間の内部トラフィックとインバウンド/アウトバウンドトラフィックを制御します。
* ネットワークブロックと許可に対するリアルタイムログ検索とバックアップ機能を提供します。
    * 顧客の環境に合わせてさまざまなバックアップ方式を提供します(Syslog, Object Storage, Log & Crach Search)。
* 安定した運営のために高可用性(冗長化)を提供します。

## Network Firewallサービス構成図
サービス構成は下記の3つの形で構成できます。

### 1つのプロジェクト
![service_architecrure_1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_1.PNG)

### 1つ以上のプロジェクト
![service_architecrure_2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_2.PNG)

### 他のリージョンのプロジェクト
![service_architecrure_3](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_3.PNG)
> [参考]
> 他のリージョンの同じプロジェクトのみ構成可能です。
