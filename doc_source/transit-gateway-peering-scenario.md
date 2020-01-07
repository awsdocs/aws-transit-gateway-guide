# Transit Gateway Example: Peering<a name="transit-gateway-peering-scenario"></a>

You can create a transit gateway peering connection between transit gateways in different Regions\. You can then route traffic between the attachments for each of the transit gateways\. In this scenario, VPC and VPN attachments are associated with the transit gateway default route tables, and they propagate to the transit gateway default route tables\. Each transit gateway route table has a static route that points to the transit gateway peering attachment\.

**Topics**
+ [Overview](#transit-gateway-peering-overview)
+ [Routing](#transit-gateway-peering-routing)

## Overview<a name="transit-gateway-peering-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. Transit gateway 1 has two VPC attachments, and transit gateway 2 has one Site\-to\-Site VPN attachment\. Packets from the subnets in VPC A and VPC B that have the internet as a destination first route through transit gateway 1, then transit gateway 2, and then route to the VPN\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-peering.png)

You create the following entities for this scenario:
+ Two VPCs\. For information about creating a VPC, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide//working-with-vpcs.html#Create-VPC) in the *Amazon Virtual Private Cloud User Guide*\.
+ Two transit gateways in different Regions\. For more information, see [Create a Transit Gateway](tgw-transit-gateways.md#create-tgw)\.
+ Two VPC attachments on the first transit gateway\. For more information, see [Create a Transit Gateway Attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.
+ A Site\-to\-Site VPN attachment on the second transit gateway\. For more information, see [Create a Transit Gateway Attachment to a VPN](tgw-vpn-attachments.md#create-vpn-attachment) and [Requirements for Your Customer Gateway](https://docs.aws.amazon.com/vpc/latest/adminguide/Introduction.html#CGRequirements) in the *AWS Site\-to\-Site VPN Network Administrator Guide*\.
+ A transit gateway peering attachment between the two transit gateways\. For more information, see [Transit Gateway Peering Attachments](tgw-peering.md)\.

When you create the VPC attachments, the CIDRs for each VPC propagate to the route table for transit gateway 1\. When the VPN is up, the following actions occur:
+ The BGP session is established
+ The Site\-to\-Site VPN CIDR propagates to the route table for transit gateway 2
+ The VPC CIDRs are added to the gateway BGP table

## Routing<a name="transit-gateway-peering-routing"></a>

Each VPC has a route table and each transit gateway has a route table\.

### VPC Route Tables<a name="transit-gateway-centralized-router-vpc-route-tables"></a>

Each VPC has a route table with 2 entries\. The first entry is the default entry for local IPv4 routing in the VPC\. This default entry enables the resources in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\. The following table shows the VPC A routes\.


| Destination | Target | 
| --- | --- | 
|  10\.0\.0\.0/16  |  local  | 
|  0\.0\.0\.0/0  |  *tgw\-1\-id*  | 

### Transit Gateway Route Tables<a name="transit-gateway-centralized-router-tgw-route-table"></a>

The following is an example of the default route table for transit gateway 1, with route propagation enabled\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  10\.0\.0\.0/16  |  *Attachment ID for VPC A*  |  propagated  | 
|  10\.2\.0\.0/16  |  *Attachment ID for VPC B*  |  propagated  | 
|  0\.0\.0\.0/0  | Attachment ID for peering connection  |  static  | 

The following is an example of the default route table for transit gateway 2, with route propagation enabled\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  172\.31\.0\.0/16  | Attachment ID for VPN connection  |  propagated  | 
|  10\.0\.0\.0/16  |  *Attachment ID for peering connection *  |  static  | 
|  10\.2\.0\.0/16  | Attachment ID for peering connection  | static | 

### Customer Gateway BGP Table<a name="transit-gateway-centralized-router-vpn-route-table"></a>

The customer gateway BGP table contains the following VPC IP Addresses\.
+ 10\.0\.0\.0/16
+ 10\.2\.0\.0/16