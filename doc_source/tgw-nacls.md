# How Network ACLs work with transit gateways<a name="tgw-nacls"></a>

A network access control list \(NACL\) is an optional layer of security\. 

Network access control list \(NACL\) rules are applied differently, depending on the scenario: 
+ [Same subnet for EC2 instances and transit gateway association](#nacl-tgw-same-subnet)
+ [Different subnets for EC2 instances and transit gateway association](#nacl-tgw-different-subnet)

## Same subnet for EC2 instances and transit gateway association<a name="nacl-tgw-same-subnet"></a>

Consider a configuration where you have an EC2 instances and a transit gateway association in the same subnet\. The same network ACL is used for both the traffic from the EC2 instances to the transit gateway and traffic from the transit gateway to the instances\.

NACL rules are applied as follows for traffic from instances to the transit gateway:
+ Outbound rules use the destination IP address for evaluation\.
+ Inbound rules use the source IP address for evaluation\.

NACL rules are applied as follows for traffic from the transit gateway to the instances:
+ Outbound rules are not evaluated\.
+ Inbound rules are not evaluated\.

## Different subnets for EC2 instances and transit gateway association<a name="nacl-tgw-different-subnet"></a>

Consider a configuration where you have EC2 instances in one subnet and the transit gateway association in a different subnet, and each subnet is associated with a different network ACL\.

Network ACL rules are applied as follows for the EC2 instance subnet:
+ Outbound rules use the destination IP address to evaluate traffic from the instances to the transit gateway\.
+ Inbound rules use the source IP address to evaluate traffic from the transit gateway to the instances\.

NACL rules are applied as follows for the transit gateway subnet:
+ Outbound rules use the destination IP address to evaluate traffic from the transit gateway to the instances\.
+ Outbound rules are not used to evaluate traffic from the instances to the transit gateway\.
+ Inbound rules use the source IP address to evaluate traffic from the instances to the transit gateway\.
+ Inbound rules are not used to evaluate traffic from the transit gateway to the instances\.

## Best Practices<a name="nacl-best-practices"></a>

Use a separate subnet for each transit gateway VPC attachment\. For each subnet, use a small CIDR, for example /28, so that you have more addresses for EC2 resources\. When you use a separate subnet, you can configure the following:
+ Keep the inbound and outbound NACL that is associated with the transit gateway subnets open\. 
+ Depending on your traffic flow, you can apply NACLs to your workload subnets\.

For more information about how VPC attachments work, see [Resource attachments](how-transit-gateways-work.md#tgw-attachments-overview)\. 