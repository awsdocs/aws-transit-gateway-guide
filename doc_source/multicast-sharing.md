# Working with shared multicast domains<a name="multicast-sharing"></a>

With multicast domain sharing, multicast domain owners can share the domain with other AWS accounts inside its organization in AWS Organizations\. As the multicast domain owner, you can create and manage the multicast domain centrally\. Consumers can perform the following operations on a shared multicast domain:
+ Register and deregister group members or group sources in the multicast domain
+ Associate a subnet with the multicast domain, and disassociate subnets from the multicast domain

A multicast domain owner can share a multicast domain with:
+ Specific AWS accounts inside its organization in AWS Organizations
+ An organizational unit inside its organization in AWS Organizations
+ Its entire organization in AWS Organizations

**Topics**
+ [Prerequisites for sharing a multicast domain](#sharing-prereqs)
+ [Related services](#sharing-related)
+ [Sharing across Availability Zones](#sharing-azs)
+ [Sharing a multicast domain](#sharing-share)
+ [Unsharing a shared multicast domain](#sharing-unshare)
+ [Identifying a shared multicast domain](#sharing-identify)
+ [Shared multicast domain permissions](#sharing-perms)
+ [Billing and metering](#sharing-billing)
+ [Quotas](#sharing-quotas)

## Prerequisites for sharing a multicast domain<a name="sharing-prereqs"></a>
+ To share a multicast domain, you must own it in your AWS account\. You cannot share a multicast domain that has been shared with you\.
+ To share a multicast domain with your organization or an organizational unit in AWS Organizations, you must enable sharing with AWS Organizations\. For more information, see [ Enable Sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html#getting-started-sharing-orgs) in the *AWS RAM User Guide*\.

## Related services<a name="sharing-related"></a>

Multicast domain sharing integrates with AWS Resource Access Manager \(AWS RAM\)\. AWS RAM is a service that enables you to share your AWS resources with any AWS account or through AWS Organizations\. With AWS RAM, you share resources that you own by creating a *resource share*\. A resource share specifies the resources to share, and the consumers with whom to share them\. Consumers can be individual AWS accounts, or organizational units or an entire organization in AWS Organizations\.

For more information about AWS RAM, see the *[AWS RAM User Guide](https://docs.aws.amazon.com/ram/latest/userguide/)*\.

## Sharing across Availability Zones<a name="sharing-azs"></a>

To ensure that resources are distributed across the Availability Zones for a Region, we independently map Availability Zones to names for each account\. This could lead to Availability Zone naming differences across accounts\. For example, the Availability Zone `us-east-1a` for your AWS account might not have the same location as `us-east-1a` for another AWS account\.

To identify the location of your multicast domain relative to your accounts, you must use the *Availability Zone ID* \(AZ ID\)\. The AZ ID is a unique and consistent identifier for an Availability Zone across all AWS accounts\. For example, `use1-az1` is an AZ ID for the `us-east-1` Region and it is the same location in every AWS account\.

**To view the AZ IDs for the Availability Zones in your account**

1. Open the AWS RAM console at [https://console\.aws\.amazon\.com/ram](https://console.aws.amazon.com/ram/)\.

1. The AZ IDs for the current Region are displayed in the **Your AZ ID** panel on the right\-hand side of the screen\.

## Sharing a multicast domain<a name="sharing-share"></a>

When an owner shares a multicast domain with a consumer, the consumer can do the following:
+ Register and deregister group members or group sources
+ Associate and disassociate subnets

To share a multicast domain, you must add it to a resource share\. A resource share is an AWS RAM resource that lets you share your resources across AWS accounts\. A resource share specifies the resources to share, and the consumers with whom they are shared\. When you share a multicast domain using the Amazon VPC Console, you add it to an existing resource share\. To add the multicast domain to a new resource share, you must first create the resource share using the [AWS RAM console](https://console.aws.amazon.com/ram)\.

If you are part of an organization in AWS Organizations and sharing within your organization is enabled, consumers in your organization are automatically granted access to the shared multicast domain\. Otherwise, consumers receive an invitation to join the resource share and are granted access to the shared multicast domain after accepting the invitation\.

You can share a multicast domain that you own using the \*Amazon VPC Console console, AWS RAM console, or the AWS CLI\.

**To share a multicast domain that you own using the \*Amazon VPC Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Multicast Domains**\.

1. Select your multicast domain, and then choose **Actions**, **Share multicast domain**\. 

1. Select your resource share and choose **Share multicast domain**\. 

**To share a multicast domain that you own using the AWS RAM console**  
See [Creating a Resource Share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-create) in the *AWS RAM User Guide*\.

**To share a multicast domain that you own using the AWS CLI**  
Use the [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) command\.

## Unsharing a shared multicast domain<a name="sharing-unshare"></a>

When a shared multicast domain is unshared, the following happens to consumer multicast domain resources:
+ Consumer subnets are disassociated from the multicast domain\. The subnets remain in the consumer account\.
+ Consumer group sources and group members are disassociated from the multicast domain, and then deleted from the consumer account\.

 To unshare a multicast domain, you must remove it from the resource share\. You can do this from the AWS RAM console or the AWS CLI\.

To unshare a shared multicast domain that you own, you must remove it from the resource share\. You can do this using the \*Amazon VPC Console, AWS RAM console, or the AWS CLI\.

**To unshare a shared multicast domain that you own using the \*Amazon VPC Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Multicast Domains**\.

1. Select your multicast domain, and then choose **Actions**, **Stop sharing**\. 

**To unshare a shared multicast domain that you own using the AWS RAM console**  
See [Updating a Resource Share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-update) in the *AWS RAM User Guide*\.

**To unshare a shared multicast domain that you own using the AWS CLI**  
Use the [disassociate\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html) command\.

## Identifying a shared multicast domain<a name="sharing-identify"></a>

Owners and consumers can identify shared multicast domains using the \*Amazon VPC Console and AWS CLI

**To identify a shared multicast domain using the \*Amazon VPC Console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Multicast Domains**\.

1. Select your multicast domain\.

1. On the **Transit Multicast Domain Details **page, view the **Owner ID** to identify the AWS account ID of the multicast domain\.

**To identify a shared multicast domain using the AWS CLI**  
Use the [describe\-transit\-gateway\-multicast\-domains](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-multicast-domains.html) command\. The command returns the multicast domains that you own and multicast domains that are shared with you\. `OwnerId` shows the AWS account ID of the multicast domain owner\.

## Shared multicast domain permissions<a name="sharing-perms"></a>

### Permissions for owners<a name="perms-owner"></a>

Owners are responsible for managing the multicast domain and the members and attachments that they register or associate with the domain\. Owners can change or revoke shared access at any time\. They can use AWS Organizations to view, modify, and delete resources that consumers create on shared multicast domains\.

### Permissions for consumers<a name="perms-consumer"></a>

Consumers can perform the following operations on shared multicast domains in the same way that they would on multicast domains that they created:
+ Register and deregister group members or group sources in the multicast domain
+ Associate a subnet with the multicast domain, and disassociate subnets from the multicast domain

Consumers are responsible for managing the resources that they create on the shared multicast domain\.

Customers cannot view or modify resources owned by other consumers or by the multicast domain owner, and they cannot modify multicast domains that are shared with them\. 

## Billing and metering<a name="sharing-billing"></a>

There are no additional charges for sharing multicast domains for either the owner, or consumers\. 

## Quotas<a name="sharing-quotas"></a>

A shared multicast domain counts toward the owner's and consumer's multicast domain quotas\.