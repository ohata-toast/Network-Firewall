## Security > Network Firewall > コンソール使用ガイド

Network Firewallを作成するための手順と作成後のコンソールの使用方法を説明します。

## はじめる

Network Firewallを使うためには、まず、Network Firewallのインスタンスを作成します。

## Network Firewallの作成

### 事前準備

Network Firewallの作成に必要な最小ネットワークサービスリソースは次のとおりです。

* 1個のプロジェクト
* 1個のVPC
* 選択されたVPCに属している3つのサブネット
* 選択されたVPCに接続されたインターネットゲートウェイ

>[参考] 上記のサービスリソースは[Network]カテゴリーで作成可能です。

### Network Firewallの作成

1. **Security > Network Firewall**に移動します。
2. 各必須項目を全て選択し、下部の**Network Firewall作成**をクリックします。
    * RBAC:インスタンスオブジェクト照会、 Network Firewallサービスの提供に必要なAPI権限を付与
    * VPC: Network Firewallインスタンスが使用するVPC
    * サブネット: Network Firewallインスタンスが内部トラフィック制御のために使用するサブネット
    * NAT: Network Firewallインスタンスが外部トラフィック制御のために使用するサブネット
    * 外部転送: Network Firewallインスタンスに作成されたトラフィックとログを転送するサブネット

>[参考]
>* サブネット、 NAT、外部転送に使用するサブネットはすべて別のサブネットを選択する必要があります。
>   * なるべくNHN Cloudコンソールで作成できる最小単位(28ビット)で作成することを推奨します。
>* Network Firewallインスタンスは、利用可能領域を分離して冗長化を基本的に提供します。
>* Security Groupsとは別のサービスなので、Network Firewallを使用すると、両方のサービスを許可しなければインスタンスにアクセスすることができません。
>* Network Firewallが所有しているCIDR帯域と接続が必要なCIDR帯域は重複してはいけません。
>* **Network > Network Interface**にてVirtual_IPタイプで作成されているIPはNetwork Firewallにて冗長化用途で使用中のため、削除すると通信が遮断される可能性があります。

### 接続設定

Network Firewallと連動するために、ピアリングゲートウェイの作成およびルーティングを設定します。

>[例]
Network Firewallが使用するVPC(Hub)は10.0.0.0/24で、Network Firewallと接続が必要なVPC(Spoke)は172.16.0.0/24の場合


1. **Network > Routing**に移動してSpoke VPCを選択した後、ルーティングテーブルを変更します。
    * Spoke VPCを選択した後、**ルーティングテーブル変更**を押して中央集中型ルーティング(CVR)方式に変更します。
2. **Network > Peering Gateway**に移動してピアリングを作成します。
    * Spoke VPCが他のプロジェクトであれば、プロジェクトピアリングを接続し、同じプロジェクトであればピアリングを接続します。
        * ピアリングゲートウェイ接続の詳細については、 [ユーザーガイド](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)を参照してください。
3. **Network > Routing**に移動してHub VPCを選択した後、下記のルーティングを設定します。
    * 対象CIDR: 172.16.0.0/24
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
4. **Network > Routing**に移動してSpoke VPCを選択した後、下記のルーティングを設定します。
    * 対象CIDR: 0.0.0.0/0
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
5. **Network > Peering Gateway**に移動してルーティングを設定します。
    * 作成されたピアリングを選択して**ルート** タブに移動します。
    * **ピア**または**ローカルルートの変更**ボタンを押して、以下のようにルーティングを設定します。
        * 対象CIDR: 0.0.0.0/0
        * ゲートウェイ: NetworkFirewall_INF_TRAFFIC_VIP

上記のルーティング設定が完了すると、Spoke VPCにあるインスタンスがNetwork Firewallを経由して公認通信が可能になります。 (**Network Firewall > NAT** タブでNAT追加必要)


---
Spoke VPCのサブネットが2つ以上あり、Network Firewallを通じてサブネット間トラフィック制御が必要な場合、以下のルーティングを追加します。

