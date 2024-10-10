## Security > Network Firewall > Console User Guide

This guide describes the procedure for creating Network Firewall and how to use the console after creation.

## Get Started

To use Network Firewall, first enable the Network Firewall service.

## Create Network Firewall

### Prerequisites

The minimum network service resources needed to create a Network Firewall are as follows.

> [Note]
> See the Network Firewall service diagram in **Network Firewall > Overview**.


[Preparations for Configuring 1 Project]

* 1 project
* 2 VPCs (Hub VPC, Spoke VPC)
* 3 subnets in Hub VPC
    * Traffic (internal) subnet, NAT (external) subnet, external transit subnet
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC

[Preparations for Configuring 2 Spoke VPCs within One Project].

* 1 project
* 3 VPCs (Hub VPC, Spoke1 VPC, Spoke2 VPC)
* 3 subnets in Hub VPC
    * Traffic (internal) subnet, NAT (external) subnet, external transit subnet
* At least one subnet each in the Spoke1 VPC and Spoke2 VPC
* Internet gateway connected to Routing in the Hub VPC

[Preparations for configuring more than one project].

* 2 projects
* 2 VPCs (Hub VPC and Spoke VPC for each project)
* 3 subnets in Hub VPC
    * Traffic (internal) subnet, NAT (external) subnet, external transit subnet
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC


[Preparations for configuring cross-region projects].

* 1 project
* 2 VPCs (Hub VPC in KR1 region and Spoke VPC in KR2 region)
* 3 subnets in Hub VPC
    * Traffic (internal) subnet, NAT (external) subnet, external transit subnet
* At least one subnet in the Spoke VPC
* Internet gateway connected to Routing in the Hub VPC


[Preparations for configuring multiple subnets within a single VPC].

* 1 project
* 1 VPC
* 3 Hub Subnets
    * Traffic (internal) subnet, NAT (external) subnet, external transit subnet
* At least one spoke subnet
* Internet gateway connected to Routing in the VPC


> [Note]
>* The above service resources can be created in the [Network] category. 
>* Only one network firewall can be created per project.

### Create Network Firewall

1. Go to **Security > Network Firewall**.
2. Select all required items and click **Create Network Firewall** at the bottom.
    * RBAC: Grant API permissions to query instance objects and provide the Network Firewall service
    * Configuration type: Select a single configuration and configure redundancy.
    * VPC: VPC that Network Firewall will use
    * Subnet: Subnet that Network Firewall uses to control internal traffic
    * NAT: Subnet that Network Firewall uses to control external traffic
    * External transmission: Subnet that sends traffic and logs created by Network Firewall
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/create.png" height="60%" />


> [Notes before Creation]
>* The created Network Firewall is not exposed in users’ projects. 
>* The subnets used for subnet, NAT, and external transmission must all be selected as different subnets.
>   * It is recommended to create subnets in the minimum unit (28 bits) that can be created in the NHN Cloud console.
>* An Internet gateway must be connected to the routing table of the VPC to which the Network Firewall will belong before it can be created.
>* If you use the Network Firewall service as a separate service from Security Groups, you must allow both to access the instances.
>* The CIDR block owned by Network Firewall and the CIDR block requiring connectivity must not overlap.
>* IPs created with the Virtual_IP type **in Network > Network Interface**are used by Network Firewall for redundancy purposes, so deleting them may block communication.
>* After you create Network Firewall by selecting a single or redundancy configuration, you can change the configuration on the **Options** tab if you need to make changes. However, availability zones cannot be changed, so for redundancy configurations, configure separate availability zones whenever possible. 

### Connection Settings

> [Example]
When the VPC (Hub) used by Network Firewall is 10.0.0.0/24, and the VPC (Spoke) that needs to be connected to the Network Firewall is 172.16.0.0/24.

1. Go to <strong>Network > Routing</strong>, select the Spoke VPC, and change the routing table.
    * After selecting Spoke VPC, click <strong>Change Routing Table</strong> to change to the Centralized Virtual Routing (CVR) method.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings1.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings2.png" height="50%" />
<br>

2. Go to <strong>Network > Peering Gateway</strong> to create a peering.
    * If the Spoke VPC is a different project, create a project peering.
    * If the Spoke VPC is in a different region, create a region peering.
    * If the spoke VPCs are the same project, create a peering.
        * For more information on connecting a peering gateway, please see the [User Guide](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/).
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />
<br>

3. Go to <strong>Network > Routing</strong>, select a Hub VPC, and set up the routing as follows.
    * Destination CIDR: 172.16.0.0/24
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

