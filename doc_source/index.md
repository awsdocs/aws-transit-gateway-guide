# Amazon Virtual Private Cloud Transit Gateways

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is a transit gateway?](what-is-transit-gateway.md)
+ [How transit gateways work](how-transit-gateways-work.md)
+ [Getting started with transit gateways](tgw-getting-started.md)
+ [Transit gateway design best practices](tgw-best-design-practices.md)
+ [Examples](TGW_Scenarios.md)
   + [Example: Centralized router](transit-gateway-centralized-router.md)
   + [Example: Isolated VPCs](transit-gateway-isolated.md)
   + [Example: Isolated VPCs with shared services](transit-gateway-isolated-shared.md)
   + [Example: Peered transit gateways](transit-gateway-peering-scenario.md)
   + [Example: Centralized outbound routing to the internet](transit-gateway-nat-igw.md)
   + [Example: Appliance in a shared services VPC](transit-gateway-appliance-scenario.md)
+ [Work with transit gateways](working-with-transit-gateways.md)
   + [Transit gateways](tgw-transit-gateways.md)
   + [Transit gateway attachments to a VPC](tgw-vpc-attachments.md)
   + [Transit gateway attachments to a Direct Connect gateway](tgw-dcg-attachments.md)
   + [Transit gateway VPN attachments](tgw-vpn-attachments.md)
   + [Transit gateway peering attachments](tgw-peering.md)
   + [Transit gateway Connect attachments and Transit Gateway Connect peers](tgw-connect.md)
   + [Transit gateway route tables](tgw-route-tables.md)
      + [Prefix list references](tgw-prefix-lists.md)
   + [Multicast on transit gateways](tgw-multicast-overview.md)
      + [Multicast routing](how-multicast-works.md)
      + [Working with multicast](working-with-multicast.md)
         + [Managing multicast domains](manage-domain.md)
         + [Managing multicast groups](manage-multicast-group.md)
         + [Working with shared multicast domains](multicast-sharing.md)
+ [Transit gateway sharing considerations](transit-gateway-share.md)
+ [Monitor your transit gateways](transit-gateway-monitoring.md)
   + [CloudWatch metrics for your transit gateways](transit-gateway-cloudwatch-metrics.md)
   + [Logging API calls for your transit gateway using AWS CloudTrail](transit-gateway-cloudtrail-logs.md)
+ [Authentication and access control for your transit gateways](transit-gateway-authentication-access-control.md)
   + [Use service-linked roles for transit gateway and Transit Gateway Network Manager](service-linked-roles.md)
      + [Transit gateway service-linked role](tgw-service-linked-roles.md)
      + [Transit Gateway Network Manager service-linked role](nm-service-linked-roles.md)
   + [AWS managed policies for transit gateways and Transit Gateway Network Manager](security-iam-awsmanpol.md)
   + [How Network ACLs work with transit gateways](tgw-nacls.md)
+ [Transit Gateway Network Manager](what-is-network-manager.md)
   + [How Transit Gateway Network Manager works](how-network-manager-works.md)
   + [Getting started with Transit Gateway Network Manager](network-manager-getting-started.md)
   + [Scenarios for Transit Gateway Network Manager](network-manager-scenarios.md)
   + [Work with Network Manager](working-with-network-manager.md)
      + [Global networks](global-networks.md)
      + [Transit gateway registrations](tgw-registrations.md)
      + [Sites](sites.md)
      + [Links](links.md)
      + [Devices](devices.md)
      + [Connections](device-connections.md)
      + [Customer gateway associations](cgw-association.md)
      + [Transit Gateway Connect peer associations](connect-peer-association.md)
   + [Visualize and monitor your global network using the Network Manager console](network-manager-monitor-console.md)
   + [Using Amazon CloudWatch metrics and events with your global network](monitoring-overview.md)
      + [Monitoring your global network with Amazon CloudWatch metrics](monitoring-cloudwatch-metrics.md)
         + [CloudWatch metrics for on-premises resources](cw-metrics-on-premises.md)
         + [Viewing global network CloudWatch metrics](viewing-metrics.md)
      + [Monitoring your global network with CloudWatch Events](monitoring-events.md)
   + [Route Analyzer](route-analyzer.md)
   + [Identity and access management for Transit Gateway Network Manager](nm-security-iam.md)
   + [Tag your Network Manager resources](network-manager-tagging.md)
   + [Log Transit Gateway Network Manager API calls using AWS CloudTrail](nm-logging-using-cloudtrail.md)
+ [Quotas for your transit gateways](transit-gateway-quotas.md)
+ [Document history for transit gateways](doc-history.md)