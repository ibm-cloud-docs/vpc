---

copyright:
  years:  2023, 2025
lastupdated: "2025-02-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPN gateway states and descriptions
{: #vpn-states}

When viewing the list of VPN gateways in IBM Cloud VPC, there are columns for lifecycle status and health status.
{: shortdesc}

## Lifecycle status
{: #lifecycle-status}

The following table shows the possible VPN gateway lifecycle statuses and a description of what each status means.

|Lifecycle status|Description|
|----------------|-----------|
|Deleting        | The resource is in the process of being deleted.|
|Failed          | The resource is unreachable, inoperative, or otherwise incapacitated.|
|Pending         | The resource has been created, but the process is not yet complete.|
|Stable          | The resource is operating as expected.|
|Suspended       | The resource has been suspended. Contact IBM Support for further clarification.|
|Updating        | The resource is currently being updated.|
|Waiting         | The resource is currently paused while waiting for a response.|
{: caption="Lifecycle status descriptions" caption-side="bottom"}

## Health status
{: #health-status}

The following table shows the possible VPN gateway health statuses and a description of what each status means.

|Health status|Description|
|----------------|-----------|
|Degraded        |The resource is experiencing compromised performance, capacity, or connectivity.|
|Faulted         |The resource is unreachable, inoperative, or otherwise incapacitated.|
|Healthy         |No abnormal behavior detected.|
|Inapplicable    |The health state doesn't apply because of the current lifecycle status of the resource. For example, a lifecycle status of `deleting` will have a health state of `inapplicable`.|
{: caption="Health status descriptions" caption-side="bottom"}

For error codes, reasons for failure, and suggested solutions for recovery, see [Diagnosing VPN gateway health](/docs/vpc?topic=vpc-vpn-health).
{: note}