4. Go to <strong>Network > Routing</strong>, select a Spoke VPC, and set up the routing as follows.
    * Destination CIDR: 0.0.0.0/0
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings6.png" height="65%" />
<br>

5. Go to <strong>Network > Peering Gateway > Project Peering</strong>.
    * Select the created peering and go to the **Route** tab.
    * Click the **Peer** or **Change Local Route** to set up routing as follows.
        * Destination CIDR: 0.0.0.0/0
        * Gateway: NetworkFirewall_INF_TRAFFIC_VIP
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings7.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings8.png" height="50%" />

Once the above routing settings are complete, instances in the Spoke VPC will be able to communicate publicly through the Network Firewall. (Requires adding NAT in <strong>Network Firewall > NAT</strong>)
<br>

***
<br>

**If the Spoke VPC has two or more subnets and traffic control between subnets is required through Network Firewall**, add the routing as follows.

> [Example]
When the subnets of Spoke VPC (172.16.0.0/24) are 172.16.0.0/25 and 172.16.0.128/25

* Go to <strong>Network > Routing</strong>, and select Spoke VPC and add the two routings as follows.
    * Destination CIDR: 172.16.0.0/25 and 172.16.0.128/25
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings9.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings10.png" height="65%" />
Once the above routing settings are complete, private communication between subnets within the Spoke VPC can be made through the Network Firewall. (Requires adding a policy in<strong>Network Firewall > Policies</strong> tab)
<br>

***
<br>

**If there are two or more spoke VPCs**, add the routing as follows.

> [Example]
With Spoke VPC1 (172.16.0.0/24) and Spoke VPC2 (192.168.0.0/24)

* Go to <strong>Network > Routing</strong> to select a Hub VPC, and add the two routings as follows.
    * Spoke VPC 1
        * Destination CIDR: 172.16.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC1
    * Spoke VPC 2
        * Destination CIDR: 192.168.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC1
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings11.png" height="65%" />


> [Note]
VPC peering between Spoke VPC2-Hub also requires the Add Route setting, as shown in **5****in Connection Settings**.

<br>

After the above routing settings are completed, communication between different Spoke VPCs can be private through Network Firewall.(Requires adding a policy in <strong>Network Firewall > Policy</strong>)
Please refer to the Network Firewall service configuration diagram to set up the connection according to your environment.
<br>

***

## Connect to Instance
After creating Network Firewall and complete all connection settings, you can access your instance through the Network Firewall.

For example, if you configure 3 subnets with 2 Spoke VPCs in 1 project and need web firewall access from outside, set up NAT and ACLs as shown below.

<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/instance-access
.png" height="65%" />

> [How to set up]
> * Go to **Network Firewall > NAT** tab
> * Click **Add** and set up NAT
>   * Create a Destination IP object on the **Objects** tab before setup and need a spare floating IP 
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/nat-add.png" height="65%" />
* Allow the required ACLs on the **Network Firewall > Policies > ACLs** tab
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/access_acl.png" height="65%" />  
> 
After setting up as above, you can access the instance if the departure IP is allowed in the security groups.

***

## Policy
After creating Network Firewall, go to the **Policies** tab.

![policy-default.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/policy-default.png)

> [Note]
> * The default-deny policy is a required policy and cannot be modified or deleted.
> * Logs blocked through the default-deny policy can be viewed on the **Log** tab after changing the **Default blocking policy log setting** to **Enable** on the **Options** tab.


## ACL
On the **ACLs** tab, you can control inbound and outbound traffic and traffic between the Network Firewall and the associated VPCs.
<br/>

### Add

* Add policies based on departure, destination, and destination port.
    * Select the departure, destination, and destination port through already created objects.
* Add policies by setting options such as the status (enabled/disabled) and action (allow/block) of the policy, setting a schedule, and whether or not to log per policy.
* The schedule feature works after you enable the policy's status (if the policy is disabled, the schedule feature does not apply).

![acl_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/acl_add.png)

### Copy

* Click **Copy**to copy the policy.
    * Copied policies will be disabled.

![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)


### Modify

* Modify the policy by clicking **Edit**.


### Move

* Move the policy by clicking **Move**.
    * Could not move below the default-deny policy.

![acl_move.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_move_1.png)

### Delete

* Delete the policy by clicking **Delete**.

> [Caution]
> Once deleted, a policy cannot be restored, and a policy with name: default-deny cannot be deleted.

