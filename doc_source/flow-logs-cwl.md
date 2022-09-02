# Publish flow logs to CloudWatch Logs<a name="flow-logs-cwl"></a>

Flow logs can publish flow log data directly to Amazon CloudWatch\.

When published to CloudWatch Logs, the flow log data is published to a log group, and each transit gateway has a unique log stream in the log group\. Log streams contain flow log records\. You can create multiple flow logs that publish data to the same log group\. If the same transit gateway is present in one or more flow logs in the same log group, it has one combined log stream\. If you've specified that one flow log should capture rejected traffic, and the other flow log should capture accepted traffic, then the combined log stream captures all traffic\.

Data ingestion and archival charges for vended logs apply when you publish flow logs to CloudWatch Logs\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

In CloudWatch Logs, the **timestamp** field corresponds to the start time that's captured in the flow log record\. The **ingestionTime** field provides the date and time when the flow log record was received by CloudWatch Logs\. The timestamp is later than the end time that's captured in the flow log record\.

For more information about CloudWatch Logs, see [Logs sent to CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AWS-logs-and-resource-policy.html#AWS-logs-infrastructure-CWL) in the *Amazon CloudWatch Logs User Guide*\.

**Topics**
+ [IAM roles for publishing flow logs to CloudWatch Logs](#flow-logs-iam)
+ [Permissions for IAM users to pass a role](#flow-logs-iam-user)
+ [Create a flow log that publishes to CloudWatch Logs](#flow-logs-cwl-create-flow-log)
+ [Process flow log records in CloudWatch Logs](#process-records-cwl)

## IAM roles for publishing flow logs to CloudWatch Logs<a name="flow-logs-iam"></a>

The IAM role that's associated with your flow log must have sufficient permissions to publish flow logs to the specified log group in CloudWatch Logs\. The IAM role must belong to your AWS account\.

The IAM policy that's attached to your IAM role must include at least the following permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": "*"
    }
  ]
}
```

Also ensure that your role has a trust relationship that allows the flow logs service to assume the role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

We recommend that you use the `aws:SourceAccount` and `aws:SourceArn` condition keys to protect yourself against [the confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html)\. For example, you could add the following condition block to the previous trust policy\. The source account is the owner of the flow log and the source ARN is the flow log ARN\. If you don't know the flow log ID, you can replace that portion of the ARN with a wildcard \(\*\) and then update the policy after you create the flow log\.

```
"Condition": {
    "StringEquals": {
        "aws:SourceAccount": "account_id"
    },
    "ArnLike": {
        "aws:SourceArn": "arn:aws:ec2:region:account_id:vpc-flow-log/flow-log-id"
    }
}
```

### Create or update an IAM role for flow logs<a name="create-flow-logs-role"></a>

You can update an existing role or use the following procedure to create a new role for use with flow logs\.

**To create an IAM role for flow logs**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\. For **Use case**, choose **EC2**\. Choose **Next: Permissions**\.

1. On the **Attach permissions policies** page, choose **Next: Tags** and optionally add tags\. Choose **Next: Review**\.

1. Enter a name for your role and optionally provide a description\. Choose **Create role**\.

1. Select the name of your role\. For **Permissions**, choose **Add inline policy**, **JSON**\.

1. Copy the first policy from [IAM roles for publishing flow logs to CloudWatch Logs](#flow-logs-iam) and paste it in the window\. Choose **Review policy**\.

1. Enter a name for your policy, and choose **Create policy**\.

1. Select the name of your role\. For **Trust relationships**, choose **Edit trust relationship**\. In the existing policy document, change the service from `ec2.amazonaws.com` to `vpc-flow-logs.amazonaws.com`\. Choose **Update Trust Policy**\.

1. On the **Summary** page, note the ARN for your role\. You need this ARN when you create your flow log\.

## Permissions for IAM users to pass a role<a name="flow-logs-iam-user"></a>

Users must also have permissions to use the `iam:PassRole` action for the IAM role that's associated with the flow log\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["iam:PassRole"],
      "Resource": "arn:aws:iam::account-id:role/flow-log-role-name"
    }
  ]
}
```

## Create a flow log that publishes to CloudWatch Logs<a name="flow-logs-cwl-create-flow-log"></a>

You can create flow logs for transit gateways\. If you perform these steps as an IAM user, ensure that you have permissions to use the `iam:PassRole` action\. For more information, see [Permissions for IAM users to pass a role](#flow-logs-iam-user)\.

**Prerequisite**  
Create the destination log group\. Open the [Log groups page](https://console.aws.amazon.com/cloudwatch/home#logsV2:log-groups) in the CloudWatch console and choose **Create log group**\. Enter a name for the log group and choose **Create**\.

**To create a transit gateway flow log using the console**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit gateways**\. 

1. Select the check boxes for one or more transit gateways and choose **Actions**, **Create flow log**\.

1. For **Destination**, choose **Send to CloudWatch Logs**\.

1. For **Destination log group**, choose the name of the destination log group that you created\.

1. For **IAM role**, specify the name of the role that has permissions to publish logs to CloudWatch Logs\.

1. For **Log record format**, select the format for the flow log record\.
   + To use the default format, choose **AWS default format**\.
   + To use a custom format, choose **Custom format** and then select fields from **Log format**\.

1. \(Optional\) Choose **Add new tag** to apply tags to the flow log\.

1. Choose **Create flow log**\.

**To create a flow log for a VPC or a subnet using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Transit gateways** or **Transit gateway attachments**\.

1. Select the checkbox for one or more transit gateways or transit gateway attachments, and then choose **Actions**, **Create flow**\.

1. For **Filter**, specify the type of traffic to log\. Choose **All** to log accepted and rejected traffic, **Reject** to log only rejected traffic, or **Accept** to log only accepted traffic\.

1. For **Destination**, choose **Send to CloudWatch Logs**\.

1. For **Destination log group**, choose the name of the destination log group that you created\.

1. For **IAM role**, specify the name of the role that has permissions to publish logs to CloudWatch Logs\.

1. For **Log record format**, select the format for the flow log record\.
   + To use the default format, choose **AWS default format**\.
   + To use a custom format, choose **Custom format** and then select fields from **Log format**\.

1. \(Optional\) Choose **Add new tag** to apply tags to the flow log\.

1. Choose **Create flow log**\.

**To create a flow log using the command line**

Use one of the following commands\.
+ [create\-flow\-logs](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-flow-logs.html) \(AWS CLI\)
+ [New\-EC2FlowLogs](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2FlowLogs.html) \(AWS Tools for Windows PowerShell\)
+ [CreateFlowLogs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateFlowLogs.html) \(Amazon EC2 Query API\)

The following AWS CLI example creates a flow log that captures transit gateway information\. The flow logs are delivered to a log group in CloudWatch Logs called `my-flow-logs`, in account 123456789101, using the IAM role `publishFlowLogs`\.

```
aws ec2 create-flow-logs --resource-type TransitGateway --resource-ids tgw-1a2b3c4d --log-group-name my-flow-logs --deliver-logs-permission-arn arn:aws:iam::123456789101:role/publishFlowLogs 
```

## Process flow log records in CloudWatch Logs<a name="process-records-cwl"></a>

You can work with flow log records as you would with any other log events collected by CloudWatch Logs\. For more information about monitoring log data and metric filters, see [Searching and Filtering Log Data](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/MonitoringLogData.html) in the *Amazon CloudWatch User Guide*\.

### Example: Create a CloudWatch metric filter and alarm for a flow log<a name="flow-logs-cw-alarm-example"></a>

In this example, you have a flow log for `eni-1a2b3c4d`\. You want to create an alarm that alerts you if there have been 10 or more rejected attempts to connect to your instance over TCP port 22 \(SSH\) within a 1\-hour time period\. First, you must create a metric filter that matches the pattern of the traffic for which to create the alarm\. Then, you can create an alarm for the metric filter\.

**To create a metric filter for rejected SSH traffic and create an alarm for the filter**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**, **Log groups**\.

1. Select the check box for the log group, and then choose **Actions**, **Create metric filter**\.

1. For **Filter Pattern**, enter the following\.

   ```
   [version, resource_type, account_id,tgw_id="tgw-123abc456bca”, tgw_attachment_id, tgw_src_vpc_account_id, tgw_dst_vpc_account_id, tgw_src_vpc_id, tgw_dst_vpc_id, tgw_src_subnet_id, tgw_dst_subnet_id, tgw_src_eni, tgw_dst_eni, tgw_src_az_id, tgw_dst_az_id, tgw_pair_attachment_id, srcaddr= "10.0.0.1", dstaddr, srcport=“80”, dstport, protocol=“6”, packets, bytes,start,end, log_status, type,packets_lost_no_route, packets_lost_blackhole, packets_lost_mtu_exceeded, packets_lost_ttl_expired, tcp_flags,region, flow_direction, pkt_src_aws_service, pkt_dst_aws_service]
   ```

1. For **Select log data to test**, select the log stream for your transit gateway\. \(Optional\) To view the lines of log data that match the filter pattern, choose **Test pattern**\. When you're ready, choose **Next**\.

1. Enter a filter name, metric namespace, and metric name\. Set the metric value to **1**\. When you're done, choose **Next** and then choose **Create metric filter**\.

1. In the navigation pane, choose **Alarms**, **All alarms**\.

1. Choose **Create alarm**\.

1. Choose the namespace for the metric filter that you created\.

   It can take a few minutes for a new metric to display in the console\.

1. Select the metric name that you created, and then choose **Select metric**\.

1. Configure the alarm as follows, and then choose **Next**:
   + For **Statistic**, choose **Sum**\. This ensure that you capture the total number of data points for the specified time period\.
   + For **Period**, choose **1 hour**\.
   + For **Whenever**, choose **Greater/Equal** and enter **10** for the threshold\.
   + For **Additional configuration**, **Datapoints to alarm**, leave the default of **1**\.

1. For **Notification**, select an existing SNS topic, or choose **Create new topic** to create a new one\. Choose **Next**\.

1. Enter a name and description for the alarm and choose **Next**\.

1. When you are done configuring the alarm, choose **Create alarm**\.