# Transit gateway Connect peer associations<a name="connect-peer-association"></a>

You can associate a transit gateway [Connect peer](tgw-connect.md) \(in a transit gateway Connect attachment\) with a device, and optionally, with a link\.

 If you specify a link, it must be associated with the specified device\.

You can create a transit gateway Connect peer association in one of the following ways:
+ On the **Transit gateways** page
+ On the **Devices** page

------
#### [ Transit gateways page ]

**To associate a Connect peer using the Transit gateways page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/](https://console.aws.amazon.com/networkmanager/)\. 

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Transit gateways**, and then choose the ID of your transit gateway\.

1. Choose **Connect peer associations**\.

1. Select the Connect peer and choose **Edit**\.

1. For **Device**, select the ID of the device to associate\. For **Link**, select the ID of the link to associate\.

1. Choose **Edit Connect peer association**\.

------
#### [ Devices page ]

**To associate a Connect peer using the Devices page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/](https://console.aws.amazon.com/networkmanager/)\. 

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of the device\.

1. Choose **Connect peer associations**\.

1. Choose **Associate**\.

1. For **Connect peer**, choose the Connect peer\.

1. \(Optional\) For **Link**, choose the link for the Connect peer association\.

1. Choose **Create Connect peer association**\.

------

You can disassociate a Connect peer from a device in one of the following ways:
+ On the **Transit gateways** page
+ On the **Devices** page

------
#### [ Transit gateways page ]

**To disassociate a Connect peer using the Transit gateways page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/](https://console.aws.amazon.com/networkmanager/)\. 

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Transit gateways**, and then choose **Connect peer associations**\.

1. Select the Connect peer and choose **Disassociate**\.

------
#### [ Devices page ]

**To disassociate a Connect peer using the Devices page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/](https://console.aws.amazon.com/networkmanager/)\. 

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and then choose the ID of your device\.

1. Choose **Connect peer associations**\.

1. Select the Connect peer and choose **Disassociate**\.

------

**Working with Connect peer associations using the AWS CLI**  
You can work with Connect peer associations using the following commands\.
+ To associate a Connect peer with a device: [associate\-transit\-gateway\-connect\-peer](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/associate-transit-gateway-connect-peer.html)
+ To view your Connect peer associations: [get\-transit\-gateway\-connect\-peer\-associations](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-transit-gateway-connect-peer-associations.html)
+ To disassociate a Connect peer from a device: [disassociate\-transit\-gateway\-connect\-peer](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/disassociate-transit-gateway-connect-peer.html)