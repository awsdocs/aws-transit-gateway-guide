# Transit gateway design best practices<a name="tgw-best-design-practices"></a>

The following are best practices for your transit gateway design:
+ Use a separate subnet for each transit gateway VPC attachment\. For each subnet, use a small CIDR, for example `/28`, so that you have more addresses for EC2 resources\. When you use a separate subnet, you can configure the following:
  + Keep the inbound and outbound network ACLs associated with the transit gateway subnets open\. 
  + Depending on your traffic flow, you can apply network ACLs to your workload subnets\.
+ Create one network ACL and associate it with all of the subnets that are associated with the transit gateway\. Keep the network ACL open in both the inbound and outbound directions\.
+ Associate the same VPC route table with all of the subnets that are associated with the transit gateway, unless your network design requires multiple VPC route tables \(for example, a middle\-box VPC that routes traffic through multiple NAT gateways\)\. 
+ Use Border Gateway Protocol \(BGP\) Site\-to\-Site VPN connections\. If your customer gateway device or firewall for the connection supports multipath, enable the feature\. 
+ Enable route propagation for AWS Direct Connect gateway attachments and BGP Site\-to\-Site VPN attachments\.
+ When migrating from VPC peering to use an AWS Transit Gateway,
  + A transit gateway does not support Security Group referencing\.
  + An MTU size mismatch between VPC peering and the transit gateway might result in some packets dropping for asymmetric traffic\. Update both VPCs at the same time to avoid jumbo packets dropping due to size mismatch\.
+ You do not need additional transit gateways for high availability, because transit gateways are highly available by design\.
+ Limit the number of transit gateway route tables unless your design requires multiple transit gateway route tables\.
+ For redundancy, use a single Transit Gateway in each Region for disaster recovery\.
+ For deployments with multiple transit gateways, we recommend that you use a unique Autonomous System Number \(ASN\) for each of your transit gateways\. Transit Gateway also supports intra\-Region peering\. For more information, see [Building a global network using AWS Transit Gateway Intra\-Region peering](https://aws.amazon.com/blogs/networking-and-content-delivery/building-a-global-network-using-aws-transit-gateway-inter-region-peering/)\.