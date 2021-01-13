# Managing multicast groups<a name="manage-multicast-group"></a>

**Topics**
+ [Registering sources with a multicast group](#add-source-multicast-group)
+ [Registering members with a multicast group](#add-members-multicast-group)
+ [Deregistering sources from a multicast group](#remove-source-multicast-group)
+ [Deregistering members from a multicast group](#remove-members-multicast-group)
+ [Viewing your multicast groups](#view-multicast-group)

## Registering sources with a multicast group<a name="add-source-multicast-group"></a>

**Note**  
This procedure is only required when you have set the **Static sources support** attribute to **enable**\.

Use the following procedure to register sources with a multicast group\. The source is the network interface that sends multicast traffic\.

You need the following information before you add a source:
+ The ID of the multicast domain
+ The IDs of the sources' network interfaces
+ The multicast group IP address

------
#### [ Console ]

**To register sources using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain, and then choose **Actions**, **Add group sources**\.

1. For **Group IP address**, enter either the IPv4 CIDR block or IPv6 CIDR block to assign to the multicast domain\.

1. Under **Choose network interfaces**, select the multicast senders' network interfaces\.

1. Choose **Add sources**\.

------
#### [ Command line ]

**To register sources using the AWS CLI**
+ Use the [register\-transit\-gateway\-multicast\-group\-sources](https://docs.aws.amazon.com/cli/latest/reference/ec2/register-transit-gateway-multicast-group-sources.html) command\.

------

## Registering members with a multicast group<a name="add-members-multicast-group"></a>

Use the following procedure to register group members with a multicast group\. 

You need the following information before you add members:
+ The ID of the multicast domain
+ The IDs of the group members' network interfaces
+ The multicast group IP address

------
#### [ Console ]

**To register members using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain, and then choose **Actions**, **Add group members**\.

1. For **Group IP address**, enter either the IPv4 CIDR block or IPv6 CIDR block to assign to the multicast domain\.

1. Under **Choose network interfaces**, select the multicast receivers' network interfaces\.

1. Choose **Add members**\.

------
#### [ Command line ]

**To register members using the AWS CLI**
+ Use the [register\-transit\-gateway\-multicast\-group\-sources](https://docs.aws.amazon.com/cli/latest/reference/ec2/register-transit-gateway-multicast-group-sources.html) command\.

------

## Deregistering sources from a multicast group<a name="remove-source-multicast-group"></a>

You don't need to follow this procedure unless you manually added a source to the multicast group\.

------
#### [ Console ]

**To remove a source using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose the **Groups** tab\.

1. Select the sources, and then choose **Remove source**\.

------
#### [ Command line ]

**To remove a source using the AWS CLI**
+ Use the [deregister\-transit\-gateway\-multicast\-group\-sources](https://docs.aws.amazon.com/cli/latest/reference/ec2/deregister-transit-gateway-multicast-group-sources.html) command\.

------

## Deregistering members from a multicast group<a name="remove-members-multicast-group"></a>

You don't need to follow this procedure unless you manually added a member to the multicast group\.

------
#### [ Console ]

**To deregister members using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose the **Groups** tab\.

1. Select the members, and then choose **Remove member**\.

------
#### [ Command line ]

**To deregister members using the AWS CLI**
+ Use the [deregister\-transit\-gateway\-multicast\-group\-members](https://docs.aws.amazon.com/cli/latest/reference/ec2/deregister-transit-gateway-multicast-group-members.html) command\.

------

## Viewing your multicast groups<a name="view-multicast-group"></a>

You can view information about your multicast groups to verify that members were discovered using the IGMPv2 protocol\. **Member type** \(in the console\), or `MemberType` \(in the AWS CLI\) displays IGMP when AWS discovered members with the protocol\.

------
#### [ Console ]

**To view multicast groups using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit Gateway Multicast**\.

1. Select the multicast domain\.

1. Choose the **Groups** tab\.

------
#### [ Command line ]

**To view multicast groups using the AWS CLI**
+ Use the [search\-transit\-gateway\-multicast\-groups](https://docs.aws.amazon.com/cli/latest/reference/ec2/search-transit-gateway-multicast-groups.html) command\.

  The following example shows that the IGMP protocol discovered multicast group members\.

  ```
  aws ec2 search-transit-gateway-multicast-groups --transit-gateway-multicast-domain tgw-mcast-domain-000fb24d04EXAMPLE
  {
      "MulticastGroups": [
          {
              "GroupIpAddress": "224.0.1.0",
              "TransitGatewayAttachmentId": "tgw-attach-0372e72386EXAMPLE",
              "SubnetId": "subnet-0187aff814EXAMPLE",
              "ResourceId": "vpc-0065acced4EXAMPLE",
              "ResourceType": "vpc",
              "NetworkInterfaceId": "eni-03847706f6EXAMPLE",
              "MemberType": "igmp"
          }
      ]
  }
  ```

------