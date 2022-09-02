# Work with Transit Gateway Flow Logs<a name="working-with-flow-logs"></a>

You can work with Transit Gateway Flow Logs using the Amazon EC2, Amazon VPC, CloudWatch, and Amazon S3 consoles\.

sad

**Topics**
+ [Control the use of flow logs](#controlling-use-of-flow-logs)
+ [Create a flow log](#create-flow-log)
+ [View flow logs](#view-flow-logs)
+ [Add or remove tags for flow logs](#modify-tags-flow-logs)
+ [View flow log records](#view-flow-log-records)
+ [Search flow log records](#search-flow-log-records)
+ [Delete a flow log](#delete-flow-log)
+ [API and CLI overview and limitations](#flow-logs-api-cli)

## Control the use of flow logs<a name="controlling-use-of-flow-logs"></a>

By default, IAM users do not have permission to work with flow logs\. You can create an IAM user policy that grants users the permissions to create, describe, and delete flow logs\. For more information, see [Granting IAM Users Required Permissions for Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/ec2-api-permissions.html) in the *Amazon EC2 API Reference*\.

The following is an example policy that grants users full permissions to create, describe, and delete flow logs\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DeleteFlowLogs",
        "ec2:CreateFlowLogs",
        "ec2:DescribeFlowLogs"
      ],
      "Resource": "*"
    }
  ]
}
```

Some additional IAM role and permission configuration is required, depending on whether you're publishing to CloudWatch Logs or Amazon S3\. For more information, see [Publish flow logs to CloudWatch Logs](flow-logs-cwl.md) and [Publish flow logs to Amazon S3](flow-logs-s3.md)\.

## Create a flow log<a name="create-flow-log"></a>

You can create flow logs for your transit gateways that can publish data to CloudWatch Logs or Amazon S3\.

For more information, see [Create a flow log that publishes to CloudWatch Logs](flow-logs-cwl.md#flow-logs-cwl-create-flow-log) and [Create a flow log that publishes to Amazon S3](flow-logs-s3.md#flow-logs-s3-create-flow-log)\.

## View flow logs<a name="view-flow-logs"></a>

You can view information about your flow logs in the Amazon VPC console by viewing the **Flow Logs** tab for a specific resource\. When you select the resource, all of the flow logs for that resource are listed\. The information displayed includes the ID of the flow log, the flow log configuration, and information about the status of the flow log\.

**To view information about flow logs for transit gateways**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit gateways** or **Transit gateway attachments**\.

1. Select a transit gateway or transit gateway attachment and choose **Flow Logs**\. Information about the flow logs is displayed on the tab\. The **Destination type** column indicates the destination to which the flow logs are published\.

## Add or remove tags for flow logs<a name="modify-tags-flow-logs"></a>

You can add or remove tags for a flow log in the Amazon EC2 and Amazon VPC consoles\.

**To add or remove tags for a transit gateway flow log**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit gateways** or **Transit gateway attachments**\.

1. Select a transit gateway or transit gateway attachment 

1. Choose **Manage tags** for the required flow log\.

1. To add a new tag, choose **Create Tag**\. To remove a tag, choose the delete button \(x\)\.

1. Choose **Save**\.

## View flow log records<a name="view-flow-log-records"></a>

You can view your flow log records using the CloudWatch Logs console or Amazon S3 console, depending on the chosen destination type\. It might take a few minutes after you've created your flow log for it to be visible in the console\.

**To view flow log records published to CloudWatch Logs**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**, and select the log group that contains your flow log\. A list of log streams for each transit gateway is displayed\.

1.  Select the log stream that contains the ID of the transit gateway that you want to view the flow log records for\. For more information, see [Transit Gateway Flow Log records](tgw-flow-logs.md#flow-log-records)\.

**To view flow log records published to Amazon S3**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. For **Bucket name**, select the bucket to which the flow logs are published\.

1. For **Name**, select the check box next to the log file\. On the object overview panel, choose **Download**\.

## Search flow log records<a name="search-flow-log-records"></a>

You can search your flow log records that are published to CloudWatch Logs by using the CloudWatch Logs console\. You can use [metric filters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) to filter flow log records\. Flow log records are space delimited\.

**To search flow log records using the CloudWatch Logs console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**, and then choose **Log groups**\. 

1. Select the log group that contains your flow log\. A list of log streams for each transit gateway is displayed\.

1. Select the individual log stream if you know the transit gateway that you are searching for\. Alternatively, choose **Search Log Group** to search the entire log group\. This might take some time if there are many transit gateways in your log group, or depending on the time range that you select\.

1. For **Filter events**, enter the following string\. This assumes that the flow log record uses the [default format](tgw-flow-logs.md#flow-logs-default)\.

   ```
   [version, resource_type, account_id,tgw_id, tgw_attachment_id, tgw_src_vpc_account_id, tgw_dst_vpc_account_id, tgw_src_vpc_id, tgw_dst_vpc_id, tgw_src_subnet_id, tgw_dst_subnet_id, tgw_src_eni, tgw_dst_eni, tgw_src_az_id, tgw_dst_az_id, tgw_pair_attachment_id, srcaddr, dstaddr, srcport, dstport, protocol, packets, bytes,start,end, log_status, type,packets_lost_no_route, packets_lost_blackhole, packets_lost_mtu_exceeded, packets_lost_ttl_expired, tcp_flags,region, flow_direction, pkt_src_aws_service, pkt_dst_aws_service]
   ```

1. Modify the filter as needed by specifying values for the fields\. The following examples filter by specific source IP addresses\.

   ```
   [version, resource_type, account_id,tgw_id, tgw_attachment_id, tgw_src_vpc_account_id, tgw_dst_vpc_account_id, tgw_src_vpc_id, tgw_dst_vpc_id, tgw_src_subnet_id, tgw_dst_subnet_id, tgw_src_eni, tgw_dst_eni, tgw_src_az_id, tgw_dst_az_id, tgw_pair_attachment_id, srcaddr= 10.0.0.1, dstaddr, srcport, dstport, protocol, packets, bytes,start,end, log_status, type,packets_lost_no_route, packets_lost_blackhole, packets_lost_mtu_exceeded, packets_lost_ttl_expired, tcp_flags,region, flow_direction, pkt_src_aws_service, pkt_dst_aws_service]
   [version, resource_type, account_id,tgw_id, tgw_attachment_id, tgw_src_vpc_account_id, tgw_dst_vpc_account_id, tgw_src_vpc_id, tgw_dst_vpc_id, tgw_src_subnet_id, tgw_dst_subnet_id, tgw_src_eni, tgw_dst_eni, tgw_src_az_id, tgw_dst_az_id, tgw_pair_attachment_id, srcaddr= 10.0.2.*, dstaddr, srcport, dstport, protocol, packets, bytes,start,end, log_status, type,packets_lost_no_route, packets_lost_blackhole, packets_lost_mtu_exceeded, packets_lost_ttl_expired, tcp_flags,region, flow_direction, pkt_src_aws_service, pkt_dst_aws_service]
   ```

   The following example filters by transit gateway ID tgw\-123abc456bca, destination port, and number of bytes\.

   ```
   [version, resource_type, account_id,tgw_id=tgw-123abc456bca, tgw_attachment_id, tgw_src_vpc_account_id, tgw_dst_vpc_account_id, tgw_src_vpc_id, tgw_dst_vpc_id, tgw_src_subnet_id, tgw_dst_subnet_id, tgw_src_eni, tgw_dst_eni, tgw_src_az_id, tgw_dst_az_id, tgw_pair_attachment_id, srcaddr, dstaddr, srcport, dstport = 80 || dstport = 8080, protocol, packets, bytes >= 500,start,end, log_status, type,packets_lost_no_route, packets_lost_blackhole, packets_lost_mtu_exceeded, packets_lost_ttl_expired, tcp_flags,region, flow_direction, pkt_src_aws_service, pkt_dst_aws_service]
   ```

## Delete a flow log<a name="delete-flow-log"></a>

You can delete a transit gateway flow log using the Amazon VPC console\. 

These procedures disable the flow log service for a resource\. Deleting a flow log does not delete the existing log streams from CloudWatch Logs or log files from Amazon S3\. Existing flow log data must be deleted using the respective service's console\. In addition, deleting a flow log that publishes to Amazon S3 does not remove the bucket policies and log file access control lists \(ACLs\)\.

**To delete a transit gateway flow log**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit gateways**\.

1. Choose a **Transit gateway ID**\.

1. In the Flow logs section, choose the flow logs that you want to delete\.

1. Choose **Actions**, and then choose **Delete flow logs**\.

1. Confirm that you want to delete the flow by choosing **Delete**\.

## API and CLI overview and limitations<a name="flow-logs-api-cli"></a>

You can perform the tasks described on this page using the command line or API\. 

The following limitations apply when using the `[CreateFlowLogs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateFlowLogs.html)` API or the [https://docs.aws.amazon.com/cli/latest/reference/ec2/create-flow-logs.html](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-flow-logs.html) CLI:
+ `--resource-ids` has a maximum constraint of 25 `TransitGateway` or `TransitGatewayAttachment` resource types\. 
+ `--traffic-type` is not a required field by default\. An error is returned if you provide this for transit gateway resource types\. This limit applies only to transit gateway resource types\.
+ `--max-aggregation-interval` has a default value of `60`, and is the only accepted value for transit gateway resource types\. An error is returned if you try to pass any other value\. This limit applies only to transit gateway resource types\. 
+ `--resource-type` supports two new resource types, `TransitGateway` and `TransitGatewayAttachment`\.
+ `--log-format` includes all log fields for transit gateway resource types if you do not set which fields you want to include\. This applies only to transit gateway resource types\.

**Create a flow log**
+ [create\-flow\-logs](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-flow-logs.html) \(AWS CLI\)
+ [New\-EC2FlowLog](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2FlowLog.html) \(AWS Tools for Windows PowerShell\)
+ [CreateFlowLogs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateFlowLogs.html) \(Amazon EC2 Query API\)

**Describe your flow logs**
+ [describe\-flow\-logs](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-flow-logs.html) \(AWS CLI\)
+ [Get\-EC2FlowLog](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2FlowLog.html) \(AWS Tools for Windows PowerShell\)
+ [DescribeFlowLogs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeFlowLogs.html) \(Amazon EC2 Query API\)

**View your flow log records \(log events\)**
+ [get\-log\-events](https://docs.aws.amazon.com/cli/latest/reference/logs/get-log-events.html) \(AWS CLI\)
+ [Get\-CWLLogEvent](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-CWLLogEvent.html) \(AWS Tools for Windows PowerShell\)
+ [GetLogEvents](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogEvents.html) \(CloudWatch API\)

**Delete a flow log**
+ [delete\-flow\-logs](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-flow-logs.html) \(AWS CLI\)
+ [Remove\-EC2FlowLog](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-EC2FlowLog.html) \(AWS Tools for Windows PowerShell\)
+ [DeleteFlowLogs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeleteFlowLogs.html) \(Amazon EC2 Query API\)