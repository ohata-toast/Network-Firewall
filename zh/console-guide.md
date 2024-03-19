## Security > Network Firewall > Console User Guide

This guide describes the procedure for creating Network Firewall and how to use the console after creation.

## Get Started

To use Network Firewall, first enable the Network Firewall service.

## Create Network Firewall

### Prerequisites

The minimum network service resources needed to create a Network Firewall are as follows.

> [Note]
> See the Network Firewall service diagram**in Network Firewall > Overview**.


[Preparations for Configuring 1 Project].

* 1 project
* 2 VPCs (Hub VPC, Spoke VPC)
* 3 subnets in the Hub VPC (Network Firewall subnet, NAT subnet, and external transport subnet)
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC

[Preparations for Configuring 2 Spoke VPCs within One Project]. 

* 1 project
* 3 VPCs (Hub VPC, Spoke1 VPC, Spoke2 VPC)
* 3 subnets in the Hub VPC (Network Firewall subnet, NAT subnet, and external transport subnet)
* At least one subnet each in the Spoke1 VPC and Spoke2 VPC
* Internet gateway connected to Routing in the Hub VPC

[Preparations for configuring more than one project].

* 2 projects
* 2 VPCs (Hub VPC and Spoke VPC for each project)
* 3 subnets in the Hub VPC (Network Firewall subnet, NAT subnet, and external transport subnet)
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC


[Preparations for configuring cross-region projects].

* 1 project
* 2 VPCs (Hub VPC in KR1 region and Spoke VPC in KR2 region)
* 3 subnets in the Hub VPC (Network Firewall subnet, NAT subnet, and external transport subnet)
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC


[Preparations for configuring multiple subnets within a single VPC].

* 1 project
* 1 VPC
* 3 hub subnets (Network Firewall subnet, NAT subnet, and external transport subnet)
* At least one spoke subnet
* Internet gateway connected to Routing in the VPC


> [Note]
>* The above service resources can be created in the [Network] category.
> 
>* Only one network firewall can be created per project.

### Create Network Firewall

1. Go to **Security > Network Firewall**.
2. Select all required items and click **Create Network Firewall** at the bottom.
    * RBAC: Grant API permissions to query instance objects and provide the Network Firewall service
    * VPC: VPC that Network Firewall will use
    * Subnet: Subnet that Network Firewall uses to control internal traffic
    * NAT: Subnet that Network Firewall uses to control external traffic
    * External transmission: Subnet that sends traffic and logs created by Network Firewall
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/nfw-create.png" height="60%">


>[Note]
> 
>* The created Network Firewall is not exposed in users’ projects.
>   * It is recommended to create subnets in the minimum unit (28 bits) that can be created in the NHN Cloud console.
>* An Internet gateway must be connected to the routing table of the VPC to which the Network Firewall will belong before it can be created.
>* Network Firewall is provided with redundancy by separating availability zones.
>* If you use the Network Firewall service as a separate service from Security Groups, you must allow both to access the instances.
>* The CIDR block owned by Network Firewall and the CIDR block requiring connectivity must not overlap.
>* IPs created with the Virtual_IP type **in Network > Network Interface**are used by Network Firewall for redundancy purposes, so deleting them may block communication.

### Connection Settings
> [Example]
When the VPC (Hub) used by Network Firewall is 10.0.0.0/24, and the VPC (Spoke) that needs to be connected to the Network Firewall is 172.16.0.0/24.

1. Go to **Network > Routing**, select the Spoke VPC, and change the routing table.
    * After selecting Spoke VPC, click **Change Routing Table** to change to the Centralized Virtual Routing (CVR) method.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings1.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings2.png" height="50%" />
<br>

2. Go to **Network > Peering Gateway** to create a peering.
    * If the Spoke VPC is a different project, create a project peering.
    * If the Spoke VPC is in a different region, create a region peering.
    * If the spoke VPCs are the same project, create a peering.
        * For more information on connecting a peering gateway, please see the [](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)user guide[](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/).
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />
<br>

