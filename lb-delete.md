---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-25"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network, delete

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
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Deleting an {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}}
{: #alb-deleting}

You can delete an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) by using the UI, CLI, or API.
{: shortdesc}

## Deleting an application load balancer by using the UI
{: #alb-deleting-ui}
{: ui}

To delete an ALB by using the IBM Cloud console, perform the following procedure:

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external}.
1. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg), then click **VPC Infrastructure > Load balancers** from the Network section.
1. Select the Region of your load balancer.
1. Click the overflow menu ![overflow menu](images/overflow.png) next to the load balancer that you want to delete, then select **Delete**.

The Status for the load balancer now shows **Deleting**. Refresh the page to confirm that the load balancer was deleted.

## Deleting an application load balancer by using the CLI
{: #alb-deleting-cli}
{: cli}

To delete an ALB by using the CLI, run the following command:

```
ibmcloud is load-balancer-delete <load_balancer_id> -f -q
```
{: pre}

Where:

* **load_balancer_id** is the ID of the load balancer (for example, `r134-99b5ab45-6357-42db-8b32-5d2c8aa62776`).
* **--force, -f** forces the operation without confirmation.
* **--quiet, -q** suppresses verbose output.

The following example is sample output:

```
This command deletes Load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 and cannot be undone. Continue [y/n] ?> y
Deleting load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...
OK
Deletion request for load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 was accepted.
```
{: screen}

## Deleting an application load balancer by using the API
{: #alb-deleting-api}
{: api}

To delete an ALB by using the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with
the right variables.

1. Run the following command to delete the load balancer:

    ```bash
    curl -H "Authorization: $iam_token" -X DELETE "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
    ```
    {: codeblock}
