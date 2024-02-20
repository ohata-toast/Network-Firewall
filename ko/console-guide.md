## Security > Network Firewall > 콘솔 사용 가이드

Network Firewall을 생성하기 위한 절차와 생성 이후 콘솔을 사용하는 방법을 설명합니다.

## 시작하기

Network Firewall을 사용하기 위해서는 가장 먼저 Network Firewall 서비스를 활성화합니다.

## Network Firewall 생성

### 사전 준비

Network Firewall 생성에 필요한 최소 네트워크 서비스 자원은 아래와 같습니다.

> [참고]
> **Network Firewall > 개요**에서 Network Firewall 서비스 구성도를 참조하십시오.


[1개의 프로젝트 구성 시 준비 사항]

* 1개의 프로젝트
* 2개의 VPC(Hub VPC, Spoke VPC)
* Hub VPC 내 3개의 서브넷(Network Firewall 서브넷, NAT 서브넷, 외부 전송 서브넷)
* Spoke VPC 내 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이

[1개의 프로젝트 내 2개의 Spoke VPC 구성 시 준비 사항]

* 1개의 프로젝트
* 3개의 VPC(Hub VPC, Spoke1 VPC, Spoke2 VPC)
* Hub VPC 내 3개의 서브넷(Network Firewall 서브넷, NAT 서브넷, 외부 전송 서브넷)
* Spoke1 VPC, Spoke2 VPC 내 각각 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이

[1개 이상의 프로젝트 구성 시 준비 사항]

* 2개의 프로젝트
* 2개의 VPC(각각 프로젝트에 Hub VPC, Spoke VPC)
* Hub VPC 내 3개의 서브넷(Network Firewall 서브넷, NAT 서브넷, 외부 전송 서브넷)
* Spoke VPC 내 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이


[다른 리전 간 프로젝트 구성 시 준비 사항]

* 1개의 프로젝트
* 2개의 VPC(KR1 리전에 Hub VPC, KR2 리전에 Spoke VPC)
* Hub VPC 내 3개의 서브넷(Network Firewall 서브넷, NAT 서브넷, 외부 전송 서브넷)
* Spoke VPC 내 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이


[단일 VPC 내 여러 개의 서브넷 구성 시 준비 사항]

* 1개의 프로젝트
* 1개의 VPC
* 3개의 Hub 서브넷(Network Firewall 서브넷, NAT 서브넷, 외부 전송 서브넷)
* 최소 1개의 Spoke 서브넷
* VPC의 Routing에 연결된 인터넷 게이트웨이


> [참고]
> 위의 서비스 자원은 [Network] 카테고리에서 생성 가능합니다.
> Network Firewall 생성은 프로젝트당 1개씩만 생성 가능합니다.

### Network Firewall 생성

1. **Security > Network Firewall**로 이동합니다.
2. 각 필수 항목을 모두 선택하고 하단의 **Network Firewall 생성**을 클릭합니다.
    * RBAC: 인스턴스 객체 조회, Network Firewall 서비스 제공에 필요한 API 권한을 부여
    * VPC: Network Firewall에서 사용할 VPC
    * 서브넷: Network Firewall에서 내부 트래픽 제어를 위해 사용할 서브넷
    * NAT: Network Firewall에서 외부 트래픽 제어를 위해 사용할 서브넷
    * 외부 전송: Network Firewall에서 생성된 트래픽과 로그를 전송할 서브넷
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/NFW-Create.png" height="60%">


> [참고]
> 
>* 서브넷, NAT, 외부 전송에 사용하는 서브넷은 모두 다른 서브넷으로 선택해야 합니다.
>   * 가급적 NHN Cloud 콘솔에서 생성할 수 있는 최소 단위(28비트)로 생성할 것을 권장합니다.
>* Network Firewall이 속할 VPC의 라우팅 테이블에 인터넷 게이트웨이가 연결되어 있어야 생성 가능합니다.
>* Network Firewall 서비스는 가용 영역을 분리하여 이중화를 기본으로 제공합니다.
>* Security Groups와는 별개의 서비스이므로 Network Firewall을 사용하면 두 서비스를 모두 허용해야 인스턴스에 접근할 수 있습니다.
>* Network Firewall이 소유하고 있는 CIDR 대역과 연결이 필요한 CIDR 대역은 중복되지 않아야 합니다.
>* **Network > Network Interface**에서 Virtual_IP 타입으로 생성되어 있는 IP는 Network Firewall에서 이중화 용도로 사용 중이므로 삭제할 경우 통신이 차단될 수 있습니다.

