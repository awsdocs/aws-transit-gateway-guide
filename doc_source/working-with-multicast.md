# Working with multicast<a name="working-with-multicast"></a>

You can configure multicast on transit gateways using the Amazon VPC console or the AWS CLI\.

Before you create a multicast domain, you need to know if your hosts use the Internet Group Management Protocol \(IGMP\) protocol for multicast traffic\.

The following table details the multicast domain attributes\.


| Attribute | Description | 
| --- | --- | 
| Igmpv2Support \(AWS CLI\) **IGMPv2 support** \(Amazon Virtual Private Cloud Console\) |  This attribute determines how group members join or leave a multicast group\. When this attribute is set to **disable**, you must add the group members to the domain using the Amazon VPC console or the AWS CLI\. Set this value to **enable** when at least one member uses the IGMP protocol\. Members join the multicast group in one of the following ways: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/tgw/working-with-multicast.html)  If you use the Amazon VPC console or the AWS CLI to manually register multicast group members, you must deregister them\. The transit gateway ignores an IGMP `LEAVE` message sent by a manually added group member\.   | 
| StaticSourcesSupport \(AWS CLI\) **Static sources support** \(Amazon Virtual Private Cloud Console\) |  This attribute determines whether there are static multicast sources for the group\.  When this attribute is set to **enable**, you need to statically add sources for a multicast domain using [register\-transit\-gateway\-multicast\-group\-sources ](https://docs.aws.amazon.com/cli/latest/reference/ec2/register-transit-gateway-multicast-group-sources.html)\. Only multicast sources can send multicast traffic\.  When you set the `Igmpv2Support` attribute to **enable**, you cannot set `staticSourcesEnable` to **enable**\.  When this attribute is set to **disable**, there are no designated multicast sources\. Any instances that are in subnets associated with the multicast domain can send multicast traffic, and the group members receive the multicast traffic\.  | 

**Topics**
+ [Managing IGMP configurations](#multicast-configurations-igmp)
+ [Managing static source configurations](#multicast-configurations-no-igmp)
+ [Managing static group member configurations](#multicast-configurations-no-igmp-source)
+ [Managing multicast domains](manage-domain.md)
+ [Managing multicast groups](manage-multicast-group.md)
+ [Working with shared multicast domains](multicast-sharing.md)

## Managing IGMP configurations<a name="multicast-configurations-igmp"></a>

When you have at least one host that uses the IGMP protocol for multicast traffic, AWS automatically creates the multicast group when it receives an IGMP `JOIN` message from an instance, and then adds the instance as a member in this group\. You can also statically add non\-IGMP hosts as members to a group using the AWS CLI\. Any instances that are in subnets associated with the multicast domain can send traffic, and the group members receive the multicast traffic\.

 Use the following steps to complete the configuration:

1. Create a VPC\. For more information about creating VPCs, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.

1. Create a subnet in the VPC\. For more information about creating subnets, see [Creating a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\.

1. Create a transit gateway configured for multicast traffic\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.

1. Create a VPC attachment\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

1. Create a multicast domain configured for IGMP support\. For more information, see [Creating an IGMP multicast domain](manage-domain.md#create-tgw-igmp-domain)\. 

   Use the following settings:
   + Set the **IGMPv2 support** attribute to **enable**\.
   + Set the **Static sources support** attribute to **disable**\.

1. Create an association between subnets in the transit gateway VPC attachment and the multicast domain\. For more information see [Associating VPC attachments and subnets with a multicast domain](manage-domain.md#associate-attachment-to-domain)\. 

1. The default IGMP version for EC2 is IGMPv3\. You need to change the version for all IGMP group members\. You can run the following command:

   ```
   sudo sysctl net.ipv4.conf.eth0.force_igmp_version=2
   ```

1. Add the members that do not use the IGMP protocol to the multicast group\. For more information, see [Registering members with a multicast group](manage-multicast-group.md#add-members-multicast-group)\.

## Managing static source configurations<a name="multicast-configurations-no-igmp"></a>

In this configuration, you need to statically add multicast sources in a group\. Hosts do not use the IGMP protocol to join or leave multicast groups\. You need to statically add the group members that receive the multicast traffic\.

 Use the following steps to complete the configuration:

1. Create a VPC\. For more information about creating VPCs, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.

1. Create a subnet in the VPC\. For more information about creating subnets, see [Creating a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\.

1. Create a transit gateway configured for multicast traffic\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.

1. Create a VPC attachment\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

1. Create a multicast domain configured for no IGMP support, and support for statically adding sources\. For more information, see [Creating a static source multicast domain](manage-domain.md#create-tgw-domain)\. 

   Use the following settings:
   + Set the **IGMPv2 support** attribute to **disable**\.
   + To manually add sources, set the **Static sources support** attribute to **enable**\.

     The sources are the only resources that can send multicast traffic when the attribute is set to **enable**\. Otherwise, any instances that are in subnets associated with the multicast domain can send multicast traffic, and the group members receive the multicast traffic\.

1. Create an association between subnets in the transit gateway VPC attachment and the multicast domain\. For more information see [Associating VPC attachments and subnets with a multicast domain](manage-domain.md#associate-attachment-to-domain)\.

1. If you set the **Static sources support** attribute to **enable**, add the source to the multicast group\. For more information, see [Registering sources with a multicast group](manage-multicast-group.md#add-source-multicast-group)\.

1. Add the members to the multicast group\. For more information, see [Registering members with a multicast group](manage-multicast-group.md#add-members-multicast-group)\.

## Managing static group member configurations<a name="multicast-configurations-no-igmp-source"></a>

In this configuration, you need to statically add multicast members to a group\. Hosts cannot use the IGMP protocol to join or leave multicast groups\. Any instances that are in subnets associated with the multicast domain can send multicast traffic, and the group members receive the multicast traffic\.

 Use the following steps to complete the configuration:

1. Create a VPC\. For more information about creating VPCs, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide*\.

1. Create a subnet in the VPC\. For more information about creating subnets, see [Creating a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\.

1. Create a transit gateway configured for multicast traffic\. For more information, see [Create a transit gateway](tgw-transit-gateways.md#create-tgw)\.

1. Create a VPC attachment\. For more information, see [Create a transit gateway attachment to a VPC](tgw-vpc-attachments.md#create-vpc-attachment)\.

1. Create a multicast domain configured for no IGMP support, and support for statically adding sources\. For more information, see [Creating a static source multicast domain](manage-domain.md#create-tgw-domain)\. 

   Use the following settings:
   + Set the **IGMPv2 support** attribute to **disable**\.
   + Set the **Static sources support** attribute to **disable**\.

1. Create an association between subnets in the transit gateway VPC attachment and the multicast domain\. For more information see [Associating VPC attachments and subnets with a multicast domain](manage-domain.md#associate-attachment-to-domain)\.

1. Add the members to the multicast group\. For more information, see [Registering members with a multicast group](manage-multicast-group.md#add-members-multicast-group)\.