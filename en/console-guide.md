## Security > Network Firewall > Console User Guide

This guide describes the procedure for creating Network Firewall and how to use the console after creation.

## Get Started

To use Network Firewall, first create a Network Firewall instance.

## Create Network Firewall

### Prerequisites

The following network resources are required before creating a Network Firewall.

* 1 project
* 1 VPC
* 3 subnets in the selected VPC
* Internet gateway attached to the selected VPC

**The above resources can be created in the [Network] category.**

### Create Network Firewall

1. Go to **Security > Network Firewall**.
2. Select all required items and click the **Create Network Firewall** button at the bottom.

* RBAC: Assign API permissions to query instance objects and provide the Network Firewall service
* VPC: Select the VPC that the Network Firewall instance will use
* Subnet: Select a subnet that Network Firewall instances will use to control internal traffic
* NAT: Select a subnet that the Network Firewall instances will use to control external traffic
* External transmission: Select a subnet to which the traffic and logs created in Network Firewall instances are transmitted

**[Note]**

* The subnets used for subnet, NAT, and external transmission must all be selected as different subnets.
    * It is recommended to create subnets in the minimum unit (28 bits) that can be created in the NHN Cloud console.
* Network Firewall instances are provided with redundancy by separating availability zones.
* If you use the Network Firewall service as a separate service from Security Groups, you must allow both to access the instances.
* The CIDR block owned by Network Firewall and the CIDR block requiring connectivity must not overlap.

### Connection Settings

Create a peering gateway and configure the routing to integrate with Network Firewall.

[Example]

* When the VPC (Hub) used by Network Firewall is 10.0.0.0/24, and the VPC (Spoke) that needs to be connected to the Network Firewall is 172.16.0.0/24.

1. Go to **Network > Routing**, select the Spoke VPC, and change the routing table.
    * After selecting Spoke VPC, click the **Change Routing Table** button to change to the Centralized Virtual Routing (CVR) method.
