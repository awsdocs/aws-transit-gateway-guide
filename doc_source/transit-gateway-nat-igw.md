# Example: Centralized outbound routing to the internet<a name="transit-gateway-nat-igw"></a>

Consider the scenario where you have applications in multiple VPCs \(VPC A and VPC B\) which need outbound only internet access\. You connect all your VPCs to a transit gateway, and configure one VPC \(VPC C\) with a NAT gateway and an internet gateway Configure the VPC A and VPC B route tables to route outbound internet traffic goes through the transit gateway, which then routes the traffic to VPC C\. The NAT gateway in VPC C routes the traffic to the internet gateway\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/tgw-nat-igw.png)

In this scenario, you create the following entities for this scenario::
+ Three VPCs\. For information about creating a VPC, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon Virtual Private Cloud User Guide*\.
+ A transit gateway\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.
+ Three VPC attachments on the transit gateway\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.
+ A NAT gateway\. For information about creating a NAT gateway, see [Creating a NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating) in the *Amazon Virtual Private Cloud User Guide*\.
+ An internet gateway\. For information about creating an internet gateway, see [Creating and attaching an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon Virtual Private Cloud User Guide*\.

When you create the VPC attachments, the CIDR blocks for each VPC propagate to the transit gateway route table\. When the VPN connection is up, the BGP session is established and the Site\-to\-Site VPN CIDR propagates to the transit gateway route table and the VPC CIDRs are added to the customer gateway BGP table\.

**Topics**
+ [Routing](#transit-gateway-nat-igw-routing)

## Routing<a name="transit-gateway-nat-igw-routing"></a>

Each VPC has a route table and there is a route table for the transit gateway\.

### VPC route tables<a name="transit-gateway-nat-igw-vpc-route-tables"></a>

Each VPC has a route table with 2 entries\. The first entry is the default entry for local IPv4 routing in the VPC; this entry enables the instances in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\. The following table shows the VPC A routes\.


| Destination | Target | 
| --- | --- | 
|  10\.2\.0\.0/16  |  local  | 
|  0\.0\.0\.0/0  |  *tgw\-id*  | 

### Transit gateway route table<a name="transit-gateway-nat-igw-tgw-route-table"></a>

The following is an example of a default route table for the attachments shown in the previous diagram, with route propagation enabled\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  10\.1\.0\.0/16  |  *Attachment for VPC A*  |  propagated  | 
|  10\.2\.0\.0/16  |  *Attachment for VPC B*  |  propagated  | 
|  10\.3\.0\.0/16  |  *Attachment for VPC C*  |  propagated  | 
| 0\.0\.0\.0/0 | Attachment for VPC C |  propagated  | 

### NAT gateway subnet route table<a name="transit-gateway-nat-igw-nat-route-table"></a>

The subnet which contains the IP address of the NAT gateway, has a route table with the following entries\.


| Destination | Target | 
| --- | --- | 
|  10\.0\.0\.0/16  |  *tgw\-id*  | 
| 0\.0\.0\.0/0 | igw\-id | 

### Internet gateway subnet route table<a name="transit-gateway-nat-igw-igw-route-table"></a>

The subnet which contains the IP address of the internet gateway, has a route table with the following entry\.


| Destination | Target | 
| --- | --- | 
|  10\.0\.0\.0/16  |  *nat\-gateway\-id*  | 