### 연결 설정
> [예시]
> Network Firewall이 사용하는 VPC(Hub)는 10.0.0.0/24이고, Network Firewall과 연결이 필요한 VPC(Spoke)는 172.16.0.0/24일 때

1. <strong>Network > Routing</strong>으로 이동하여 Spoke VPC를 선택한 후 라우팅 테이블을 변경합니다.
    * Spoke VPC를 선택한 후 <strong>라우팅 테이블 변경</strong>을 클릭해 중앙 집중형 라우팅(CVR) 방식으로 변경합니다.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings1.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings2.png" height="50%" />
<br>

2. <strong>Network > Peering Gateway</strong>로 이동하여 피어링을 생성합니다.
    * Spoke VPC가 다른 프로젝트라면 프로젝트 피어링을 생성합니다.
    * Spoke VPC가 다른 리전이라면 리전 피어링을 생성합니다.
    * Spoke VPC가 같은 프로젝트라면 피어링을 생성합니다.
        * 피어링 게이트웨이 연결에 대한 자세한 사항은 [사용자 가이드](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)를 참조해 주세요.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />
<br>

3. <strong>Network > Routing</strong>으로 이동하여 Hub VPC를 선택한 후 아래의 라우팅을 설정합니다.
    * 대상 CIDR: 172.16.0.0/24
    * 게이트웨이: 피어링 연결 후 추가된 피어링 타입의 게이트웨이
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

4. <strong>Network > Routing</strong>으로 이동하여 Spoke VPC를 선택한 후 아래의 라우팅을 설정합니다.
    * 대상 CIDR: 0.0.0.0/0
    * 게이트웨이: 피어링 연결 후 추가된 피어링 타입의 게이트웨이
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings6.png" height="65%" />
<br>

5. <strong>Network > Peering Gateway</strong>로 이동하여 라우팅을 설정합니다.
    * 생성된 피어링을 선택하여 **라우트** 탭으로 이동합니다.
    * **피어** 또는 **로컬 라우트 변경** 버튼을 눌러 아래와 같이 라우팅을 설정합니다.
        * 대상 CIDR: 0.0.0.0/0
        * 게이트웨이: NetworkFirewall\_INF\_TRAFFIC\_VIP
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings7.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings8.png" height="50%" />

위의 라우팅 설정이 완료되면 Spoke VPC에 있는 인스턴스가 Network Firewall을 경유하여 공인 통신을 할 수 있습니다. (<strong>Network Firewall > NAT</strong> 탭에서 NAT 추가 필요)
<br>

***
<br>

**만약 Spoke VPC의 서브넷이 2개 이상이고, Network Firewall을 통해 서브넷 간 트래픽 제어가 필요한 경우** 아래의 라우팅을 추가합니다.

> [예시]
> Spoke VPC(172.16.0.0/24)의 서브넷이 172.16.0.0/25와 172.16.0.128/25일 때

* <strong>Network > Routing</strong>으로 이동하여 Spoke VPC를 선택한 후 아래의 라우팅 2개를 추가합니다.
    * 대상 CIDR: 172.16.0.0/25과 172.16.0.128/25
    * 게이트웨이: 피어링 연결 후 추가된 피어링 타입의 게이트웨이
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings9.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings10.png" height="65%" />
위의 라우팅 설정이 완료되면 Spoke VPC 안에 있는 서브넷 간 Network Firewall을 경유하여 사설 통신을 할 수 있습니다. (<strong>Network Firewall > 정책</strong> 탭에서 정책 추가 필요)
<br>

***
<br>

**만약 Spoke VPC가 2개 이상**이라면 아래의 라우팅을 추가합니다.

> [예시]
> Spoke VPC1(172.16.0.0/24)과 Spoke VPC2(192.168.0.0/24)일 때

* <strong>Network > Routing</strong>으로 이동하여 Hub VPC를 선택한 후 아래의 라우팅 2개를 추가합니다.
    * Spoke VPC 1
        * 대상 CIDR: 172.16.0.0/24
        * 게이트웨이: Hub VPC와 Spoke VPC1 사이에 추가된 피어링 타입의 게이트웨이
    * Spoke VPC 2
        * 대상 CIDR: 192.168.0.0/24
        * 게이트웨이: Hub VPC와 Spoke VPC2 사이에 추가된 피어링 타입의 게이트웨이
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings11.png" height="65%" />


