## Security > Network Firewall > 개요

Network Firewall은 NHN Cloud에서 사용하는 인프라 자산들을 안전하게 보호하기 위해 제공하는 네트워크 보안 서비스입니다.
NHN Cloud에 특화된 접근 제어를 적용할 수 있고, 별도의 방화벽 제품을 사용하지 않아도 간편하게 방화벽 기능을 사용할 수 있습니다.


> Network Firewall 서비스는 한국(판교) 리전의 경우 신규 네트워크 환경에서만 이용할 수 있습니다.
> 한국(판교) 리전에서 2022년 3월 7일 이전에 생성한 프로젝트는 개선하기 전의 네트워크 환경이므로 Network Firewall 서비스를 이용하려면 프로젝트를 새로 생성해야 합니다.

## 주요 기능
* 효율적으로 네트워크 통신 정책을 관리할 수 있습니다.
    * Stateful 방식으로 하나의 정책으로 트래픽을 제어합니다.
* Hub - Spoke 구조로 외부 공격으로부터 안전하게 인스턴스를 보호할 수 있습니다.
    * VPC 간 내부 트래픽과 인바운드/아웃바운드 트래픽을 제어합니다.
* 인터넷 환경에서 사이트 간 암호화된 터널을 통해 안전한 가상 사설 네트워크(VPN)를 제공합니다.    
* 네트워크 차단과 허용에 대한 실시간 로그 검색과 백업 기능을 제공합니다.
    * 고객의 환경에 맞춰 여러 가지 백업 방식을 제공합니다(Syslog, Object Storage, Log & Crash Search).
* 안정적인 운영을 위해 고가용성(이중화)을 제공합니다.

## Network Firewall 서비스 구성도
서비스는 아래의 5가지 형태로 구성할 수 있습니다.

### 1개의 프로젝트
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture1.png" height="70%">

### 1개 이상의 프로젝트
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture2.png" height="70%" width="100%" />


### 다른 리전 간 프로젝트
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture3.png" height="70%" width="100%" />


### 1개의 프로젝트 내 2개의 Spoke VPC
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture4.png" height="70%" width="100%" />


### 1개의 VPC 내 여러 개의 서브넷
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture5.png" height="50%" width="100%" />


> [참고]
> 
> * 위의 구성도는 일반적인 구성이며, 고객의 환경에 따라 Network Firewall을 제외한 WEB, WAS, Load Balancer 등의 구성이 다를 수 있습니다.
> 
> * 다른 리전의 프로젝트 환경에서는 같은 프로젝트만 구성 가능합니다. 자세한 내용은 [사용자 가이드](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)를 참조하세요.
> 
> * 서비스 구성 시 2022년 3월 7일 이전에 구성한 네트워크 환경과는 연결할 수 없습니다.
> 예를 들어, 2022년 3월 7일 이전에 구성한 네트워크 환경을 사용하는 프로젝트와 이후에 구성한 네트워크 환경을 사용하는 프로젝트가 생성되어 있을 경우 신규 네트워크 환경에 Network Firewall을 생성할 수 있지만 개선하기 전의 네트워크 환경을 Spoke VPC로 사용할 수 없습니다.