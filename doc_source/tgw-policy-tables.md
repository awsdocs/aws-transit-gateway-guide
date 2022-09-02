# Transit gateway policy tables<a name="tgw-policy-tables"></a>

Transit gateway dynamic routing uses policy tables to route network traffic for AWS Cloud WAN\. The table contains policy rules for matching network traffic by policy attributes, and then maps the traffic that matches the rule to a target route table\. 

**Note**  
Transit gateway policy tables are currently only supported in Cloud WAN when creating a transit gateway peering connection\. When creating a peering connection, you can associate that table with the connection\. The association then populates the table automatically with the policy rules\.   
For more information about peering connections in Cloud WAN, see [ Peerings](https://docs.aws.amazon.com/vpc/latest/cloudwan/cloudwan-peerings.html) in the *AWS Cloud WAN User Guide*\.

## Create a transit gateway policy table<a name="tgw-policy-tables-create"></a>

**To create a transit gateway policy table using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit gateway policy table**\.

1. Choose **Create transit gateway policy table**\.

1. \(Optional\) For **Name tag**, enter a name for the transit gateway policy table\. This creates a tag, where the tag value is the name that you specify\.

1. For Transit gateway ID, select the transit gateway for the policy table\.

1. Choose **Create transit gateway policy table**\.

**To create a transit gateway policy table using the AWS CLI**  
Use the [create\-transit\-gateway\-policy\-table](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-transit-gateway-policy-table.html) command\.

## Delete a transit gateway policy table<a name="tgw-policy-tables-disable"></a>

Delete a transit gateway policy table\. When a table is deleted, all policy rules within that table are deleted\.

**To delete a transit gateway policy table using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. On the navigation pane, choose **Transit gateway policy tables**\.

1. Choose the transit gateway policy table to delete\.

1. Choose **Actions**, and then choose **Delete policy table**\. 

1. Confirm that you want to delete the table\.

**To delete a transit gateway policy table using the AWS CLI**  
Use the [delete\-transit\-gateway\-policy\-table](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-transit-gateway-policy-table.html) command\.