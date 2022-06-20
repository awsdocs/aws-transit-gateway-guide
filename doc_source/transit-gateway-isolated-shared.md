# Example: Isolated VPCs with shared services<a name="transit-gateway-isolated-shared"></a>

You can configure your transit gateway as multiple isolated routers that use a shared service\. This is similar to using multiple transit gateways, but provides more flexibility in cases where the routes and attachments might change\. In this scenario, each isolated router has a single route table\. All attachments associated with an isolated router propagate and associate with its route table\. Attachments associated with one isolated router can route packets to each other, but cannot route packets to or receive packets from the attachments for another isolated router\. Attachments can route packets to or receive packets from the shared services\. You can use this scenario when you have groups that need to be isolated, but use a shared service, for example a production system\.

**Topics**
+ [Overview](#Ttransit-gateway-isolated-shared-overview)
+ [Resources](#transit-gateway-isolated-shared-resources)
+ [Routing](#transit-gateway-isolated-shared-routes)

## Overview<a name="Ttransit-gateway-isolated-shared-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. Packets from the subnets in VPC A, VPC B, and VPC C that have the internet as a destination, first route through the transit gateway and then route to the customer gateway for Site\-to\-Site VPN\. Packets from subnets in VPC A, VPC B, or VPC C that have a destination of a subnet in VPC A, VPC B, or VPC C route through the transit gateway, where they are blocked because there is no route for them in the transit gateway route table\. Packets from VPC A, VPC B, and VPC C that have VPC D as the destination route through the transit gateway and then to VPC D\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-isolated_shared.png)

## Resources<a name="transit-gateway-isolated-shared-resources"></a>

Create the following resources for this scenario:
+ Four VPCs\. For information about creating a VPC, see [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.
+ A transit gateway\. For more information, see [Create a transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html)\.
+ Four attachments on the transit gateway, one per VPC\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.
+ A Site\-to\-Site VPN attachment on the transit gateway\. For more information, see [Create a transit gateway attachment to a VPN](tgw-vpn-attachments.md#create-vpn-attachment)\.

  Ensure that you review the [requirements for your customer gateway device](https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#CGRequirements) in the *AWS Site\-to\-Site VPN User Guide*\.

When the VPN connection is up, the BGP session is established and the VPN CIDR propagates to the transit gateway route table and the VPC CIDRs are added to the customer gateway BGP table\.

## Routing<a name="transit-gateway-isolated-shared-routes"></a>

Each VPC has a route table, and the transit gateway has two route tables—one for the VPCs and one for the VPN connection and shared services VPC\.

### VPC A, VPC B, VPC C, and VPC D route tables<a name="transit-gateway-isolated-shared-route-tables"></a>

Each VPC has a route table with two entries\. The first entry is the default entry for local routing in the VPC; this entry enables the instances in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\.


| Destination | Target | 
| --- | --- | 
| VPC CIDR | local | 
| 0\.0\.0\.0/0 | transit gateway ID | 

### Transit gateway route tables<a name="transit-gateway-isolated-shared-route-table-tgw-route-table"></a>

This scenario uses one route table for the VPCs and one route table for the VPN connection\. 

The VPC A, B, and C attachments are associated with the following route table, which has a propagated route for the VPN attachment and a propagated route for the attachment for VPC D\.


| Destination | Target | Route type | 
| --- | --- | --- | 
| Customer gateway IP address | Attachment for VPN connection | propagated | 
| VPC D CIDR | Attachment for VPC D | propagated | 

The VPN attachment and shared services VPC \(VPC D\) attachments are associated with the following route table, which has entries that point to each of the VPC attachments\. This enables communication to the VPCs from the VPN connection and the shared services VPC\.


| Destination | Target | Route type | 
| --- | --- | --- | 
| VPC A CIDR | Attachment for VPC A | propagated | 
| VPC B CIDR | Attachment for VPC B | propagated | 
| VPC C CIDR | Attachment for VPC C | propagated | 

For more information, see [Propagate a route to a transit gateway route table](tgw-route-tables.md#enable-tgw-route-propagation)\.

### Customer gateway BGP table<a name="transit-gateway-isolated-shared-route-table-bgp-table"></a>

The customer gateway BGP table contains the CIDRs for all four VPCs\.