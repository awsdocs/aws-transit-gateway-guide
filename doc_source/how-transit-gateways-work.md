# Como funciona um Gateway de Transito<a name="how-transit-gateways-work"></a>

A *transit gateway* acts as a regional virtual router for traffic flowing between your virtual private clouds \(VPC\) and VPN connections\. A transit gateway scales elastically based on the volume of network traffic\. Routing through a transit gateway operates at layer 3, where the packets are sent to a specific next\-hop attachment, based on their destination IP addresses\.

## Resource Attachments<a name="tgw-attachments-overview"></a>

A transit gateway attachment is both a source and a destination of packets\. You can attach the following resources to your transit gateway, if they are in the same Region as the transit gateway:
+ One or more VPCs
+ One or more VPN connections

## Availability Zones<a name="tgw-az-overview"></a>

When you attach a VPC to a transit gateway, you must enable one or more Availability Zones to be used by the transit gateway to route traffic to resources in the VPC subnets\. To enable each Availability Zone, you specify exactly one subnet\. The transit gateway places a network interface in that subnet using one IP address from the subnet\. After you enable an Availability Zone, traffic can be routed to all subnets in that Availability Zone, not just the specified subnet\.

We recommend that you enable multiple Availability Zones to ensure availability\. If one Availability Zone becomes unavailable or has no healthy attachments, the transit gateway can route traffic to your VPC using a healthy attachment in a different Availability Zone\.

## Routing<a name="tgw-routing-overview"></a>

Your transit gateway routes IPv4 and IPv6 packets between attachments using transit gateway route tables\. You can configure these route tables to propagate routes from the route tables for the attached VPCs and VPN connections\. You can also add static routes to the transit gateway route tables\. When a packet comes from one attachment, it is routed to another attachment using the route table that matches the destination IP address\.

### Route Tables<a name="tgw-route-tables-overview"></a>

Your transit gateway automatically comes with a default route table\. By default, this route table is the default association route table and the default propagation route table\. Alternatively, if you disable route propagation, we do not create a default route table for the transit gateway

You can create additional route tables for your transit gateway\. This enables you to isolate subnets of attachments\. Each attachment can be related to one or more route tables through route table association and route table propagation\.

### Route Table Association<a name="tgw-route-table-association-overview"></a>

You can associate a transit gateway attachment with a single route table\. Each route table can be associated with zero to many attachments and forward packets to attachments or other route tables\.

### Route Propagation<a name="tgw-route-propagation-overview"></a>

Each attachment comes with routes that can be installed to one or more transit gateway route tables\. For a VPC attachment, these are the CIDR blocks of the VPC\. For a VPN connection attachment, these are the prefixes that are advertised over the BGP session established with the VPN connection\. When an attachment is propagated to a transit gateway route table, these routes are installed in the route table\.

## Scenarios<a name="transit-gateway-scenarios"></a>

The following are common use cases for transit gateways\. Your transit gateways are not limited to these use cases\.

### Centralized Router<a name="tgw-centralized-router"></a>

You can configure your transit gateway as a centralized router that connects all of your VPCs and VPN connections\. In this scenario, all attachments are associated with the transit gateway route table and propagate to the transit gateway route table\. Therefore, all attachments can route packets to each other, with the transit gateway serving as a simple layer 3 IP hub\.

### Isolated Routers<a name="tgw-isolated-routers"></a>

You can configure your transit gateway as multiple isolated routers\. This is similar to using multiple transit gateways, but provides more flexibility in cases where the routes and attachments might change\. In this scenario, each isolated router has a single route table\. All attachments associated with an isolated router propagate and associate with its route table\. Attachments associated with one isolated router can route packets to each other, but cannot route packets to or receive packets from the attachments for another isolated router\.

### Edge Consolidator<a name="tgw-edge-consolidator"></a>

You can configure your transit gateway such that your VPCs can route packets to one or more VPN connections but your VPCs cannot route packets to each other\. In this scenario, you create a route table for the VPCs and a route table for the VPN connections\.
