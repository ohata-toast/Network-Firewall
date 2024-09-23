## Security > Network Firewall > 콘솔 사용 가이드

Network Firewall을 생성하기 위한 절차와 생성 이후 콘솔을 사용하는 방법을 설명합니다.

## 시작하기

Network Firewall을 사용하기 위해서는 가장 먼저 Network Firewall 서비스를 활성화합니다.

## Network Firewall 생성

### 사전 준비

Network Firewall 생성에 필요한 최소 네트워크 서비스 자원은 아래와 같습니다.

> [참고]
> 
> **Network Firewall > 개요**에서 Network Firewall 서비스 구성도를 참조하세요.


[1개의 프로젝트 구성 시 준비 사항]

* 1개의 프로젝트
* 2개의 VPC(Hub VPC, Spoke VPC)
* Hub VPC 내 3개의 서브넷
    * 트래픽(내부) 서브넷, NAT(외부) 서브넷, 외부 전송 서브넷
* Spoke VPC 내 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이

[1개의 프로젝트 내 2개의 Spoke VPC 구성 시 준비 사항]

* 1개의 프로젝트
* 3개의 VPC(Hub VPC, Spoke1 VPC, Spoke2 VPC)
* Hub VPC 내 3개의 서브넷
    * 트래픽(내부) 서브넷, NAT(외부) 서브넷, 외부 전송 서브넷
* Spoke1 VPC, Spoke2 VPC 내 각각 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이

[1개 이상의 프로젝트 구성 시 준비 사항]

* 2개의 프로젝트
* 2개의 VPC(각 프로젝트에 Hub VPC, Spoke VPC)
* Hub VPC 내 3개의 서브넷
    * 트래픽(내부) 서브넷, NAT(외부) 서브넷, 외부 전송 서브넷
* Spoke VPC 내 최소 1개의 서브넷
* Hub VPC의 Routing에 연결된 인터넷 게이트웨이

[단일 VPC 내 여러 개의 서브넷 구성 시 준비 사항]

* 1개의 프로젝트
* 1개의 VPC
* 3개의 Hub 서브넷
    * 트래픽(내부) 서브넷, NAT(외부) 서브넷, 외부 전송 서브넷
* 최소 1개의 Spoke 서브넷
* VPC의 Routing에 연결된 인터넷 게이트웨이

> [참고]
> 
>* 위의 서비스 자원은 [Network] 카테고리에서 생성 가능합니다.
>* Network Firewall 생성은 프로젝트당 1개씩만 생성 가능합니다.

### Network Firewall 생성

1. **Security > Network Firewall**로 이동합니다.
2. 각 필수 항목을 모두 선택하고 하단의 **Network Firewall 생성**을 클릭합니다.
    * RBAC: 인스턴스 객체 조회, Network Firewall 서비스 제공에 필요한 API 권한을 부여
    * 구성 방식: 단일 구성과 이중화 구성을 선택합니다.
    * VPC: Network Firewall에서 사용할 VPC
    * 서브넷: Network Firewall에서 내부 트래픽 제어를 위해 사용할 서브넷
    * NAT: Network Firewall에서 외부 트래픽 제어를 위해 사용할 서브넷
    * 외부 전송: Network Firewall에서 생성된 트래픽과 로그를 전송할 서브넷
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.07.12/nfw_add.png" height="60%">


