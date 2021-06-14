# Sites<a name="sites"></a>

You can represent your on\-premises network in your global network through sites, devices, and links\. For more information, see [Define and associate your on\-premises network](how-network-manager-works.md#nm-how-it-works-on-premises)\. You then associate a device with a site and one or more links\.

A site is created for a specific global network and cannot be shared with other global networks\.

**Topics**
+ [Creating a site](#creating-a-site)
+ [Updating a site](#updating-a-site)
+ [Deleting a site](#deleting-a-site)

## Creating a site<a name="creating-a-site"></a>

Create a site to represent the physical location of your network\. The location information is used for visualization in the Network Manager console\.

**To create a site**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Sites**\. Choose **Create site**\.

1. For **Name** and **Description**, enter a name and description for the site\.

1. For **Address**, enter the physical address of the site, for example, `New York, NY 10004`\.

1. For **Latitude**, enter the latitude coordinates for the site, for example, `40.7128`\.

1. For **Longitude**, enter the longitude coordinates for the site, for example, `-74.0060`\.

1. Choose **Create site**\.

**Creating and viewing a site using the AWS CLI**  
Use the following commands:
+ To create a site: [create\-site](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/create-site.html)
+ To view your sites: [get\-sites](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/get-sites.html)

## Updating a site<a name="updating-a-site"></a>

You can update the details of your site, including the description, address, latitude, and longitude\.

**To update your site**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID for your global network\.

1. In the navigation pane, choose **Sites**, and select your site\.

1. Choose **Edit**\.

1. Update the description, address, latitude, longitude, and tags as needed\.

1. Choose **Edit site**\.

**Updating a site using the AWS CLI**  
Use the [update\-site](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/update-site.html) command\.

## Deleting a site<a name="deleting-a-site"></a>

If you no longer need a site, you can delete it\. You must first disassociate the site from any devices and delete any links for the site\.

**To delete a site**

1. Open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

1. In the navigation pane, choose **Global networks**\.

1. Choose the ID of your global network\.

1. In the navigation pane, choose **Sites**\.

1. Select the site and choose **Delete**\.

1. In the confirmation dialog box, choose **Delete**\.

**Deleting a site using the AWS CLI**  
Use the [delete\-site](https://docs.aws.amazon.com/cli/latest/reference/networkmanager/delete-site.html) command\.