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
* At least one spoke subnet that does not overlap the hub subnet
* Routing table to connect to the spoke subnet
* Internet gateway connected to Routing in the VPC


> [Note]
>
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
>
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
> When the VPC (Hub) used by Network Firewall is 10.0.0.0/24, and the VPC (Spoke) that needs to be connected to the Network Firewall is 172.16.0.0/24.

1. Go to <strong>Network > Peering Gateway</strong> to create a peering.
    * For more information on connecting a peering gateway, please see the [User Guide]
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings3.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings4.png" height="65%" />

> [Note]
> 
> Create a peering according to the location of the Spoke VPC.
> * If the spoke VPCs are the same project, create a peering.
> * If the Spoke VPC is a different project, create a project peering.
> * If the Spoke VPC is in a different region, create a region peering.

<br>

2. Go to <strong>Network > Routing</strong>, select a Hub VPC, and set up the routing as follows.
    * Destination CIDR: 172.16.0.0/24
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings5.png" height="65%" />
<br>

3. Go to <strong>Network > Routing</strong>, select a Spoke VPC, and set up the routing as follows.
    * Destination CIDR: 0.0.0.0/0
    * Gateway: Gateway of peering type added after peering connection
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings6.png" height="65%" />

> [Note]
> 
> * By setting up routing as above, all communication from the Spoke VPCs will pass through the Network Firewall.
>   * If you need to branch communications, explicitly set a destination that is not 0.0.0.0/0.

<br>

4. Go to <strong>Network > Peering Gateway > Project Peering</strong>.
    * Select the created peering and go to the **Route** tab.
    * Click the **Peer** or **Change Local Route** to set up routing as follows.
        * Destination CIDR: 0.0.0.0/0
        * Gateway: NetworkFirewall_INF_TRAFFIC_VIP
        <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings7.png" height="65%" />
<br>
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.12.19/ConnectionSettings8.png" height="50%" />

Once the above routing settings are complete, instances in the Spoke VPC will be able to communicate publicly through the Network Firewall. (Requires adding NAT in <strong>Network Firewall > NAT</strong>)

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
> VPC peering between Spoke VPC2-Hub also requires the Add Route setting, as shown in **4in Connection Settings**.

<br>

If you **configure spoke subnets in the same VPC**, create a new routing table to associate the subnets and add routes. 
* In **Network > Routing**, create a routing table and add routes.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/routetable_create.png" height="65%" />
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/route_create.png" height="65%" />

<br>

* In **Network > Subnet**, create a new spoke subnet that does not overlap the Network Firewall and associate a routing table with it.
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/subnet_create.png" height="65%" />
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/routetable_connect.png" height="65%" />

<br>

After the above routing settings are completed, communication between different Spoke VPCs can be private through Network Firewall.(Requires adding a policy in <strong>Network Firewall > Policy</strong>)
Please refer to the Network Firewall service configuration diagram to set up the connection according to your environment.

***

## Connect to Instance
After creating Network Firewall and complete all connection settings, you can access your instance through the Network Firewall.

For example, if you configure 3 subnets with 2 Spoke VPCs in 1 project and need web firewall access from outside, set up NAT and ACLs as shown below.

<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/instance-access
.png" height="65%" />

> [How to set up]
>
> * Go to **Network Firewall > NAT** tab
> * Click **Add** and set up NAT
>   * Create a Destination IP object on the **Objects** tab before setup and need a spare floating IP 
> <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/nat-add.png" height="65%" />
* Allow the required ACLs on the **Network Firewall > Policies > ACLs** tab
<img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/access_acl.png" height="65%" />  
> 
After setting up as above, you can access the instance if the departure IP is allowed in the security groups.

<br>>

## Policy
After creating Network Firewall, go to the **Policies** tab.

![policy-default.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.09.12/policy-default.png)

> [Note]
>
> * The default-deny policy is a required policy and cannot be modified or deleted.
> * Logs blocked through the default-deny policy can be viewed on the **Log** tab after changing the **Default blocking policy log setting** to **Enable** on the **Options** tab.

<br>

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
    * Copy: Copy the same policy as the one you want to copy
    * Reverse copy: Copy by changing the source and destination of the policy you want to copy

![acl_copy.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/23.09.07/acl_copy_1.png)

> [Note]
> 
> The copied policy will be disabled. If you have to use it, click **Modify** to enable the policy to use.

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
>
> * The default gateway for Network Firewall is NAT Ethernet, which cannot be modified or deleted.
> * If the route settings change, there may be communication issues, so set them carefully.  

### Add

* Click **Add** to select Ethernet, and enter the destination and gateway. 
    * Destination: Enter in subnet format
    * Ethernet: Select from NAT, TRAFFIC, or VPN (when using the IPSec VPN feature)
    * Gateway: Enter in host format

