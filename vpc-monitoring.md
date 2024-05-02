---

copyright:
  years:  2024
lastupdated: "2024-01-25"

keywords: status, health status

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring a VPC for status and health
{: #vpc-states}

When viewing the list of VPCs in IBM Cloud VPC, there are columns for the VPC status and health status.
{: shortdesc}

## Monitoring VPC status
{: #monitor-vpc-status}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Network > VPCs**.
2. For each Virtual Private Cloud in the table, refer to columns for **Status** and **Health status**. See the following sections for available statuses and definitions. 

## VPC resource status
{: #vpc-resource-status}

The following table shows the possible VPC resource statuses and a description of what each status means.

| Status| Description|
|----------------|-----------|
| Available       | The resource is available.|
| Deleting        | The resource is in the process of being deleted.|
| Failed          | The resource is unreachable, inoperative, or otherwise incapacitated.|
| Pending         | The resource has been created, but the process is not yet complete.|
| Stable          | The resource is operating as expected.|
| Suspended       | The resource has been suspended. Contact IBM Support for further clarification.|
| Updating        | The resource is currently being updated.|
| Waiting         | The resource is currently paused while waiting for a response.|
{: caption="Table 1: VPC status descriptions" caption-side="bottom"}

## Health status
{: #vpc-health-status}

The following table shows the possible VPC health statuses and a description of what each status means.

|Health status|Description|
|----------------|-----------|
| Degraded        | The resource is experiencing compromised performance, capacity, or connectivity.|
| Faulted         | The resource is unreachable, inoperative, or otherwise incapacitated.|
| Healthy         | No abnormal behavior detected.|
| Inapplicable    | The health state doesn't apply because of the current lifecycle status of the resource. For example, a lifecycle status of `deleting` will have a health state of `inapplicable`.|
{: caption="Table 2: Health status descriptions" caption-side="bottom"}


