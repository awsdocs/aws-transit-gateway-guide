# Multicast routing<a name="how-multicast-works"></a>

When you enable multicast on a transit gateway, it acts as a multicast router\. We send all multicast traffic to the transit gateway that is associated with a multicast domain, when you add a subnet to that multicast domain\.

## Network ACLs<a name="multicast-nacl"></a>

Network ACL rules operate at the subnet level and apply to multicast traffic, because transit gateways reside outside of the subnet\. For information about network ACLs, see [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) in the * Amazon VPC User Guide*\.

You must have the following inbound rules at a minimum for IGMP multicast traffic\.


|  |  |  |  |  | 
| --- |--- |--- |--- |--- |
|  Type  | Protocol  | Source | Destination |  Description  | 
|  Custom Protocol  | IGMP\(2\) | 0\.0\.0\.0/32 | 224\.0\.0\.1/32 |  IGMP query   | 
| Custom UDP Protocol | UDP | The remote host IP addressThis is the IP address of the host that sends the multicast traffic\. | Multicast group IP address | Inbound multicast traffic | 

You must have the following outbound rules at a minimum for IGMP multicast traffic\.


|  |  |  |  |  | 
| --- |--- |--- |--- |--- |
|  Type  | Protocol  | Source | Destination |  Description  | 
|  Custom Protocol  | IGMP\(2\) | Host IP address | 224\.0\.0\.2/32 |  IGMP leave  | 
| Custom Protocol | IGMP\(2\) | Host IP address | Multicast group IP address | IGMP join | 
| Custom UDP Protocol | UDP | Host IP address | Multicast group IP address | Outbound multicast traffic | 

## Security groups<a name="mulicast-security-group"></a>

Security group rules operate at the instance level and can be applied to both inbound and outbound multicast traffic\. The behavior is the same as with unicast traffic\. For all group member instances, you must allow inbound traffic from the group source\. For information about security groups, see [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

You must have the following inbound rules at a minimum for IGMP multicast traffic\.


|  |  |  |  | 
| --- |--- |--- |--- |
|  Type  | Protocol  | Source |  Description  | 
|  Custom Protocol  | 2 | Custom/0\.0\.0\.0/32 |  IGMP query   | 
| Custom UDP Protocol | UDP | Custom/Remote host IP addressThis is the IP address of the host that sends the multicast traffic\. | Inbound multicast traffic | 

You must have the following outbound rules at a minimum for IGMP multicast traffic\.


|  |  |  |  | 
| --- |--- |--- |--- |
|  Type  | Protocol  | Destination |  Description  | 
|  Custom Protocol  | 2 | Custom/224\.0\.0\.2/32 |  IGMP leave  | 
| Custom Protocol | 2 | Custom/Multicast group IP address | IGMP join | 
| CustomUDP Protocol | UDP | Custom/Multicast group IP address | Outbound multicast traffic | 