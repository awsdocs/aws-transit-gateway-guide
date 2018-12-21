# Monitor Your Transit Gateways<a name="transit-gateway-monitoring"></a>

You can use the following features to monitor your transit gateways, analyze traffic patterns, and troubleshoot issues with your transit gateways\.

**CloudWatch metrics**  
You can use Amazon CloudWatch to retrieve statistics about data points for your transit gateways as an ordered set of time series data, known as *metrics*\. You can use these metrics to verify that your system is performing as expected\. For more information, see [CloudWatch Metrics for Your Transit Gateways](transit-gateway-cloudwatch-metrics.md)\.

**VPC Flow Logs**  
You can use VPC Flow Logs to capture detailed information about the traffic going to and from your transit gateways\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) in the *Amazon VPC User Guide*\.

**CloudTrail logs**  
You can use AWS CloudTrail to capture detailed information about the calls made to the transit gateway API and store them as log files in Amazon S3\. You can use these CloudTrail logs to determine which calls were made, the source IP address where the call came from, who made the call, when the call was made, and so on\. For more information, see [Logging API Calls for Your Transit Gateway Using AWS CloudTrail](transit-gateway-cloudtrail-logs.md)\.