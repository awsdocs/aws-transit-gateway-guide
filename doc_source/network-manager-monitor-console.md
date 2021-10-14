# Visualize and monitor your global network using the Network Manager console<a name="network-manager-monitor-console"></a>

The Network Manager console provides a dashboard that enables you to visualize and monitor your global network\. It includes information about the resources in your global network, their geographic location, the network topology, Amazon CloudWatch metrics and events, and enables you to perform route analysis\.

To access the dashboard for your global network, open the Network Manager console at [https://console\.aws\.amazon\.com/networkmanager/home](https://console.aws.amazon.com/networkmanager/home)\.

**Topics**
+ [Overview](#network-manager-main-dashboard)
+ [Details](#network-manager-view-details)
+ [Geographic](#network-manager-geographic)
+ [Topology](#network-manager-topology)
+ [Events](#network-manager-events)
+ [Monitoring](#network-manager-monitoring)
+ [Route Analyzer](#network-manager-route-analyzer)

## Overview<a name="network-manager-main-dashboard"></a>

On the **Overview** page, you can view the following information:
+ The inventory of your global network\.
+ A list of the transit gateways that are registered in your global network, and the overall status of the VPN connection attachments for those transit gateways\. To visualize and monitor an individual transit gateway, choose the transit gateway ID, or choose **Transit gateways** in the navigation pane\.
+ A list of the transit gateways that are registered in your global network, and the overall status of the Transit Gateway Connect peers for those transit gateways\. To visualize and monitor an individual transit gateway, choose the transit gateway ID, or choose **Transit gateways** in the navigation pane\.
+ A summary of network events, for example, topology changes in your global network\.

![\[Overview page in the Network Manager console\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/nm-overview.png)

## Details<a name="network-manager-view-details"></a>

On the **Details** page, you can view information about the global network resource, including the following: 
+ The Amazon Resource Name \(ARN\)
+ The name and description
+ The state
+ The AWS account ID
+ The assigned tags

## Geographic<a name="network-manager-geographic"></a>

On the **Geographic** page, you can view the locations of the resources that are registered in your global network on a map\. You can choose any of the following location points to view information about the resources in that location and their connections:
+ Circles represent resource types, such as Transit Gateways, VPNs, VPCs, and on\-premises devices\. Numbers within circles represent the quantity of that resource in a specific Region of the global network\.
+ Lines represent connections between resources\. The color of the line represents the type and state of the connection, such as if a VPN connection to a device is up or down or if multiple TGWs have a peering connection\.

![\[Geographic page in the Network Manager console\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/nm-geographic.png)

## Topology<a name="network-manager-topology"></a>

On the **Topology** page, you can view the network tree for your global network\. By default, the page displays all resources in your global network and the logical relationships between them\. You can filter the network tree to show specific on\-premises resource types only, for example, the preceding image shows sites and devices, and excludes customer gateways\. You can choose any of the nodes to view information about the specific resource it represents\. The line colors represent the state of the relationships between AWS and the on\-premises resources\.

![\[Topology page in the Network Manager console\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/nm-topology.png)

Customer gateways are represented as the resource that you define in AWS\. They are displayed to the left of the VPN connection and tunnel endpoint IP addresses, as shown in the following example\.

![\[Topology of a customer gateway\]](http://docs.aws.amazon.com/vpc/latest/tgw/images/nm-topology-cgw.png)

## Events<a name="network-manager-events"></a>

On the **Events** page, you can view the system events that describe changes in your global network\. For more information, see [Monitoring your global network with CloudWatch Events](monitoring-events.md)\.

## Monitoring<a name="network-manager-monitoring"></a>

On the **Monitoring** page, you can view CloudWatch metrics for the transit gateways, VPN connections, and on\-premises resources in your global network\. For more information, see [Monitoring your global network with Amazon CloudWatch metrics](monitoring-cloudwatch-metrics.md)\.

## Route Analyzer<a name="network-manager-route-analyzer"></a>

On the **Route Analyzer** page, you can perform an analysis of the routes in your transit gateway route tables\. This enables to you visualize routing paths and troubleshoot route\-related connectivity issues\. For more information, see [Route Analyzer](route-analyzer.md)\.