---

copyright:
  years: 2020
lastupdated: "2020-05-04"

keywords: flow logs, getting started, features

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:beta: .beta}
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

# About flow logs
{: #flow-logs}

The IBM Cloud Flow Logs Beta is only available to whitelisted users. Contact your IBM Sales representative if you are interested in getting early access.
{: beta}

{{site.data.keyword.cloud}} Flow Logs enable the collection, storage, and presentation of Internet Protocol (IP) traffic going to and from networks within your Virtual Private Cloud (VPC). Flow logs can help with a number of tasks, including:

* Troubleshooting why specific traffic isn't reaching an instance, which helps to diagnose restrictive security group rules.
* Monitoring the traffic that is reaching your instance.
* Adhering to compliance regulations.
* Determining the overall health of network monitoring.
* Assisting with root cause analysis.

## Overview of features
{: #fl-overview-of-features}

* Availability in all Multi-Zone Regions (MZRs) worldwide, providing global solutions
* Network-centric lifecycle and operations management
* Conveniently stores collector output in IBM Cloud Object Storage (COS)
* Metered pricing for services billed on a consumption basis
* Fault tolerance
* Collector output is in a non-proprietary format

## Configuring flow log collectors

You can configure flow log collectors with different collection granularities. For example, a collector that targets a Virtual Private Cloud (VPC) and aggregates in-transit data from _all_ network interfaces within that VPC. Or, a collector that targets a virtual server instance (VSI) and aggregates in-transit data from _only_ that VSI's network interfaces. After data is collected, flow logs are stored in a Cloud Object Storage (COS) bucket, which you configure when creating the flow log collector.
{: shortdesc}

You can set the granularity of a flow log collector for the following target scopes. Keep in mind that if you create a flow log for a subnet or VPC, each network interface in that subnet or VPC is monitored.

| Target | What data gets collected |
|---|---|
| VPC | Collects data for all network interfaces on a specific VPC. |
| Subnet | Collects data for all network interfaces on a specific subnet. |
| Instance | Collects data for all network interfaces on a specific virtual server. |
| Interface | Collects data for a specific network interface on a specific virtual server. |

During each collector upload interval, there are two flow logs (ingress and egress) per network interface that is written to the designated COS bucket.
{: note}

### Finest granularity wins
{: #flow-logs-granularity-wins}

Each flow resource type can have a single flow log collector. This can lead to overlaps. For example,

* A VSI interface might have a flow log collector.
* The VSI that the interface is attached to might have a separate flow log collector.
* The subnet that the VSI is attached to might have its own flow log collector.
* The VPC that the subnet is part of might have its own flow log collector.

If there is an overlap, the most targeted flow log collector takes precedence. This is important because each flow log collector might log to a different COS bucket, and where flow log data is stored can change over the lifetime of a VSI.  

## Getting started
{: #fl-getting-started}  

To get started using flow log collectors, follow these steps:

1. Complete any [prerequisites](/docs/vpc?topic=vpc-ordering-flow-log-collector#fl-before-you-begin) before ordering IBM Cloud Flow Logs.
2. Decide on the collection scope and [create one or more flow log collectors](/docs/vpc?topic=vpc-ordering-flow-log-collector).
3. Review the flow logs that were generated. See [Viewing flow log objects](/docs/vpc?topic=vpc-fl-analyze) for details.

## Related links
{: #fl-related-links}

These links provide additional information about the IBM Cloud Flow Logs beta release.

* [Flow logs CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#flow-logs-cli-ref)
* [Flow logs API reference](https://{DomainName}/apidocs/vpc#list-all-flow-log-collectors)
* [FAQs for flow log collectors](/docs/vpc?topic=vpc-fl-faq)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-flow-logs)
