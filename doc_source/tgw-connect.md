# Transit gateway Connect attachments and Transit Gateway Connect peers<a name="tgw-connect"></a>

You can create a *transit gateway Connect attachment* to establish a connection between a transit gateway and third\-party virtual appliances \(such as SD\-WAN appliances\) running in a VPC\. A Connect attachment supports the Generic Routing Encapsulation \(GRE\) tunnel protocol for high performance, and Border Gateway Protocol \(BGP\) for dynamic routing\. After you create a Connect attachment, you can create one or more GRE tunnels \(also referred to as *Transit Gateway Connect peers*\) on the Connect attachment to connect the transit gateway and the third\-party appliance\. You establish two BGP sessions over the GRE tunnel to exchange routing information\. The two BGP sessions are for redundancy\.

A Connect attachment uses an existing VPC or AWS Direct Connect attachment as the underlying transport mechanism\. This is referred to as the *transport attachment*\. The transit gateway identifies matched GRE packets from the third\-party appliance as traffic from the Connect attachment\. It treats any other packets, including GRE packets with incorrect source or destination information, as traffic from the transport attachment\. 

**Topics**
+ [Transit Gateway Connect peers](#tgw-connect-peer)
+ [Requirements and considerations](#tgw-connect-requirements)
+ [Create a transit gateway Connect attachment](#create-tgw-connect-attachment)
+ [Create a Transit Gateway Connect peer \(GRE tunnel\)](#create-tgw-connect-peer)
+ [View your transit gateway Connect attachments and Transit Gateway Connect peers](#view-tgw-connect-attachments)
+ [Modify your Connect attachment and Transit Gateway Connect peer tags](#modify-connect-attachment-tag)
+ [Delete a Transit Gateway Connect peer](#delete-tgw-connect-peer)
+ [Delete a transit gateway Connect attachment](#delete-tgw-connect-attachment)

## Transit Gateway Connect peers<a name="tgw-connect-peer"></a>

A Transit Gateway Connect peer \(GRE tunnel\) consists of the following components\.

**Inside CIDR blocks \(BGP addresses\)**  
The inside IP addresses that are used for BGP peering\. You must specify a /29 CIDR block from the `169.254.0.0/16` range for IPv4\. You can optionally specify a /125 CIDR block from the `fd00::/8` range for IPv6\. The following CIDR blocks are reserved and cannot be used:  
+ 169\.254\.0\.0/29
+ 169\.254\.1\.0/29
+ 169\.254\.2\.0/29
+ 169\.254\.3\.0/29
+ 169\.254\.4\.0/29
+ 169\.254\.5\.0/29
+ 169\.254\.169\.248/29
You must configure the first address from the IPv4 range on the appliance as the BGP IP address\. When you use IPv6, if your inside CIDR block is fd00::/125, then you must configure the first address in this range \(fd00::1\) on the tunnel interface of the appliance\.   
The BGP addresses must be unique across all tunnels on a transit gateway\.

**Peer IP address**  
The peer IP address \(GRE outer IP address\) on the appliance side of the Transit Gateway Connect peer\. This can be any IP address\. The IP address can be an IPv4 or IPv6 address, but it must be the same IP address family as the transit gateway address\.

**Transit gateway address**  
The peer IP address \(GRE outer IP address\) on the transit gateway side of the Transit Gateway Connect peer\. The IP address must be specified from the transit gateway CIDR block, and must be unique across Connect attachments on the transit gateway\. If you don't specify an IP address, we use the first available address from the transit gateway CIDR block\.   
You can add a transit gateway CIDR block when you [create](tgw-transit-gateways.md#create-tgw) or [modify](tgw-transit-gateways.md#tgw-modifying) a transit gateway\.  
The IP address can be an IPv4 or IPv6 address, but it must be the same IP address family as the peer IP address\.

The peer IP address and transit gateway address are used to uniquely identify the GRE tunnel\. You can reuse either address across multiple tunnels, but not both in the same tunnel\.

You can use different IP address families for the BGP addresses and the GRE outer IP addresses\. For example, you can configure IPv4 addresses for the GRE outer IP addresses, and an IPv6 CIDR block for the BGP addresses\.

The following example shows a Connect attachment between a transit gateway and an appliance in a VPC\.

![\[Transit gateway Connect attachment and Transit Gateway Connect peer\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/transit-gateway-connect-peer.png)


| Diagram component | Description | 
| --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/VPC-attachment.png)  | VPC attachment | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/connect-attachment.png)  | Connect attachment | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/GRE-tunnel.png)  | GRE tunnel \(Transit Gateway Connect peer\) | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/bgp-peering.png)  | BGP peering session | 

In the preceding example, a transit gateway Connect attachment is created on an existing VPC attachment \(the transport attachment\)\. A Transit Gateway Connect peer is created on the Connect attachment to establish a connection to an appliance in the VPC\. The transit gateway address is `192.0.2.1`, and the range of BGP addresses is `169.254.6.0/29`\. The first IP address in the range \(`169.254.6.1`\) is configured on the appliance as the peer BGP IP address\.

The subnet route table for VPC C has a route that points traffic destined for the transit gateway CIDR block to the transit gateway\.


| Destination | Target | 
| --- | --- | 
| 172\.31\.0\.0/16 | Local | 
| 192\.0\.2\.0/24 | tgw\-id | 

## Requirements and considerations<a name="tgw-connect-requirements"></a>

The following are the requirements and considerations for a Connect attachment\.
+ For information about what Regions support Connect attachments, see [AWS Transit Gateways FAQs](http://aws.amazon.com/transit-gateway/faqs/)\.
+ The third\-party appliance must be configured to send and receive traffic over a GRE tunnel to and from the transit gateway using the Connect attachment\.
+ The third\-party appliance must be configured to use BGP for dynamic route updates and health checks\.
+ The following types of BGP are supported:
  + Exterior BGP \(eBGP\): Used for connecting to routers that are in a different autonomous system than the transit gateway\. If you use eBGP, you must configure ebgp\-multihop with a time\-to\-live \(TTL\) value of 2\.
  + Interior BGP \(iBGP\): Used for connecting to routers that are in the same autonomous system as the transit gateway\. The transit gateway will not install routes from an iBGP peer \(third\-party appliance\), unless the routes are originated from an eBGP peer\. The routes advertised by third\-party appliance over the iBGP peering must have an ASN\.
  + MP\-BGP \(multiprotocol extensions for BGP\): Used for supporting multiple protocol types, such as IPv4 and IPv6 address families\.
+ The default BGP keep\-alive timeout is 30 seconds and the default hold timer is 90 seconds\.
+ IPv6 BGP peering is not supported; only IPv4\-based BGP peering is supported\. IPv6 prefixes are exchanged over IPv4 BGP peering using MP\-BGP\.
+ Bidirectional Forwarding Detection \(BFD\) is not supported\.
+ BGP graceful restart is supported\.
+ When you create a transit gateway peer, if you do not specify a peer ASN number, we pick the transit gateway ASN number\. This means that your appliance and transit gateway will be in the same autonomous system doing iBGP\.
+ To use equal\-cost multi\-path \(ECMP\) routing between multiple appliances, you must configure the appliance to advertise the same prefixes to the transit gateway with the same BGP AS\-PATH attribute\. For the transit gateway to choose all of the available ECMP paths, the AS\-PATH and Autonomous System Number \(ASN\) must match\. The transit gateway can use ECMP between Transit Gateway Connect peers for the same Connect attachment or between Connect attachments on the same transit gateway\. The transit gateway cannot use ECMP between the BGP peerings of the same Transit Gateway Connect peer\.
+ With a Connect attachment, the routes are propagated to a transit gateway route table by default\.
+ Static routes are not supported\.

## Create a transit gateway Connect attachment<a name="create-tgw-connect-attachment"></a>

To create a Connect attachment, you must specify an existing attachment as the transport attachment\. You can specify a VPC attachment or an AWS Direct Connect attachment as the transport attachment\.

**To create a Connect attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Choose **Create transit gateway attachment**\.

1. \(Optional\) For **Name tag**, specify a name tag for the attachment\.

1. For **Transit gateway ID**, choose the transit gateway for the attachment\.

1. For **Attachment type**, choose **Connect**\.

1. For **Transport attachment ID**, choose the ID of an existing attachment \(the transport attachment\)\.

1. Choose **Create transit gateway attachment**\.

**To create a Connect attachment using the AWS CLI**  
Use the [create\-transit\-gateway\-connect](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-connect.html) command\.

## Create a Transit Gateway Connect peer \(GRE tunnel\)<a name="create-tgw-connect-peer"></a>

You can create a Transit Gateway Connect peer \(GRE tunnel\) for an existing Connect attachment\. Before you begin, ensure that you have configured a transit gateway CIDR block\. You can configure a transit gateway CIDR block when you [create](tgw-transit-gateways.md#create-tgw) or [modify](tgw-transit-gateways.md#tgw-modifying) a transit gateway\. 

When you create the Transit Gateway Connect peer, you must specify the GRE outer IP address on the appliance side of the Transit Gateway Connect peer\.

**To create a Transit Gateway Connect peer using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment, and choose **Actions**, **Create connect peer**\.

1. \(Optional\) For **Name tag**, specify a name tag for the Transit Gateway Connect peer\.

1. \(Optional\) For **Transit gateway GRE Address**, specify the GRE outer IP address for the transit gateway\. By default, the first available address from the transit gateway CIDR block is used\.

1. For **Peer GRE address**, specify the GRE outer IP address for the appliance side of the Transit Gateway Connect peer\.

1. For **BGP Inside CIDR blocks IPv4**, specify the range of inside IPv4 addresses that are used for BGP peering\. Specify a /29 CIDR block from the `169.254.0.0/16` range\.

1. \(Optional\) For **BGP Inside CIDR blocks IPv6**, specify the range of inside IPv6 addresses that are used for BGP peering\. Specify a /125 CIDR block from the `fd00::/8` range\.

1. \(Optional\) For **Peer ASN**, specify the Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) for the appliance\. You can use an existing ASN assigned to your network\. If you do not have one, you can use a private ASN in the 64512–65534 range\. 

   The default is the same ASN as the transit gateway\. If you configure the **Peer ASN** to be different than the transit gateway ASN \(eBGP\), you must configure ebgp\-multihop with a time\-to\-live \(TTL\) value of 2\.

1. Choose **Create connect peer**\.

**To create a Transit Gateway Connect peer using the AWS CLI**  
Use the [create\-transit\-gateway\-connect\-peer](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-connect-peer.html) command\.

## View your transit gateway Connect attachments and Transit Gateway Connect peers<a name="view-tgw-connect-attachments"></a>

You can view your transit gateway Connect attachments and Transit Gateway Connect peers\.

**To view your Connect attachments and Transit Gateway Connect peers using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment\.

1. To view the Transit Gateway Connect peers for the attachment, choose the **Connect Peers** tab\.

**To view your Connect attachments and Transit Gateway Connect peers using the AWS CLI**  
Use the [describe\-transit\-gateway\-connects](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-connects.html) and [describe\-transit\-gateway\-connect\-peers](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-transit-gateway-connect-peers.html) commands\.

## Modify your Connect attachment and Transit Gateway Connect peer tags<a name="modify-connect-attachment-tag"></a>

You can modify the tags for your Connect attachment\.

**To modify your Connect attachment tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment, and then choose **Actions**, **Manage tags**\.

1. To add a tag, choose **Add new tag** and specify the key name and key value\.

1. To remove a tag, choose **Remove**\.

1. Choose **Save**\. 

You can modify the tags for your Transit Gateway Connect peer\.

**To modify your Transit Gateway Connect peer tags using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment, and then choose **Connect peers**\.

1. Select the Transit Gateway Connect peer and then choose **Actions**, **Manage tags**\.

1. To add a tag, choose **Add new tag** and specify the key name and key value\.

1. To remove a tag, choose **Remove**\.

1. Choose **Save**\. 

**To modify your Connect attachment and Transit Gateway Connect peer tags using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) and [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-tags.html) commands\.

## Delete a Transit Gateway Connect peer<a name="delete-tgw-connect-peer"></a>

If you no longer need a Transit Gateway Connect peer, you can delete it\.

**To delete a Transit Gateway Connect peer using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment\.

1. In the **Connect Peers** tab, select the Transit Gateway Connect peer and choose **Actions**, **Delete connect peer**\.

**To delete a Transit Gateway Connect peer using the AWS CLI**  
Use the [delete\-transit\-gateway\-connect\-peer](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-connect-peer.html) command\.

## Delete a transit gateway Connect attachment<a name="delete-tgw-connect-attachment"></a>

If you no longer need a transit gateway Connect attachment, you can delete it\. You must first delete any Transit Gateway Connect peers for the attachment\.

**To delete a Connect attachment using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit Gateway Attachments**\.

1. Select the Connect attachment, and choose **Actions**, **Delete transit gateway attachment**\.

1. Enter **delete** and choose **Delete**\.

**To delete a Connect attachment using the AWS CLI**  
Use the [delete\-transit\-gateway\-connect](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-connect.html) command\.