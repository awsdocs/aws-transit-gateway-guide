# Example: Centralized outbound routing to the internet<a name="transit-gateway-nat-igw"></a>

You can configure a transit gateway to route outbound internet traffic from a VPC without an internet gateway to a VPC that contains a NAT gateway and an internet gateway\.

**Topics**
+ [Overview](#transit-gateway-nat-igw-overview)
+ [Routing](#transit-gateway-nat-igw-routing)

## Overview<a name="transit-gateway-nat-igw-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. You have applications in multiple VPCs \(VPC A and VPC B\) that need outbound only internet access\. You connect your VPCs to a transit gateway, and configure one VPC \(VPC C\) with a NAT gateway and an internet gateway\. Configure the route tables for VPC A and VPC B to route outbound internet traffic through the transit gateway\. Configure the transit gateway route table to route the traffic to VPC C\. The NAT gateway in VPC C routes the traffic to the internet gateway\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/tgw-centralized-nat-igw.png)

You create the following resources for this scenario:
+ Three VPCs with IP address ranges that do not overlap\. For more information, see [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.
+ Two subnets per Availability Zone in VPC C\. One subnet is a public subnet for the NAT gateway, and the other is a private subnet for routing traffic to the NAT gateway\.
+ One NAT gateway in each public subnet of VPC C\. For more, see [Create a NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) in the *Amazon VPC User Guide*\.
+ An internet gateway for VPC C\. For more information, see [Create and attach an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.
+ One transit gateway\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.
+ Three VPC attachments on the transit gateway\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

When you create the VPC attachments, the CIDR blocks for each VPC propagate to the transit gateway route table\.

## Routing<a name="transit-gateway-nat-igw-routing"></a>

Each VPC has a route table and there is a route table for the transit gateway\.

### Route tables for VPC A and VPC B<a name="transit-gateway-nat-igw-vpc-route-tables"></a>

The following is an example route table\. The first entry enables the instances in the VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\.


| Destination | Target | 
| --- | --- | 
|  *VPC CIDR*  |  local  | 
|  0\.0\.0\.0/0  |  *transit\-gateway\-id*  | 

### Route tables for VPC C<a name="transit-gateway-nat-igw-nat-route-table"></a>

The following is an example route table for the public subnet that contains the NAT gateway\.


| Destination | Target | 
| --- | --- | 
| VPC C CIDR | local | 
| 0\.0\.0\.0/0 | internet\-gateway\-id | 
| VPC A CIDR | transit\-gateway\-id | 
| VPC B CIDR | transit\-gateway\-id | 

The following is an example for the private subnet\.


| Destination | Target | 
| --- | --- | 
| VPC C CIDR | local | 
| 0\.0\.0\.0/0 | nat\-gateway\-id | 

### Transit gateway route table<a name="transit-gateway-nat-igw-tgw-route-table"></a>

The following is an example of the transit gateway route table\. If you want to prevent inter\-VPC communication through the NAT gateway, add a blackhole route for each VPC CIDR to this route table\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  *VPC A CIDR*  |  *Attachment for VPC A*  |  propagated  | 
|  *VPC B CIDR*  |  *Attachment for VPC B*  |  propagated  | 
|  *VPC C CIDR*  |  *Attachment for VPC C*  |  propagated  | 
| 0\.0\.0\.0/0 |  *Attachment for VPC C*  | static | 