> [참고]
> 
>* 생성된 Network Firewall은 사용자의 프로젝트에 노출되지 않습니다. 
>* 서브넷, NAT, 외부 전송에 사용하는 서브넷은 모두 다른 서브넷으로 선택해야 합니다.
>   * 가급적 NHN Cloud 콘솔에서 생성할 수 있는 최소 단위(28비트)로 생성할 것을 권장합니다.
>* Network Firewall이 속할 VPC의 라우팅 테이블에 인터넷 게이트웨이가 연결되어 있어야 생성 가능합니다.
>* Network Firewall 서비스는 가용 영역을 분리하여 이중화를 기본으로 제공합니다.
>* Security Groups와는 별개의 서비스이므로 Network Firewall을 사용하면 두 서비스를 모두 허용해야 인스턴스에 접근할 수 있습니다.
>* Network Firewall이 소유하고 있는 CIDR 대역과 연결이 필요한 CIDR 대역은 중복되지 않아야 합니다.
>* **Network > Network Interface**에서 Virtual_IP 타입으로 생성되어 있는 IP는 Network Firewall에서 이중화 용도로 사용 중이므로 삭제할 경우 통신이 차단될 수 있습니다.
>* 단일 또는 이중화 구성을 선택하여 Network Firewall을 생성한 뒤 변경이 필요할 경우 **옵션** 탭에서 구성을 변경할 수 있습니다. 하지만 가용성 영역은 변경이 불가능하므로 이중화 구성의 경우 가급적 가용성 영역을 분리하여 구성하세요.

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
        * 피어링 게이트웨이 연결에 대한 자세한 사항은 [사용자 가이드](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)를 참조하세요.
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
> 
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
> 
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
> 
> **연결 설정**의 **5**와 같이 Spoke VPC2-Hub 간 VPC 피어링에도 라우트 추가 설정이 필요합니다.

위의 라우팅 설정이 완료되면 서로 다른 Spoke VPC 간 Network Firewall을 경유하여 사설 통신을 할 수 있습니다. (<strong>Network Firewall > 정책</strong> 탭에서 ACL 추가 필요)
Network Firewall 서비스 구성도를 참고하여 고객의 환경에 맞게 연결을 설정하세요.
<br>

***

## 인스턴스 접속
Network Firewall을 생성하고 연결 설정을 모두 완료한 후 Network Firewall을 경유하여 인스턴스에 접속할 수 있습니다.

예를 들어, 1개의 프로젝트 내 2개의 Spoke VPC로 3개의 서브넷을 구성하고, 외부에서 웹방화벽 접속이 필요할 경우 아래와 같이 NAT, ACL을 설정합니다.

<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/instance-access.png" height="65%" />

> [설정 방법]
>
> * **Network Firewall > NAT** 탭으로 이동
> * **추가** 버튼 클릭 후 NAT 설정
>   * 설정 전 **객체** 탭에서 목적지 IP 객체 생성과 여분의 플로팅 IP 필요 
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/nat-add.png" height="65%" />
> * **Network Firewall > 정책 > ACL** 탭에서 필요한 ACL을 허용
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/access_acl.png" height="65%" />  

위와 같이 설정 후 출발지 IP를 보안 그룹에서 허용하면 인스턴스에 접속 가능합니다.

***

## 정책
Network Firewall을 생성하면 정책 탭으로 이동합니다.

**정책** 탭에서는 Network Firewall과 연결된 VPC 간 트래픽과 인바운드/아웃바운드 트래픽을 제어할 수 있는 **ACL**과 트래픽의 경로를 지정할 수 있는 **라우트**를 설정할 수 있습니다.

![main_page.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/main_page_1.png)

> [참고]
>
> * default-deny는 필수 정책이며, 수정하거나 삭제할 수 없습니다.
> * default-deny 정책을 통해 차단된 로그는 **옵션** 탭의 **기본 차단 정책 로그 설정**을 **사용**으로 변경한 후 **로그** 탭에서 확인 가능합니다.

## ACL

### 추가

* 출발지, 목적지, 목적지 포트를 기반으로 정책을 추가할 수 있습니다.
    * 이미 만들어진 객체를 통해 출발지, 목적지, 목적지 포트를 선택합니다.
* 정책의 상태(활성화/비활성화)와 동작(허용/차단), 스케줄을 설정 및 정책별 로깅 여부 등의 옵션을 설정하여 정책을 추가할 수 있습니다.
* 스케줄 기능은 정책의 상태를 활성화 한 이후에 동작합니다(정책이 비활성화되어 있을 경우 스케줄 기능이 적용되지 않습니다.).

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/acl_add.png)

### 복사

* **복사**를 클릭해 정책을 복사할 수 있습니다.
    * 복사된 정책은 비활성화됩니다.

