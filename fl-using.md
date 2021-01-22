---

copyright:
  years: 2020
lastupdated: "2020-07-23"

keywords: flow logs, getting started

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:beta: .beta}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# About {{site.data.keyword.cloud_notm}} Flow Logs for VPC
{: #flow-logs}

{{site.data.keyword.cloud}} Flow Logs for VPC enable the collection, storage, and presentation of information about the Internet Protocol (IP) traffic going to and from network interfaces within your Virtual Private Cloud (VPC).
{: shortdesc}

23 July 2020: This service is now generally available. Starting 23 August 2020, all flow log collectors that are created during the beta period will be automatically converted to the standard paid plan (see [Pricing](https://www.ibm.com/cloud/vpc/pricing#product-faq-section){: external}). If you do not want these flow log collectors to incur fees, delete the collectors before this date by using the UI, CLI, or API.
{: important}

Flow logs can help with a number of tasks, including:

* Troubleshooting why specific traffic isn't reaching an instance, which helps to diagnose restrictive security group rules
* Recording the metadata network traffic that is reaching your instance
* Determining source and destination traffic from the network interfaces
* Adhering to compliance regulations
* Assisting with root cause analysis

## Overview of features
{: #fl-overview-of-features}

* Availability in all {{site.data.keyword.cloud_notm}} Multi-Zone Regions (MZRs) worldwide, providing global solutions
* Network-centric lifecycle and operations management
* Suspend (stop) and resume (start) collector activity
* Conveniently stores collector output in IBM Cloud Object Storage (COS)
* No impact to network performance
* Built-in fault tolerance
* Pricing is metered per GB of metadata that is collected per flow log target

   See [Pricing for IBM Cloud Flow Logs on VPC](https://www.ibm.com/cloud/vpc/pricing#product-faq-section){: external} for details.
   {: note}

## Configuring flow log collectors

You can configure flow log collectors with different collection scopes. For example, a collector that targets a VPC and aggregates in-transit data from _all_ network interfaces within that VPC. Or, a collector that targets a virtual server instance (VSI) and aggregates in-transit data from _only_ that VSI's network interfaces. After data is collected, flow logs are stored in a Cloud Object Storage (COS) bucket, which you configure when you create the flow log collector.

You can set the granularity of a flow log collector for the following target scopes. Keep in mind that if you create a flow log for a subnet or VPC, each network interface in that subnet or VPC is monitored.

| Target | What data gets collected |
|---|---|
| VPC | Collects data for all network interfaces on a specific VPC. |
| Subnet | Collects data for all network interfaces on a specific subnet. |
| Instance | Collects data for all network interfaces on a specific virtual server. |
| Interface | Collects data for a specific network interface on a specific virtual server. |

During each collector upload interval, there are two flow logs (ingress and egress) per network interface that are written to the designated COS bucket.
{: note}

### Finest granularity wins
{: #flow-logs-granularity-wins}

Each flow log target can have a single flow log collector, which can lead to overlaps. Here are examples, with the finest granularity listed first:

* A VSI interface might have a flow log collector.
* The VSI that the interface is attached to might have a separate flow log collector.
* The subnet that the VSI is attached to might have its own flow log collector.
* The VPC that the subnet is part of might have its own flow log collector.

If an overlap exists, the most targeted flow log collector takes precedence. This precedence is important because each flow log collector might log to a different COS bucket, and where flow log data is stored, can change over the lifetime of a virtual server instance.

## Getting started
{: #fl-getting-started}  

To get started using flow log collectors, follow these steps:

1. Complete any [prerequisites](/docs/vpc?topic=vpc-ordering-flow-log-collector#fl-before-you-begin) before ordering IBM Cloud Flow Logs.
2. Decide on the collection scope and [create one or more flow log collectors](/docs/vpc?topic=vpc-ordering-flow-log-collector).
3. Review the flow logs that were generated. See [Viewing flow log objects](/docs/vpc?topic=vpc-fl-analyze) for details.

Creating or deleting a flow log doesn't impact network performance. Flow log data is collected outside of the path of your network traffic so that it doesn't affect network latency or throughput.
{: note}

## Flow logs use cases
{: #flow-logs-use-cases}

With IBM Cloud Flow Logs for VPC, you can validate that network connections and data are arriving to destination targets successfully, and troubleshoot if it is not. This is useful when diagnosing security group rules, denied flows, and so on.

### Use case 1: Analyzing flow logs using LogDNA
{: #analyzing-flow-logs-using-logdna-example}

You can review and download a [flow log example solution](https://github.com/IBM-Cloud/vpc-flowlogs-logdna){: external} of how to use a trigger function to read a flow log COS object and write it to LogDNA.

### Use case 2: Setting up finest granularity wins
{: #finest-granularity-wins-example}

The following diagram shows possible ways that you can configure flow log collectors. This example highlights different flow log targets defined within the same VPC, depicting which collectors get the data from their associated instances. For example:

* The **VPC** flow log collector (blue) is an umbrella collector. Any instances that are within the VPC now, or in the future, flow into Bucket-E if no other collectors are targeting the bucket.
* The **Subnet** flow log collector (green) is also an umbrella collector, but for a specific subnet within a VPC. Any instances in this subnet, or in the future within this subnet, flow into Bucket-C.
* The **Instance** flow log collector (red) is used when you need even finer granularity. For example, suppose that Instance-5 was using Bucket-E and you are trying to troubleshoot a connectivity issue on a specific instance. You can narrow down your troubleshooting by creating a flow log collector just for that instance, sending all its traffic to a separate bucket for a certain amount of time.

   ![Finest granularity wins example](/images/fl-granularity.png "Finest granularity wins example")

### Use case 3: Troubleshooting security groups and network ACLs
{: #troubleshooting-perf-problems-example}

Similar to use case 2, this use case illustrates how to set the granularity of flow log collectors for different target scopes. It also goes one step further to depict how flow logs can help you troubleshoot using security groups and network ACLs (NACLs).  

Scenario:

1. NACL 2 on Subnet 3 is preventing you from running a web server there. Notice that this network ACL allows ingress, but denies egress.  
2. A VSI on Subnet 2 (NACL 1 allows ingress and egress) tries to query the web server that you are running on Subnet 3.
3. The query gets through and you see that the traffic is accepted on the ingress path to Subnet 3 into VSI 31.  
4. When VSI 31 receives the request, it generates a reply and tries to send it. Unfortunately, the response cannot be returned and your connection hangs on Subnet 2 until it times out.
5. Flow logs show that the request was sent and that it was accepted on VSI 31. Flow logs show that the response traffic was rejected.

![Troubleshooting security groups and network ACLs](/images/fl-sc-acls.png "Troubleshooting security groups and network ACLs")

### Use case 4: Detecting port vulnerabilities
{: #detect-port-vulnerabilities-example}

Consider a scenario where an attacker initiates connections to different TCP ports. In turn, these connections are blocked by the security group filter. Flow logs collect all the flows that were rejected by the security group and reports them to Cloud Object Storage (COS).

![Example of detecting port vulnerabilities](/images/fl-attack.png "Example of detecting port vulnerabilities")

## Related links
{: #fl-related-links}

These links provide additional information about {{site.data.keyword.cloud_notm}} Flow Logs for VPC.

* [Flow logs CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#flow-logs-cli-ref)
* [Flow logs API reference](https://{DomainName}/apidocs/vpc#list-flow-log-collectors)
* [FAQs for flow log collectors](/docs/vpc?topic=vpc-fl-faq)
* [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-flow-logs)
* [Blog: Use IBM Log Analysis with LogDNA to Analyze VPC Network Traffic from IBM Cloud Flow Logs for VPC](https://www.ibm.com/cloud/blog/use-ibm-log-analysis-with-logdna-to-analyze-vpc-network-traffic-from-ibm-cloud-flow-logs-for-vpc)
* [Blog: Indexing and Searching VPC Flow Logs in IBM Cloud Databases for Elasticsearch](https://www.ibm.com/cloud/blog/indexing-and-searching-vpc-flow-logs-in-ibm-cloud-databases-for-elasticsearch)
* [Article: Provisioning VPC Flow Logs in IBM Cloud using Terraform](https://developer.ibm.com/recipes/tutorials/provisioning-vpc-flow-logs-in-ibm-cloud-using-terraform/)
