---

copyright:
  years: 2019, 2026
lastupdated: "2026-01-03"

keywords: application load balancer, session stickiness, vpc network, update

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating session stickiness for application load balancers
{: #alb-updating-session-stickiness}

You can update the session stickiness for an {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) with the console, CLI, or API.

## Updating session stickiness for application load balancers using the UI
{: #alb-updating-session-stickiness-ui}
{: ui}

To update session stickiness for an ALB by in the {{site.data.keyword.cloud_notm}} console, perform the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Select the Region of your load balancer.
1. Select the load balancer that you want to update.
1. Select Back-end pool where you need to edit the session stickiness.
1. After you're done editing, select Save to save your changes.

   The Active button in the upper left of your window should now show as Updating. When the Updating status changes back to Active, the update is done and the new changes are applied.
   {: important}

## Updating session stickiness for application load balancers using the CLI
{: #alb-updating-session-stickiness-cli}
{: cli}

To use the CLI to update session stickiness for your application load balancer, follow these steps:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account using the CLI. After you enter the password, the system prompts which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Update your ALB's session stickiness:

    ```sh
    ibmcloud is load-balancer-pool-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 --session-persistence-type app_cookie --session-persistence-cookie-name my-cookie-name where: 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 = Load Balancer ID 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 = VPC ID session-persistence-type = app_cookie session-persistence-cookie-name = my-cookie-name
    ```
    {: pre}

There are multiple ways to update your ALB from the CLI. For other options, see [VPC CLI reference for load balancers](/docs/vpc?topic=vpc-vpc-reference#lb-anchor).

## Updating session stickiness for application load balancers using the API
{: #alb-updating-session-stickiness-api}
{: api}

The following example illustrates how to use the API to update session stickiness for an application load balancer.

To update session stickiness for an application load balancer with the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

   * `loadBalancerId` - First, get your load balancer and then populate the variable:

      ```sh
      export ResourceGroupId=<your_resourcegroup_id>
      ```
      {: pre}
1. Alternatively, you can find your resource group ID by using IBM Cloud console. From your browser, open the IBM Cloud console and log in to your account. Select Manage > Account > Resource Groups.
1. Use the following example to update session stickiness for your application load balancer:


   ```bash
   curl -X PATCH 
   "$vpc_api_endpoint/v1/load_balancers/$load_balancer_id/pools/$pool_id?version=2025-11-18&generation=2" -H "Authorization: Bearer $iam_token" 
   \
      -d '{    "name": "my-pool", 
               "session_persistence": 
                        { 
                          "type": "source_ip" 
                        } 
         }'
   ```
   {: codeblock}

   There are multiple ways to update your ALB from the API. For other options, see [VPC API reference for load balancers](/apidocs/vpc/latest#update-load-balancer-pool).