![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### 수정

* **수정**을 클릭해 정책을 수정할 수 있습니다.


### 이동

* **이동**을 클릭해 정책을 이동할 수 있습니다.
    * default-deny 정책 아래로는 이동이 불가능합니다.

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### 정책 삭제

* **삭제**를 클릭해 정책을 삭제할 수 있습니다.

>[주의]
> 
>한번 삭제한 정책은 복구할 수 없으며, default-deny 정책은 삭제할 수 없습니다.

### 정책 일괄 다운로드

* 정책 탭에 생성되어 있는 정책 전체를 한 번에 다운로드할 수 있습니다.

### 정책 일괄 등록

* 내려받은 템플릿을 사용하여 정책을 한 번에 등록할 수 있습니다.

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)

## 라우트
사진 추가

> [참고]
> 
> * Network Firewall의 기본 게이트웨이는 NAT 이더넷이며, 수정이나 삭제할 수 없습니다.
> * 라우트 설정이 변경될 경우 통신에 문제가 있을수 있으므로 유의하여 설정하세요.  

### 추가

* **추가**를 클릭해 이더넷을 선택하고, 목적지와 게이트웨이를 입력합니다. 
    * 목적지: 서브넷 형식으로 입력
    * 이더넷: NAT, TRAFFIC, VPN(IPSec VPN 기능 사용시) 중 선택
    * 게이트웨이: 호스트 형식으로 입력

> [참고]
> 
> * 이더넷을 VPN으로 선택할 경우 게이트웨이는 지정하지 않아도 됩니다.
> * IPSec VPN과 연동된 사설 IP 대역에 대한 라우트 설정은 반드시 이더넷을 VPN으로 설정하세요.


### 수정

* **수정**을 클릭해 라우트를 수정할 수 있습니다.

### 삭제

* **삭제**를 클릭해 라우트를 삭제할 수 있습니다.

***

## 객체

**객체** 탭에서는 정책을 생성할 때 사용할 IP, 포트, 도메인을 생성하고 관리합니다.

### 추가

* 필수 항목을 입력하여 객체를 생성합니다.
    * 객체는 IP, 포트, 도메인의 3가지 형태로 추가할 수 있습니다.

> [참고]
> 
> * 그룹 객체 생성시 그룹 객체는 추가할 수 없습니다. (단일이나 범위 객체만 선택하여 추가 가능)
> * 도메인 객체는 아래와 같이 활용할 수 있습니다.
>   * 목적지 도메인의 IP 주소가 여러개일 때 자동으로 IP를 수집하여 허용 또는 차단(수집 주기: 5분)

### 수정
* **수정**을 클릭해 객체를 수정할 수 있습니다.
    * 타입은 수정이 불가능합니다.

### 삭제

* **삭제**를 클릭해 객체를 삭제할 수 있습니다.

    * 자동으로 Network Firewall에서 생성한 객체는 수정이나 삭제할 수 없습니다.

>[주의]
> 
>정책에서 사용 중인 객체는 삭제 후 ALL 객체로 변경됩니다.

### 인스턴스 객체 추가
* Network Firewall이 생성된 프로젝트 내에 있는 인스턴스를 활용하여 객체를 추가할 수 있습니다.

> [참고]
>
> 인스턴스와 관계없이 단순히 인스턴스의 이름과 사설 IP 주소만 참고하여 객체를 생성합니다.(생성 후에는 객체 탭에서 관리)


### 객체 일괄 다운로드

* **객체** 탭에 생성되어 있는 IP와 포트 객체 전체를 각각 한 번에 다운로드할 수 있습니다.

## NAT

**NAT**(네트워크 주소 변환) 탭에서는 외부에서 접속할 인스턴스와 전용으로 사용할 공인 IP를 선택하여 연결합니다.

>[참고]
> 
> * NAT는 목적지 기반 및 1:1 방식만 제공합니다.
> * 포트 기반의 NAT는 제공하지 않습니다.
> * NAT를 생성한 뒤 **정책** 탭에 허용 정책을 추가해야만 공인 통신이 가능합니다.
> * NHN Cloud(공공기관용)에서 제공하는 SSL VPN 서비스와 Network Firewall을 연동하여 사용할 수 있습니다. (**옵션 > SSL VPN 설정**에서 **사용**으로 설정 시)
> * NAT에 설정된 NAT 후 사설 IP를 소유한 인스턴스에 직접 Floating IP를 할당할 경우 통신에 문제가 있을 수 있습니다.
> * NAT 삭제 후 사용하지 않는 NAT 전 공인 IP는 **Network > Floating IP**에서 직접 삭제하세요.

