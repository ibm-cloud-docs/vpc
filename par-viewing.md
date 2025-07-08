---

copyright:
  years: 2025
lastupdated: "2025-07-08"

keywords: viewing, deleting, public address range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing public address ranges
{: #par-viewing} 

Accounts with special approval can now create public address ranges and use their IPs in custom route tables to route ingress traffic to VPC resources. Available in Frankfurt and Madrid.
{: preview} 

You can view public address ranges with the console, CLI, and API.
{: shortdesc}

## Viewing public address ranges in the console
{: #view-par-ui}
{: ui}

To view public address ranges in the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Public address ranges**.

The Public address ranges for VPC page shows the following information:

- **Name:** The name of the public address range object.
- **Status:** States whether the address range is bound or unbound to the specified VPC.
- **Lifecycle state:** States whether the binding or unbinding of the address range is successful, and if it is stable or not.
- **IP range:** The range of IP addresses included in the address range.
- **Zone:** States the zone that the address range is bound to (if applicable).
- **Virtual Private Cloud:** States the VPC that the address range is bound to in the previous zone (if applicable).

## Viewing public address ranges from the CLI
{: #par-view-cli}
{: cli}

To view public address ranges from the command line, follow these steps:

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
   ibmcloud is public-address-range PUBLIC_ADDRESS_RANGE [--output JSON] [-q, --quiet]
   ```

   Where:

   `PUBLIC_ADDRESS_RANGE`
   :   The ID or name of the public address range to delete.

   `--output`
   :   The output format, only JSON is supported. One of: JSON.

   `-q, --quiet`
   :   Suppress verbose output.

### Command examples
{: #par-cli-view-examples}

View the public address range `$par-id`:

```sh
ibmcloud is public-address-range $par-id
```
{: pre}

View all public address ranges:

```sh
ibmcloud is public address ranges
```
{: pre}

## Viewing public address ranges with the API
{: #par-view-api}
{: api}

To view public address ranges with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to use in the API command:

   For example: `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, do one of the following:

   * View all public address ranges for an account:

      ```sh
      curl -sX GET \
               "$e/v1/public_address_ranges?version=$dt&generation=2" \
               -H "Authorization: Bearer $iam_token"
      ```
      {: pre}

   * View a specific public address range:

      ```sh
      curl -sX GET \
               "$e/v1/public_address_ranges/$par-id?version=$dt&generation=2" \
               -H "Authorization: Bearer $iam_token"
      ```
      {: pre}

## Related links
{: #after-view-par}

- [IAM roles and actions](/docs/vpc?topic=vpc-about-par#par-access-management)
- [Quotas and service limits](/docs/vpc?topic=vpc-quotas#par-quotas)
- [Creating public address ranges](/docs/vpc?topic=vpc-par-creating&interface=ui)
- [Binding, unbinding, and moving public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui)
- [Deleting public address ranges](/docs/vpc?topic=vpc-par-deleting&interface=ui)
