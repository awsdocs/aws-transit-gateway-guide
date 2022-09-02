# Logging network traffic using Transit Gateway Flow Logs<a name="tgw-flow-logs"></a>

Transit Gateway Flow Logs is a feature that enables you to capture information about the IP traffic going to and from your transit gateways\. Flow log data can be published to Amazon CloudWatch Logs or Amazon S3\. After you create a flow log, you can retrieve and view its data in the chosen destination\. Flow log data is collected outside of the path of your network traffic, and therefore does not affect network throughput or latency\. You can create or delete flow logs without any risk of impact to network performance\. Transit Gateway Flow Logs capture information related only to transit gateways, described in [Transit Gateway Flow Log records](#flow-log-records)\. If you want to capture information about IP traffic going to and from network interfaces in your VPCs, use VPC Flow Logs\. See [Logging IP traffic using VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) in the *Amazon VPC User Guide* for more information\.

**Note**  
To create a transit gateway flow log, you must either be the owner of the transit gateway, or if you are not the owner, the transit gateway owner must give you permission\.

Flow log data for a monitored transit gateway is recorded as *flow log records*, which are log events consisting of fields that describe the traffic flow\. For more information, see [Transit Gateway Flow Log records](#flow-log-records)\.

To create a flow log, you specify:
+ The resource for which to create the flow log
+ The destinations to which you want to publish the flow log data

After you create a flow log, it can take several minutes to begin collecting and publishing data to the chosen destinations\. Flow logs do not capture real\-time log streams for your transit gateways\. For more information, see [Create a flow log](working-with-flow-logs.md#create-flow-log)\. 

You can apply tags to your flow logs\. Each tag consists of a key and an optional value, both of which you define\. Tags can help you organize your flow logs, for example by purpose or owner\.

If you no longer require a flow log, you can delete it\. Deleting a flow log disables the flow log service for the resource, and no new flow log records are created or published to CloudWatch Logs or Amazon S3\. Deleting the flow log does not delete any existing flow log records or log streams \(for CloudWatch Logs\) or log file objects \(for Amazon S3\) for a transit gateway\. To delete an existing log stream, use the CloudWatch Logs console\. To delete existing log file objects, use the Amazon S3 console\. After you've deleted a flow log, it can take several minutes to stop collecting data\. For more information, see [Delete a flow log](working-with-flow-logs.md#delete-flow-log)\.

**Note**  
Querying a transit gateway flow log using Amazon Athena is not supported\.

**Topics**
+ [Transit Gateway Flow Log records](#flow-log-records)
+ [Publish flow logs to CloudWatch Logs](flow-logs-cwl.md)
+ [Publish flow logs to Amazon S3](flow-logs-s3.md)
+ [Work with Transit Gateway Flow Logs](working-with-flow-logs.md)

## Transit Gateway Flow Log records<a name="flow-log-records"></a>

A flow log record represents a network flow in your transit gateway\. Each record is a string with fields separated by spaces\. A record includes values for the different components of the traffic flow, for example, the source, destination, and protocol\.

When you create a flow log, you can use the default format for the flow log record, or you can specify a custom format\.

**Topics**
+ [Default format](#flow-logs-default)
+ [Custom format](#flow-logs-custom)
+ [Available fields](#flow-logs-fields)

### Default format<a name="flow-logs-default"></a>

With the default format, the flow log records includes all version 2 to version 6 fields, in the order shown in the [available fields](#flow-logs-fields) table\. You cannot customize or change the default format\. To capture additional fields or a different subset of fields, specify a custom format instead\.

### Custom format<a name="flow-logs-custom"></a>

With a custom format, you specify which fields are included in the flow log records and in which order\. This enables you to create flow logs that are specific to your needs, and to omit fields that are not relevant\. Using a custom format can reduce the need for separate processes to extract specific information from the published flow logs\. You can specify any number of the available flow log fields, but you must specify at least one\.

### Available fields<a name="flow-logs-fields"></a>

The following table describes all of the available fields for a transit gateway flow log record\. The **Version** column indicates which version the field was introduced in\.

When publishing flow log data to Amazon S3, the data type for the fields depends on the flow log format\. If the format is plain text, all fields are of type STRING\. If the format is Parquet, see the table for the field data types\.

If a field is not applicable or could not be computed for a specific record, the record displays a '\-' symbol for that entry\. Metadata fields that do not come directly from the packet header are best effort approximations, and their values might be missing or inaccurate\.


| Field | Description | Version | 
| --- | --- | --- | 
|  version  |  Indicates the version in which the field was introduced\. The default format includes all version 2 fields, in the same order that they appear in the table\. **Parquet data type:** INT\_32  | 2 | 
| resource\-type | The type of resource on which the subscription is created\. This can be TransitGateway or TransitGatewayAttachment\.Parquet data type: STRING | 6 | 
| account\-id |  The AWS account ID of the owner of the source transit gateway\. **Parquet data type:** STRING  | 2 | 
|  tgw\-id  | The ID of the transit gateway for which traffic is being recorded\.**Parquet data type: **STRING | 6 | 
|  tgw\-attachment\-id  | The ID of the transit gateway attachment for which traffic is being recorded\.**Parquet data type: ** STRING | 6 | 
|  tgw\-src\-vpc\-account\-id  |  The AWS account ID for the source VPC traffic\. **Parquet data type:** STRING   | 6 | 
|  tgw\-dst\-vpc\-account\-id  |  The AWS account ID for the destination VPC traffic\. **Parquet data type:** STRING   | 6 | 
|  tgw\-src\-vpc\-id  |  The ID of the source VPC for the transit gateway **Parquet data type:** STRING  | 6 | 
|  tgw\-dst\-vpc\-id  |  The ID of the destination VPC for the transit gateway\. **Parquet data type:** STRING   | 6 | 
|  tgw\-src\-subnet\-id  |  The ID of the subnet for the transit gateway source traffic\. **Parquet data type:** STRING   | 6 | 
|  tgw\-dst\-subnet\-id  |  The ID of the subnet for the transit gateway destination traffic\. **Parquet data type:** STRING   | 6 | 
| tgw\-src\-eni |  The ID of the source transit gateway attachment ENI for the flow\. **Parquet data type:** STRING   | 6 | 
| tgw\-dst\-eni | The ID of the destination transit gateway attachment ENI for the flow\.**Parquet data type:** STRING  | 6 | 
|  tgw\-src\-az\-id  |  The ID of the Availability Zone that contains the source transit gateway for which traffic is recorded\. If the traffic is from a sublocation, the record displays a '\-' symbol for this field\. **Parquet data type:** STRING  | 6 | 
|  tgw\-dst\-az\-id  |  The ID of the Availability Zone that contains the destination transit gateway for which traffic is recorded\.  **Parquet data type:** STRING  | 6 | 
| tgw\-pair\-attachment\-id |   Depending on the flow direction, this is either the egress or ingress attachment ID of the flow\. **Parquet data type:** STRING  | 6 | 
|  srcaddr  |  The source address for incoming traffic, or the IPv4 or IPv6 address of the transit gateway for outgoing traffic on the transit gateway\. The IPv4 address of the transit gateway is always its private IPv4 address\.  **Parquet data type: ** STRING  | 2 | 
|  dstaddr  |  The destination address for outgoing traffic, or the IPv4 or IPv6 address of the transit gateway for incoming traffic on the transit gateway\. The IPv4 address of the transit gateway is always its private IPv4 address\.  **Parquet data type: ** STRING  | 2 | 
|  srcport  |  The source port of the traffic\. **Parquet data type: **INT\_32   | 2 | 
|  dstport  |  The destination port of the traffic\. **Parquet data type: ** INT\_32  | 2 | 
|  protocol  |  The IANA protocol number of the traffic\. For more information, see [ Assigned Internet Protocol Numbers](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml)\. **Parquet data type: ** INT\_64  | 2 | 
|  packets  |  The number of packets transferred during the flow\. **Parquet data type:** INT\_64  | 2 | 
|  bytes  |  The number of bytes transferred during the flow\. **Parquet data type: ** INT\_64  | 2 | 
|  start  |  The time, in Unix seconds, when the first packet of the flow was received within the aggregation interval\. This might be up to 60 seconds after the packet was transmitted or received on the transit gateway\. **Parquet data type: ** INT\_64  | 2 | 
|  end  |  The time, in Unix seconds, when the last packet of the flow was received within the aggregation interval\. This might be up to 60 seconds after the packet was transmitted or received on the transit gateway\. **Parquet data type: ** INT\_64  | 2 | 
| log\-status |  The status of the flow log: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/tgw/tgw-flow-logs.html) **Parquet data type:** STRING  | 2 | 
| type |  The type of traffic\. Possible values are IPv4 \| IPv6 \| EFA\. For more information, see [Elastic Fabric Adapter](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa.html) in the *Amazon EC2 User Guide for Linux Instances*\. **Parquet data type:** STRING  | 3 | 
|  packets\-lost\-no\-route  |  The packets lost due to no route being specified\. **Parquet data type: **INT\_64   | 6 | 
|  packets\-lost\-blackhole  |  The packets lost due to a black hole\. **Parquet data type: **INT\_64   | 6 | 
|  packets\-lost\-mtu\-exceeded  |  The packets lost due to the size exceeding the MTU\. **Parquet data type:** INT\_64   | 6 | 
|  packets\-lost\-ttl\-expired  |  The packets lost due to the expiration of time\-to\-live\. **Parquet data type:** INT\_64   | 6 | 
|  tcp\-flags  |  The bitmask value for the following TCP flags: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpc/latest/tgw/tgw-flow-logs.html)  When a flow log entry consists of only ACK packets, the flag value is 0, not 16\.  For general information about TCP flags \(such as the meaning of flags like FIN, SYN, and ACK\), see [TCP segment structure ](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure) on Wikipedia\. TCP flags can be OR\-ed during the aggregation interval\. For short connections, the flags might be set on the same line in the flow log record, for example, 19 for SYN\-ACK and FIN, and 3 for SYN and FIN\. **Parquet data type:** INT\_32  | 3 | 
|  region  |  The Region that contains the transit gateway where traffic is recorded\. **Parquet data type:** STRING  | 4 | 
|  flow\-direction  |  The direction of the flow with respect to the interface where traffic is captured\. The possible values are: ingress \| egress\. **Parquet data type:** STRING  | 5 | 
|  pkt\-src\-aws\-service  |  The name of the subset of [IP address ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) for the srcaddr if the source IP address is for an AWS service\. The possible values are: AMAZON \| AMAZON\_APPFLOW \| AMAZON\_CONNECT \| API\_GATEWAY \| CHIME\_MEETINGS \| CHIME\_VOICECONNECTOR \| CLOUD9 \| CLOUDFRONT \| CODEBUILD \| DYNAMODB \| EBS \| EC2 \| EC2\_INSTANCE\_CONNECT \| GLOBALACCELERATOR \| KINESIS\_VIDEO\_STREAMS \| ROUTE53 \| ROUTE53\_HEALTHCHECKS \| ROUTE53\_HEALTHCHECKS\_PUBLISHING \| ROUTE53\_RESOLVER \| S3 \| WORKSPACES\_GATEWAYS\. **Parquet data type:** STRING  | 5 | 
| pkt\-dst\-aws\-service | The name of the subset of IP address ranges for the dstaddr field, if the destination IP address is for an AWS service\. For a list of possible values, see the pkt\-src\-aws\-service field\.Parquet data type: STRING | 5 | 