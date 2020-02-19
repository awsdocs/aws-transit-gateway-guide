# Quotas for Your Transit Gateways<a name="transit-gateway-limits"></a>

Your AWS account has the following service quotas related to transit gateways\. To request an increase, use the [Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=vpc)\. For more information about service quotas, see [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.

## General<a name="general-quota"></a>
+ Number of transit gateways per Region per account: 5

## Routing<a name="general-quota"></a>
+ Number of transit gateway route tables per transit gateway: 20
+ Number of static routes per transit gateway: 10,000

  For VPC route table quotas, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.

## Transit Gateway Attachments<a name="attachments-quota"></a>
+ Total number of transit gateway attachments per transit gateway: 5,000
+ Number of transit gateway attachments per VPC: 5

  This value cannot be increased\.
+ Number of transit gateway peering attachments per transit gateway: 50
+ Number of pending transit gateway peering attachments transit gateway:10

## Bandwidth<a name="bandwidth-quota"></a>
+ Maximum bandwidth \(burst\) per VPC connection: 50 Gbps
+ Maximum bandwidth per VPN connection: 1\.25 Gbps

  This is a hard value\. You can use ECMP to get higher VPN bandwidth by aggregating multiple VPN connections\.

## AWS Direct Connect Gateways<a name="direct-connect-quota"></a>
+ Number of AWS Direct Connect gateways per transit gateway: 20

  This value cannot be increased\.
+ Transit gateways per AWS Direct Connect gateway: 3

  This value cannot be increased\.

## Multicast<a name="multicast-quota"></a>
+ Number of multicast domains per transit gateway: 20
+ Number of multicast group members and sources per transit gateway: 1000
+ Number of multicast domains per transit gateway: 20
+ Number of members per transit gateway multicast group: 100
+ Number of multicast domain associations per VPC: 20
+ Number of sources per transit gateway multicast group: 1

## Additional Quota Resources<a name="additional-quota"></a>

For information about quotas that apply to VPN attachments, see [Site\-to\-Site VPN Quotas](https://docs.aws.amazon.com/vpn/latest/s2svpn/vpn-limits.html) in the *AWS Site\-to\-Site VPN User Guide*\.

For information about quotas that apply to VPC attachments see [Amazon VPC Quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.

For information about quotas that apply to Direct Connect gateway attachments see [AWS Direct Connect Quotas](https://docs.aws.amazon.com/directconnect/latest/UserGuide/limits.html) in the* AWS Direct Connect User Guide*\.

For more information about service quotas for Transit Gateway Network Manager, see [Network Manager Quotas](how-network-manager-works.md#network-manager-limits)\.