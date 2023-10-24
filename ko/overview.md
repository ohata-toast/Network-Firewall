## Security > Network Firewall > 개요

Network Firewall은 NHN Cloud에서 사용하는 인프라 자산들을 안전하게 보호하기 위해 제공하는 네트워크 보안 서비스입니다.
NHN Cloud에 특화된 접근제어를 적용할 수 있고, 별도의 방화벽 제품을 사용하지 않아도 간편하게 방화벽 기능을 사용할 수 있습니다.

## 주요 기능
* 효율적으로 네트워크 통신 정책을 관리할 수 있습니다.
    * Stateful 방식으로 하나의 정책으로 트래픽을 제어합니다.
* Hub - Spoke 구조로 외부 공격으로부터 안전하게 인스턴스를 보호할 수 있습니다.
    * VPC 간 내부 트래픽과 인바운드/아웃바운드 트래픽을 제어합니다.
* 네트워크 차단과 허용에 대한 실시간 로그 검색과 백업 기능을 제공합니다.
    * 고객의 환경에 맞춰 여러 가지 백업 방식을 제공합니다(Syslog, Object Storage, Log & Crach Search).
* 안정적인 운영을 위해 고가용성(이중화)을 제공합니다.

## Network Firewall 서비스 구성도
서비스 구성은 아래의 3가지 형태로 구성할 수 있습니다.

### 1개의 프로젝트
![service_architecrure_1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/architecture_1.PNG)

### 1개 이상의 프로젝트
![service_architecrure_2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/architecture_2.PNG)

### 다른 리전의 프로젝트
![service_architecrure_3](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/architecture_3.PNG)

> [참고]
> 다른 리전의 같은 프로젝트에서만 구성 가능합니다.