> [참고]
> **연결 설정**의 **5**와 같이 Spoke VPC2-Hub 간 VPC 피어링에도 라우트 추가 설정이 필요합니다.

위의 라우팅 설정이 완료되면 서로 다른 Spoke VPC 간 Network Firewall을 경유하여 사설 통신을 할 수 있습니다. (<strong>Network Firewall > 정책</strong> 탭에서 정책 추가 필요)
Network Firewall 서비스 구성도를 참고하여 고객의 환경에 맞게 연결을 설정하십시오.
<br>

***

Network Firewall 생성과 연결 설정을 완료하면 Network Firewall의 여러 기능을 활용하여 접근 제어를 구성할 수 있습니다.
<br>


## 정책
Network Firewall 인스턴스를 생성하면 정책 초기 페이지로 이동합니다.

**정책** 탭에서는 Network Firewall 인스턴스와 연결된 VPC 간 트래픽과 인바운드/아웃바운드 트래픽을 제어할 수 있는 정책을 관리할 수 있습니다.

### 메인 페이지

* default-deny는 필수 정책이며, 수정하거나 삭제할 수 없습니다.
>[참고]
default-deny 정책을 통해 차단된 로그는 **옵션** 탭의 **기본 차단 정책 로그 설정**을 **사용**으로 변경한 후 **로그** 탭에서 확인 가능합니다.

![main_page.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/main_page_1.png)

### 정책 추가

* 출발지, 목적지, 목적지 포트를 기반으로 정책을 추가할 수 있습니다.
    * 이미 만들어진 객체를 통해 출발지, 목적지, 목적지 포트를 선택합니다.
* 정책의 상태(활성화/비활성화)와 동작(허용/차단), 스케줄을 선택하여 정책을 추가할 수 있습니다.
* 스케줄 기능은 정책의 상태를 활성화 한 이후에 동작합니다(정책이 비활성화되어 있을 경우 스케줄 기능이 적용되지 않습니다.).

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_add_1.png)

### 정책 복사

* **복사**를 클릭해 정책을 복사할 수 있습니다.
    * 복사된 정책은 비활성화됩니다.

![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### 정책 수정

* **수정**을 클릭해 정책을 수정할 수 있습니다.

![acl_edit.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_edit_1.png)


### 정책 이동

* **이동**을 클릭해 정책을 이동할 수 있습니다.
    * 이름: default-deny 정책 아래로는 이동이 불가능합니다.

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### 정책 삭제

* **삭제**를 클릭해 정책을 삭제할 수 있습니다.
>[주의]
>한번 삭제한 정책은 복구할 수 없으며, default-deny 정책은 삭제할 수 없습니다.

### 정책 일괄 다운로드

* 정책 탭에 생성되어 있는 정책 전체를 한번에 다운로드할 수 있습니다.

### 정책 일괄 등록

* 내려받은 템플릿을 사용하여 정책을 한 번에 등록할 수 있습니다.

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)

## 객체

**객체** 탭에서는 정책을 생성할 때 사용할 IP와 포트를 생성하고 관리합니다.

### 추가

필수 항목을 입력하여 객체를 생성합니다.
IP와 포트는 아래의 타입과 프로토콜을 추가할 수 있습니다.

* IP
    * 타입: 서브넷, 범위, 그룹
* 포트
    * 타입: 포트, 범위, 그룹
    * 프로토콜: TCP, UDP, ICMP

### 삭제

* **삭제**를 클릭해 객체를 삭제할 수 있습니다.

    * 자동으로 Network Firewall에서 생성한 객체는 수정이나 삭제할 수 없습니다.
>[주의]
>정책에서 사용 중인 객체는 삭제 후 ALL 객체로 변경됩니다.

### 객체 일괄 다운로드

* **객체** 탭에 생성되어 있는 IP와 포트 객체 전체를 각각 한 번에 다운로드할 수 있습니다.

## NAT

**NAT**(네트워크 주소 변환) 탭에서는 외부에서 접속할 인스턴스를 지정하여 전용 공인 IP를 생성합니다.

