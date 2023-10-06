## Security > Network Firewall > Overview

Network Firewall is a network security service to safely protect infrastructure assets used by NHN Cloud. 
You can easily use access control specialized for NHN Cloud and the firewall feature without using a separate firewall product.

## Main Features
* Allows you to manage network communication policies efficiently.
    * Controls traffic with a single policy in the stateful manner.
* Allows for safe protection of instances against external attacks with the Hub-Spoke structure.
    * Controls internal traffic and inbound/outbound traffic between VPCs.
* Provides real-time log search and backup for blocking and allowing networks.
    * Provides several backup methods to suit the customer's environment (Syslog, Object Storage, Log & Crach Search).
* Provides high availability (redundancy) for reliable operation.

## Network Firewall Service Configuration
Service configurations can take three forms as follows.

### 1 Project
![service_architecrure_1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_1.PNG)

### 1 or More Projects
![service_architecrure_2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_2.PNG)

### Project in other regions
![service_architecrure_3](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.26/service_architecture_3.PNG)
> [Note]
Service can only be configured in the same project of different regions.