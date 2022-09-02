# Quotas for your transit gateways<a name="transit-gateway-quotas"></a>

Your AWS account has the following quotas \(previously referred to as *limits*\) related to transit gateways\.

The Service Quotas console provides information about Network Manager quotas\. You can use the Service Quotas console to view default quotas and [request quota increases](https://console.aws.amazon.com/servicequotas/home?) for adjustable quotas\. For more information, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

If an adjustable quota is not yet available in Service Quotas, you can open a support case\.

## General<a name="general-quotas"></a>

**Note**  
AWS Transit Gateway doesn't support Security Group referencing when migrating from VPC peering to use transit gateways\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Transit gateways per account | 5 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-A2478D36) | 
| CIDR blocks per transit gateway | 5 | No | 

The CIDR blocks are used in the [Transit gateway Connect attachments and Transit Gateway Connect peers](tgw-connect.md) feature\.

## Routing<a name="routing-quotas"></a>


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Transit gateway route tables per transit gateway | 20 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-43872EB7) | 
| Static routes per transit gateway | 10,000 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-BCC1FB47) | 
| Dynamic routes advertised from a virtual router appliance to a Transit Gateway Connect peer | 1,000 | No | 
| Routes advertised from a Transit Gateway Connect peer on a transit gateway to a virtual router appliance | 5,000 | No | 
| Static routes for a prefix to a single attachment | 1 | No | 

Advertised routes come from the route table that's associated with the Connect attachment\.

## Transit gateway attachments<a name="attachments-quotas"></a>

A transit gateway cannot have more than one VPC attachment to the same VPC\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Attachments per transit gateway | 5,000 | No | 
| Transit gateways per VPC | 5 | No | 
| Peering attachments per transit gateway | 50 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-A1B5A36F) | 
| Pending peering attachments per transit gateway | 10 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-62499967) | 
| Peering attachments between two transit gateways | 1 | No | 
| Transit Gateway Connect peers \(GRE tunnels\) per transit gateway Connect attachment | 4 | No | 

## Bandwidth<a name="bandwidth-quotas"></a>

There are many factors that can affect realized bandwidth through a Site\-to\-Site VPN connection, including but not limited to: packet size, traffic mix \(TCP/UDP\), shaping or throttling policies on intermediate networks, internet weather, and specific application requirements\.


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Maximum bandwidth per VPC attachment, AWS Direct Connect gateway, or peered transit gateway connection | Up to 50 Gbps | No | 
| Maximum packets per second per transit gateway attachment \(VPC, VPN, Direct Connect, and peering attachments\) | Up to 5,000,000 | No | 
| Maximum bandwidth per VPN tunnel | Up to 1\.25 Gbps | No | 
| Maximum packets per second per VPN tunnel | Up to 140,000 | No | 
| Maximum bandwidth per Transit Gateway Connect peer \(GRE tunnel\) per Connect attachment | Up to 5 Gbps | No | 
| Maximum packets per second per Connect peer | Up to 300,000 | No | 

You can use equal\-cost multipath routing \(ECMP\) to get higher VPN bandwidth by aggregating multiple VPN tunnels\. To use ECMP, the VPN connection must be configured for dynamic routing\. ECMP is not supported on VPN connections that use static routing\.

You can create up to 4 Transit Gateway Connect peers per Connect attachment \(up to 20 Gbps in total bandwidth per Connect attachment\), as long as the underlying transport \(VPC or AWS Direct Connect\) attachment supports the required bandwidth\. You can use ECMP to get higher bandwidth by scaling horizontally across multiple Transit Gateway Connect peers of the same Connect attachment or across multiple Connect attachments on the same transit gateway\. The transit gateway cannot use ECMP between the BGP peerings of the same Transit Gateway Connect peer\. 

## AWS Direct Connect gateways<a name="direct-connect-quotas"></a>


| Name | Default | Adjustable | 
| --- | --- | --- | 
| AWS Direct Connect gateways per transit gateway | 20 | No | 
| Transit gateways per AWS Direct Connect gateway | 3 | No | 

## MTU<a name="mtu-quotas"></a>
+ The maximum transmission unit \(MTU\) of a network connection is the size, in bytes, of the largest permissible packet that can be passed over the connection\. The larger the MTU of a connection, the more data that can be passed in a single packet\. A transit gateway supports an MTU of 8500 bytes for traffic between VPCs, Direct Connect gateway, and peering attachments\. Traffic over VPN connections can have an MTU of 1500 bytes\. 
+ When migrating from VPC peering to use a transit gateway, an MTU size mismatch between VPC peering and the transit gateway might result in some asymmetric traffic packets dropping\. Update both VPCs at the same time to avoid jumbo packets dropping due to a size mismatch\.
+ Packets with a size larger than 8500 bytes that arrive at the transit gateway are dropped\.
+ The transit gateway does not generate the FRAG\_NEEDED for ICMPv4 packet, or the Packet Too Big \(PTB\) for ICMPv6 packet\. Therefore, the Path MTU Discovery \(PMTUD\) is not supported\.
+ The transit gateway enforces Maximum Segment Size \(MSS\) clamping for all packets\. For more information, see [RFC879](https://tools.ietf.org/html/rfc879)\.

## Multicast<a name="multicast-quotas"></a>


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Multicast domains per transit gateway | 20 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-31775423) | 
| Multicast network interfaces per transit gateway | 1,000 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-C673935A) | 
|  Members per transit gateway multicast group  | 100 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-C768F2D6) | 
| Multicast domain associations per VPC | 20 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-9F8FA74B) | 
| Sources per transit gateway multicast group | 1 | [Yes](https://console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-4F2F99E3) | 
|  Static and IGMPv2 multicast group members and sources per transit gateway  | 10,000 | No | 
| Static and IGMPv2 multicast group members per transit gateway multicast group | 100 | No | 
| Maximum multicast throughput per flow | 1 Gbps | No | 
| Maximum aggregate multicast throughput per Availability Zone | 20 Gbps | No | 

## AWS Network Manager<a name="network-manager-quotas"></a>


| Name | Default | Adjustable | 
| --- | --- | --- | 
| Global networks per AWS account | 5 | Yes | 
| Devices per global network | 200 | Yes | 
| Links per global network | 200 | Yes | 
| Sites per global network | 200 | Yes | 
| Connections per global network | 500 | No | 

## Additional quota resources<a name="additional-quotas"></a>

For more information, see the following:
+ [Site\-to\-Site VPN quotas](https://docs.aws.amazon.com/vpn/latest/s2svpn/vpn-limits.html) in the *AWS Site\-to\-Site VPN User Guide*
+ [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*
+ [AWS Direct Connect quotas](https://docs.aws.amazon.com/directconnect/latest/UserGuide/limits.html) in the *AWS Direct Connect User Guide*