* NAT는 목적지 기반 및 1:1 방식만 제공합니다.
* 포트 기반의 NAT는 제공하지 않습니다.
* 생성된 공인 IP는 **Network > Floating IP**에서 확인 가능합니다.

>[참고]
>NAT를 생성한 뒤 허용 정책을 추가해야만 공인 통신이 가능합니다.

### 추가

* **추가(+)**를 클릭해 객체를 선택합니다.
    * 선택할 객체는 미리 생성되어 있어야 합니다.
* **확인**을 누른 뒤 연결된 **NAT 전 공인 IP**를 확인할 수 있습니다.
    * 생성된 NAT 전 공인 IP는 임의로 변경할 수 없습니다.

![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/nat_add_1.png)

### 삭제

* **삭제**를 클릭해 생성된 NAT를 삭제합니다.
    * 삭제 후 NAT 전 공인 IP는 자동으로 삭제됩니다.

## 로그

**로그** 탭에서는 Network Firewall에서 생성된 로그를 검색할 수 있습니다.

### 검색

* 트래픽: Network Firewall을 경유할 때 허용 또는 차단 정책에 의해 생성된 트래픽 로그를 검색
    * 조회는 1개월 단위로 최대 3개월까지의 과거 데이터만 검색 가능합니다.
        * 최대 저장 로그 개수는 800만 개이며, 트래픽의 양에 따라 저장되는 로그의 양이 달라지므로 과거의 데이터가 조회되지 않을 수 있습니다.
    * 별도의 데이터 저장이 필요한 경우 **옵션** 탭의 **로그 원격 전송 설정**을 참고하십시오.

* Audit: 정책 생성 및 삭제 등 Network Firewall의 변경 사항에 대한 로그를 검색
    * 조회는 최대 1개월 단위로 검색 가능하며, 조직 서비스인 CloudTrail에서도 검색할 수 있습니다.

### 엑셀 내려받기

* **엑셀 내려받기**를 클릭해 트래픽과 Audit 로그의 검색 결과를 다운로드할 수 있습니다..
    * 트래픽 로그의 최대 다운로드 개수는 30만 건입니다.

## 모니터

**모니터** 탭에서는 Network Firewall의 상태를 실시간으로 확인할 수 있습니다.
검색은 최대 24시간(1일) 내에서만 가능합니다.

### 검색

* 세션: 현재 Network Firewall을 통해 사용하는 세션의 수량
* 네트워크 사용량: 현재 Network Firewall을 경유하는 인바운드/아웃바운드 트래픽

## 옵션

**옵션** 탭에서는 Network Firewall 운영에 필요한 옵션을 설정할 수 있습니다.

### 로그 설정

* 기본 차단정책 로그 설정: Network Firewall 생성 후 필수로 생성되는 기본 차단정책 로그의 저장여부를 선택합니다.
    * 사용 선택 시 기본 차단 정책으로 생성된 로그는 트래픽 로그에서 검색 가능합니다.
* 로그 원격 전송 설정: 원격지로 트래픽 로그를 저장할 수 있는 옵션을 선택합니다.
    * syslog: 최대 2개의 원격지 주소로 로그를 저장
    * Object Storage: NHN Cloud에서 제공하는 Object Storage 서비스로 로그를 저장
    * Log & Crash Search: NHN Cloud에서 제공하는 Log&Crash Search 서비스로 로그를 저장

### 일반 설정

* MTU(Maximum Transmission Unit) 크기 설정: Network Firewall이 소유한 이더넷의 MTU 크기를 설정합니다.
    * 트래픽: 피어링을 포함하여 NHN Cloud 내부통신에 사용하는 이더넷
    * NAT: 외부통신에 사용하는 이더넷

> [참고]
> 
> 트래픽, NAT 이더넷의 기본 MTU 크기는 1450Byte 입니다.

## 서비스 비활성화

**프로젝트 관리 > 이용 중인 서비스** 에서 Network Firewall 서비스를 비활성화 할 수 있습니다.

> [비활성화 전 주의사항]
>
> * Network Firewall 서비스 비활성화는 판교 리전과 평촌 리전 모두 Network Firewall 서비스가 비활성화 됩니다.
>     예를 들어, 동일한 프로젝트의 판교 리전과 평촌 리전에 모두 Network Firewall 서비스를 활성화 한 후 판교 리전 또는 평촌 리전 중 하나의 Network Firewall 서비스만 비활성화 할 수 없습니다.(기능개선 예정)