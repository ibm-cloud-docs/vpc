---

copyright:
  years: 2025
lastupdated: "2025-04-29"

keywords: public address range, bind, unbind

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Binding, unbinding, and moving public address ranges
{: #par-unbinding-binding}

Public Address Ranges for VPC is only available for evaluation and testing purposes for users with special access.
{: beta}

You can bind, unbind, and move public address ranges to a VPC in an availability zone with the console, CLI, and API.  
{: shortdesc}

## Before you begin
{: #before-you-begin-binding-public-address-range}

* Make sure to review [planning considerations](/docs/vpc?topic=vpc-about-par#par-planning) and [limitations](/docs/vpc?topic=vpc-par-limitations&interface=api) for public address ranges.
* The binding of a public address range must include both a VPC and an availability zone.

## Binding, unbinding and moving public address ranges in the console
{: #bind-unbind-par-ui} 
{: ui}

You can bind, unbind, and move a public address range to a VPC in an availability zone in the console.

### Binding a public address range in the console
{: #bind-par-ui}

To bind a public address range in the {{site.data.keyword.cloud}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** ![menu icon](../icons/icon_hamburger.svg), then click **Infrastructure > Network > Public address ranges**. The Public Address Ranges for VPC page appears.
1. Highlight the row of the address range in the table, then click **Bind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. From the Bind public address range side panel, select the VPC and its corresponding availability zone where you want to bind the address range.
1. Click **Bind** to bind the public address range to the VPC.

### Unbinding a public address range in the console
{: #unbind-par-ui}

To unbind a public address range in the IBM Cloud console, follow these steps:

After a public address is unbound, it is still reserved and available to be bound again.
{: tip}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** ![menu icon](../icons/icon_hamburger.svg), then click **Infrastructure > Network > Public address ranges**. The Public Address Ranges for VPC page appears.
1. Highlight the row of the address range in the table, then click **Unbind** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. Click **Unbind** to confirm that you want to unbind this address range from the VPC.

### Moving a public address range in the console
{: #moving-par-ui}

If the public address range is bound to a VPC in an availability zone, you can update and bind (move) it to a different VPC and availability zone. The public address range is automatically unbound from its VPC and availability zone and then bound to the newly specified VPC and availability zone.

You don't need to unbind the address range from its original target first.
{: tip}

To move a public address range in the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** ![menu icon](../icons/icon_hamburger.svg), then click **Infrastructure > Network > Public address ranges**. The Public Address Ranges for VPC page appears.
1. Highlight the row of the address range in the table, then click **Edit** from the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. From the address range side panel, select the VPC and its availability zone where you want to move the address range.
1. Click **Save** to bind the public address range to the new VPC.

## Binding, unbinding, and moving public address ranges from the CLI
{: #par-binding-unbinding-cli} 
{: cli}

To bind, unbind, or move a reserved IP address from the command line, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environments. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_PUBLIC_ADDRESS_RANGE=true
   ```
   {: pre}

   You'll receive a notification in the command line when updates to the IBM Cloud CLI and its plug-ins are available. It's important to keep your CLI up to date to access the latest commands. To check the current version of all installed plug-ins, run **`ibmcloud plugin list`**.
   {: tip}

1. Run the following command:

   ```sh
   ibmcloud is public-address-range-update PUBLIC_ADDRESS_RANGE [--name NAME] [--vpc VPC] [--zone ZONE] | --reset-target] [--output JSON] [-q, --quiet] 
   ```  

   Where: 

   `PUBLIC_ADDRESS_RANGE`
   :   The ID or name of the public address range to update.

   `--name`
   :   A new identifier for the public address range, if you want to rename it.

   `--vpc`
   :   The VPC you want to attach the public address range to. When specifying the  `--vpc` option, `--zone` is required.

   `--zone`
   :   The zone where you want this public address range to reside. When specifying the `--zone` option, `--vpc` is required.

   `--reset-target`
   :   Unbind a public address range.

   `-f, --force`
   :   Force the operation without confirmation.

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.

### Command examples
{: #par-cli-updating-examples}

If a public address is not bound to a VPC, you can bind it to a VPC in any availability zone:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --name public-address-range-1 --vpc cli-test-vpc --zone us-south-1
```
{: pre}

Unbind a public address range from a VPC and leave it unbound:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --name public-address-range-1 --reset-target
```
{: pre}

Move a public address range from one VPC (in any availability zone) to another:

```sh
ibmcloud is public-address-range-update r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 --name public-address-range-1 --vpc cli-test-vpc --zone us-south-1
```
{: pre}

## Binding, unbinding, and moving public address ranges with the API
{: #par-bind-unbind-api} 
{: api}

To bind, unbind, or move public address ranges with the API, follow these steps:
 
1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Store the following values in variables to be used in the API command:

   `version` (string): The API version, in format `YYYY-MM-DD`.   

1. When all variables are initiated, do one of the following:

   * Bind a public address range to a specific VPC:

      ```sh
        curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par-id?version=$version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -d '{
                     "target": {
                        "vpc": {
                           "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
                        },
                        "zone": {
                           "name": "us-south-2"
                        }
                     }
                  }'

      ```
      {: pre}

   * Unbind a public address range from a specific VPC:

      ```sh
         curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par-id?version=$version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -d '{
                     "target": null
                  }'
      ```
      {: pre}

      After a public address is unbound, it is still reserved and can be bound again.
      {: note}

   * Move a public address range from one VPC to another:

      ```sh
         curl -X PATCH \
               "$vpc_api_endpoint/v1/public_address_ranges/$par-id?version=$version&generation=2" \
               -H "Authorization: Bearer $iam_token" \
               -d '{
                     "target": {
                        "vpc": {
                           "id": "r006-4727d842-f94f-4a2d-824a-9bc9b02c523b"
                        },
                        "zone": {
                           "name": "us-south-2"
                        }
                     }
                  }'
      ```
      {: pre}

## Related links
{: #after-binding-par}

- [IAM roles and actions](/docs/vpc?topic=vpc-about-par#par-access-management)
- [Quotas and service limits](/docs/vpc?topic=vpc-quotas#par-quotas)
- [Creating public address ranges](/docs/vpc?topic=vpc-par-creating&interface=ui)
- [Viewing public address ranges](/docs/vpc?topic=vpc-par-viewing&interface=ui)
- [Deleting public address ranges](/docs/vpc?topic=vpc-par-deleting&interface=ui)
