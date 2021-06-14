# Customer gateway associations<a name="cgw-association"></a>

To add your on\-premises network to your global network, you associate a customer gateway with your device, and optionally, a link\. The customer gateway must already be in your global network as part of a VPN attachment in your transit gateway\. If you specify a link, it must already be associated with the specified device\.

For more information about creating a customer gateway, see [Create a Customer Gateway](https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html#vpn-create-cgw) in the *AWS Site\-to\-Site VPN User Guide*\. For more information about creating a VPN attachment to a transit gateway, see [Transit Gateway VPN Attachments](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-vpn-attachments.html) in *Amazon VPC Transit Gateways*\.

For more information about viewing the topology of your on\-premises network in Network Manager, see [Topology](network-manager-monitor-console.md#network-manager-topology)\.

You can associate a customer gateway with a device and link in one of the following ways:
+ On the **Transit gateways** page
+ On the **Devices** page

------
#### [ Transit gateways page ]

**To associate a customer gateway using the Transit gateways page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Transit gateways**, and then choose the ID of your transit gateway\.

1. Choose **On\-premises associations**\.

1. Select your customer gateway and choose **Associate**\.

1. For **Device**, select the ID of the device to associate\. For **Link**, select the ID of the link to associate\.

1. Choose **Edit on\-premises association**\.

------
#### [ Devices page ]

**To associate a customer gateway using the Devices page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and then choose the ID of your device\.

1. Choose **On\-premises associations**\.

1. Choose **Associate**\.

1. For **Customer gateway**, select the ID of the customer gateway to associate\. For **Link**, select the ID of the link to associate\.

1. Choose **Create on\-premises association**\.

------

You can disassociate a customer gateway from a device or link in one of the following ways:
+ On the **Transit gateways** page
+ On the **Devices** page

------
#### [ Transit gateways page ]

**To disassociate a customer gateway using the Transit gateways page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Transit gateways**, and then choose **On\-premises associations**\.

1. Select your customer gateway and choose **Disassociate**\.

------
#### [ Devices page ]

**To disassociate a customer gateway using the Devices page**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and then choose the ID of your device\.

1. Choose **On\-premises associations**\.

1. Select your customer gateway and choose **Disassociate**\.

------

**Working with customer gateway associations using the AWS CLI**  
You can work with customer gateway associations using the following commands\.
+ To associate a customer gateway with a device and link: [associate\-customer\-gateway](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/associate-customer-gateway.html)
+ To view your customer gateway associations: [get\-customer\-gateway\-associations](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-customer-gateway-associations.html)
+ To disassociate a customer gateway from a device and link: [disassociate\-customer\-gateway](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/disassociate-customer-gateway.html)