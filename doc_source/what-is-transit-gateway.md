# What is a transit gateway?<a name="what-is-transit-gateway"></a>

A *transit gateway* is a network transit hub that you can use to interconnect your virtual private clouds \(VPCs\) and on\-premises networks\.

For more information, see [AWS Transit Gateway](https://aws.amazon.com/transit-gateway)\.

## Transit gateway concepts<a name="concepts"></a>

The following are the key concepts for transit gateways:
+ **Attachments** — You can attach the following:
  + One or more VPCs
  + A Connect SD\-WAN/third\-party network appliance
  + An AWS Direct Connect gateway
  + A peering connection with another transit gateway
  + A VPN connection to a transit gateway
+ **Transit gateway Maximum Transmission Unit \(MTU\)** — The maximum transmission unit \(MTU\) of a network connection is the size, in bytes, of the largest permissible packet that can be passed over the connection\. The larger the MTU of a connection, the more data that can be passed in a single packet\. A transit gateway supports an MTU of 8500 bytes for traffic between VPCs, AWS Direct Connect, Transit Gateway Connect, and peering attachments\. Traffic over VPN connections can have an MTU of 1500 bytes\.
+ **Transit gateway route table** — A transit gateway has a default route table and can optionally have additional route tables\. A route table includes dynamic and static routes that decide the next hop based on the destination IP address of the packet\. The target of these routes could be any transit gateway attachment\. By default, transit gateway attachments are associated with the default transit gateway route table\.
+ **Associations** — Each attachment is associated with exactly one route table\. Each route table can be associated with zero to many attachments\.
+ **Route propagation** — A VPC, VPN connection, or Direct Connect gateway can dynamically propagate routes to a transit gateway route table\. With a Connect attachment, the routes are propagated to a transit gateway route table by default\. With a VPC, you must create static routes to send traffic to the transit gateway \. With a VPN connection or a Direct Connect gateway, routes are propagated from the transit gateway to your on\-premises router using Border Gateway Protocol \(BGP\)\. With a peering attachment, you must create a static route in the transit gateway route table to point to the peering attachment\.

## How to get started with transit gateways<a name="how-to-get-started"></a>

Use the following resources to help you create and use a transit gateway\.
+ [How transit gateways work](how-transit-gateways-work.md)
+ [Getting started](tgw-getting-started.md)
+ [Design best practices](tgw-best-design-practices.md)

## Work with transit gateways<a name="tgw-interfaces"></a>

You can create, access, and manage your transit gateways using any of the following interfaces:
+ **AWS Management Console** — Provides a web interface that you can use to access your transit gateways\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Amazon VPC, and is supported on Windows, macOS, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS SDKs** — Provides language\-specific API operations and takes care of many of the connection details, such as calculating signatures, handling request retries, and handling errors\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.
+ **Query API** — Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Amazon VPC, but it requires that your application handle low\-level details such as generating the hash to sign the request, and handling errors\. For more information, see the [Amazon EC2 API Reference](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/)\.

## Pricing<a name="pricing"></a>

You are charged hourly for each attachment on a transit gateway, and you are charged for the amount of traffic processed on the transit gateway\. For more information, see [AWS Transit Gateway pricing](https://aws.amazon.com/transit-gateway)\.