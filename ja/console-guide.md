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
* Hub VPC内の3つのサブネット(Network Firewallサブネット、NATサブネット、外部転送サブネット)
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ

[1つのプロジェクト内に2つのSpoke VPCを構成する場合の準備事項］ 

* 1つのプロジェクト
* 3つのVPC(Hub VPC、Spoke1 VPC、Spoke2 VPC)
* Hub VPC内の3つのサブネット(Network Firewallサブネット、NATサブネット、外部転送サブネット)
* Spoke1 VPC、Spoke2 VPC内のそれぞれ最低1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ

[1つ以上のプロジェクトを構成する際の準備事項］

* 2つのプロジェクト
* 2つのVPC(それぞれプロジェクトにHub VPC、Spoke VPC)
* Hub VPC内の3つのサブネット(Network Firewallサブネット、NATサブネット、外部転送サブネット)
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ


[他のリージョン間プロジェクトを構成する際の準備事項]

* 1つのプロジェクト
* 2個のVPC(KR1リージョンにHub VPC, KR2リージョンにSpoke VPC)
* Hub VPC内の3つのサブネット(Network Firewallサブネット、NATサブネット、外部転送サブネット)
* Spoke VPC内の最小1つのサブネット
* Hub VPCのRoutingに接続されたインターネットゲートウェイ


[単一VPC内の複数のサブネットを構成する際の準備事項]

* 1個のプロジェクト
* 1個のVPC
* 3つのHubサブネット(Network Firewallサブネット、 NATサブネット、外部転送サブネット)
* 最低1つのSpokeサブネット
* VPCのRoutingに接続されたインターネットゲートウェイ


> [参考]
>* 上記のサービスリソースは[Network]カテゴリーで作成可能です。
> 
>* Network Firewallは、プロジェクトごとに1つずつしか作成できません。

### Network Firewallの作成

1. **Security > Network Firewall**に移動します。
2. 各必須項目を全て選択し、下部の**Network Firewall作成**をクリックします。
    * RBAC:インスタンスオブジェクト照会、 Network Firewallサービスの提供に必要なAPI権限を付与
    * VPC: Network Firewallで使用するVPC
    * サブネット: Network Firewallで内部トラフィック制御のために使用するサブネット
    * NAT: Network Firewallで外部トラフィック制御のために使用するサブネット
    * 外部転送: Network Firewallで成されたトラフィックとログを転送するサブネット
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/nfw-create.png" height="60%">


> [参考]
> 
>* 作成されたNetwork Firewallはユーザーのプロジェクトに表示されません。 
>* サブネット、 NAT、外部転送に使用するサブネットはすべて別のサブネットを選択する必要があります。
>   * なるべくNHN Cloudコンソールで作成できる最小単位(28ビット)で作成することを推奨します。
>* Network Firewallが属するVPCのルーティングテーブルにインターネットゲートウェイが接続されている必要があります。
>* Network Firewallサービスは、利用可能領域を分離して冗長化を基本的に提供します。
>* Security Groupsとは別のサービスなので、Network Firewallを使用すると、両方のサービスを許可しなければインスタンスにアクセスすることができません。
>* Network Firewallが所有しているCIDR帯域と接続が必要なCIDR帯域は重複してはいけません。
>* **Network > Network Interface**にてVirtual_IPタイプで作成されているIPはNetwork Firewallにて冗長化用途で使用中のため、削除すると通信が遮断される可能性があります。

### 接続設定
> [例]
> Network Firewallが使用するVPC(Hub)は10.0.0.0/24で、Network Firewallと接続が必要なVPC(Spoke)は172.16.0.0/24の場合
1. <strong>Network > Routing</strong> に移動し、Spoke VPCを選択した後、ルーティングテーブルを変更します。
    * Spoke VPCを選択した後、<strong>ルーティングテーブルの変更</strong>をクリックして中央集中型ルーティング(CVR)方式に変更します。
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings1.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings2.png" height="50%" />
<br>

2. <strong>Network > Peering Gateway</strong> に移動してピアリングを作成します。
    * Spoke VPCが別のプロジェクトの場合、プロジェクトピアリングを作成します。
    * Spoke VPCが別のリージョンの場合、リージョンピアリングを作成します。
    * Spoke VPCが同じプロジェクトの場合、ピアリングを作成します。
        * ピアリングゲートウェイ接続の詳細については、 [ユーザーガイド](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)を参照してください。
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />
<br>

3. <strong>Network > Routing</strong> に移動してHub VPCを選択した後、下記のルーティングを設定します。
    * 対象CIDR: 172.16.0.0/24
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

4. <strong>Network > Routing</strong> に移動してSpoke VPCを選択した後、下記のルーティングを設定します。
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

***
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

***
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
> **接続設定**の**5**のようにSpoke VPC2-Hub間のVPCピアリングにもルートの追加設定が必要です。
上記のルーティング設定が完了すると、異なるSpoke VPC間のNetwork Firewallを経由してプライベート通信を行うことができます。 (<strong>Network Firewall > ポリシー</strong>タブでポリシーの追加が必要)
Network Firewallサービス構成図を参考にして、お客様の環境に合わせて接続を設定してください。
<br>

***

Network Firewallの作成と接続設定が完了すると、Network Firewallの様々な機能を活用してアクセス制御を構成できます。
<br>


## ポリシー
Network Firewallインスタンスを作成すると、ポリシー初期ページに移動します。

**ポリシー**タブではNetwork Firewallインスタンスと接続されたVPC間のトラフィックとインバウンド/アウトバウンドトラフィックを制御できるポリシーを管理できます。

### メインページ

* default-denyは必須ポリシーであり、修正や削除ができません。

> [参考]
> default-denyポリシーでブロックされたログは、**オプション**タブの基本ブロックポリシーログ設定**を**使用**に変更した後、**ログ**タブで確認できます。

![main_page.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/main_page_1.png)

### ポリシー追加

* 出発地、目的地、宛先ポートを基にポリシーを追加できます。
    * すでに作成されたオブジェクトを通じて出発地、目的地、宛先ポートを選択します。
* ポリシーの状態(有効化/無効化)と動作(許可/ブロック)、スケジュールを選択してポリシーを追加できます。
* スケジュール機能は、ポリシーの状態を有効にした後に動作します(ポリシーが無効になっている場合、スケジュール機能は適用されません)。
![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_add_1.png)

### ポリシーのコピー

* **コピー**をクリックしてポリシーをコピーできます。
    * コピーされたポリシーは無効になります。


![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### ポリシーの修正

* **修正**をクリックしてポリシーを修正できます。

![acl_edit.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_edit_1.png)


### ポリシーの移動

* **移動**をクリックしてポリシーを移動できます。
    * 名前: default-denyポリシーの下には移動できません。

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### ポリシーの削除

* **削除**をクリックしてポリシーを削除できます。

>[注意]
>一度削除したポリシーは復元することができず、 default-denyポリシーは削除できません。

### ポリシーの一括ダウンロード

* ポリシータブに作成されているポリシー全体を一度にダウンロードできます。

### ポリシーの一括登録

* ダウンロードしたテンプレートを使って、ポリシーを一括で登録できます。

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)

## オブジェクト

**オブジェクト**タブでは、ポリシーを作成する時に使用するIPとポートを作成して管理します。

### 追加

* 必須項目を入力してオブジェクトを作成します。
IPとポートは下記のタイプとプロトコルを追加できます。

    * IP
        * タイプ:サブネット、範囲、グループ
    * ポート
        * タイプ:ポート、範囲、グループ
        * プロトコル: TCP, UDP, ICMP

### 削除

* **削除**をクリックしてオブジェクトを削除できます。

    * 自動的にNetwork Firewallで生成されたオブジェクトは修正や削除ができません。

>[注意]
>ポリシーで使用中のオブジェクトは削除後、ALLオブジェクトに変更されます。

### オブジェクトの一括ダウンロード

* **オブジェクト**タブに作成されているIPとポートオブジェクト全体をそれぞれ一度にダウンロードできます。

## NAT

**NAT**(ネットワークアドレス変換)タブでは、外部から接続するインスタンスを指定して専用グローバルIPを作成します。

* NATは目的地ベースおよび1:1方式のみを提供します。
* ポートベースのNATは提供しません。
* 作成されたグローバルIPは**Network > Floating IP**で確認できます。

>[参考]
>NATを生成した後、許可ポリシーを追加すると公認通信が可能になります。

### 追加

    * 選択するオブジェクトはあらかじめ作成されている必要があります。
* **確認**を押すと、接続された**NAT前グローバルIP**を確認できます。
    * 作成されたNAT前グローバルIPは任意に変更できません。

![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/nat_add_1.png)

### 削除

* **削除**をクリックして作成されたNATを削除します。
    * 削除後、NAT前グローバルIPは自動的に削除されます。

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
    * Log & Crash Search: NHN Cloudで提供するLog & Crash Searchサービスでログを転送

### 一般設定

* MTU(maximum transmission unit)サイズ設定: Network Firewallに連結されたイーサネットのMTUサイズを設定します。
    * トラフィック: NHN Cloud内部通信に使用するイーサネット(ピアリング通信を含む)
    * NAT:外部通信に使用するイーサネット

> [参考] 
> トラフィック、 NATイーサネットの基本MTUサイズは1450Byteです。


## サービスの無効化

**プロジェクト管理 > 利用中中のサービス**でNetwork Firewallサービスを無効にできます。

> [無効化前の注意事項］
> 
> * Network Firewallサービスの無効化はパンギョリージョンとピョンチョンリージョンの両方に適用されます。
> 例えば、同じプロジェクトのパンギョリージョンとピョンチョンリージョンの両方でNetwork Firewallサービスを有効にした場合、2つのリージョンのうち1つのNetwork Firewallサービスだけを無効にすることはできません。 (機能改善予定)
