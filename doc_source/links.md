# Links<a name="links"></a>

You can represent your on\-premises network in your global network through sites, devices, and links\. For more information, see [Define and associate your on\-premises network](how-network-manager-works.md#nm-how-it-works-on-premises)\. You then associate a device with a site and one or more links\.

A link is created for a specific global network and cannot be shared with other global networks\.

**Topics**
+ [Create a link](#creating-a-link)
+ [Update a link](#updating-a-link)
+ [Delete a link](#deleting-a-link)

## Create a link<a name="creating-a-link"></a>

Create a link to represent an internet connection from a device\. A link is created for a specific site, therefore you must create a site before you create a link\.

**To create a link**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Sites**\. Choose the ID of the site for which to create the link, and the choose **Links**\.

1. Choose **Create link**\.

1. For **Name** and **Description**, enter a name and description for the link\.

1. For **Upload speed**, enter the upload speed in Mbps\.

1. For **Download speed**, enter the download speed in Mbps\.

1. For **Provider**, enter the name of the service provider\.

1. For **Type**, enter the type of link, for example, `broadband`\.

1. Choose **Create link**\.

**Creating and viewing a link using the AWS CLI**  
Use the following commands:
+ To create a link: [create\-link](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/create-link.html)
+ To view your links: [get\-links](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-links.html)

## Update a link<a name="updating-a-link"></a>

You can update the details of your link, including the bandwidth information, description, provider, and type\.

**To update a link**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Sites** and choose the ID for the site\. Choose **Links**\.

1. Select the link and choose **Edit**\.

1. Update the link details as needed, then choose **Edit link**\.

**Updating a link using the AWS CLI**  
Use the [update\-link](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/update-link.html) command\.

## Delete a link<a name="deleting-a-link"></a>

If you no longer need a link, you can delete it\. You must first disassociate the link from any devices and customer gateways\.

**To delete a link**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Sites** and choose the ID for the site\. Choose **Links**\.

1. Select the link and choose **Delete**\.

1. In the confirmation dialog box, choose **Delete**\.

**Deleting a link using the AWS CLI**  
Use the [delete\-link](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/delete-link.html) command\.