### 추가

* **추가**를 클릭해 NAT를 생성합니다.
    * 타입을 선택합니다. 
    * NAT 전 공인 IP는 **Network > Floating IP**에서 미리 생성한 IP 중 하나를 선택합니다.  
    * NAT 후 사설 IP에서 선택할 객체는 **객체** 탭에서 미리 생성해야만 **추가**를 클릭해 추가할 수 있습니다.


![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/nat_add.png)

>[참고]
> 
> * 옵션 - SSL VPN 설정에서 사용으로 설정했을 경우에만 타입이 노출됩니다.
> * 타입의 선택에 따라 아래의 NAT 전 공인 IP가 노출됩니다.
>   * Network Firewall: **Network > Floating IP**에서 Public Network 로 생성된 Floating IP
>   * SSL VPN: **Network > Floating IP**에서 VPN Network로 생성된 Floating IP
> * 인스턴스 접속은 NAT를 추가하면서 설정한 NAT 전 공인 IP로 접속 가능합니다. (인스턴스에 직접 Floating IP 연결 불필요)

### 수정

* **수정**을 클릭해 생성된 NAT를 수정합니다.
    * 공인 IP와 사설 IP 모두 수정할 수 있습니다.

### 삭제

* **삭제**를 클릭해 생성된 NAT를 삭제합니다.

## VPN

**VPN** 탭에서는 사이트간 암호화된 터널을 통해 안전한 사설 통신을 지원합니다.

### 게이트웨이 생성

* **게이트웨이 생성**을 클릭해 피어 VPN 장비와 연결하기 위한 게이트웨이를 생성합니다.

![gw_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/gw_add.png)

> [참고]
>
> * VPC와 서브넷은 수정할 수 없습니다.
> * 게이트웨이는 최대 10개까지 생성 가능합니다.

### 게이트웨이 수정

* **수정** 버튼을 클릭해 게이트웨이를 수정합니다.

### 게이트웨이 삭제

* **삭제** 버튼을 클릭해 게이트웨이를 삭제합니다.
    * 게이트웨이에 연결된 터널이 있을 경우 삭제가 되지 않으므로 불필요한 터널을 삭제한 후 게이트웨이를 삭제 하세요.

### 플로팅 IP 연결

* 피어 장비와 연결에 필요한 플로팅 IP를 설정합니다.
    * 플로팅 IP는 **Network > Floating IP**에 생성된 목록중 미사용 중인 항목이 노출됩니다.

![fip.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/fip.png)

### 터널 생성

* 피어 장비와 연결할 터널을 생성합니다.

![tunnel_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/tunnel_add.png)

* 터널 설정
    * 게이트웨이: 게이트웨이 탭에서 생성된 게이트웨이가 노출되며, 터널과 연결할 게이트웨이를 선택합니다.
        * 생성된 게이트웨이가 없을 경우 노출되지 않습니다.
    * 피어 IP 주소: 연결할 피어 VPN 장비 IP 주소를 입력합니다.
    * IKE 버전: 피어 VPN 장비와 동일한 버전으로 설정합니다.
        * IKE 버전 1은 Main Mode만 지원됩니다.
    * Pre-Shared Key: 피어 VPN 장비와 동일한 키값을 입력합니다.
    * DPD(dead peer detection): 10초 단위로 총 5회의 재전송을 시도하며, 비활성화 선택 시 피어 VPN 장비의 DPD 요청에 대한 응답만 지원합니다.
    * NAT-Traversal: 터널 생성간 발생되는 패킷의 삭제를 방지하기 위한 기능으로 일반적으로 피어 VPN 장비가 공인 IP 일경우 사용으로 설정합니다.