3. Go to **Network > Routing**, select a Hub VPC, and set up the routing as follows.
    * Destination CIDR: 172.16.0.0/24
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

4. Go to **Network > Routing*, select a Spoke VPC, and set up the routing as follows.
    * Destination CIDR: 0.0.0.0/0
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings6.png" height="65%" />
<br>

5. Go to **Network > Peering Gateway > Project Peering**.
    * Select the created peering and go to the **Route** tab.
    * Click the **Peer** or **Change Local Route** to set up routing as follows.
        * Destination CIDR: 0.0.0.0/0
       * Gateway: NetworkFirewall\_INF\_TRAFFIC\_VIP
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings7.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings8.png" height="50%" />

Once the above routing settings are complete, instances in the Spoke VPC will be able to communicate publicly through the Network Firewall. (Requires adding NAT in **Network Firewall > NAT**)
<br>

***
<br>

**If the Spoke VPC has two or more subnets and traffic control between subnets is required through Network Firewall**, add the routing as follows.

> [Example]
> When the subnets of Spoke VPC (172.16.0.0/24) are 172.16.0.0/25 and 172.16.0.128/25

* Go to **Network > Routing**, and select Spoke VPC and add the two routings as follows.
    * Destination CIDR: 172.16.0.0/25 and 172.16.0.128/25
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings9.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings10.png" height="65%" />
Once the above routing settings are complete, private communication between subnets within the Spoke VPC can be made through the Network Firewall. (Requires adding a policy in**Network Firewall > Policies** tab)
<br>

***
<br>

**If there are two or more spoke VPCs**, add the routing as follows.

> [Example]
> With Spoke VPC1 (172.16.0.0/24) and Spoke VPC2 (192.168.0.0/24)

* Go to **Network > Routing** to select a Hub VPC, and add the two routings as follows.
    * Spoke VPC 1
        * Destination CIDR: 172.16.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC1
    * Spoke VPC 2
        * Destination CIDR: 192.168.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC1
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings11.png" height="65%" />


> [Note]
> VPC peering between Spoke VPC2-Hub also requires the Add Route setting, as shown in **5****in Connection Settings**.

After the above routing settings are completed, communication between different Spoke VPCs can be private through Network Firewall.(Requires adding a policy in **Network Firewall > Policy**)
Please refer to the Network Firewall service configuration diagram to set up the connection according to your environment.
<br>

***

Once you've created Network Firewall and established a connection, you can leverage the many features of Network Firewall to configure access control.
<br>


## Policy
When you create a Network Firewall instance, you will be moved to the initial policy page.

In the **Policies** tab, you can manage policies to control inbound/outbound traffic and traffic between the VPCs connected to your Network Firewall instance.

### Main Page

* The default-deny policy is a required policy and cannot be modified or deleted.

>[Note]
> Logs blocked through the default-deny policy can be viewed on the **Log** tab after changing the **Default Blocking Policy Log Setting**to **Enable**on the **Options** tab.

![main_page.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/main_page_1.png)

### Add Policy

* Add policies based on departure, destination, and destination port.
    * Select the departure, destination, and destination port through already created objects.
* Add a policy by selecting the policy's status (enabled/disabled), action (allowed/blocked), and schedule.
* The schedule feature works after enabling the policy's status (If the policy's status is disabled, the schedule feature does not apply).

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_add_1.png)

### Copy Policy

* Click **Copy**to copy the policy.
    * Copied policies will be disabled.

![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### Modify Policy

* You can modify the policy by clicking **Edit**.

![acl_edit.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_edit_1.png)


### Move Policy

* You can move the policy by clicking **Move**.
    * Name: Could not move below the default-deny policy.

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### Delete Policy

* You can delete the policy by clicking **Delete**.

>[Caution]
>Once deleted, a policy cannot be restored, and a policy with name: default-deny cannot be deleted.

### Batch Download of Policies

* Download all policies created in the Policies tab at once.

### Batch Register Policies

* You can register policies at once using the downloaded template.

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)