### Batch Download of Policies

* Download all policies created in the Policies tab at once.

### Batch Register Policies

* Register policies at once using the downloaded template.

![acl_batch.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_batch_1.png)


## Route

On the **Route** tab, specify the path of communication through the Network Firewall.

![policy-route.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/policy-route.png)

> [Note]
> * The default gateway for Network Firewall is NAT Ethernet, which cannot be modified or deleted.
> * If the route settings change, there may be communication issues, so set them carefully.  

### Add

* Click **Add** to select Ethernet, and enter the destination and gateway. 
    * Destination: Enter in subnet format
    * Ethernet: Select from NAT, TRAFFIC, or VPN (when using the IPSec VPN feature)
    * Gateway: Enter in host format

> [Note]
> * If you select Ethernet as VPN, you don't need to specify a gateway.
> * For setting up routes for private IP bands associated with an IPSec VPN, must set Ethernet to VPN.
> * If you see a validation message like the one below when entering the destination subnet, pre-check the subnet range and enter it as the starting IP of the subnet.
>   * [Example]
>       * 192.168.199.0/21 (x) → 192.168.192.0/21 (o)
>       * 172.16.100.0/20 (x) → 172.16.96.0/20 (o)
>       * 10.10.10.130/25 (x) → 10.10.10.128/25 (o)
> 
> ![route_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/route_add.png)

### Modify

* Modify the route by clicking **Edit**.

### Delete

* Delete a route by clicking **Delete**.

***

## Object

In the **Object** tab, create and manage IPs, ports, and domains to use when creating policies.

### Add

* Create an object by entering the required fields.
    * Objects can be added in three forms: IP, port, and domain.

> [Note]
> * Group objects cannot be added when creating a group object (only single or range objects can be added by selecting them).
> * Domain objects can be utilized as follows
>   * Automatically collect IPs to allow or block when the destination domain has multiple IP addresses (collection interval: 5 minutes)

### Modify

* Modify the object by clicking **Edit**.
    * Types cannot be modified.

### Delete

* You can delete an object by clicking **Delete**.
    * Objects automatically created by Network Firewall cannot be modified or deleted.

>[Note]
> Objects in use by a policy will be changed to ALL objects after deletion (caution required).

### Add Instance Objects
* Add an object by using the instances in the project in which Network Firewall is created.

> [Note]
> * Create an object by simply referencing the instance's name and private IP address, regardless of instances (once created, manage on the Object tab).


### Batch Download of Objects

* Download all IPs and port objects created in the **Object** tab at once.

## NAT

In the **NAT** (Network Address Translation) tab, select and connect a dedicated public IP with the instance to be accessed from the outside.

>[Note]
> * NAT offers only destination-based and 1:1 methods.
> * Port-based NAT is not provided.
> * After creating a NAT, you must add an allow policy to enable authorized communication.
> * If you assign a floating IP directly to an instance that owns a private IP after NAT has been set up, there may be communication issues.
> * After deleting NAT, delete the unused public IP before NAT directly from **Network - Floating**.

### Add

* Click **Add** to create NAT.
    * For the public IP before NAT, select one of the pre-created IPs in **Network - Floating IP**.  
    * For the objects to be selected in Private IP after NAT, pre-create them on the **Objects** tab to add by clicking **Add**. 

![nat_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.04.05/nat_add_2.png)

>[Note]
> * Instances can be accessed from the pre-NAT public IP that you set when adding NAT (Not required to connect a floating IP directly to the instance).

### Modify

* Click **Modify** to modify the created NAT.
    * You can modify both public and private IPs.

### Delete

* Click **Delete** to delete the created NAT.

## VPN

The **VPN** tab enables secure, private communication over an encrypted tunnel between sites.

### Create Gateway

* Click **Create Gateway** to create a gateway to connect with peer VPN equipment.

![gw_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/gw_add.png)

> [Note]
> * VPCs and subnets cannot be modified.
> * You can create up to 10 gateways.

### Modify Gateway

* Click **Modify** to modify gateways.

### Delete Gateway

* Click **Delete** to delete gateways.
    * If there is a tunnel connected to gateways, it will not be deleted.

### Associate Floating IP

* Set the floating IP required to connect to the peer equipment.
    * Floating IPs that are not used appear in the list created in **Network > Floating IP**.

![fip.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/fip.png)

### Create Tunnel

* Create a tunnel to connect with the peer device.

![tunnel_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/tunnel_add.png)

