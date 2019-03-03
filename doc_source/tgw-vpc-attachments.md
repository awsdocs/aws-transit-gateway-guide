# Transit Gateway Attachments to a VPC<a name="tgw-vpc-attachments"></a>

When you attach a VPC to a transit gateway, you must specify one subnet from each Availability Zone to be used by the transit gateway to route traffic\. Specifying one subnet from an Availability Zone enables traffic to reach resources in every subnet in that Availability Zone\.

**Limits**  
The resources in a VPC attached to a transit gateway and that have the transit gateway in a subnet route table can only forward traffic to the transit gateway, when the transit gateway has an attachment in any subnet in that VPC in the same Availability Zone\. 

The resources in a VPC attached to a transit gateway cannot access the security groups of a different VPC that is also attached to the same transit gateway\.

## Create a Transit Gateway Attachment to a VPC<a name="create-vpc-attachment"></a>

**To create a VPC attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Choose **Create Transit Gateway Attachment**\.

1. For **Transit Gateway ID**, choose the transit gateway for the attachment\. You can choose a transit gateway that you own or a transit gateway that was shared with you\.

1. For **Attachment type**, choose **VPC**\.

1. Under **VPC Attachment**, optionally type a name for **Attachment name tag**\.

1. Choose whether to enable **DNS Support** and **IPv6 Support**\.

1. For **VPC ID**, choose the VPC to attach to the transit gateway\.

   This VPC must have at least one subnet associated with it\.

1. For **Subnet IDs**, select one subnet for each Availability Zone to be used by the transit gateway to route traffic\. You must select at least one subnet\. You can select only one subnet per Availability Zone\.

1. Choose **Create attachment**\.

**To create a VPC attachment using the AWS CLI**  
Use the [create\-transit\-gateway\-vpc\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-vpc-attachment.html) command\.

## View Your VPC Attachments<a name="view-vpc-attachment"></a>

**To view your VPC attachments using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Choose the search bar, select **Resource type** from the menu, and then select **VPC**\.

1. The VPC attachments are displayed\. Choose an attachment to view its details or to add tags\.

**To view your VPC attachments using the AWS CLI**  
Use the [describe\-transit\-gateway\-vpc\-attachments](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-vpc-attachments.html) command\.

## Delete a VPC Attachment<a name="delete-vpc-attachment"></a>

**To delete a VPC attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the VPC attachment\.

1. Choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Delete**\.

**To delete a VPC attachment using the AWS CLI**  
Use the [delete\-transit\-gateway\-vpc\-attachment](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-vpc-attachment.html) command\.