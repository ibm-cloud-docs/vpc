---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-04"

keywords: flow logs, delete

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Deleting a flow log collector
{: #deleting-a-flow-log-collector}

You can delete a flow log collector by using the UI, the CLI, or the API.

**Notes**:

* Any logs that were collected, but weren't sent to {{site.data.keyword.cos_full}} yet, are dropped immediately on deletion. If dropping immediately is a problem, you can suspend the collector instead.
* Flow logs that were shipped to your {{site.data.keyword.cos_full}} buckets are not deleted.

## Deleting a flow log collector in the console
{: #fl-deleting-ui}
{: ui}

To delete a flow log collector, click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") next to the collector that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

## Deleting a flow log collector from the CLI
{: #fl-deleting-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete a flow log collector by using the CLI, run the following command:

```sh
ibmcloud is flow-log-delete FLOW_LOG [-f, --force]
```
{: pre}

Where:

- **FLOW_LOG** is the ID of the flow log instance.
- **--force, -f** forces the operation without confirmation.

## Deleting a flow log collector with the API
{: #fl-deleting-api}
{: api}

To delete a flow log collector by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
1. Store your `FlowLogID01` in a variable to be used in the API command:

   ```sh
   export FlowLogID01="<your_flow_log_id>"
   ```
   {: pre}

1. To delete a flow log collector:

   ```sh
   curl -sS -X DELETE \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" | jq
   ```
   {: pre}

   The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
   {: note}
