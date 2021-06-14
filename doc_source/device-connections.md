# Connections<a name="device-connections"></a>

You can create a connection between two devices in your global network\. The connection can be between a physical or virtual appliance and a third\-party appliance in a VPC, or between physical appliances in an on\-premises network\.

A connection is created for a specific global network and cannot be shared with other global networks\. 

**Topics**
+ [Creating a connection](#creating-a-connection)
+ [Updating a connection](#updating-a-connection)
+ [Deleting a connection](#deleting-a-connection)

## Creating a connection<a name="creating-a-connection"></a>

Create a connection between two existing devices in your global network\.

**To create a connection**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of the device\.

1. Choose **Connections**, and then choose **Create connection**\.

1. For **Name** and **Description**, enter a name and description for the connection\.

1. \(Optional\) For **Link**, choose a link to associate with the first device in the connection\.

1. For **Connected device**, choose the ID of the second device in the connection\.

1. \(Optional\) For **Connected link**, choose a link to associate with the second device in the connection\.

1. Choose **Create connection**\.

**To create a connection using the AWS CLI**  
Use the [create\-connection](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/create-connection.html) command\.

## Updating a connection<a name="updating-a-connection"></a>

You can update the information for an existing connection\.

**To update a connection**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and select the device\.

1. Choose **Connections**, and select the connection\.

1. Choose **Edit**\.

1. Update the connection details as needed, and then choose **Edit connection**\.

**To update a connection using the AWS CLI**  
Use the [update\-connection](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/update-connection.html) command\.

## Deleting a connection<a name="deleting-a-connection"></a>

If you no longer need a connection, you can delete it\.

**To delete a connection**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and select the device\.

1. Choose **Connections**, and select the connection\.

1. Choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.

**To delete a connection using the AWS CLI**  
Use the [delete\-connection](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/delete-connection.html) command\.