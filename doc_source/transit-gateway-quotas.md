# Quotas for your transit gateways<a name="transit-gateway-quotas"></a>

Your AWS account has the following service quotas \(previously referred to as *quotas*\) related to transit gateways\. Unless indicated otherwise, you can request an increase for a quota\.

The Service Quotas console provides information about Network Manager quotas\. You can use the Service Quotas console to view default quotas and [request quota increases](https://console.aws.amazon.com/servicequotas/home?) for adjustable quotas\. For more information, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.

## General<a name="general-quotas"></a>
+ Number of transit gateways per Region per account: 5
+ Number of transit gateways per VPC: 5

  This quota cannot be increased\.
+ Number of transit gateway CIDR blocks per transit gateway: 5

  The transit gateway CIDR blocks are used in the [Transit gateway Connect attachments and Transit Gateway Connect peers](tgw-connect.md) feature\. This quota cannot be increased\.

## Routing<a name="routing-quotas"></a>
+ Number of transit gateway route tables per transit gateway: 20
+ Number of routes per transit gateway: 10,000

  For VPC route table quotas, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*\.
+ Dynamic routes advertised from a virtual router appliance to a Transit Gateway Connect peer: 1,000
+ Routes advertised from a Transit Gateway Connect peer on a transit gateway to a virtual router appliance: 5,000

  Advertised routes come from the route table that's associated with the Connect attachment\.
+ Number of static route for a prefix to a single attachment: 1

## Transit gateway attachments<a name="attachments-quotas"></a>
+ Total number of transit gateway attachments per transit gateway: 5,000
+ Number of unique transit gateway attachments per VPC: 5

  This value cannot be increased\. A transit gateway cannot have more than one attachment to the same VPC\.
+ Number of transit gateway peering attachments per transit gateway: 50
+ Number of transit gateway peering attachments between two transit gateways: 1
+ Number of pending transit gateway peering attachments per transit gateway: 10
+ Number of Transit Gateway Connect peers \(GRE tunnels\) per transit gateway Connect attachment: 4

  This value cannot be increased\.

## Bandwidth<a name="bandwidth-quotas"></a>
+ Maximum bandwidth \(burst\) per VPC, Direct Connect gateway, or peered transit gateway connection: 50 Gbps
+ Maximum bandwidth per VPN tunnel: 1\.25 Gbps

  This is a hard value\. You can use ECMP to get higher VPN bandwidth by aggregating multiple VPN tunnels\. To use ECMP, the VPN connection must be configured for dynamic routing\. ECMP is not supported on VPN connections that use static routing\.
+ Maximum bandwidth \(burst\) per Transit Gateway Connect peer \(GRE tunnel\) per Connect attachment: 5 Gbps 

  This is a hard value\. You can create up to 4 Transit Gateway Connect peers per Connect attachment \(up to 20 Gbps in total bandwidth per Connect attachment\), as long as the underlying transport \(VPC or AWS Direct Connect\) attachment supports the required bandwidth\. You can use equal\-cost multi\-path routing \(ECMP\) to get higher bandwidth by scaling horizontally across multiple Transit Gateway Connect peers of the same Connect attachment or across multiple Connect attachments on the same transit gateway\. The transit gateway cannot use ECMP between the BGP peerings of the same Transit Gateway Connect peer\. 

## AWS Direct Connect gateways<a name="direct-connect-quotas"></a>
+ Number of AWS Direct Connect gateways per transit gateway: 20

  This value cannot be increased\.
+ Transit gateways per AWS Direct Connect gateway: 3

  This value cannot be increased\.

## MTU<a name="mtu-quotas"></a>
+ The maximum transmission unit \(MTU\) of a network connection is the size, in bytes, of the largest permissible packet that can be passed over the connection\. The larger the MTU of a connection, the more data that can be passed in a single packet\. A transit gateway supports an MTU of 8500 bytes for traffic between VPCs, Direct Connect gateway and peering attachments\. Traffic over VPN connections can have an MTU of 1500 bytes\. 
+ Packets with a size larger than 8500 bytes that arrive at the transit gateway are dropped\.
+ The transit gateway does not generate the FRAG\_NEEDED for ICMPv4 packet, or the Packet Too Big \(PTB\) for ICMPv6 packet\. Therefore, the Path MTU Discovery \(PMTUD\) is not supported\.
+ The transit gateway enforces Maximum Segment Size \(MSS\) clamping for all packets\. For more information, see [RFC879](https://tools.ietf.org/html/rfc879)\.

## Multicast<a name="multicast-quotas"></a>
+ Number of multicast domains per transit gateway: 20
+ Number of multicast group members and sources per transit gateway: 1000
+ Number of multicast group members per transit gateway multicast group: 100
+ Number of multicast domain associations per VPC: 20
+ Number of sources per transit gateway multicast group: 1
+ Number of static multicast group and IGMPv2 multicast group members and sources per transit gateway: 10,000 
+ Number of static multicast group and IGMPv2 multicast group members per transit gateway multicast group: 100
+ Maximum multicast throughput per flow: 1 Gbps 
+ Maximum aggregate multicast throughput per subnet: 4 Gbps 
+ Maximum aggregate multicast throughput per subnet including unicast traffic: 50 Gbps 

## Transit Gateway Network Manager<a name="network-manager-quotas"></a>
+ Global networks per AWS account: 5
+ Devices per global network: 200
+ Links per global network: 200
+ Sites per global network: 200
+ Connections per global network: 500

## Additional quota resources<a name="additional-quotas"></a>

For more information, see the following:
+ [Site\-to\-Site VPN quotas](https://docs.aws.amazon.com/vpn/latest/s2svpn/vpn-limits.html) in the *AWS Site\-to\-Site VPN User Guide*
+ [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html) in the *Amazon VPC User Guide*
+ [AWS Direct Connect quotas](https://docs.aws.amazon.com/directconnect/latest/UserGuide/limits.html) in the *AWS Direct Connect User Guide*