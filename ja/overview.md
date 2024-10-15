## Security > Network Firewall > 概要

Network Firewallは、NHN Cloudで使用するインフラ資産を安全に保護するために提供するネットワークセキュリティサービスです。
NHN Cloudに特化したアクセス制御を適用することができ、別途ファイアウォール製品を使用しなくても簡単にファイアウォール機能を使用できます。

> [参考]
> Network  Firewallサービスは韓国(パンギョ)リージョンの場合、新規ネットワーク環境でのみ利用できます。
> 韓国(パンギョ)リージョンで2022年3月7日以前に作作成したプロジェクトは、改善前のネットワーク環境であるため、Network Firewallサービスを利用するには、プロジェクトを新たに作成する必要があります。

## 主な機能
* 効率的にネットワーク通信ポリシーを管理できます。
    * Stateful方式により、1つのポリシーでトラフィックを制御します。
* Hub - Spoke構造で外部攻撃から安全にインスタンスを保護できます。
    * VPC間の内部トラフィックとインバウンド/アウトバウンドトラフィックを制御します。
* インターネット環境でサイト間の暗号化されたトンネルを通じて、安全な仮想プライベートネットワーク(VPN)を提供します。
* ネットワークブロックと許可に対するリアルタイムログ検索とバックアップ機能を提供します。
    * 顧客の環境に合わせてさまざまなバックアップ方式を提供します(Syslog, Object Storage, Log & Crash Search)。
* 安定した運営のために高可用性(冗長化)を提供します。

## Network Firewallサービス構成図
サービスは下記の5つの形で構成できます。

### 1つのプロジェクト
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture1.png" height="70%">

### 1つ以上のプロジェクト
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture2.png" height="70%" width="100%" />


### 他のリージョン間のプロジェクト
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture3.png" height="70%" width="100%" />

### 1つのプロジェクト内の2つのSpoke VPC
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture4.png" height="70%" width="100%" />


### 1つのVPC内の複数のサブネット
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture5.png" height="50%" width="100%" />


> [参考]
> * 上記の構成図は一般的な構成であり、お客様の環境によってNetwork Firewallを除いたWEB、WAS、Load Balancerなどの構成が異なる場合があります。
> 
> * 他のリージョンのプロジェクト環境では、同じプロジェクトのみ設定可能です。詳細は[ユーザーガイド](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)を参照してください。
> 
> * サービス構成時、2022年3月7日以前に構成したネットワーク環境とは接続できません。
> 例えば、2022年3月7日以前に構成したネットワーク環境を使用するプロジェクトと、それ以降に構成したネットワーク環境を使用するプロジェクトが作成されている場合、新規ネットワーク環境にNetwork Firewallを作成することができますが、改善前のネットワーク環境をSpoke VPCとして使用することはできません。