## Object

In the **Object** tab, create and manage IPs and ports to use when creating policies.

### Add

* Enter the required fields to create the object.
IP and port are required, you can add a type and protocol below.

    * IP
        * Type: Subnet, Range, Group
    * Port
        * Type: Port, Range, Group
        * Protocol: TCP, UDP, ICMP

### Delete

* You can delete an object by clicking **Delete**.

    * Objects automatically created by Network Firewall cannot be modified or deleted.

>[Note]
>Objects in use by a policy will be changed to ALL objects after deletion (caution required).

### Batch Download of Objects

* Download all IPs and port objects created in the **Object** tab at once.

## NAT

In the **NAT** (Network Address Translation) tab, create a dedicated public IP by specifying the instance to be accessed from the outside.

* NAT offers only destination-based and 1:1 methods.
* Port-based NAT is not provided.
* The created public IP can be checked in **Network > Floating IP**.

>[Note]
After creating a NAT, you must add an allow policy to enable authorized communication.

### Add

* Click  Add (+) to select an object.
    * The object you want to select must be created in advance.
* Click **Confirm** to check the connected **public IP before NAT**.
    * The created public IP before NAT cannot be changed arbitrarily.

![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/nat_add_1.png)

### Delete

* Click **Delete** to delete the created NAT.
    * After deletion, the public IP before NAT is automatically deleted.

## Log

In the **Log** tab, search logs created in Network Firewall.

### Search

* Traffic: Search traffic logs generated by allow or block policies when passing through the Network Firewall.
    * You can search only historical data up to 3 months in 1-month increments.
        * The maximum number of stored logs is 8 million, and the amount of logs stored depends on the amount of traffic, so past data may not be retrieved.
    * If separate data storage is required, see the **log remote transmission settings** in the **Options** tab.

* Audit: Search logs for changes to Network Firewall, including policy creation and deletion.
    * You can search for up to one month, and can search through CloudTrail, an organizational service.

### Download Excel

* Download traffic and audit log search results through **Download Excel**.
    * The maximum number of downloads in a traffic log is 300,000.

## Monitor

In the **Monitor** tab, check the status of Network Firewall in real time.
Searches are only available for up to 24 hours (1 day).

### Search

* Sessions: Quantity of sessions currently in use through Network Firewall.
* Network Usage: Inbound/outbound traffic currently passing through Network Firewall

## Options

In the **Options** tab, set options required for operation of Network Firewall.

### Log Settings

* Default blocking policy log settings: Select whether to save the default blocking policy log that is required after creating a Network Firewall.
    * When enabled, you can search logs created with the default blocking policy in the traffic log.
* Log remote transmission settings: Select the option to save traffic logs remotely.
    * Syslog: Send logs with up to 2 remote addresses
        * Two remote locations can be configured individually (IP address, protocol, port number)
    * Object Storage: Send logs with the Object Storage service provided by NHN Cloud
    * Log & Crash Search: Send logs with the Log&Crash Search service provided by NHN Cloud

### General Settings

* Maximum transmission unit (MTU) size setting: Set the MTU size of the Ethernet owned by Network Firewall.
    * Traffic: Ethernet used for internal NHN Cloud communication (including peering communication)
    * NAT: Ethernet used for external communication

> [Note]
> The default MTU size for traffic, NAT Ethernet is 1450 bytes.

## Disable Service

You can disable the Network Firewall service in **Project Management > Services in Use**.

> [Precautions]
> 
> * Disabling the Network Firewall service applies to both the Pangyo and Pyeongchon regions.
> For example, if you enable the Network Firewall service for both the Pangyo and Pyeongchon regions of the same project, you cannot disable the Network Firewall service for only one of the two regions. (Improvement scheduled)