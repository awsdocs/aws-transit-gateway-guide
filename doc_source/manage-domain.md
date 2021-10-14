# Managing multicast domains<a name="manage-domain"></a>

To begin using multicast with a transit gateway, create a multicast domain, and then associate subnets with the domain\.

**Topics**
+ [Creating an IGMP multicast domain](#create-tgw-igmp-domain)
+ [Creating a static source multicast domain](#create-tgw-domain)
+ [Associating VPC attachments and subnets with a multicast domain](#associate-attachment-to-domain)
+ [Viewing your multicast domain associations](#view-tgw-domain-association)
+ [Disassociating subnets from a multicast domain](#remove-subnet-association)
+ [Adding tags to a multicast domain](#tgw-domain-tagging)
+ [Deleting a multicast domain](#delete-tgw-domain)

## Creating an IGMP multicast domain<a name="create-tgw-igmp-domain"></a>

If you have not already done so, review the available multicast domain attributes\. For more information, see [Working with multicast](working-with-multicast.md)\.

------
#### [ Console ]

**To create an IGMP multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Choose **Create transit gateway multicast domain**\.

1. For **Name tag**, enter a name for the domain\.

1. For **Transit gateway ID**, choose the transit gateway that processes the multicast traffic\.

1. For **IGMPv2 support**, select the check box\.

1. For **Static sources support**, clear the check box\.

1. To automatically accept cross\-account subnet associations for this multicast domain, select **Auto accept shared associations**\.

1. Choose **Create transit gateway multicast domain**\.

------
#### [ Command line ]

**To create an IGMP multicast domain using the AWS CLI**  
Use the [create\-transit\-gateway\-multicast\-domain](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-multicast-domain.html) command\.

```
aws ec2 create-transit-gateway-multicast-domain --transit-gateway-id tgw-0xexampleid12345 --options StaticSourcesSupport=disable,Igmpv2Support=enable
```

------

## Creating a static source multicast domain<a name="create-tgw-domain"></a>

If you have not already done so, review the available multicast domain attributes\. For more information, see [Working with multicast](working-with-multicast.md)\.

------
#### [ Console ]

**To create a static multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Choose **Create transit gateway multicast domain**\.

1. For **Name tag**, enter a name to identify the domain\.

1. For **Transit gateway ID**, choose the transit gateway that processes the multicast traffic\.

1. For **IGMPv2 support**, clear the check box\.

1. For **Static sources support**, select the check box\.

1. To automatically accept cross\-account subnet associations for this multicast domain, select **Auto accept shared associations**\.

1. Choose **Create transit gateway multicast domain**\.

------
#### [ Command line ]

**To create a static multicast domain using the AWS CLI**  
Use the [create\-transit\-gateway\-multicast\-domain](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-multicast-domain.html) command\.

```
aws ec2 create-transit-gateway-multicast-domain --transit-gateway-id tgw-0xexampleid12345 --options StaticSourcesSupport=enable,Igmpv2Support=disable
```

------

## Associating VPC attachments and subnets with a multicast domain<a name="associate-attachment-to-domain"></a>

Use the following procedure to associate a VPC attachment with a multicast domain\. When you create an association, you can then select the subnets to include in the multicast domain\. 

Before you begin, you must create a VPC attachment on your transit gateway\. For more information, see [Transit gateway attachments to a VPC](tgw-vpc-attachments.md)\.

------
#### [ Console ]

**To associate VPC attachments with a multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain, and then choose **Actions**, **Create association**\.

1. For **Choose attachment to associate**, select the transit gateway attachment\.

1. For **Choose subnets to associate**, select the subnets to include in the multicast domain\.

1. Choose **Create association**\.

------
#### [ Command line ]

**To associate VPC attachments with a multicast domain using the AWS CLI**  
Use the [associate\-transit\-gateway\-multicast\-domain](https://docs.aws.amazon.com/cli/latest/reference/ec2/associate-transit-gateway-multicast-domain.html) command\.

------

## Viewing your multicast domain associations<a name="view-tgw-domain-association"></a>

You can view your multicast domains to verify that they are available, and that they contain the appropriate subnets and attachments\.

------
#### [ Console ]

**To view a multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose the **Associations** tab\.

------
#### [ Command line ]

**To view a multicast domain using the AWS CLI**  
Use the [describe\-transit\-gateway\-multicast\-domains](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-multicast-domains.html) command\.

------

## Disassociating subnets from a multicast domain<a name="remove-subnet-association"></a>

Use the following procedure to disassociate subnets from a multicast domain\.

------
#### [ Console ]

**To disassociate subnets using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose the **Associations** tab\.

1. Select the subnet, and then choose **Actions**, **Delete association**\.

------
#### [ Command line ]

**To disassociate subnets using the AWS CLI**  
Use the [disassociate\-transit\-gateway\-multicast\-domain](https://docs.aws.amazon.com/cli/latest/reference/ec2/disassociate-transit-gateway-multicast-domain.html) command\.

------

## Adding tags to a multicast domain<a name="tgw-domain-tagging"></a>

Add tags to your resources to help organize and identify them, such as by purpose, owner, or environment\. You can add multiple tags to each multicast domain\. Tag keys must be unique for each multicast domain\. If you add a tag with a key that is already associated with the multicast domain, it updates the value of that tag\. For more information, see [Tagging your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

------
#### [ Console ]

**To add tags to a multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose **Actions**, **Manage tags**\.

1. For each tag, choose **Add new tag** and enter a **Key** and **Value** for the tag\.

1. Choose **Save**\.

------
#### [ Command line ]

**To add tags to a multicast domain using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command\.

------

## Deleting a multicast domain<a name="delete-tgw-domain"></a>

Use the following procedure to delete a multicast domain\.

------
#### [ Console ]

**To delete a multicast domain using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain, and then choose **Actions**, **Delete multicast domain**\.

1. When prompted for confirmation, enter **delete** and then choose **Delete**\.

------
#### [ Command line ]

**To delete a multicast domain using the AWS CLI**  
Use the [delete\-transit\-gateway\-multicast\-domain](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-multicast-domain.html) command\.

------