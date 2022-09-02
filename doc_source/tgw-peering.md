# Transit gateway peering attachments<a name="tgw-peering"></a>

You can peer both Intra\-Region and Inter\-Region transit gateways, and route traffic between them, which includes IPv4 and IPv6 traffic\. To do this, create a peering attachment on your transit gateway, and specify a transit gateway\. The peer transit gateway can be in your account or a different AWS account\.

After you create a peering attachment request, the owner of the peer transit gateway \(also referred to as the *accepter transit gateway*\) must accept the request\. To route traffic between the transit gateways, add a static route to the transit gateway route table that points to the transit gateway peering attachment\.

We recommend using unique ASNs for the peered transit gateways to take advantage of future route propagation capabilities\.

Transit gateway peering does not support resolving public or private IPv4 DNS host names to private IPv4 addresses across VPCs on either side of the transit gateway peering attachment\.

Inter\-Region gateway peering uses the same network infrastructure as VPC peering\. Therefore traffic is encrypted using AES\-256 encryption at the virtual network layer as it travels between Regions\. Traffic is also encrypted using AES\-256 encryption at the physical layer when it traverses network links that are outside of the physical control of AWS\. As a result, traffic is double encrypted on network links outside the physical control of AWS\. Within the same Region, traffic is encrypted at the physical layer only when it traverses network links that are outside of the physical control of AWS\.

For information about which Regions support transit gateway peering attachments, see [AWS Transit Gateways FAQs](http://aws.amazon.com/transit-gateway/faqs/)\.

## Create a peering attachment<a name="tgw-peering-create"></a>

Before you begin, ensure that you have the ID of the transit gateway that you want to attach\. If the transit gateway is in another AWS account, ensure that you have the AWS account ID of the owner of the transit gateway\.

After you create the peering attachment, the owner of the accepter transit gateway must accept the attachment request\.

**To create a peering attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Choose **Create transit gateway attachment**\.

1. For **Transit gateway ID**, choose the transit gateway for the attachment\. You can choose a transit gateway that you own or a transit gateway that was shared with you\.

1. For **Attachment type**, choose **Peering Connection**\.

1. Optionally enter a name tag for the attachment\.

1. For **Account**, do one of the following:
   + If the transit gateway is in your account, choose **My account**\.
   + If the transit gateway is in different AWS account, choose **Other account**\. For **Account ID**, enter the AWS account ID\.

1. For **Region**, choose the Region that the transit gateway is located in\.

1. For **Transit gateway \(accepter\)**, enter the ID of the transit gateway that you want to attach\.

1. Choose **Create transit gateway attachment**\.

**To create a peering attachment using the AWS CLI**  
Use the [create\-transit\-gateway\-peering\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-peering-attachment.html) command\.

## Accept or reject a peering attachment request<a name="tgw-peering-accept-reject"></a>

To activate the peering attachment, the owner of the accepter transit gateway must accept the peering attachment request\. This is required even if both transit gateways are in the same account\. The peering attachment must be in the `pendingAcceptance` state\. Accept the peering attachment request from the Region that the accepter transit gateway is located in\.

Alternatively, you can reject any peering connection request that you've received that's in the `pendingAcceptance` state\. You must reject the request from the Region that the accepter transit gateway is located in\.

**To accept a peering attachment request using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the transit gateway peering attachment that's pending acceptance\.

1. Choose **Actions**, **Accept transit gateway attachment**\.

1. Add the static route to the transit gateway route table\. For more information, see [Create a static route](tgw-route-tables.md#tgw-create-static-route)\.

**To reject a peering attachment request using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the transit gateway peering attachment that's pending acceptance\.

1. Choose **Actions**, **Reject transit gateway attachment**\.

**To accept or reject a peering attachment using the AWS CLI**  
Use the [accept\-transit\-gateway\-peering\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/accept-transit-gateway-peering-attachment.html) and [reject\-transit\-gateway\-peering\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/reject-transit-gateway-peering-attachment.html) commands\.

## Add a route to the transit gateway route table<a name="tgw-peering-add-route"></a>

To route traffic between the peered transit gateways, you must add a static route to the transit gateway route table that points to the transit gateway peering attachment\. The owner of the accepter transit gateway must also add a static route to their transit gateway's route table\.

**To create a static route using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Route Tables**\.

1. Select the route table for which to create a route\.

1. Choose **Actions**, **Create static route**\.

1. On the **Create static route** page, enter the CIDR block for which to create the route\. For example, specify the CIDR block of a VPC that's attached to the peer transit gateway\.

1. Choose the peering attachment for the route\.

1. Choose **Create static route**\.

**To create a static route using the AWS CLI**  
Use the [create\-transit\-gateway\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-route.html) command\.

**Important**  
After you create the route, associate the transit gateway route table with the transit gateway peering attachment\. For more information, see [Associate a transit gateway route table](tgw-route-tables.md#associate-tgw-route-table)\.

## View your transit gateway peering connection attachments<a name="tgw-peering-view-attachments"></a>

You can view your transit gateway peering attachments and information about them\.

**To view your peering attachments using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. In the **Resource type** column, look for **Peering**\. These are the peering attachments\. 

1. Choose an attachment to view its details\.

**To view your transit gateway peering attachments using the AWS CLI**  
Use the [describe\-transit\-gateway\-peering\-attachments](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-peering-attachments.html) command\.

## Delete a peering attachment<a name="tgw-peering-delete"></a>

You can delete a transit gateway peering attachment\. The owner of either of the transit gateways can delete the attachment\.

**To delete a peering attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the transit gateway peering attachment\.

1. Choose **Actions**, **Delete transit gateway attachment**\.

1. Enter **delete** and choose **Delete**\.

**To delete a peering attachment using the AWS CLI**  
Use the [delete\-transit\-gateway\-peering\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-peering-attachment.html) command\.

## Opt\-in AWS Region considerations<a name="opt-in-considerations"></a>

You can peer transit gateways across opt\-in Region boundaries\. For information about these Regions, and how to opt in, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *Amazon Web Services General Reference*\. Take the following into consideration when you use transit gateway peering in these Regions:
+ You can peer into an opt\-in Region as long as the account that accepts the peering attachment has opted into that Region\. 
+ Regardless of the Region opt\-in status, AWS shares the following account data with the account that accepts the peering attachment:
  + AWS account ID
  + Transit gateway ID
  + Region code
+ When you delete the transit gateway attachment, the above account data is deleted\.
+ We recommend that you delete the transit gateway peering attachment before you opt out of the Region\. If you do not delete the peering attachment, traffic might continue to go over the attachment and you continue to incur charges\. If you do not delete the attachment, you can opt back in, and then delete the attachment\.
+ In general, the transit gateway has a sender pays model\. By using a transit gateway peering attachment across an opt in boundary, you might incur charges in a Region accepting the attachment, including those Regions you have not opted into\. For more information, see [AWS Transit Gateway Pricing](http://aws.amazon.com/transit-gateway/pricing/)\.