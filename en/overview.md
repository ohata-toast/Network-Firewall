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
* Provides a secure virtual private network (VPN) over an encrypted tunnel between sites in an Internet environment.   
* Provides real-time log search and backup for blocking and allowing networks.
    * Provides several backup methods to suit the customer's environment (Syslog, Object Storage, Log & Crash Search).
* Provides high availability (redundancy) for reliable operation.

## Network Firewall Service Configuration
You can configure the service in the following five forms.

### 1 Project
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture1.png" height="70%">

### 1 or More Projects
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture2.png" height="70%" width="100%" />


### Projects Between Different Regions
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture3.png" height="70%" width="100%" />


### 2 Spoke VPCs in 1 Project
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture4.png" height="70%" width="100%" />


### Multiple Subnets in a Single VPC
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Architectures/24.10.10/Architecture5.png" height="50%" width="100%" />


> [Note]
>  * The above configuration diagram is a typical configuration, and the configuration of WEB, WAS, Load Balancer, etc. except Network Firewall may differ depending on the customer's environment.
>
> * In a project environment in a different region, it can only be configured in the same project. For more information, see the [user guide](https://docs.nhncloud.com/en/Network/Peering%20Gateway/en/console-guide/).
> 
> * When you configure a service, you cannot associate it with the network environment configured before March 7, 2022.
> For example, if an organization has projects created that uses a network environment configured before 7 March 2022 and another that uses a network environment configureed afterwards, you can create a Network Firewall in the new network environment, but you cannot use the pre-improvement network environment as a Spoke VPC.