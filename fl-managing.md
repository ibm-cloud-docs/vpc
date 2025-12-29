---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-29"

keywords:

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Listing flow log collectors
{: #listing-all-flow-log-collectors}

You can list your flow log collectors by using the UI, the CLI, or the API.

##  Listing flow log collectors in the console
{: #fl-list-ui}
{: ui}

To list your flow log collectors by using the IBM Cloud console:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Flow logs**. If available, a list of provisioned flow log collectors shows.

Flow log collector attributes shown in the table are as follows:

* **Name** - The flow log name.
* **Status** - The status of the flow log collector (Example: Stable, Failed, Pending).
* **Active** - On/Off flag for the flow log. If Active, the flow log collector is set to write log files. If Suspended, the collector is deactivated and not writing log files.
* **Target** - The specified resource the collector logs traffic for.
* **Target type** - The type of target (for example, VPC, Subject, Instance).*
* **Object Storage Bucket** - The selected {{site.data.keyword.cos_full_notm}} bucket where the system saves flow log files.
* **Resource group** - Resource group associated with this flow log.
* **Created date (Local)** - Date created, specified in your local browser time zone.

##  Listing flow log collectors from the CLI
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

`--resource-group-id`
:   ID of the resource group. This option is mutually exclusive with `--resource-group-name`.
`--resource-group-name`
:   Name of the resource group. This option is mutually exclusive with `--resource-group-id`.
`--json`
:   Formats the output in JSON.



## Listing flow log collectors with the API
{: #fl-list-api}
{: api}

To list flow log collectors by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
1. When all variables are initiated, list your flow log collectors:

```curl
curl -sS -X GET \
  -H "Authorization: $iam_token" \
  "$vpc_api_endpoint/v1/flow_log_collectors?version=$api_version&generation=2" | jq
```
{: pre}

The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
{: note}
