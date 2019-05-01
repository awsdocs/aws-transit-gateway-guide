# Limits for Your Transit Gateways<a name="transit-gateway-limits"></a>

Your AWS account has the following limits related to transit gateways\. To request a limit increase, use the [Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=vpc)\.
+ Number of transit gateways per Region per account: 5
+ Number of transit gateway attachments per VPC: 5
+ Number of transit gateway route tables per transit gateway: 20
+ Number of routes per transit gateway route table: 10,000
+ Total number of transit gateway attachments per Region per account: 5,000
+ Maximum bandwidth \(burst\) per VPC connection: 50 Gbps1
+ Maximum bandwidth per VPN connection: 1\.25 Gbps

  This is a hard limit\.
+ Number of AWS Direct Connect gateways per transit gateway: 20

  This limit cannot be increased\.
+ Transit gateways per AWS Direct Connect gateway: 3

  This limit cannot be increased\.

1:You can use ECMP to get higher VPN bandwidth by aggregating multiple VPN connections\. 