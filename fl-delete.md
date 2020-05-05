---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-22"

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

# Deleting a flow log collector
{: #deleting-a-flow-log-collector}

You can delete a flow log collector by using the UI, the CLI, or the API.

**Notes**:

* Any logs that were collected, but weren't sent to Cloud Object Storage (COS) yet, are dropped immediately on deletion. If this is a problem, you can suspend the collector instead.
* Flow logs that were already shipped to your COS buckets are not deleted.

## Using the UI
{: #fl-deleting-ui}

To delete a flow log collector, click the **Overflow** ![Overflow menu](images/overflow.png) menu next to the collector that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

## Using the CLI
{: #fl-deleting-cli}

To delete a flow log collector by using the CLI, run the following command:

```
ibmcloud is flow-log-delete FLOW_LOG [-f, --force]
```

Where **--force, -f** forces the operation without confirmation.

## Using the API
{: #fl-deleting-api}

To delete a flow log collector by using the API, follow these steps:

1. Provision these variables with the appropriate values:

   * `token` - Use the following command:

      ```sh
      export token="$(ibmcloud iam oauth-tokens | awk '{ print $4 }')"
      ```
      {:pre}

   * `api_endpoint` - Set the environment's end point. For example, for production in `us-south` use:

      ```sh
      export api_endpoint=https://us-south.iaas.cloud.ibm.com
      ```
      {:pre}

   * `FlowLogID01t` - Set the requested flow-logs ID in variable:

      ```sh
      export FlowLogID01="<your_flow_log_id>"
      ```
      {:pre}

2. To delete a flow log collector:

```sh
curl -v -sS -X DELETE \
  -H "Authorization: $token" \
  $api_endpoint/v1/flow_log_collectors/$FlowLogID04?version=2020-05-03 | jq
```
{:pre}
