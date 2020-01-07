# Multicast Routing<a name="how-multicast-works"></a>

Learn how route tables, Network ACLs, and security groups handle multicast traffic\.

## Route Tables<a name="multicast-route-tables-overview"></a>

Route tables are not used to handle multicast traffic\. Instead, we send all multicast traffic to the transit gateway that is associated with a multicast domain, when you add a subnet to that multicast domain\.

## Network ACLs<a name="multicast-nacl"></a>

Network ACL rules operate at the subnet level and apply to multicast traffic, because transit gateways reside outside of the subnet\. For information about Network ACLs, see [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) in the * Amazon VPC User Guide*\.

To control multicast traffic, you can create allow and deny rules\. For example, to allow outbound multicast traffic create the following outbound rule using the console or CLI:
+ Rule number \- A rule number, for example 100\.
+ CIDR block \- The CIDR block of the multicast group, for example 224\.0\.0\.0/24\.
+ Protocol \- The protocol that your multicast applications use\.
+ Action \- Set this to Allow\.
+ Description \- A description for the rule, for example, "Allow all outbound multicast traffic"\.

## Security Groups<a name="mulicast-security-group"></a>

Security group rules operate at the instance level\. Inbound security groups apply to multicast traffic\. The behavior is the same as unicast traffic\. For all group member instances, you must allow inbound traffic from the group source\. For information about security groups, see [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

You can use outbound security group rules to control multicast traffic\. 

To control multicast traffic, you can create security groups with outbound rules which allow multicast traffic\. Use the following values for the parameters\.
+ Port range \- The IP addresses that are involved in multicast traffic\.
+ Protocol \- The protocol that your multicast applications use\.
+ Description \- A description for the rule, for example, "Allow all outbound multicast traffic"\.