* Set up Tunnel
    * Gateway: On the Gateway tab, the created gateways appear, and select the gateway you want to associate with the tunnel.
        * If no gateways are created, they are not exposed.
    * Peer IP address: Enter the IP address of the peer VPN equipment to connect to.
    * IKE version: Set to the same version as the peer VPN equipment.
        * IKE version 1 is only supported in Main Mode.
    * Pre-Shared Key: Enter the same key value as the peer VPN equipment.
    * DPD(dead peer detection): Attempts a total of five retransmissions in 10-second increments and only supports responses to DPD requests from peer VPN equipment when Disabled is selected.
    * NAT-Traversal: Prevents packet dropping that occurs during tunnel creation and is typically enabled when the peer VPN equipment is a public IP.
* Set Phase 1/2
    * Enter the necessary setup information to create an IPSec VPN tunnel.

 > [Precautions for Setup]
 > * Set all settings identically to the peer VPN equipment.
 > * The local ID is optional, depending on how the peer VPN equipment is set up.
 > * You can add up to three Phase 2s.
 > * Set the private IP for Phase 2 to /24 bits or less; if you need to set a value higher than /24 bits, check the subnet range in advance and enter it as the starting IP for the subnet.
 >   * [Example]
 >       * 192.168.100.0/20 (X) → 192.168.96.0/20 (O)
 >       * 172.16.30.0/21 (X) → 172.16.24.0/21 (O)
 >       * 10.0.50.0/22 (X) → 10.0.48.0/22 (O)
 > * The local private IP and peer private IP must not overlap each other. This range includes all private bands that connect to Network Firewall, including VPC peering.
  > * The CIDRs below cannot be added to local private IPs and peer private IPs, and if they are, there may be issues with communication through Network Firewall.
 >   * 10.0.0.0/8
 >   * 172.16.0.0/12
 >   * 192.168.0.0/16

### Connect Tunnel

* When the tunnel is created, it will be created as a pending connection, and you can click **Connect** to connect the created tunnel to the peer VPN equipment.

> [Note]
> * In the **Status** column, you can see the status of the tunnel by color.
 >   * Green: Healthy connection with the peer VPN equipment
 >   * Red: Connection between peer VPN devices fails due to issues with settings or communication status.
 >   * Gray: Waiting for connection (newly created tunnel)
 >   * Orange: Click **Stop** to stop the connection between the peer VPN equipment.
> * After the tunnel creation is complete, depending on the type of peer device and its settings, you may not need to click **Connect** to connect.

### Modify Tunnel

* Click **Modify** to modify the tunnel.
    * All of these values can be modified except the Gateway, and if you do, you must also modify the peer VPN devices to the same values.

### Stop Tunnel

* Click **Stop** to stop the tunnel.
    * If you stop, private communication over the peer VPN device will be stopped. 

### Delete Tunnel

* Click **Delete** to delete the tunnel.

### Event

* You can search event logs that occur during tunnel connections with peer VPN devices.

> [Note]
> * Under Events, you can only search the event log for the tunnel.
> * Check the **Log** tab for logs of communication over the VPN tunnel or audit logs, such as tunnel creation and deletion.


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

* Maximum transmission unit (MTU) size setting: Set the MTU size of the Ethernet associated with Network Firewall.
    * Traffic: Ethernet used for internal NHN Cloud communication (including peering communication)
    * NAT: Ethernet used for external communication

> [Note]
> The default MTU size for traffic, NAT Ethernet is 1450 bytes.

<br>

* Network Firewall configuration: You can set how Network Firewall is configured: single or redundant.

> [Note]
> * Changing your configuration takes a few minutes and may impact your service until the configuration change is complete.
> * It is recommended to make changes to Network Firewall, such as changing policies and NAT after configuration changes are complete.

<br>

* Network Firewall Deletion: You can delete a running Network Firewall.
    * Network Firewall can be deleted from the Korea (Pangyo) region and Korea (Pyongchon) region respectively.

> [Precautions when deleting]
> * If you are deleting a running Network Firewall, consider other services associated with the Network Firewall before proceeding.     

## Disable Service

You can disable the Network Firewall service in **Project Management > Services in Use**.

> [Notes]
> * Disabling the Network Firewall service applies to both the Pangyo and Pyeongchon regions.
> For example, if you enable the Network Firewall service for both the Pangyo and Pyeongchon regions of the same project, you cannot disable the Network Firewall service for only one of the two regions. 
> * To disable, delete Network Firewall from the Korea (Pangyo) region and Korea (Pyeongchon) region before proceeding.
