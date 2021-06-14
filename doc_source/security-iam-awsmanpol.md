# AWS managed policies for transit gateways and Transit Gateway Network Manager<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

## AWS managed policy: AWSNetworkManagerServiceRolePolicy<a name="security-iam-AWSServiceRoleForNetworkManager"></a>

This policy is attached to the service\-linked role named **AWSServiceRoleForNetworkManager** to allow Network Manager to call API actions on your behalf when you work with global networks\. For more information, see [Transit Gateway Network Manager service\-linked role](nm-service-linked-roles.md)\.

## Network Manager updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for Network Manager since this service began tracking these changes in April 2021\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Network Manager Document history page\.


| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSServiceRoleForNetworkManager](https://docs.aws.amazon.com/vpc/latest/tgw/nm-service-linked-roles.html): updated existing policy  | Network Manager added permissions to call the following API actions: directconnect:DescribeDirectConnectGateways, ec2:DescribeVpnConnections, ec2:DescribeVpcs, ec2:GetTransitGatewayRouteTableAssociations, ec2:SearchTransitGatewayRoutes, ec2:DescribeTransitGatewayPeeringAttachments, ec2:DescribeTransitGatewayConnects and ec2:DescribeTransitGatewayConnectPeers\.  | June 1, 2021 | 