2. Go to **Network > Peering Gateway** to create a peering.
    * If the spoke VPC is a different project, connect the project peering. If the spoke VPC is the same project, connect the peering.
        * For more information on connecting a peering gateway, please see the [](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/)user guide[](https://docs.nhncloud.com/ko/Network/Peering%20Gateway/ko/console-guide/).
3. Go to **Network > Routing**, select a Hub VPC, and set up the routing as follows.
    * Destination CIDR: 172.16.0.0/24
    * Gateway: Gateway of peering type added after peering connection
4. Go to **Network > Routing**, select a Spoke VPC, and set up the routing as follows.
    *  Destination CIDR: 0.0.0.0/0
    *  Gateway: Gateway of peering type added after peering connection
5. Go to **Network > Peering Gateway > Project Peering**.
    * Select the created peering and go to the **Route** tab.
    * Click the **Peer** or **Change Local Route** to set up routing as follows.
        * Destination CIDR: 0.0.0.0/0
        * Gateway: NFW_TRAFFIC_SUBNET_INTERFACE_VIP

If the Spoke VPC has two or more subnets and traffic control is required through Network Firewall, add the routing as follows.

[Example]
When the subnets of Spoke VPC (172.16.0.0/24) are 172.16.0.0/25 and 172.16.0.128/25
* Go to **Network > Routing**, and select Spoke VPC and add the two routings as follows.
    * Destination CIDR: 172.16.0.0/25 and 172.16.0.128/25
    * Gateway: Gateway of peering type added after peering connection
     
If there are two or more spoke VPCs, add the routing as follows.

[Example]
With Spoke VPC1 (172.16.0.0/24) and Spoke VPC2 (192.168.0.0/24)
* Go to **Network > Routing** to select a Hub VPC, and add the two routings as follows.
    * Spoke VPC 1
        * Destination CIDR: 172.16.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC1
    * Spoke VPC 2
        * Destination CIDR: 192.168.0.0/24
        * Gateway: Gateway of peering type added between Hub VPC and Spoke VPC2

## Policy
When you create a Network Firewall instance, you will be moved to the initial policy page.


* In the **Policies** tab, you can manage policies to control inbound/outbound traffic and traffic between the VPCs connected to your Network Firewall instance.

### Main Page

![main_page.PNG](/ko/images/main_page.png)

* The default-deny policy is a required policy and cannot be modified or deleted.
* Logs blocked through the default-deny policy can be checked through the default blocking policy log settings in the **Options** tab.

### Add Policy

![acl_add.PNG](/ko/images/acl_add.png)

* Add policies based on departure, destination, and destination port.
    * Select the departure, destination, and destination port through already created objects.
* Add a policy by selecting the policy's status (enabled/disabled), action (allowed/blocked), and schedule.

### Copy Policy

![acl_copy.PNG](/ko/images/acl_copy.png)

* Copy a policy by clicking the created policy and **Copy**.
    * Copied policies will be disabled.

### Modify Policy

![acl_edit.PNG](/ko/images/acl_edit.png)

* Modify a policy by clicking the created policy and **Modify**.

### Move Policy

![acl_move.PNG](/ko/images/acl_move.png)

* Move a policy by clicking the created policy and **Move**.
    * Name: Could not move below the default-deny policy.

### Delete Policy

* Delete a policy by clicking the created policy and **Delete**.
    * **Once deleted, a policy cannot be restored, and a policy with name: default-deny cannot be deleted.**

### Batch Download of Policies

* Download all policies created in the Policies tab at once.

### Batch Register Policies

![acl_batch.PNG](/ko/images/acl_batch.png)

* You can register policies at once using the downloaded template.

## Object

* In the **Object** tab, create IPs and ports to use when creating policies.
    * Group objects can be managed by grouping subnet, port, and range objects into multiple objects.

### Add

Create an object by entering the required fields.

* IP
    * Type: Subnet, Range, Group
* Port
    * Type: Port, Range, Group
    * Protocol: TCP, UDP, ICMP

### Delete

Delete an object by clicking the created object and **Delete**.

* Objects automatically created by Network Firewall cannot be modified or deleted.
* Objects in use by a policy will be changed to ALL objects after deletion (caution required).

### Batch Download of Objects

* Download all IPs and port objects created in the **Object** tab at once.

## NAT

* In the **NAT** (Network Address Translation) tab, create a dedicated public IP by specifying the instance to be accessed from the outside.
    * **NAT only provides destination-based NAT and 1:1 NAT.**
    * **Port-based NAT is not provided.**
* The created public IP can be checked in **Network > Floating IP**.
* To enable communication, you must add an allow policy separately from creating NAT.

### Add

![nat_add.PNG](/ko/images/nat_add.png)

* Click  **Add (+)** to select an object.
    * **The object you want to select must be created in advance.**
* Click **Confirm** to check the connected **public IP before NAT**.
    * **The created public IP before NAT cannot be changed arbitrarily.**

### Delete

* Click **Delete** to delete the created NAT.
    * **After deletion, the public IP before NAT is automatically deleted.**

## Log

In the **Log** tab, search logs created in Network Firewall.

### Search

* Traffic: Search traffic logs generated by allow or block policies when passing through the Network Firewall.
    * **You can search only historical data up to 3 months in 1-month increments.**
    * If separate data storage is required, see the **log remote transmission settings** in the **Options** tab.
* Audit: Search logs for changes to Network Firewall, including policy creation and deletion.
    * **You can search for up to one month, and can search through CloudTrail, an organizational service.**

### Download Excel

* Download traffic and audit log search results through **Download Excel**.

## Monitor

* In the **Monitor** tab, check the status of your Network Firewall instances in real time.
    * Search is only possible within a maximum of 24 hours (1 day).

### Search

* Sessions: Quantity of sessions currently in use through Network Firewall.
* Network Usage: Inbound/outbound traffic currently passing through Network Firewall

## Options

* In the **Options** tab, set options required for operation of Network Firewall.

### Log Settings

* Default blocking policy log settings: Select whether to save the default blocking policy log that is required after creating a Network Firewall.
    * When enabled, you can search logs created with the default blocking policy in the traffic log.
* Log remote transmission settings: Select the option to save traffic logs remotely.
    * syslog: Save logs with up to 2 remote addresses
    * Object Storage: Save logs with the Object Storage service provided by NHN Cloud
    * Log & Crash Serach: Save logs with the Log&Crash Serach service provided by NHN Cloud

### General Settings

* NAT Settings