---

copyright:
  years: 2019, 2025
lastupdated: "2025-01-10"

keywords: flow logs, activate, deactivate, suspend, resume

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Suspending and resuming a flow log collector
{: #managing-flow-log-collectors_activate}

You can suspend and resume a flow log collector by using the UI, the CLI, or the API.

After you create a flow log collector, its default state is `Active`.
{: note}

## Suspending and resuming a flow log collector in the UI
{: #fl-managing-ui}
{: ui}

To suspend an `Active` flow log collector, follow these steps:

1. From the Flow Logs for VPC page, click the name of the flow log that you want details for. 

   The Flow logs details page appears.

1. Switch **Active** to **Inactive**. 

   Suspending the flow log stops the flow log from writing to the {{site.data.keyword.cos_full}} bucket.
   {: note}

To resume a suspended flow log, switch the toggle back to **Active**. 

## Suspending and resuming a flow log collector from the CLI
{: #fl-managing-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To suspend or resume a flow log collector by using the CLI, you must pass a **true** or **false** value to the **--active** flag on the **flow-log-update** command.

```sh
ibmcloud is flow-log-update FLOW_LOG --active (true|false) [--json]
```
{: pre}

Where:

* **FLOW_LOG** is the ID of the flow log instance.
* **--active** is the intended `active` status after the update. Set to **true** to resume or **false** to suspend.
* **--json** formats the output in JSON.

## Suspending and resuming a flow log collector with the API
{: #fl-managing-api}
{: api}

To suspend and resume flow log collectors by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
1. Store your `FlowLogID01` in a variable to be used in the API command:

   ```sh
   export FlowLogID01="<your_flow_log_id>"
   ```
   {: pre}

1. Choose either to suspend or resume a flow log:

   * To suspend a flow logs collector:

      ```sh
      curl -s -X PATCH \
      "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{ "active": false }' | jq
      ```
      {: pre}

   * To resume a flow log collector's operation:

      ```sh
      curl -s -X PATCH \
      "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{ "active": true }' | jq
      ```
      {: pre}