* Phase 1/2 설정
    * IPSec VPN 터널을 생성하기 위해 필요한 설정 정보를 입력합니다.

 > [설정 시 주의 사항]
 >
 > * 모든 설정은 피어 VPN 장비와 동일하게 설정합니다.
 > * 로컬 ID는 피어 VPN 장비의 설정 방식에 따라 선택적으로 설정합니다.
 > * Phase 2 추가는 최대 3개까지 가능합니다.
 > * Phase 2의 프라이빗 IP는 /24비트 이하로 설정하세요. /24비트 이상의 값을 설정해야 할 경우 서브넷 범위를 사전에 확인하여 서브넷의 시작 IP로 입력하세요.
 >   * [예시]
 >       * 192.168.100.0/20 (X) → 192.168.96.0/20 (O)
 >       * 172.16.30.0/21 (X) → 172.16.24.0/21 (O)
 >       * 10.0.50.0/22 (X) → 10.0.48.0/22 (O)
 > * 로컬 프라이빗 IP와 피어 프라이빗 IP는 서로 중복되지 않아야 합니다. 이 범위는 VPC 피어링을 포함한 Network Firewall과 연결되는 모든 사설 대역이 포함됩니다.
 > * 아래의 CIDR은 로컬 프라이빗 IP와 피어 프라이빗 IP에 추가할 수 없으며, 추가할 경우 Network Firewall을 경유하는 통신에 문제가 있을 수 있습니다.
 >   * 10.0.0.0/8
 >   * 172.16.0.0/12
 >   * 192.168.0.0/16


### 터널 연결

* 터널 생성이 완료되면 연결 대기 상태로 생성되며, 생성된 터널을 **연결** 버튼을 클릭해 피어 VPN 장비와 연결합니다.

> [참고]
>
> * **상태** 열에서 색상별로 터널의 상태를 확인할 수 있습니다.
 >   * 녹색: 피어 VPN 장비와 정상적으로 연결 중인 상태
 >   * 빨간색: 설정 또는 통신 상태 등의 문제로 피어 VPN 장비 간 연결이 실패된 상태
 >   * 회색: 연결 대기 상태(새로 생성된 터널)
 >   * 주황색: **중지** 버튼을 클릭해 피어 VPN 장비와 연결이 중지된 상태
> * 터널 생성이 완료된 이후 피어 장비의 종류와 설정에 따라 연결 버튼을 클릭하지 않아도 터널이 연결될 수 있습니다.

### 터널 수정

* **수정** 버튼을 클릭해 터널을 수정합니다.
    * 설정값 중 게이트웨이를 제외한 모든 값은 수정이 가능하며, 수정할 경우 피어 VPN 장비도 동일한 값으로 수정해야 합니다.

### 터널 중지

* **중지** 버튼을 클릭해 터널을 중지합니다.
    * 중지할 경우 피어 VPN 장비를 통한 사설 통신이 중단됩니다. 

### 터널 삭제

* **삭제** 버튼을 클릭해 터널을 삭제합니다.

### 이벤트

* 피어 장비와의 터널 연결 시 발생하는 이벤트 로그를 검색할 수 있습니다.

> [참고]
>
> * 이벤트에서는 터널에 대한 이벤트 로그만 검색할 수 있습니다.
> * VPN 터널을 통한 통신 로그 또는 터널 생성과 삭제 등의 감사 로그는 **로그** 탭에서 확인하세요.

## 로그

**로그** 탭에서는 Network Firewall에서 생성된 로그를 검색할 수 있습니다.

### 검색

* 트래픽: Network Firewall을 경유할 때 허용 또는 차단 정책에 의해 생성된 트래픽 로그를 검색
    * 조회는 1개월 단위로 최대 3개월까지의 과거 데이터만 검색 가능합니다.
        * 최대 저장 로그 개수는 800만 개이며, 트래픽의 양에 따라 저장되는 로그의 양이 달라지므로 과거의 데이터가 조회되지 않을 수 있습니다.
    * 별도의 데이터 저장이 필요한 경우 **옵션** 탭의 **로그 원격 전송 설정**을 참고하세요.

* Audit: 정책 생성 및 삭제 등 Network Firewall의 변경 사항에 대한 로그를 검색
    * 조회는 최대 1개월 단위로 검색 가능하며, 조직 서비스인 CloudTrail에서도 검색할 수 있습니다.

### 엑셀 내려받기