>[例]
Spoke VPC(172.16.0.0/24)のサブネットが172.16.0.0/25と172.16.0.128/25の場合
* **Network > Routing**に移動してSpoke VPCを選択した後、下記のルーティング2個を追加します。
    * 対象CIDR: 172.16.0.0/25と172.16.0.128/25
    * ゲートウェイ:ピアリング接続後に追加されたピアリングタイプのゲートウェイ
     
上記のルーティング設定が完了すると、Spoke VPC内にあるサブネット間の通信がNetwork Firewallを経由してプライベート通信が可能になります。 (**Network Firewall > Policy** タブでポリシーの追加が必要)

 ---    
もしSpoke VPCが2つ以上ある場合は、下記のルーティングを追加します。

>[例]
Spoke VPC1(172.16.0.0/24)とSpoke VPC2(192.168.0.0/24)の場合
* **Network > Routing**に移動してHub VPCを選択した後、下記のルーティング2個を追加します。
    * Spoke VPC 1
        * 対象CIDR: 172.16.0.0/24
        * ゲートウェイ: Hub VPCとSpoke VPC1の間に追加されたピアリングタイプのゲートウェイ
    * Spoke VPC 2
        * 対象CIDR: 192.168.0.0/24
        * ゲートウェイ: Hub VPCとSpokr VPC2の間に追加されたピアリングタイプのゲートウェイ

上記のルーティング設定が完了すると、異なるSpoke VPC間の通信がNetwork Firewallを経由してプライベート通信が可能になります。 (**Network Firewall > Policy** タブでポリシーの追加が必要)
Network Firewallサービス構成図を参考にして、お客様の環境に合わせて接続を設定してください。

---
Network Firewallの作成と接続設定が完了すると、Network Firewallの様々な機能を活用してアクセス制御を構成できます。

## ポリシー
Network Firewallインスタンスを作成すると、ポリシー初期ページに移動します。

**ポリシー**タブではNetwork Firewallインスタンスと接続されたVPC間のトラフィックとインバウンド/アウトバウンドトラフィックを制御できるポリシーを管理できます。

### メインページ

* default-denyは必須ポリシーであり、修正や削除ができません。
>[参考]
default-denyポリシーでブロックされたログは、**オプション**タブの基本ブロックポリシーログ設定**を**使用**に変更した後、**ログ**タブで確認できます。

![main_page.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/main_page_1.png)

### ポリシー追加

* 出発地、目的地、宛先ポートを基にポリシーを追加できます。
    * すでに作成されたオブジェクトを通じて出発地、目的地、宛先ポートを選択します。
* ポリシーの状態(有効化/無効化)と動作(許可/ブロック)、スケジュールを選択してポリシーを追加できます。

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_add_1.png)

### ポリシーのコピー

* **コピー**をクリックしてポリシーをコピーできます。
    * コピーされたポリシーは無効になります。

![acl_copy.PNG](/ko/images/acl_copy.png)


### ポリシーの修正

![acl_edit.PNG](/ko/images/acl_edit.png)

* **修正**をクリックしてポリシーを修正できます。

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

必須項目を入力してオブジェクトを作成します。
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
    * 別途のデータ保存が必要な場合、**オプション**タブの**ログ遠隔転送設定**を参照してください。
* Audit:ポリシーの作成および削除など、Network Firewallの変更事項に関するログを検索
    * 照会は最大1か月単位で検索可能で、組織サービスであるCloudTrailでも検索できます。

### Excelダウンロード

* **Excelダウンロード**をクリックしてトラフィックとAuditログの検索結果をダウンロードできます。
    * トラフィックログの最大ダウンロード数は30万件です。

## モニター

**モニター**タブではNetwork Firewallインスタンスの状態をリアルタイムで確認できます。
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
    * syslog:最大2つの遠隔地アドレスにログを保存
    * Object Storage: NHN Cloudで提供するObject Storageサービスでログを保存
    * Log & Crash Search: NHN Cloudが提供するLog&Crash Searchサービスでログを保存

### 一般設定

* NAT設定:NATを使用するかどうかのオプションを設定します。
> [注意]
NAT使用中に**使用しない**に変更した場合、NATタブで設定した情報は全て削除されます。
