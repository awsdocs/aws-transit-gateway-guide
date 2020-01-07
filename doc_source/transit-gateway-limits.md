# Quotas for Your Transit Gateways<a name="transit-gateway-limits"></a>

Your AWS account has the following service quotas related to transit gateways\. To request an increase, use the [Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=vpc)\. For more information about service quotas, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.
+ Total number of transit gateway attachments per transit gateway: 5,000
+ Number of transit gateway attachments per VPC: 5

  This value cannot be increased\.
+ Number of transit gateways per Region per account: 5
+ Number of transit gateway route tables per transit gateway: 20
+ Number of static routes per transit gateway: 10,000

  For VPC route table quotas, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.
+ Maximum bandwidth \(burst\) per VPC connection: 50 Gbps
+ Maximum bandwidth per VPN connection: 1\.25 Gbps

  This is a hard value\. You can use ECMP to get higher VPN bandwidth by aggregating multiple VPN connections\.
+ Number of AWS Direct Connect gateways per transit gateway: 20

  This value cannot be increased\.
+ Transit gateways per AWS Direct Connect gateway: 3

  This value cannot be increased\.
+ Number of multicast domains per transit gateway: 20
+ Number of multicast group members and sources per transit gateway: 1000
+ Number of pending transit gateway peering attachments transit gateway:10

For more information about service quotas for Transit Gateway Network Manager, see [Network Manager Quotas](how-network-manager-works.md#network-manager-limits)\.