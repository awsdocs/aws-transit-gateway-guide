# Transit Gateway Scenario: Isolated VPCs<a name="transit-gateway-isolated"></a>

You can configure your transit gateway as multiple isolated routers\. This is similar to using multiple transit gateways, but provides more flexibility in cases where the routes and attachments might change\. In this scenario, each isolated router has a single route table\. All attachments associated with an isolated router propagate and associate with its route table\. Attachments associated with one isolated router can route packets to each other, but cannot route packets to or receive packets from the attachments for another isolated router\.

**Topics**
+ [Overview](#transit-gateway-isolated-overview)
+ [Routing](#transit-gateway-isolated-routes)

## Overview<a name="transit-gateway-isolated-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. Packets from VPC A, VPC B, and VPC C route to the transit gateway\. Packets from the subnets in VPC, A, VPC B, and VPC C that have the internet as a destination, route first through the transit gateway and then route to the Site\-to\-Site VPN\. Packets from one VPC that have a destination of a subnet in another VPC, for example from 10\.1\.0\.0 to 10\.2\.0\.0, route through the transit gateway, where they are blocked because there is no route for them in the transit gateway route table\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-centralized.png)

In this scenario, you create the following entities:
+ Three VPCs\. For information about creating a VPC, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide//working-with-vpcs.html#Create-VPC) in the *Amazon Virtual Private Cloud User Guide*\.
+ A transit gateway\. For more information, see [Create a Transit Gateway](tgw-transit-gateways.md#create-tgw)\.
+ Three attachments on the transit gateway for the three VPCs\. For more information, see [Create a Transit Gateway Attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.
+ A Site\-to\-Site VPN\. For more information about creating a VPN, see [Create a Site\-to\-Site VPN Connection and Configure the Customer Gateway](https://docs.aws.amazon.com/vpn/latest/s2svpn//SetUpVPNConnections.html#vpn-create-vpn-connection) in the *AWS Site\-to\-Site VPN User Guide* and [Requirements for Your Customer Gateway](https://docs.aws.amazon.com/vpc/latest/adminguide/Introduction.html#CGRequirements) in the *Amazon VPC Network Administrator Guide*\.
+ A VPN attachment on the transit gateway\. For more information, see [Create a Transit Gateway Attachment to a VPN](tgw-vpn-attachments.md#create-vpn-attachment)\.

When the VPN is up, the BGP session is established and the VPN CIDR propagates to the transit gateway route table and the VPC CIDRs are added to the gateway BGP table\.

## Routing<a name="transit-gateway-isolated-routes"></a>

Each VPC has a route table and there is a VPC route table and a VPN route table in the transit gateway\.

### VPC Route Tables<a name="transit-gateway-isolated-route-tables"></a>

Each VPC has a route table with 2 entries\. The first entry is the default entry for local IPv4 routing in the VPC; this entry enables the instances in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\. The following table shows the VPC A routes\.


| Destination | Target | 
| --- | --- | 
|  10\.1\.0\.0/16  |  local  | 
| 0\.0\.0\.0/0 |  *tgw\-id*  | 

### Transit Gateway Route Table<a name="transit-gateway-isolated-route-table-tgw-route-table"></a>

This scenario uses a route table for the VPCs and a route table for the VPN\. The route table for the VPC has an entry that points to the VPN attachment\.


| Destination | Target | Route type | 
| --- | --- | --- | 
| 0\.0\.0\.0/0 | Attachment for VPN connection  |  propagated  | 

The route table for the VPN has entries for each of the VPCs


| Destination | Target | Route type | 
| --- | --- | --- | 
|  10\.1\.0\.0/16  |  *Attachment for VPC A*  |  propagated  | 
|  10\.2\.0\.0/16  |  *Attachment for VPC B*  |  propagated  | 
|  10\.3\.0\.0/16  |  *Attachment for VPC C*  |  propagated  | 

### Customer Gateway BGP Table<a name="transit-gateway-isolated-route-table-bgp-table"></a>

The customer gateway BGP table contains the following VPC IP Addresses\.
+ 10\.1\.0\.0/16
+ 10\.2\.0\.0/16
+ 10\.3\.0\.0/16