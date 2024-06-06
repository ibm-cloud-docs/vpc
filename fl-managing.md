---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Listing flow log collectors
{: #listing-all-flow-log-collectors}

You can list your flow log collectors by using the UI, the CLI, or the API.

## Using the UI
{: #fl-list-ui}
{: ui}

To list your flow log collectors by using the IBM Cloud console:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg), then click > **VPC Infrastructure** ![VPC icon](../../icons/vpc.svg)  > **Flow logs** in the Network section. If available, a list of provisioned flow log collectors shows.

Flow log collector attributes shown in the table are as follows:

* **Status** - The status of the flow log collector (Example: Stable, Failed, Pending).
* **Active** - On/Off flag for the flow log. If Active, the flow log collector is set to write log files. If Suspended, the collector is deactivated and not writing log files.
* **Name** - The flow log name.
* **Target** - The specified resource the collector logs traffic for.
* **Date Created** - The date the flow log collector was provisioned.
* **Object Storage Bucket** - The selected {{site.data.keyword.cos_full_notm}} bucket where the system saves flow log files.

Notice that the page includes tabbed views, which show flow log collectors that were created with that target type.

* **VPC** - Shows flow log collectors that are attached directly to a VPC.
* **Subnet** - Shows flow log collectors that are attached directly to a subnet within the specified VPC.
* **Instance** - Shows all flow log collectors that are attached directly to a virtual server instance within the specified VPC.
* **Interface** - Shows all flow log collectors that are attached directly to a network interface of a virtual server instance within the specified VPC.

## Using the CLI
{: #fl-list-cli}
{: cli}

To list all your flow logs, run the following command:

```sh
ibmcloud is flow-logs \
  [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
  [--json]
```
{: pre}

Where:

- **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--json** formats the output in JSON.

## Using the API
{: #fl-list-api}
{: api}

To list flow log collectors by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
2. When all variables are initiated, list your flow log collectors:

```curl
curl -sS -X GET \
  -H "Authorization: $iam_token" \
  "$vpc_api_endpoint/v1/flow_log_collectors?version=$api_version&generation=2" | jq
```
{: pre}
