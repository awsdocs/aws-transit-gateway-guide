# Example: Isolated VPCs with shared services<a name="transit-gateway-isolated-shared"></a>

You can configure your transit gateway as multiple isolated routers that use a shared service\. This is similar to using multiple transit gateways, but provides more flexibility in cases where the routes and attachments might change\. In this scenario, each isolated router has a single route table\. All attachments associated with an isolated router propagate and associate with its route table\. Attachments associated with one isolated router can route packets to each other, but cannot route packets to or receive packets from the attachments for another isolated router\. Attachments can route packets to or receive packets from the shared services\. You can use this scenario when you have groups that need to be isolated, but use a shared service, for example a production system\.

**Topics**
+ [Overview](#Ttransit-gateway-isolated-shared-overview)
+ [Routing](#transit-gateway-isolated-shared-routes)

## Overview<a name="Ttransit-gateway-isolated-shared-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. Packets from VPC A, VPC B, and VPC C route to the transit gateway\. Packets from the subnets in VPC A, VPC B, and VPC C that have the internet as a destination, route first through the transit gateway and then route to the Site\-to\-Site VPN\. Packets from one VPC that have a destination of a subnet in another VPC, for example from 10\.1\.0\.0 to 10\.2\.0\.0, route through the transit gateway, where they are blocked because there is no route for them in the transit gateway route table\. Packets from VPC A, VPC B, and VPC C that have VPC D as the destination route through the transit gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-isolated_shared.png)

In this scenario, you create the following entities:
+ Four VPCs\. For information about creating a VPC, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon Virtual Private Cloud User Guide*\.
+ A transit gateway\. For more information, see [Create a transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html)\.
+ Four attachments on the transit gateway for the four VPCs\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.
+ A Site\-to\-Site VPN\. For more information, see the following topics in the *AWS Site\-to\-Site VPN User Guide*:
  + [Create a Site\-to\-Site VPN connection](https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html#vpn-create-vpn-connection)
  + [Requirements for your customer gateway device](https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#CGRequirements)
+ A VPN attachment on the transit gateway\. For more information, see [Create a transit gateway attachment to a VPN](tgw-vpn-attachments.md#create-vpn-attachment)\.

When the VPN is up, the BGP session is established and the VPN CIDR propagates to the transit gateway route table and the VPC CIDRs are added to the gateway BGP table\.

## Routing<a name="transit-gateway-isolated-shared-routes"></a>

Each VPC has a route table and there is a VPC route table and a Site\-to\-Site VPN route table in the transit gateway\.

### VPC A, VPC B, VPC C, and VPC D route tables<a name="transit-gateway-isolated-shared-route-tables"></a>

Each VPC has a route table with 2 entries\. The first entry is the default entry for local IPv4 routing in the VPC; this entry enables the instances in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\. The following table shows the VPC A routes\.


| Destination | Target | 
| --- | --- | 
|  10\.1\.0\.0/16  |  local  | 
| 0\.0\.0\.0/0 |  *tgw\-id*  | 

### Transit gateway route tables<a name="transit-gateway-isolated-shared-route-table-tgw-route-table"></a>

This scenario uses two transit gateway route tables—one for the VPCs and one for the VPN\. The route table for the VPCs has an entry that points to the VPN attachment and an entry that points to VPC D\.


| Destination | Target | Route type | 
| --- | --- | --- | 
| 10\.99\.99\.0/24 | *Attachment for VPN connection * |  propagated  | 
| 10\.4\.0\.0/16 |  *Attachment for VPC D*  |  propagated  | 

The route table for the Site\-to\-Site VPN connection is associated with the VPN attachment, and has entries that point to each of the VPCs\. This enables communication to the VPCs from the VPN connection\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  10\.1\.0\.0/16  |  *Attachment for VPC A*  |  propagated  | 
|  10\.2\.0\.0/16  |  *Attachment for VPC B*  |  propagated  | 
|  10\.3\.0\.0/16  |  *Attachment for VPC C*  |  propagated  | 
| 10\.4\.0\.0/16 | *Attachment for VPC D* | propagated | 

### Customer gateway BGP table<a name="transit-gateway-isolated-shared-route-table-bgp-table"></a>

The customer gateway BGP table contains the following VPC IP Addresses\.
+ 10\.1\.0\.0/16
+ 10\.2\.0\.0/16
+ 10\.3\.0\.0/16
+ 10\.4\.0\.0/16