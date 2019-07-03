# How Transit Gateways Work<a name="how-transit-gateways-work"></a>

A *transit gateway* acts as a regional virtual router for traffic flowing between your virtual private clouds \(VPC\) and VPN connections\. A transit gateway scales elastically based on the volume of network traffic\. Routing through a transit gateway operates at layer 3, where the packets are sent to a specific next\-hop attachment, based on their destination IP addresses\.

## Resource Attachments<a name="tgw-attachments-overview"></a>

A transit gateway attachment is both a source and a destination of packets\. You can attach the following resources to your transit gateway, if they are in the same Region as the transit gateway:
+ One or more VPCs
+ One or more VPN connections
+ One or more AWS Direct Connect gateways

## Availability Zones<a name="tgw-az-overview"></a>

When you attach a VPC to a transit gateway, you must enable one or more Availability Zones to be used by the transit gateway to route traffic to resources in the VPC subnets\. To enable each Availability Zone, you specify exactly one subnet\. The transit gateway places a network interface in that subnet using one IP address from the subnet\. After you enable an Availability Zone, traffic can be routed to all subnets in that Availability Zone, not just the specified subnet\.

We recommend that you enable multiple Availability Zones to ensure availability\. If one Availability Zone becomes unavailable or has no healthy attachments, the transit gateway can route traffic to your VPC using a healthy attachment in a different Availability Zone\.

## Routing<a name="tgw-routing-overview"></a>

Your transit gateway routes IPv4 and IPv6 packets between attachments using transit gateway route tables\. You can configure these route tables to propagate routes from the route tables for the attached VPCs and VPN connections\. You can also add static routes to the transit gateway route tables\. When a packet comes from one attachment, it is routed to another attachment using the route table that matches the destination IP address\.

### Route Tables<a name="tgw-route-tables-overview"></a>

Your transit gateway automatically comes with a default route table\. By default, this route table is the default association route table and the default propagation route table\. Alternatively, if you disable route propagation, we do not create a default route table for the transit gateway

You can create additional route tables for your transit gateway\. This enables you to isolate subnets of attachments\. Each attachment can be associated with one route table\. An attachment can propagate their routes to one or more route tables\.

### Route Table Association<a name="tgw-route-table-association-overview"></a>

You can associate a transit gateway attachment with a single route table\. Each route table can be associated with zero to many attachments and forward packets to attachments or other route tables\.

### Route Propagation<a name="tgw-route-propagation-overview"></a>

Each attachment comes with routes that can be installed to one or more transit gateway route tables\. For a VPC attachment, these are the CIDR blocks of the VPC\. For a VPN connection attachment, these are the prefixes that are advertised over the BGP session established with the VPN connection\. When an attachment is propagated to a transit gateway route table, these routes are installed in the route table\.

### Route Evaluation Order<a name="tgw-route-evaluation-overview"></a>

Transit gateway routes are evaluated in the following order:
+ The longest prefix route for the destination address\.
+ If routes are the same with different targets:
  + Static routes have a higher precedence than propagated routes\.
  + Among propagated routes, VPCs CIDRs have a higher precedence than Direct Connect gateways than Client VPN\.

Consider the following VPC route table\. The VPC local roue has the highest priority, followed by the routes that are the most specific\. When a static and propagated route have the destination, the static route has a higher priority\.


| Destination | Target | Priority | 
| --- | --- | --- | 
| 10\.0\.0\.0/6 |  local  | 1 | 
| 192\.168\.0\.0/16 | pcx\-12345 | 2 | 
| 172\.31\.0\.0/16 | vgw\-12345 \(static\) ortgw\-12345 \(static\) | 2 | 
| 172\.31\.0\.0/16 | vgw\-12345 \(propagated\) | 3 | 
| 0\.0\.0\.0/0 | igw\-12345 | 4 | 

Consider the following transit gateway route table\. When a static and propagated route have the destination, the static route has a higher priority\. If you want to use the AWS Direct Connect gateway attachment over the VPN attachment, then use a BGP VPN connection and propagate the routes in the transit gateway route table\.


| Destination | Attachment \(Target\) | Resource type | Route type | Priority | 
| --- | --- | --- | --- | --- | 
| 10\.0\.0\.0/6 | tgw\-attach\-123 \| vpc\-1234 | VPC | Static or propagated | 1 | 
| 192\.168\.0\.0/16 | tgw\-attach\-789 \| vpn\-5678 | VPN | Static | 2 | 
| 172\.31\.0\.0/16 | tgw\-attach\-456 \| dxgw\_id | AWS Direct Connect gateway | Propagated | 3 | 
| 172\.31\.0\.0/16 | tgw\-attach\-789 \| vpn\-5678 | VPN | Propagated | 4 | 