# Multicast on transit gateways<a name="tgw-multicast-overview"></a>

Multicast is a communication protocol used for delivering a single stream of data to multiple receiving computers simultaneously\. Transit Gateway supports routing multicast traffic between subnets of attached VPCs, and it serves as a multicast router for instances sending traffic destined for multiple receiving instances\. 

## Multicast concepts<a name="concepts"></a>

The following are the key concepts for multicast:
+ **Multicast domain** — Allows segmentation of a multicast network into different domains, and makes the transit gateway act as multiple multicast routers\. You define multicast domain membership at the subnet level\. 
+ **Multicast group** — Identifies a set of hosts that will send and receive the same multicast traffic\. A multicast group is identified by a group IP address\. Multicast group membership is defined by individual elastic network interfaces attached to EC2 instances\.
+ **Internet Group Management Protocol \(IGMP\)** — An internet protocol that allows hosts and routers to dynamically manage multicast group membership\. An IGMP multicast domain contains hosts that use the IGMP protocol to join, leave, and send messages\. AWS supports the IGMPv2 protocol and both IGMP and static \(API\-based\) group membership multicast domains\.
+ **Multicast source** — An elastic network interface associated with a supported EC2 instance that is statically configured to send multicast traffic\. A multicast source only applies to static source configurations\. 

  A static source multicast domain contains hosts that do not use the IGMP protocol to join, leave, and send messages\. You use the AWS CLI to add a source and group members\. The statically\-added source sends multicast traffic and the members receive multicast traffic\.
+ **Multicast group member** — An elastic network interface associated with a supported EC2 instance that receives multicast traffic\. A multicast group has multiple group members\. In a static source group membership configuration, multicast group members can only receive traffic\. In an IGMP group configuration, members can both send and receive traffic\. 

## Considerations<a name="limits"></a>
+ For information about supported Regions, see [AWS Transit Gateway FAQs](https://aws.amazon.com/transit-gateway/faqs)\.
+ You must create a new transit gateway to support multicast\.
+ Multicast group membership is managed using the Amazon VPC Console or the AWS CLI, or IGMP\. 
+ A subnet can only be in one multicast domain\. 
+ If you use a non\-Nitro instance, you must disable the **Source/Dest** check\. For information about disabling the check, see [Changing the source or destination checking](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#change_source_dest_check) in the *Amazon EC2 User Guide for Linux Instances*\.
+ A non\-Nitro instance cannot be a multicast sender\.
+ Multicast routing is not supported over AWS Direct Connect, Site\-to\-Site VPN, or peering attachments\.
+ A transit gateway does not support fragmentation of multicast packets\. Fragmented multicast packets will get dropped\. For more information, see [MTU](transit-gateway-quotas.md#mtu-quota)\.
+ At startup, an IGMP host sends multiple IGMP `JOIN` messages to join a multicast group \(typically 2 to 3 retries\)\. In the unlikely event that all the IGMP `JOIN` messages get lost, the host will not become part of transit gateway multicast group\. In such a scenario you will need to re\-trigger the IGMP `JOIN` message from the host using application specific methods\.
+ The transit gateway keeps track of hosts that successfully joined the group\. In the event of a transit gateway outage, the transit gateway continues to send multicast data to the host for 7 minutes \(420 seconds\) after the last successful IGMP `JOIN` message\. The transit gateway continues to send membership queries to the host for up to 12 hours or until it receives a IGMP `LEAVE` message from the host\.
+ The transit gateway sends membership query packets to all the IGMP members so that it can track multicast group membership\. The source IP of these IGMP query packets is 0\.0\.0\.0/32, and the destination IP is 224\.0\.0\.1/32 and the protocol is 2\. Your security group configuration on the IGMP hosts \(instances\), and any ACLs configuration on the host subnets must allow these IGMP protocol messages\. 