# Example: Appliance in a shared services VPC<a name="transit-gateway-appliance-scenario"></a>

You can configure an appliance \(such as a security appliance\) in a shared services VPC\. All traffic that's routed between transit gateway attachments is first inspected by the appliance in the shared services VPC\.

**Topics**
+ [Overview](#transit-gateway-appliance-overview)
+ [Stateful appliances and appliance mode](#transit-gateway-appliance-support)
+ [Routing](#transit-gateway-appliance-routing)

## Overview<a name="transit-gateway-appliance-overview"></a>

The following diagram shows the key components of the configuration for this scenario\. The transit gateway has three VPC attachments\. VPC C is a shared services VPC\. Traffic between VPC A and VPC B is routed to the transit gateway, then routed to a security appliance in VPC C for inspection before it's routed to the final destination\. The appliance is a stateful appliance, therefore both the request and response traffic is inspected\. For high availability, there is an appliance in each Availability Zone in VPC C\. 

![\[An appliance in a shared services VPC\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-appliance.png)

You create the following resources for this scenario:
+ Three VPCs\. For information about creating a VPC, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon Virtual Private Cloud User Guide*\.
+ A transit gateway\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.
+ Three VPC attachments \- one for each of the VPCs\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

  For each VPC attachment, specify a subnet in each Availability Zone\. For the shared services VPC, these are the subnets where traffic is routed to the VPC from the transit gateway\. In the preceding example, these are subnets A and C\.

  For the VPC attachment for VPC C, enable appliance mode support so that response traffic is routed to the same Availability Zone in VPC C as the source traffic\.

  The Amazon VPC console does not support appliance mode\. You can use the Amazon VPC API, an AWS SDK, or the AWS CLI to enable appliance mode\. For example, add `--options ApplianceModeSupport=enable` to the [create\-transit\-gateway\-vpc\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-vpc-attachment.html) or [modify\-transit\-gateway\-vpc\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-transit-gateway-vpc-attachment.html) command\.

## Stateful appliances and appliance mode<a name="transit-gateway-appliance-support"></a>

When appliance mode is enabled, a transit gateway selects a single network interface in the appliance VPC, using a flow hash algorithm, to send traffic to for the life of the flow\. The transit gateway uses the same network interface for the return traffic\. This ensures that bidirectional traffic is routed symmetrically—it's routed through the same Availability Zone in the VPC attachment for the life of the flow\. If you have multiple transit gateways in your architecture, each transit gateway maintains its own session affinity, and each transit gateway can select a different network interface\. 

If your VPC attachments span multiple Availability Zones and you require traffic between source and destination hosts to be routed through the same appliance for stateful inspection, enable appliance mode support for the VPC attachment in which the appliance is located\.

For more information, see [Centralized inspection architecture](http://aws.amazon.com/blogs/networking-and-content-delivery/centralized-inspection-architecture-with-aws-gateway-load-balancer-and-aws-transit-gateway/) in the AWS blog\.

**Behavior when appliance mode is not enabled**  
When appliance mode is not enabled, a transit gateway attempts to keep traffic routed between VPC attachments in the originating Availability Zone until it reaches its destination\. Traffic crosses Availability Zones between attachments only if there is an Availability Zone failure or if there are no subnets associated with a VPC attachment in that Availability Zone\.

The following diagram shows a traffic flow when appliance mode support is not enabled\. The response traffic that originates from Availability Zone 2 in VPC B is routed by the transit gateway to the same Availability Zone in VPC C\. The traffic is therefore dropped, because the appliance in Availability Zone 2 is not aware of the original request from the source in VPC A\.

![\[Dropped response traffic to an appliance\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-appliance-dropped-traffic.png)

## Routing<a name="transit-gateway-appliance-routing"></a>

Each VPC has one or more route tables and the transit gateway has two route tables\.

### VPC route tables<a name="transit-gateway-appliance-vpc-route-table"></a>

**VPC A and VPC B**

VPCs A and B have route tables with 2 entries\. The first entry is the default entry for local IPv4 routing in the VPC\. This default entry enables the resources in this VPC to communicate with each other\. The second entry routes all other IPv4 subnet traffic to the transit gateway\. The following is the route table for VPC A\.


| Destination | Target | 
| --- | --- | 
|  10\.0\.0\.0/16  |  local  | 
|  0\.0\.0\.0/0  |  *tgw\-id*  | 

**VPC C**

The shared services VPC \(VPC C\) has different route tables for each subnet\. Subnet A is used by the transit gateway \(you specify this subnet when you create the VPC attachment\)\. The route table for subnet A routes all traffic to the appliance in subnet B\.


| Destination | Target | 
| --- | --- | 
|  192\.168\.0\.0/16  |  local  | 
|  0\.0\.0\.0/0  |  *appliance\-eni\-id*  | 

The route table for subnet B \(which contains the appliance\) routes the traffic back to the transit gateway\.


| Destination | Target | 
| --- | --- | 
|  192\.168\.0\.0/16  |  local  | 
|  0\.0\.0\.0/0  |  *tgw\-id*  | 

### Transit gateway route tables<a name="transit-gateway-appliance-route-table"></a>

This transit gateway uses one route table for VPC A and VPC B, and one route table for the shared services VPC \(VPC C\)\. 

The VPC A and VPC B attachments are associated with the following route table\. The route table routes all traffic to VPC C\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  0\.0\.0\.0/0  | *Attachment ID for VPC C* |  static  | 

The VPC C attachment is associated with the following route table\. It routes traffic to VPC A and VPC B\.


| Destination | Target | Route type | 
| --- | --- | --- | 
|  10\.0\.0\.0/16  |  *Attachment ID for VPC A*  |  propagated  | 
|  10\.1\.0\.0/16  | *Attachment ID for VPC B* | propagated | 