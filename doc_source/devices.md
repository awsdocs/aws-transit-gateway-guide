# Devices<a name="devices"></a>

You can represent your on\-premises network in your global network through sites, devices, and links\. For more information, see [Define and associate your on\-premises network](how-network-manager-works.md#nm-how-it-works-on-premises)\. You can then associate a device with a site and one or more links\.

You can also create a device to represent a virtual appliance in your AWS network\. For more information, see [Connection between devices](network-manager-scenarios.md#scenario-tgw-connect)\.

A device is created for a specific global network and cannot be shared with other global networks\.

**Topics**
+ [Create a device](#creating-a-device)
+ [Update a device](#updating-a-device)
+ [Delete a device](#deleting-a-device)
+ [Associate a device](#device-associations)

## Create a device<a name="creating-a-device"></a>

Create a device to represent a physical or virtual appliance\.

**To create a device**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**\. Choose **Create device**\.

1. For **Name** and **Description**, enter a name and description for the device\.

1. For **Model**, enter the device model number\.

1. For **Serial number**, enter the serial number for the device\.

1. For **Type**, enter the device type\.

1. For **Vendor**, enter the name of the vendor, for example, `Cisco`\.

1. For **Location type**, specify whether the device is located in a remote location \(on\-premises network, data center, or other cloud provider\) or in AWS\.

   If you choose **AWS Cloud**, specify the location of the device within AWS\. For **Zone**, specify the name of an Availability Zone, Local Zone, Wavelength Zone, or an Outpost\. For **Subnet**, specify the Amazon Resource Name \(ARN\) of a subnet \(for example, arn:aws:ec2:us\-east\-1:111111111111:subnet/subnet\-abcd1234\)\.

1. For **Address**, enter the physical address of the site, for example, `New York, NY 10004`\.

1. For **Latitude**, enter the latitude coordinates for the site, for example, `40.7128`\.

1. For **Longitude**, enter the longitude coordinates for the site, for example, `-74.0060`\.

1. Choose **Create device**\.

**Creating and viewing a device using the AWS CLI**  
Use the following commands:
+ To create a device: [create\-device](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/create-device.html)
+ To view your devices: [get\-devices](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-devices.html)

## Update a device<a name="updating-a-device"></a>

You can update the details of your device, including the description, model, serial number, type, vendor, and location information\.

**To update a device**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices** and select the device\.

1. Choose **Edit**\.

1. Update the device details as needed, then choose **Edit device**\.

**Updating a device using the AWS CLI**  
Use the [update\-device](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/update-device.html) command\.

## Delete a device<a name="deleting-a-device"></a>

If you no longer need a device, you can delete it\. You must first disassociate the device from any sites, links, and customer gateways\.

**To delete a device**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**\.

1. Select the site and choose **Delete**\.

1. In the confirmation dialog box, choose **Delete**\.

**Deleting a device using the AWS CLI**  
Use the [delete\-device](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/delete-device.html) command\.

## Associate a device<a name="device-associations"></a>

You can associate a device with a site, and a device with one or more links\.

**Topics**
+ [Device and site associations](#device-site-association)
+ [Device and link associations](#device-link-association)

### Device and site associations<a name="device-site-association"></a>

 A site can have multiple devices associated with it, but a device can only be associated with a single site\.

**To associate a device and site**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of your device\.

1. Choose **Associate site**\.

1. For **Site**, choose the name of your site from the list\.

1. Choose **Edit site association**\.

You can remove the association between a device and a site\.

**To disassociate a device and site**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of your device\.

1. Choose **Disassociate site**\.

**Working with device and site associations using the AWS CLI**  
When you create a new device using the [create\-device](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/create-device.html) AWS CLI command, you can specify the site to associate with the device\. For an existing device, you can use the [update\-device](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/update-device.html) AWS CLI command to associate or disassociate a site\.

### Device and link associations<a name="device-link-association"></a>

A link can be associated with more than one device\. The device must be associated with a site\.

**To associate a link and a device**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of your device\.

1. Choose **Links**\.

1. Choose **Associate link**\.

1. Choose the link to associate, then choose **Associate link**\.

You can remove the association between a link and a device\.

**To disassociate a link and a device**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Devices**, and choose the ID of your device\.

1. Choose **Links**\.

1. Select the link and choose **Disassociate**\.

**Working with device and link associations using the AWS CLI**  
You can work with device associations using the following commands\.
+ To associate a link with a device: [associate\-link](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/associate-link.html)
+ To view your link associations: [get\-link\-associations](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-link-associations.html)
+ To disassociate a link from a device: [disassociate\-link](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/disassociate-link.html)