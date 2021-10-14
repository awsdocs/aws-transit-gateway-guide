# Multicast routing<a name="how-multicast-works"></a>

When you enable multicast on a transit gateway, it acts as a multicast router\. When you add a subnet to a multicast domain, we send all multicast traffic to the transit gateway that is associated with that multicast domain\.

## Network ACLs<a name="multicast-nacl"></a>

Network ACL rules operate at the subnet level\. They apply to multicast traffic, because transit gateways reside outside of the subnet\. For more information, see [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) in the * Amazon VPC User Guide*\.

For Internet Group Management Protocol \(IGMP\) multicast traffic, you must have the following inbound rules at a minimum\. The remote host is the host sending the multicast traffic\.


| Type | Protocol | Source | Destination | Description | 
| --- | --- | --- | --- | --- | 
| Custom Protocol | IGMP\(2\) | 0\.0\.0\.0/32 | 224\.0\.0\.1/32 | IGMP query  | 
| Custom UDP Protocol | UDP | Remote host IP address | Multicast group IP address | Inbound multicast traffic | 

For IGMP multicast traffic, you must have the following outbound rules at a minimum\.


| Type | Protocol | Source | Destination | Description | 
| --- | --- | --- | --- | --- | 
| Custom Protocol | IGMP\(2\) | Host IP address | 224\.0\.0\.2/32 | IGMP leave | 
| Custom Protocol | IGMP\(2\) | Host IP address | Multicast group IP address | IGMP join | 
| Custom UDP Protocol | UDP | Host IP address | Multicast group IP address | Outbound multicast traffic | 

## Security groups<a name="mulicast-security-group"></a>

Security group rules operate at the instance level\. They can be applied to both inbound and outbound multicast traffic\. The behavior is the same as with unicast traffic\. For all group member instances, you must allow inbound traffic from the group source\. For more information, see [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

For IGMP multicast traffic, you must have the following inbound rules at a minimum\. The remote host is the host sending the multicast traffic\. You can't specify a security group as the source of the UDP inbound rule\.


| Type | Protocol | Source | Description | 
| --- | --- | --- | --- | 
| Custom Protocol | 2 | 0\.0\.0\.0/32 | IGMP query  | 
| Custom UDP Protocol | UDP | Remote host IP address | Inbound multicast traffic | 

For IGMP multicast traffic, you must have the following outbound rules at a minimum\.


| Type | Protocol | Destination | Description | 
| --- | --- | --- | --- | 
| Custom Protocol | 2 | 224\.0\.0\.2/32 | IGMP leave | 
| Custom Protocol | 2 | Multicast group IP address | IGMP join | 
| Custom UDP Protocol | UDP | Multicast group IP address | Outbound multicast traffic | 