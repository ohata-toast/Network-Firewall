## Security > Network Firewall > コンソール使用ガイド

Network Firewallを作成するための手順と作成後のコンソールの使用方法を説明します。

## はじめる

Network Firewallを使うためには、まず、Network Firewallサービスを有効化します。

## Network Firewallの作成

### 事前準備

Network Firewallの作成に必要な最小ネットワークサービスリソースは次のとおりです。

> [参考]
> **Network Firewall > 概要**のNetwork Firewallサービス構成図を参照してください。

[1つのプロジェクトを構成する際の準備事項］

* 1個のプロジェクト
* 2個のVPC(Hub VPC, Spoke VPC)
* Hub VPC内の3つのサブネット
    * トラフィック(内部)サブネット、 NAT(外部)サブネット、外部転送サブネット
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ

[1つのプロジェクト内に2つのSpoke VPCを構成する場合の準備事項］ 

* 1つのプロジェクト
* 3つのVPC(Hub VPC、Spoke1 VPC、Spoke2 VPC)
* Hub VPC内の3つのサブネット
    * トラフィック(内部)サブネット、 NAT(外部)サブネット、外部転送サブネット
* Spoke1 VPC、Spoke2 VPC内のそれぞれ最低1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ

[1つ以上のプロジェクトを構成する際の準備事項］

* 2つのプロジェクト
* 2つのVPC(それぞれプロジェクトにHub VPC、Spoke VPC)
* Hub VPC内の3つのサブネット
    * トラフィック(内部)サブネット、 NAT(外部)サブネット、外部転送サブネット
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ


[他のリージョン間プロジェクトを構成する際の準備事項]

* 1つのプロジェクト
* 2個のVPC(KR1リージョンにHub VPC, KR2リージョンにSpoke VPC)
* Hub VPC内の3つのサブネット
    * トラフィック(内部)サブネット、 NAT(外部)サブネット、外部転送サブネット
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ


[単一VPC内の複数のサブネットを構成する際の準備事項]

* 1個のプロジェクト
* 1個のVPC
* 3つのHubサブネット
    * トラフィック(内部)サブネット、 NAT(外部)サブネット、外部転送サブネット
* Hubサブネットと重複しない最低1つのSpokeサブネット。
* Spokeサブネットに接続するルーティングテーブル
* VPCのRoutingに接続されたインターネットゲートウェイ


> [参考]
>* 上記のサービスリソースは[Network]カテゴリーで作成可能です。
>* Network Firewallは、プロジェクトごとに1つずつしか作成できません。

### Network Firewallの作成

1. **Security > Network Firewall**に移動します。
2. 各必須項目を全て選択し、下部の**Network Firewall作成**をクリックします。
    * RBAC:インスタンスオブジェクト照会、 Network Firewallサービスの提供に必要なAPI権限を付与
    * 構成方式：単一構成と冗長構成を選択します。
    * VPC: Network Firewallで使用するVPC
    * サブネット: Network Firewallで内部トラフィック制御のために使用するサブネット
    * NAT: Network Firewallで外部トラフィック制御のために使用するサブネット
    * 外部転送: Network Firewallで成されたトラフィックとログを転送するサブネット
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/create.png" height="60%" />


> [作成前の参考事項]
>* 作成されたNetwork Firewallはユーザーのプロジェクトに表示されません。 
>* サブネット、 NAT、外部転送に使用するサブネットはすべて別のサブネットを選択する必要があります。
>   * なるべくNHN Cloudコンソールで作成できる最小単位(28ビット)で作成することを推奨します。
>* Network Firewallが属するVPCのルーティングテーブルにインターネットゲートウェイが接続されている必要があります。
>* Security Groupsとは別のサービスなので、Network Firewallを使用すると、両方のサービスを許可しなければインスタンスにアクセスすることができません。
>* Network Firewallが所有しているCIDR帯域と接続が必要なCIDR帯域は重複してはいけません。
>* **Network > Network Interface**にてVirtual_IPタイプで作成されているIPはNetwork Firewallにて冗長化用途で使用中のため、削除すると通信が遮断される可能性があります。
>* 単一または冗長構成を選択してNetwork Firewallを作成した後、変更が必要な場合、**オプション** タブで構成を変更できます。ただし、アベイラビリティゾーンは変更ができないため、冗長構成の場合、アベイラビリティゾーンを分離して構成してください。

### 接続設定

> [例]
> Network Firewallが使用するVPC(Hub)は10.0.0.0/24で、Network Firewallと接続が必要なVPC(Spoke)は172.16.0.0/24の場合
1. <strong>Network > Peering Gateway</strong> に移動してピアリングを作成します。
    * ピアリングゲートウェイ接続の詳細については、 [ユーザーガイド](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)を参照してください。
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />
<br>

> [参考]
> 
> Spoke VPCの位置に応じて適切なピアリングを作成します。
> * Spoke VPCが同じプロジェクトであれば、ピアリングを作成します。
> * Spoke VPCが他のプロジェクトの場合、プロジェクトピアリングを作成します。
> * Spoke VPCが異なるリージョンの場合、リージョンピアリングを作成します。

2. <strong>Network > Routing</strong> に移動してHub VPCを選択した後、下記のルーティングを設定します。
    * 対象CIDR: 172.16.0.0/24
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

3. <strong>Network > Routing</strong> に移動してSpoke VPCを選択した後、下記のルーティングを設定します。
    * 対象CIDR: 0.0.0.0/0
    * ゲートウェイ：ピアリング接続後に追加したピアリングタイプのゲートウェイ
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings6.png" height="65%" />
<br>

5. <strong>Network > Peering Gateway</strong> に移動してルーティングを設定します。
    * 作成されたピアリングを選択して**ルート** タブに移動します。
    * **ピア**または**ローカルルートの変更**ボタンを押して、以下のようにルーティングを設定します。
        * 対象CIDR: 0.0.0.0/0
        * ゲートウェイ: NetworkFirewall\_INF\_TRAFFIC\_VIP
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings7.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings8.png" height="50%" />

上記のルーティング設定が完了すると、Spoke VPCにあるインスタンスがNetwork Firewallを経由して公認通信をすることができます。 (<strong>Network Firewall > NAT</strong> タブでNATを追加する必要があります)

<br>

**Spoke VPCのサブネットが2つ以上あり、Network Firewallを介してサブネット間のトラフィック制御が必要な場合、**以下のルーティングを追加します。

> [例]
> Spoke VPC(172.16.0.0/24)のサブネットが172.16.0.0/25と172.16.0.128/25の場合

* <strong>Network > Routing</strong> に移動して Spoke VPCを選択した後、下記の2つのルーティングを追加します。
    * 対象CIDR: 172.16.0.0/25と172.16.0.128/25
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings9.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings10.png" height="65%" />
上記のルーティング設定が完了したら、Spoke VPC内にあるサブネット間のNetwork Firewallを経由してプライベート通信をすることができます。 (<strong>Network Firewall > Policy</strong> タブでポリシーを追加する必要があります)

<br>

**Spoke VPCが2つ以上**ある場合は、以下のルーティングを追加します。

> [例]
> Spoke VPC1(172.16.0.0/24)とSpoke VPC2(192.168.0.0/24)の場合
* <strong>Network > Routing</strong> に移動してHub VPCを選択した後、下記の2つのルーティングを追加します。
    * Spoke VPC 1
        * 対象CIDR: 172.16.0.0/24
        * ゲートウェイ: Hub VPCとSpoke VPC1の間に追加されたピアリングタイプのゲートウェイ
    * Spoke VPC 2
        * 対象CIDR: 192.168.0.0/24
        * ゲートウェイ: Hub VPCとSpokr VPC2の間に追加されたピアリングタイプのゲートウェイ
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings11.png" height="65%" />


> [参考]
> **接続設定**の**4**のようにSpoke VPC2-Hub間のVPCピアリングにもルートの追加設定が必要です。

<br>

**同じVPCでSpokeサブネットを構成する場合、**新しいルーティングテーブルを作成してサブネットを接続し、ルートを追加します。
* **Network > Routing**でルーティングテーブルを作成し、ルートを追加します。
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/routetable_create.png" height="65%" />
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/route_create.png" height="65%" />

<br>

* **Network > Subnet**でNetwork Firewallと重ならないSpokeサブネットを新規作成し、ルーティングテーブルを接続します。
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/subnet_create.png" height="65%" />
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/routetable_connect.png" height="65%" />

上記のルーティング設定が完了すると、異なるSpoke VPC間のNetwork Firewallを経由してプライベート通信を行うことができます。 (<strong>Network Firewall > ポリシー</strong>タブでポリシーの追加が必要)
Network Firewallサービス構成図を参考にして、お客様の環境に合わせて接続を設定してください。


***

## インスタンス接続
Network Firewallを作成し、接続設定を全て完了した後、Network Firewallを経由してインスタンスに接続できます。

例えば、1つのプロジェクト内の2つのSpoke VPCで3つのサブネットを構成し、外部からWebファイアウォール接続が必要な場合、下記のようにNAT、ACLを設定します。

<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/instance-access
.png" height="65%" />

> [設定方法]
> * **Network Firewall > NAT** タブに移動
> * **追加**ボタンをクリックし、NATを設定
>   * 設定前に**オブジェクト**タブで目的地IPオブジェクトを作成し、余分なFloating IPが必要 
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/nat-add.png" height="65%" />
> * **Network Firewall > ポリシー > ACL** タブで必要なACLを許可
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/access_acl.png" height="65%" />  
上記のように設定後、送信元IPをセキュリティグループで許可すると、インスタンスに接続可能です。

<br>>

## ポリシー
Network Firewallを作成すると、**ポリシー**タブに移動します。

![policy-default.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/policy-default.png)

> [参考]

> * default-denyは必須ポリシーであり、修正または削除できません。
> * default-denyポリシーでブロックされたログは、**オプション**タブの**基本ブロックポリシーログ設定**を**使用**に変更した後、**ログ**タブで確認できます。

<br>

## ACL
**ACL**タブでは、Network Firewallと接続されたVPC間のトラフィックとインバウンド/アウトバウンドトラフィックを制御できます。
<br/>

### 追加

* 出発地、目的地、宛先ポートを基にポリシーを追加できます。
    * すでに作成されたオブジェクトを通じて出発地、目的地、宛先ポートを選択します。
* ポリシーの状態(有効/無効)と動作(許可/ブロック)、スケジュールを設定し、ポリシーごとのロギングの有無などのオプションを設定してポリシーを追加できます。
* スケジュール機能は、ポリシーの状態を有効にした後に動作します(ポリシーが無効になっている場合、スケジュール機能は適用されません)。

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/acl_add.png)

### コピー

* **コピー**をクリックしてポリシーをコピーできます。
    * コピーされたポリシーは無効になります。


![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### 修正

* **修正**をクリックしてポリシーを修正できます。


### 移動

* **移動**をクリックしてポリシーを移動できます。
    * default-denyポリシーの下には移動できません。

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### 削除

* **削除**をクリックしてポリシーを削除できます。

>[注意]
>一度削除したポリシーは復元することができず、 default-denyポリシーは削除できません。

### ポリシーの一括ダウンロード

* ポリシータブに作成されているポリシー全体を一度にダウンロードできます。

### ポリシーの一括登録

* ダウンロードしたテンプレートを使って、ポリシーを一括で登録できます。

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)

<br>

## ルート

**ルート**タブでは、Network Firewallを経由する通信の経路を指定できます。

![policy-route.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/policy-route.png)

> [参考]
> * Network FirewallのデフォルトゲートウェイはNATイーサネットであり、修正または削除できません。
> * ルートの設定が変更された場合、通信に問題が発生する可能性があるため、注意して設定してください。 
### 追加

* **追加**をクリックしてイーサネットを選択し、目的地とゲートウェイを入力します。
    * 目的地：サブネット形式で入力
    * イーサネット：NAT、TRAFFIC、VPN(IPSec VPN機能使用時)の中から選択
    * ゲートウェイ：ホスト形式で入力

> [参考]
> * イーサネットをVPNとして選択した場合、ゲートウェイは指定する必要はありません。
> * IPSec VPNと連動したプライベートIP帯域に対するルート設定は、必ずイーサネットをVPNとして設定してください。
> * 目的地サブネットを入力する際、以下のような有効性メッセージが表示される場合は、サブネット範囲を事前に確認し、サブネットの開始IPで入力してください。
>   * [例]
>       * 192.168.199.0/21 (X) → 192.168.192.0/21 (O)
>       * 172.16.100.0/20 (X) → 172.16.96.0/20 (O)
>       * 10.10.10.130/25 (X) → 10.10.10.128/25 (O)
> 
> ![route_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/route_add.png)
### 修正

* **修正**をクリックしてルートを修正できます。

### 削除

* **削除**をクリックしてルートを削除できます。

<br>

## オブジェクト

**オブジェクト**タブでは、ポリシーを作成する時に使用するIP、ポートを作成して管理します。

### 追加

* 必須項目を入力してオブジェクトを作成します。
    * オブジェクトはIP、ポートの2つの形で追加できます。

> [参考]
> グループオブジェクトを作成する場合、グループオブジェクトは追加できません(単一または範囲オブジェクトのみ選択して追加可能)。

### 修正

* **修正**をクリックしてオブジェクトを修正できます。
    * タイプは修正ができません。

### 削除

* **削除**をクリックしてオブジェクトを削除できます。

    * 自動的にNetwork Firewallで生成されたオブジェクトは修正や削除ができません。

>[注意]
>ポリシーで使用中のオブジェクトは削除後、ALLオブジェクトに変更されます。

### インスタンスオブジェクトの追加
* Network Firewallが作成されたプロジェクト内にあるインスタンスを活用して、オブジェクトを追加できます。

> [参考]
> * インスタンスに関係なく、単にインスタンスの名前とプライベートIPアドレスだけを参考にしてオブジェクトを作成します。作成したオブジェクトは**オブジェクト**タブで管理します。


### オブジェクトの一括ダウンロード

* **オブジェクト**タブに作成されているIPとポートオブジェクト全体をそれぞれ一度にダウンロードできます。

<br>

## NAT

**NAT**(ネットワークアドレス変換)タブでは、外部から接続するインスタンスと専用に使用するグローバルIPを選択して接続します。

>[参考]
> * NATは目的地ベースおよび1:1方式のみ提供します。
> * ポートベースのNATは提供しません。
> * NATを作成した後、**ポリシー**タブに許可ポリシーを追加すると公認通信が可能です。
> * NAT削除後、使用しないNAT前のグローバルIPは**Network - Floating**で直接削除してください。

### 追加

* **追加**をクリックしてNATを作成します。
    * NAT前のグローバルIPは**Network > Floating IP**であらかじめ作成したIPのいずれかを選択します。  
    * NAT後、プライベートIPで選択するオブジェクトは、**オブジェクト**タブであらかじめ作成しておく必要があり、**追加**をクリックして追加できます。

![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.04.05/nat_add_2.png)

>[参考]
> インスタンスへの接続は、NATを追加しながら設定したNAT前のグローバルIPで行うことができます。 (インスタンスに直接Floating IPを接続する必要はありません)

### 修正

* **修正**をクリックして作成されたNATを修正します。
    * 修正はグローバルIPとプライベートIPの両方を修正できます。

### 削除

* **削除**をクリックして作成されたNATを削除します。

<br>

## ミラーリング

**ミラーリング**タブでは、Network Firewallを通過するネットワークパケットをIDS/IPS、SIEM、NDRなどの脅威検出及び分析ソリューションにコピーして、ネットワークの脅威をリアルタイムで検出し、対応できるようにします。

> [参考]
> **オプション - ミラーリング設定**で**使用**に設定して有効化した後、使用できます。 (有効化まで約30秒かかります)
<br>
>     ![Mirorring_Config_Activation_800.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirorring_Config_Activation_800.png)
<br>

### ミラーリングルール

* ミラーリングルールを追加してコピーしたパケットを希望の対象端末に送信します。
![Mirroring_Rule_Contents_Explain_1_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Contents_Explain_1_900.png)
    * 名前：設定した名前を表示します。
    * 方向：設定した方向を表示します。
    * ミラー指定インターフェイス：選択したNetwork Firewallのインターフェイスを表示します。
    * ミラーリング送信IP：ミラーリングインターフェイスのIPを表示します。
    * ミラーリング対象IP：ミラーリングパケットを送信する宛先IPを表示します。
    * フィルタグループ：選択したフィルタグループを表示します。
    * 状態：該当ミラーリングルールの状態をバッジで表示します。
        * Active：有効化
        * Inactive：無効
    * 詳細表示：設定したミラーリングルールの詳細情報を確認します。

<br>

### 追加

* **追加**をクリックしてミラーリングルールを追加できます。
    ![Mirroring_Rule_Add_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Add_900.png)
    * 状態：ミラーリングルールの有効/無効を設定します。
    * 方向：ミラー指定インターフェイスでミラーリングする受信/送信パケットを設定します。該当設定により、特定方向のパケットのみミラーリングできます。
        * 受信(Rx)：ミラー指定インターフェイスで受信するパケット
        * 送信(Tx)：ミラー指定インターフェイスから送信するパケット
    * ミラー指定インターフェイス：Network Firewallの以下のインターフェイスの中から選択します。
        * NetworkFirewall\_INF\_NAT: Network Firewallの外部制御用の上部インターフェイス
        * NetworkFirewall\_INF\_TRAFFIC: Network Firewallの内部制御用の下部インターフェイス
    * ミラーリング送信IP：外部送信サブネットのミラーリングインターフェイスがデフォルトで設定されます。
    * ミラーリング対象IP：ミラーリングパケットを受信する対象のプライベートIPを入力します。
    * VNI(virtual network identifier)：VNIを入力します。

> [参考]
>
> * ミラーリング対象端末がVXLANパケットを受信できるようにポリシー(セキュリティグループ及びファイアウォールなど)でミラーリング送信IPとUDPポート4789番への接続許可設定が必要です。
> * ミラーリングルールは最大3つまで作成できます。
> * ミラーリングルールを適用する際、お客様の環境によって多くの通信データが発生する可能性があるため、ミラーリング対象IP情報を正確に入力する必要があります。
> * Network FirewallはVXLANトンネルを介してミラーリングパケットを送信するため、VNI設定が必要です。VNI値は1～16,777,215の間の数字で入力し、ミラーリング対象機器と同じに設定する必要があります。
* **フィルタグループ**を選択します。
    * 以前に追加したフィルタグループがない場合は、**フィルタグループ追加**をクリックしてフィルタグループを追加できます。
    * 詳細については、 [フィルタグループの説明](#%ED%95%84%ED%84%B0%20%EA%B7%B8%EB%A3%B9)を参照してください。
        ![Mirroring_Rule_Filter_Group_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Filter_Group_900.png)

> [参考]
> フィルタグループはルールごとに1つだけ適用可能です。
<br>

### 修正

* **修正**をクリックしてミラーリングルールを修正できます。

> [参考]
> 名前、説明、状態、フィルタグループのみ修正可能です。
<br>

### 削除

* **削除**をクリックしてミラーリングルールを削除できます。

<br>

### フィルタグループ

* **フィルタグループ**を通じてミラーリングルールに適用するフィルタを設定すると、ユーザーが希望するパケットだけを選別して送信できます。
![Filter_Group_Contents_Explain_1_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Group_Contents_Explain_1_900.png)
    * 名前:設定した名前を表示します。
    * 接続されたミラーリングルール：該当フィルタグループを使用するミラーリングルールを表示します。
    * 説明：説明を表示します。
    * フィルタルール表示：該当フィルタグループに設定されたルールを確認します。

<br>

### 追加
* **追加**をクリックしてフィルタグループを追加できます。
    ![Filter_Group_Add_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Group_Add_900.png)
    * フィルタルール定義
        * 優先順位：数字が小さいほど優先度が高く、優先度の高いものからルールを適用してミラーリングパケットを送信します。
        * プロトコル：プロトコルを指定します。
            * ALL：全てのプロトコルを指定します。選択すると出発地/目的地設定が無効になります。
            * TCP： TCPを指定します。
            * UDP： UDPを指定します。
            * ICMP： ICMPを指定します。選択すると出発地/目的地ポート設定が無効になります。
        * 出発地/目的地CIDR：出発地と目的地CIDRを設定します。
        * 出発地/宛先ポート： ALL、ポート、ポート範囲を選択して設定します。
            * ALL：全てのポートを指定します。
            * ポート： 1～65535の範囲のポートを1つ指定します。
            * ポート範囲： 1～65535の範囲のポート範囲を指定します。
        * 送信するかどうか：該当ルールに合致するパケットを送信するかどうかを設定します。
            * 送信：ルールに合致するパケットを送信します。
            * 未送信：ルールに合致するパケットを送信しません。

> [参考]
>
> * 各ルールの[－]、[＋]ボタンをクリックして削除または追加できます。
> * 各ルールの上、下ボタンをクリックして、ルールの優先順位を変更できます。
>     ![Filter_Rule_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Rule_900.png)
> * フィルタグループはdefaultフィルタグループを含めて最大10個まで設定可能です。
> * フィルタルールは最大30個まで設定可能です。
> * フィルタルールは優先順位が高い順から低い順に適用されます。したがって、未送信ルールに既に適用されたパケットは、次の優先順位のルールには適用されません。
<br>

### 修正
* **修正**をクリックしてフィルタグループを修正できます。

<br>

### 削除
* **削除**をクリックしてフィルタグループを削除できます。

> [参考]
> defaultフィルタグループは削除できません。
<br>

## VPN

**VPN**タブでは、サイト間の暗号化されたトンネルを通じて安全なプライベート通信をサポートします。

### ゲートウェイ作成

* **ゲートウェイの作成**をクリックして、ピアVPN機器と接続するためのゲートウェイを作成します。

![gw_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/gw_add.png)

> [参考]
> * VPCとサブネットは修正できません。
> * ゲートウェイは最大10個まで作成可能です。

### 修正

* **修正**ボタンをクリックしてゲートウェイを修正します。

### 削除

* **削除**ボタンをクリックしてゲートウェイを削除します。
    * ゲートウェイに接続されたトンネルがある場合、削除されません。

### Floating IP接続

* ピア機器との接続に必要なFloating IPを設定します。
    * Floating IPは **Network > Floating IP**に作成されたリストのうち、未使用中の項目が表示されます。

![fip.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/fip.png)

### トンネル作成

* ピア機器と接続するトンネルを作成します。

![tunnel_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/tunnel_add.png)

* トンネル設定
    * ゲートウェイ：ゲートウェイタブで作成されたゲートウェイが表示され、トンネルと接続するゲートウェイを選択します。
        * 作成されたゲートウェイがない場合は、表示されません。
    * ピアIPアドレス：接続するピアVPN機器のIPアドレスを入力します。
    * IKEバージョン：ピアVPN機器と同じバージョンに設定します。
        * IKEバージョン1はMain Modeのみサポートされます。
    * Pre-Shared Key：ピアVPN機器と同じキー値を入力します。
    * DPD(dead peer detection)： 10秒単位で合計5回の再送信を試行し、無効を選択した場合、ピアVPN機器のDPDリクエストに対するレスポンスのみをサポートします。
    * NAT-Traversal：トンネル作成時に発生するパケットの削除を防止するための機能で、一般的にピアVPN装置がグローバルIPの場合、使用に設定します。
* Phase 1/2設定
    * IPSec VPN トンネルを作成するために必要な設定情報を入力します。

 > [設定時の注意事項］
 > * 全ての設定はピアVPN機器と同じように設定します。
 > * ローカルIDはピアVPN機器の設定方式によって選択的に設定します。
 > * Phase 2の追加は最大3つまで可能です。
 > * ローカルプライベートIPとピアプライベートIPは互いに重複しないようにしてください。 この範囲には、VPCピアリングを含むNetwork Firewallと接続するすべてのプライベート帯域が含まれます。
  > 下記のCIDRは、ローカルプライベートIPとピアプライベートIPに追加することができず、追加する場合、Network Firewallを経由する通信に問題がある可能性があります。
 >   * 10.0.0.0/8
 >   * 172.16.0.0/12
 >   * 192.168.0.0/16

### トンネル接続

* トンネルは接続待機状態で作成され、**接続**をクリックして作成されたトンネルとピアVPN機器を接続します。

> [参考]
> * **状態**列で色別にトンネルの状態を確認できます。
 >   * 緑：ピアVPN機器と正常に接続している状態です。
 >   * 赤：設定値または通信状態などの問題でピアVPN機器間の接続に失敗した状態。
 >   * 灰色: 接続待機状態(新しく作成されたトンネル)
> * トンネル作成が完了した後、ピア機器の種類と設定により、**接続**をクリックしなくても接続できる場合があります。

### トンネル修正

* **修正**ボタンをクリックしてトンネルを修正します。
    * 設定値のうち、ゲートウェイを除くすべての値を修正することができ、修正する場合、ピアVPN機器も同じ値に修正する必要があります。

### トンネル停止

* **停止**ボタンをクリックしてトンネルを停止します。
    * 停止する場合、ピアVPN機器を介したプライベート通信が中断されます。

### トンネル削除

* **削除**ボタンをクリックしてトンネルを削除します。

### イベント

* ピアVPN機器とのトンネル接続時に発生するイベントログを検索できます。

> [参考]
> * イベントでは、トンネルに関するイベントログのみを検索できます。
> * VPN トンネルを介した通信ログまたはトンネル作成と削除などの監査ログは、**ログ**タブでご確認ください。

## ログ

**ログ**タブでは、Network Firewallで作成されたログを検索できます。

### 検索

* トラフィック: Network Firewallを経由する際に、許可またはブロックポリシーによって作成されたトラフィックログを検索します。
    * 照会は1か月単位で最大3か月までの過去のデータのみ検索可能です。
        * 最大保存ログ数は800万個であり、トラフィックの量によって保存されるログの量が変わるため、過去のデータが照会されない場合があります。
    * 別のデータ保存が必要な場合は、**オプション**タブの**ログリモート送信設定**を参照してください。

* Audit:ポリシーの作成および削除など、Network Firewallの変更事項に関するログを検索
    * 照会は最大1か月単位で検索可能で、組織サービスであるCloudTrailでも検索できます。

### Excelダウンロード

* **Excelダウンロード**をクリックしてトラフィックとAuditログの検索結果をダウンロードできます。
    * トラフィックログの最大ダウンロード数は30万件です。

## モニター

**モニター**タブではNetwork Firewallの状態をリアルタイムで確認できます。
検索は最大24時間(1日)以内でのみ可能です。

### 検索

* セッション:現在Network Firewallを介して使用するセッションの数量
* ネットワーク使用量:現在Network Firewallを経由するインバウンド/アウトバウンドトラフィック

## オプション

**オプション**タブではNetwork Firewall運営に必要なオプションを設定できます。

### ログ設定

* 基本ブロックポリシーログ設定: Network Firewall作成後に必ず作成される基本ブロックポリシーログを保存するかどうかを選択します。
    * 使用を選択すると、基本ブロックポリシーで作成されたログはトラフィックログから検索できます。
* ログ遠隔転送設定:遠隔地にトラフィックログを保存できるオプションを選択します。
    * Syslog:最大2つの遠隔地アドレスにログを保存
        * 2つの遠隔地は 個別に設定可能(IPアドレス、プロトコル、ポート番号)
    * Object Storage: NHN Cloudで提供するObject Storageサービスでログを転送
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/OBS_5.png" height="65%" />
        * アクセスキー / 秘密鍵: Object StorageサービスでS3 API認証情報を登録する際に確認可能なアクセスキー情報を入力
        * バケット名: Object Storageサービスで作成したコンテナ名を入力
        * エンドポイント:リージョン別のエンドポイントを確認した後、位置に合わせてエンドポイントを入力
        * リージョン:リージョン別の名前を確認した後、リージョンの位置に合わせて名前を入力
    * Log & Crash Search: NHN Cloudで提供するLog & Crash Searchサービスでログを転送
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/LNCS_2.png" height="65%" />
        * AppKey: Log & Crash Searchサービスを有効にした後、作成されたAppKeyを入力

> [参考]
> * Object Storage設定時、[ユーザーガイド](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/s3-api-guide/#aws-sdk)を参考にして入力してください。
> * Log & Crash Searchサービスを使用すると、ログアラーム設定機能を活用して異常行為を検出できます。
例えば、Network Firewallに特定の目的地に向かうSSH通信に対するACLブロックポリシーを追加した後、そのポリシーで発生するログに対するアラーム条件を設定します。 (例：1分間、SSH接続試行ログが20回以上発生)
ユーザーが設定した条件を満たした場合、アラームを受信できます。

<br>

### 一般設定

* MTU(maximum transmission unit)サイズ設定: Network Firewallに連結されたイーサネットのMTUサイズを設定します。
    * トラフィック: NHN Cloud内部通信に使用するイーサネット(ピアリング通信を含む)
    * NAT:外部通信に使用するイーサネット

> [参考] 
> トラフィック、 NATイーサネットの基本MTUサイズは1450Byteです。

<br>

* ミラーリング設定： Network Firewallが提供する機能のうち、ミラーリングの使用有無を選択できます。
    * 使用選択時に必要なサブネットはNetwork Firewall作成に使用したサブネットを使用します。

> [参考]
> * ACL設定に必要なミラーリングインターフェイスのIP情報は**Network - Network Interface**で確認できます。
>   * インターフェイス名： NetworkFirewall_INF_MIRRORING_S_NAT_VIP
<br>

* Network Firewall構成：単一または冗長化でNetwork Firewallの構成方法を設定できます。

> [参考]
> * 構成方法を変更する場合、数分程度の時間がかかり、設定変更が完了するまでサービスに影響を及ぼす可能性があります。
> * ポリシー、NATなどNetwork Firewallの変更作業は、構成方法の変更が完了した後に行うことを推奨します。
* Network Firewallの削除：運用中のNetwork Firewallを削除できます。
    * Network Firewallは韓国(パンギョ)リージョンと韓国(ピョンチョン)リージョンでそれぞれ削除できます。

> [削除時の注意事項]
> * 運営中のNetwork Firewallを削除する場合、Network Firewallと接続されている他のサービスを考慮して実行してください。     

<br>

## サービスの無効化

**プロジェクト管理 > 利用中中のサービス**でNetwork Firewallサービスを無効にできます。

> [参考]
> * Network Firewallサービスの無効化は、韓国(パンギョ)リージョンと韓国(ピョンチョン)リージョンの両方に適用されます。
> 例えば、Network Firewallサービスを同じプロジェクトの韓国(パンギョ)リージョンと韓国(ピョンチョン)リージョンの両方で有効にした場合、2つのリージョンのうち1つのNetwork Firewallサービスだけを無効にすることはできません。
> * 無効化するには、韓国(パンギョ)リージョンと韓国(ピョンチョン)リージョでそれぞれNetwork Firewallを削除してください。
