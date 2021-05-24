---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-24"

keywords: flow logs, delete

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Deleting a flow log collector
{: #deleting-a-flow-log-collector}

You can delete a flow log collector by using the UI, the CLI, or the API.

**Notes**:

* Any logs that were collected, but weren't sent to Cloud Object Storage (COS) yet, are dropped immediately on deletion. If dropping immediately is a problem, you can suspend the collector instead.
* Flow logs that were shipped to your COS buckets are not deleted.

## Using the UI
{: #fl-deleting-ui}
{: ui}

To delete a flow log collector, click the overflow menu ![overflow menu](images/overflow.png) next to the collector that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

## Using the CLI
{: #fl-deleting-cli}
{: cli}

To delete a flow log collector by using the CLI, run the following command:

```
ibmcloud is flow-log-delete FLOW_LOG [-f, --force]
```

Where:

- **FLOW_LOG** is the ID of the flow log instance.
- **--force, -f** forces the operation without confirmation.

## Using the API
{: #fl-deleting-api}
{: api}

To delete a flow log collector by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with
the right variables.
2. Store your `FlowLogID01` in a variable to be used in the API command:
    ```sh
    export FlowLogID01="<your_flow_log_id>"
    ```
    {:pre}
3. To delete a flow log collector:

   ```sh
   curl -sS -X DELETE \
     -H "Authorization: $iam_token" \
     "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" | jq
   ```
   {:pre}
