---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-09"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network, delete

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an application load balancer
{: #alb-deleting}

You can delete an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) with the console, CLI, or API.
{: shortdesc}

## Deleting an application load balancer in the console
{: #alb-deleting-ui}
{: ui}

To delete an ALB by in the IBM Cloud console, perform the following procedure:

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
1. Select the **Navigation Menu** ![Navigation Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers** from the Network section.
1. Select the Region of your load balancer.
1. Click the Actions menu ![Actions menu](images/overflow.png) next to the load balancer that you want to delete, then select **Delete**.

The Status for the load balancer now shows **Deleting**. Refresh the page to confirm that the load balancer was deleted.

## Deleting an application load balancer from the CLI
{: #alb-deleting-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete an ALB from the CLI, run the following command:

```sh
ibmcloud is load-balancer-delete <load_balancer_id> -f -q
```
{: pre}

Where:

* **load_balancer_id** is the ID of the load balancer (for example, `r006-99b5ab45-6357-42db-8b32-5d2c8aa62776`).
* **--force, -f** forces the operation without confirmation.
* **--quiet, -q** suppresses verbose output.

The following example is sample output:

```sh
This command deletes Load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 and cannot be undone. Continue [y/n] ?> y
Deleting load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...
OK
Deletion request for load balancer r006-99b5ab45-6357-42db-8b32-5d2c8aa62776 was accepted.
```
{: screen}

## Deleting an application load balancer with the API
{: #alb-deleting-api}
{: api}

To delete an ALB with the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Run the following command to delete the load balancer:

    ```bash
    curl -H "Authorization: $iam_token" -X DELETE "$vpc_api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
    ```
    {: codeblock}
