## Security > Network Firewall > Overview

Network Firewall is a network security service to safely protect infrastructure assets used by NHN Cloud. 
You can easily use access control specialized for NHN Cloud and the firewall feature without using a separate firewall product.

> [Note]
> The Network Firewall service is only available in the new network environment for the Korea (Pangyo) region.
> Projects created before March 7, 2022 in the Korea (Pangyo) region are in the old network environment before the improvements, so you need to create a new project to use the Network Firewall service.

## Main Features
* Allows you to manage network communication policies efficiently.
    * Controls traffic with a single policy in the stateful manner.
* Allows for safe protection of instances against external attacks with the Hub-Spoke structure.
    * Controls internal traffic and inbound/outbound traffic between VPCs.
* Provides real-time log search and backup for blocking and allowing networks.
    * Provides several backup methods to suit the customer's environment (Syslog, Object Storage, Log & Crach Search).
* Provides high availability (redundancy) for reliable operation.

## Network Firewall Service Configuration
Service configurations can take five forms as follows.

### 1 Project
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/23.12.19/Architecture1.png" height="70%">

### 1 or More Projects
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/23.12.19/Architecture2.png" height="70%" />


### Projects Between Different Regions
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/23.12.19/Architecture3.png" height="70%" />


### 2 Spoke VPCs in 1 Project
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/23.12.19/Architecture5.png" height="50%">


### Multiple Subnets in a Single VPC
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/23.12.19/Architecture4.png" height="50%" />


> [Note]
> * In a project environment in a different region, it can only be configured in the same project. For more information, see the [user guide](https://docs.nhncloud.com/en/Network/Peering%20Gateway/ko/console-guide/).
> 
> * When you configure a service, you cannot associate it with the network environment before improvements.
> For example, if an organization has projects created that use the pre-improvement network environment and projects that use the new network environment, you can create a Network Firewall in the new network environment, but you cannot use the pre-improvement network environment as a Spoke VPC.