> [Note]
>
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
<br>

## Object

In the **Object** tab, create and manage IPs and ports to use when creating policies.

### Add

* Create an object by entering the required fields.
    * Objects can be added in two forms: IP and port.

> [Note]
> * Group objects cannot be added when creating a group object (only single or range objects can be added by selecting them).

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

<br>

## NAT

In the **NAT** (Network Address Translation) tab, select and connect a dedicated public IP with the instance to be accessed from the outside.

>[Note]
>
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
 
 <br>

 ## Mirroring
 
 The **Mirroring** tab allows you to copy network packets that pass through the Network Firewall to threat detection and analysis solutions such as IDS/IPS, SIEM, NDR, and others, so that you can detect and respond to network threats in real time.
 
 > [Note]
 **Options -** **Enable****in Mirroring settings**to **enable**and use after activation (takes about 30 seconds to activate)
 <br>
 >     ![Mirorring_Config_Activation_800.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirorring_Config_Activation_800.png)
 
 <br>
 
 ### Mirroring rules
 
 * Add a mirroring rule to send the copied packets to the desired destination terminal.
 ![Mirroring_Rule_Contents_Explain_1_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Contents_Explain_1_900.png)
     * Name: Displays the name you set.
     * Orientation: Displays the orientation you set.
     * Mirror specified interface: Displays the interface of the selected Network Firewall.
     * Mirroring transmit IP: Displays the IP of the mirroring interface.
     * Mirroring target IP: Displays the destination IP to send mirroring packets to.
     * Filter group: Displays the selected filter groups.
     * Status: Displays the status of this mirroring rule via a badge.
         * Active: Active 
         * Inactive: Inactive
     * View Details: Views the details of the mirroring rule you set up.
 
 <br>
 
 ### Add
 
 * You can add a mirroring rule by clicking **Add**.
 ![Mirroring_Rule_Add_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Add_900.png)
     * Status: Sets whether the mirroring rule is active or not.
     * Direction: Sets the incoming/outgoing packets to be mirrored on the mirroring interface. This setting allows you to mirror only packets in a specific direction.
         * Receive (Rx): Packets received on the mirror-designated interface
         * Transmit (Tx): Packets outgoing from the mirror-designated interface.
     * Mirror specified interface: Choose from the interfaces below on the Network Firewall.
         * NetworkFirewall_INF_NAT: Top interface for external control of Network Firewall
         * NetworkFirewall_INF_TRAFFIC: Bottom interface for internal control of Network Firewall
     * Mirroring egress IP: The mirroring interface on the external egress subnet is set to default.
     * Mirror destination IP: Enter the private IP of the destination that will receive mirroring packets.
     * Virtual network identifier (VNI): Type the VNI.
 
 > [Note]
 >
 > * Policies (such as security groups and firewalls) need to allow access to the mirroring egress IP and UDP port 4789 in order for the mirroring target terminal to receive VXLAN packets.
 > * You can create up to three mirroring rules.
 > * When applying mirroring rules, make sure to enter the mirroring destination IP information correctly because it can generate a lot of communication data depending on your environment.
 > * Network Firewall sends mirroring packets over a VXLAN tunnel, which requires a VNI setting. The VNI value can be entered as a number between 1 and 16,777,215 and must be the same as the device being mirrored.
 
 * Select a **filter group**.
     * If you haven't added a filter group before, you can add one by clicking **Add filter group**.
     * See the [filter group description](#%ED%95%84%ED%84%B0%20%EA%B7%B8%EB%A3%B9) for more information.
 ![Mirroring_Rule_Filter_Group_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Mirroring_Rule_Filter_Group_900.png)
 
 > [Note]
 Only one filter group can be applied per rule.
 
 <br>
 
 ### Modify
 
 * You can modify the mirroring rule by clicking **Modify**.
 
 > [Note]
 Only the name, description, status, and filter group can be modified.
 
 <br>
 
 ### Delete
 
 * You can delete a mirroring rule by clicking **Delete**.
 
 <br>
 
 ### Filter Group
 
 * **Filter group** allow you to set filters to apply to mirroring rules so that only the packets you want are sent.
 ![Filter_Group_Contents_Explain_1_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Group_Contents_Explain_1_900.png)
     * Name: Displays the name you set.
     * Associated mirroring rules: Displays mirroring rules that use this filter group.
     * Description: Displays a description.
     * View filter rules: View the rules set for that filter group.
 
 <br>
 
 ### Add
 * You can add a filter group by clicking **Add**.
 ![Filter_Group_Add_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Group_Add_900.png)
     * Define filter rules
         * Priority: The smaller the number, the higher the priority. Apply the rule to send mirroring packets starting with the highest priority.
         * Protocol: Specifies the protocol.
             * ALL: Specifies all protocols. When selected, the From/Destination setting is disabled.
             * TCP: Specifies TCP.
             * UDP: Specifies UDP.
             * ICMP: Specifies ICMP. When selected, the From/Destination port setting is disabled.
         * Origin/destination CIDRs: Set the origin and destination CIDRs.
         * From/Destination Port: Set by selecting ALL, Port, or Port Range.
             * ALL: Specifies all ports.
             * Port: Specifies one port in the range 1-65535.
             * Port range: Specify a range of ports within the range 1-65535.
         * Send or not: Sets whether packets that match the rule are sent or not.
             * Send: Send packets that match the rule.
             * Unsent: No packets matching the rule are sent.
 
 > [Note]
 >
 > * You can delete or add rules by clicking the [-], [＋] buttons for each rule.
 > * You can change the priority of the rule by clicking the up and down buttons on each rule.
 ![Filter_Rule_900.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/Mirroring/25.03.06/Filter_Rule_900.png)
 > * You can set up to 10 filter groups, including the default filter group.
 > * You can set up to 30 filter rules.
 > * Filter rules are applied from highest priority to lowest priority, so packets that have already been applied to a do not send rule will not be applied to the next priority rule.
 
 <br>
 
 ### Modify
 * You can modify the filter group by clicking **Edit**.
 
 <br>
 
 ### Delete
 * You can delete a filter group by clicking **Delete**.
 
 > [Note]
 You cannot delete the default filter group.
 
 <br>

## VPN

The **VPN** tab enables secure, private communication over an encrypted tunnel between sites.

### Create Gateway

* Click **Create Gateway** to create a gateway to connect with peer VPN equipment.

![gw_add.PNG](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.05.27/gw_add.png)

> [Note]
>
> * VPCs and subnets cannot be modified.
> * You can create up to 10 gateways.

### Modify

* Click **Modify** to modify gateways.

### Delete

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
 >
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
>
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
>
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
    <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/OBS_5.png" height="65%" />
      * Access key/secret key: Enter the access key information that can be verified when registering S3 API credentials in the Object Storage service.
      * Bucket name: Enter the name of the container created by the Object Storage service
      * Endpoint: Check the endpoints by region and enter the endpoint according to your location.
      * Region: Check the region-specific name and enter the name according to the region location.
  * Log & Crash Search: Send logs to the Log &amp; Crash Search service provided by NHN Cloud.
 <img src="https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_nfw/24.11.07/LNCS_2.png" height="65%" />
      * AppKey: Enter the AppKey generated after activating the Log &amp; Crash Search service.
  
> [Note]
> * Refer to the [user guide](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/s3-api-guide/#aws-sdk) when setting up Object Storage.
> * When using the Log & Crash Search service, you can leverage the log alarm setting feature to detect abnormal behavior.
For example, you can add an ACL blocking policy for SSH communications to a specific destination to Network Firewall, and then set an alarm condition for logs generated by that policy. (For example, 20 or more SSH connection attempts logs in a one-minute period.)
You can receive an alarm when the conditions you set are met.  

<br>

### General Settings

* Maximum transmission unit (MTU) size setting: Set the MTU size of the Ethernet associated with Network Firewall.
    * Traffic: Ethernet used for internal NHN Cloud communication (including peering communication)
    * NAT: Ethernet used for external communication

> [Note]
> The default MTU size for traffic, NAT Ethernet is 1450 bytes.

<br>

* Mirroring settings: You can choose whether to enable mirroring among the features provided by Network Firewall.
    * When you select Use, the required subnet is the subnet that was used to create the Network Firewall.

> [Note]
> * The IP information of the mirroring interface required for ACL settings can be found **in Network - Network Interface**.
>   * Interface name: NetworkFirewall_INF_MIRRORING_S_NAT_VIP

<br>

* Network Firewall configuration: You can set how Network Firewall is configured: single or redundant.

> [Note]
>
> * Changing your configuration takes a few minutes and may impact your service until the configuration change is complete.
> * It is recommended to make changes to Network Firewall, such as changing policies and NAT after configuration changes are complete.

<br>

* Network Firewall Deletion: You can delete a running Network Firewall.
    * Network Firewall can be deleted from the Korea (Pangyo) region and Korea (Pyongchon) region respectively.

> [Precautions when deleting]
> * If you are deleting a running Network Firewall, consider other services associated with the Network Firewall before proceeding.     

<br>

## Disable Service

You can disable the Network Firewall service in **Project Management > Services in Use**.

> [Note]
>
> * Disabling the Network Firewall service applies to both the Pangyo and Pyeongchon regions.
> For example, if you enable the Network Firewall service for both the Pangyo and Pyeongchon regions of the same project, you cannot disable the Network Firewall service for only one of the two regions. 
> * To disable, delete Network Firewall from the Korea (Pangyo) region and Korea (Pyeongchon) region before proceeding.
