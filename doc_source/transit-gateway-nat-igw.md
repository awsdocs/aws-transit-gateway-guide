# Example: Centralized outbound routing to the internet<a name="transit-gateway-nat-igw"></a>

You can configure a transit gateway to route outbound internet traffic from a VPC without an internet gateway to a VPC that contains a NAT gateway and an internet gateway\.

**Topics**
+ [Overview](#transit-gateway-nat-igw-overview)
+ [Resources](#transit-gateway-nat-igw-resources)
+ [Routing](#transit-gateway-nat-igw-routing)

## Overview<a name="transit-gateway-nat-igw-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. You have applications in VPC A and VPC B that need outbound only internet access\. You configure VPC C with a NAT gateway and an internet gateway\. Connect all VPCs to a transit gateway\. Configure routing so that outbound internet traffic from VPC A and VPC B traverses the transit gateway to VPC C\. The NAT gateway in VPC C routes the traffic to the internet gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/tgw-centralized-nat-igw.png)

## Resources<a name="transit-gateway-nat-igw-resources"></a>

Create the following resources for this scenario:
+ Three VPCs with IP address ranges that do not overlap\. For more information, see [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.
+ VPC A and VPC B each have private subnets with EC2 instances\.
+ VPC C has the following:
  + An internet gateway attached to the VPC\. For more information, see [Create and attach an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.
  + Two subnets\.
  + A NAT gateway using one of the subnets\. For more information, see [Create a NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) in the *Amazon VPC User Guide*\.
+ One transit gateway\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.
+ Three VPC attachments on the transit gateway\. The CIDR blocks for each VPC propagate to the transit gateway route table\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

## Routing<a name="transit-gateway-nat-igw-routing"></a>

There are route tables for each VPC and a route table for the transit gateway\.

**Topics**
+ [VPC A](#transit-gateway-nat-igw-vpc-a-route-table)
+ [VPC B](#transit-gateway-nat-igw-vpc-b-route-table)
+ [VPC C](#transit-gateway-nat-igw-nat-vpc-c-route-tables)
+ [Transit gateway](#transit-gateway-nat-igw-tgw-route-table)

### Route table for VPC A<a name="transit-gateway-nat-igw-vpc-a-route-table"></a>

The following is an example route table\. The first entry enables instances in the VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\.


| Destination | Target | 
| --- | --- | 
|  *VPC A CIDR*  |  local  | 
|  0\.0\.0\.0/0  |  *transit\-gateway\-id*  | 

### Route table for VPC B<a name="transit-gateway-nat-igw-vpc-b-route-table"></a>

The following is an example route table\. The first entry enables the instances in the VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\.


| Destination | Target | 
| --- | --- | 
|  *VPC B CIDR*  |  local  | 
|  0\.0\.0\.0/0  |  *transit\-gateway\-id*  | 

### Route tables for VPC C<a name="transit-gateway-nat-igw-nat-vpc-c-route-tables"></a>

Configure the subnet with the NAT gateway as a public subnet by adding a route to the internet gateway\. Leave the other subnet as a private subnet\.

The following is an example route table for the public subnet\. The first entry enables instances in the VPC to communicate with each other\. The second and third entries route traffic for VPC A and VPC B to the transit gateway\. The remaining entry routes all other IPv4 subnet traffic to the internet gateway\.


| Destination | Target | 
| --- | --- | 
| VPC C CIDR | local | 
| VPC A CIDR | transit\-gateway\-id | 
| VPC B CIDR | transit\-gateway\-id | 
| 0\.0\.0\.0/0 | internet\-gateway\-id | 

The following is an example route table for the private subnet\. The first entry enables instances in the VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the NAT gateway\.


| Destination | Target | 
| --- | --- | 
| VPC C CIDR | local | 
| 0\.0\.0\.0/0 | nat\-gateway\-id | 

### Transit gateway route table<a name="transit-gateway-nat-igw-tgw-route-table"></a>

The following is an example of the transit gateway route table\. The CIDR blocks for each VPC propagate to the transit gateway route table\. The static route sends outbound internet traffic to VPC C\. You can optionally prevent inter\-VPC communication by adding a blackhole route for each VPC CIDR\.


| CIDR | Attachment | Route type | 
| --- | --- | --- | 
|  *VPC A CIDR*  |  *Attachment for VPC A*  |  propagated  | 
|  *VPC B CIDR*  |  *Attachment for VPC B*  |  propagated  | 
|  *VPC C CIDR*  |  *Attachment for VPC C*  |  propagated  | 
| 0\.0\.0\.0/0 |  *Attachment for VPC C*  | static | 