* **엑셀 내려받기**를 클릭해 트래픽과 Audit 로그의 검색 결과를 다운로드할 수 있습니다.
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

* 기본 차단 정책 로그 설정: Network Firewall 생성 후 필수로 생성되는 기본 차단 정책 로그의 저장 여부를 선택합니다.
    * 사용 선택 시 기본 차단 정책으로 생성된 로그는 트래픽 로그에서 검색 가능합니다.
* 로그 원격 전송 설정: 원격지로 트래픽 로그를 저장할 수 있는 옵션을 선택합니다.
    * Syslog: 최대 2개의 원격지 주소로 로그를 전송
        * 2개의 원격지는 개별적으로 설정 가능(IP주소, 프로토콜, 포트 번호)
    * Object Storage: NHN Cloud에서 제공하는 Object Storage 서비스로 로그를 전송
    * Log & Crash Search: NHN Cloud에서 제공하는 Log & Crash Search 서비스로 로그를 전송

### 일반 설정

* MTU(maximum transmission unit) 크기 설정: Network Firewall에 연결된 이더넷의 MTU 크기를 설정합니다.
    * 트래픽: NHN Cloud 내부 통신에 사용하는 이더넷(피어링 통신 포함)
    * NAT: 외부 통신에 사용하는 이더넷

> [참고]
> 
> 트래픽, NAT 이더넷의 기본 MTU 크기는 1450Byte입니다.

* 연동 설정: NHN Cloud(공공기관용) 에서 제공하는 SSL VPN, 백신, 백업 서비스를 Network Firewall과 연동하는 옵션을 제공합니다.
    * 사용 시 **정책** 탭에서 ACL 허용과 라우트 설정이 필요하며, 설정에 필요한 IP와 포트 정보는 각 서비스 별 콘솔 사용 가이드를 참조하세요.
        * [SSL VPN](https://docs.gov-nhncloud.com/ko/Security/SSL%20VPN/ko/console-guide/)
        * [백신](https://docs.gov-nhncloud.com/ko/Security/Vaccine/ko/console-guide-gov/)
        * [백업](https://docs.gov-nhncloud.com/ko/Storage/Backup/ko/console-guide-gov/)

> [참고]
> 
> * SSL VPN 서비스를 연동하여 사용할 경우 NHN Cloud(공공기관용)에서 인스턴스 접속 시 사용하는 전용 Floating IP를 콘솔에서 생성 해야 하며, Network Firewall의 **NAT** 탭에서 해당 Floating IP를 설정한 후 인스턴스에 접속할 수 있습니다. (전용 Floating IP를 인스턴스에 직접 할당하지 않음)


* Network Firewall 구성: 단일 또는 이중화로 Network Firewall의 구성 방식을 설정할 수 있습니다.

> [참고]
> 
> * 구성 방식 변경 시 몇 분 정도의 시간이 소요되며, 구성 변경이 완료되기 전까지 서비스에 영향이 있을 수 있습니다.
> * 정책, NAT 등 Network Firewall 변경 작업은 구성 방식 변경이 완료된 뒤 진행할 것을 권장합니다.

* Network Firewall 삭제: 운영 중인 Network Firewall을 삭제할 수 있습니다.
    * Network Firewall은 한국(판교) 리전과 한국(평촌) 리전에서 각각 삭제할 수 있습니다.

> [삭제 시 주의 사항]
> 
> * 운영 중인 Network Firewall을 삭제할 경우 Network Firewall과 연결된 다른 서비스를 고려하여 진행하세요.

## 서비스 비활성화

**프로젝트 관리 > 이용 중인 서비스**에서 Network Firewall 서비스를 비활성화할 수 있습니다.

> [참고]
> 
> * Network Firewall 서비스 비활성화는 한국(판교) 리전과 한국(평촌) 리전에 모두 적용됩니다.
> 예를 들어 Network Firewall 서비스를 동일한 프로젝트의 한국(판교) 리전과 한국(평촌) 리전에 모두 활성화한 경우 두 리전 중 하나의 Network Firewall 서비스만 비활성화할 수 없습니다.
> * 비활성화하려면 한국(판교) 리전과 한국(평촌) 리전에서 각각 Network Firewall을 삭제한 뒤 진행하세요.