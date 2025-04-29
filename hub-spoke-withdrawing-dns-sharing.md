---

copyright:
  years: 2023, 2025
lastupdated: "2025-04-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Disconnecting DNS sharing to a hub VPC
{: #remove-sharing-hub}

To disconnect DNS sharing to a DNS hub VPC, the DNS-shared VPC authorized user can unshare the Virtual Private Endpoint (VPE) gateways to the DNS hub VPC.
{: shortdesc}

You can disconnect DNS sharing to a hub VPC with the console, CLI, API, or Terraform.

## Before you begin
{: #dns-sharing-before-you-begin}

Before you disconnect DNS sharing, review the following prerequisites:

* To disconnect DNS sharing from a DNS-shared VPC with its DNS resolver type set to `Delegated`, you must first update the resolver type to `System` or `Manual`. Then, delete the DNS resolution binding.
* To disconnect DNS sharing from a hub VPC, the hub VPC user must be assigned the `DNSBindingConnector` IAM role.

## Disconnecting DNS sharing to a hub VPC in the console
{: #vpe-dns-sharing-disconnect-from-hub-ui}
{: ui}

To disconnect DNS sharing to a DNS hub VPC, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to the account where the DNS-shared VPC resides.
1. Select the **Navigation Menu** ![menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure > Network > VPCs**.
1. Click the DNS-shared VPC whose DNS sharing you want to disconnect.
1. Scroll to the Optional DNS settings section, then expand the DNS resolver settings and click **Edit**.
1. In the Edit DNS resolver settings side panel, disable the DNS hub.
1. Select **System** as the DNS resolver type, then click **Save**.
1. Expand the DNS resolution binding section and click the **Delete** icon. Confirm deletion of the resolution binding to disconnect this shared VPC from the DNS hub VPC.

The hub VPC administrator can revoke the IAM service-to-service (s2s) authorization from the DNS-shared VPC. As a result, the DNS-shared VPC will not be able to create a DNS resolution binding to the hub VPC until the hub VPC reinitiates the IAM s2s authorization delegation to the DNS-shared VPC.
{: note}

## Disconnecting DNS sharing to a hub VPC from the CLI
{: #vpe-dns-sharing-disconnect-from-hub-cli}
{: cli}

To disconnect DNS sharing to a DNS hub VPC with the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts you for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre} 

1. To disconnect DNS sharing to a DNS hub VPC, enter the following command:

   ```sh
   ibmcloud is vpc-dns-resolution-binding-delete VPC (DNS_RESOLUTION_BINDING1 DNS_RESOLUTION_BINDING2 ...) [--output JSON] [-f, --force] [-q, --quiet]
   ```
   {: pre}

   Where:

   `VPC`
   :    Specifies the ID or name of the VPC.

   `DNS_RESOLUTION_BINDINGS1`
   :    Specifies the ID or name of the VPC DNS resolution binding.

   `DNS_RESOLUTION_BINDINGS2`
   :    Specifies the ID or name of the second VPC DNS resolution binding.

   `--output`
   :    Specifies the output format, only JSON is supported. One of: `JSON`.

   `--f, --force`
   :    Specifies to force the operation without confirmation.

   `-q, --quiet`
   :    Suppresses verbose output.

## Disconnecting DNS sharing to a hub VPC with the API
{: #hub-dns_shared-disconnect-from-hub-api}
{: api}

To disconnect DNS sharing to a DNS hub VPC with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API commands:

   ```sh
   export dns_shared_vpc_id=<dns_shared_vpc_id>
   export dns_binding_id=<dns_shared_vpc_dns_binding_id>
   export iam_policy_id=<cross_account_iam_policy_id>
    ```
    {: pre}

1. Run these commands in the following order:

   ```sh
   curl -sX PATCH "$vpc_api_endpoint/v1/vpcs/$dns_shared_vpc_id?version=$version&generation=2" -H "Authorization: Bearer $token" -d '{"dns": {"resolver": {"type": "system"}}}'
   ```
   {: codeblock}

   ```sh
   curl -sX DELETE "$vpc_api_endpoint/v1/vpcs/$dns_shared_vpc_id/dns_resolution_bindings/$dns_binding_id?version=$version&generation=2" -H "Authorization: Bearer ${iam_token}"
   ```
   {: codeblock}

   ```curl
   curl -sX DELETE "$iam_api_endpoint/v1/policies/$iam_policy_id" -H "Authorization: Bearer ${iam_token}"
   ```
   {: codeblock}

## Disconnecting DNS sharing to a DNS hub VPC with Terraform
{: #vpe-dns-sharing-disconnect-from-hub-terraform}
{: terraform}

You can use Terraform to disconnect DNS sharing to a DNS hub VPC.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a regional specific endpoint, which targets `us-south` by default. If you create your VPC in another region, make sure to target the appropriate region in the provider block of the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: codeblock}

To disconnect DNS sharing to a DNS hub VPC:

```terraform
resource "ibm_is_vpc" "example" {
  name = "example-vpc"
  dns {
		enable_hub = true
		resolver {
        type = "system"
		}
	}
}


terraform destroy -target=ibm_is_vpc_dns_resolution_binding.my_dns_resolution_binding
```
{: codeblock}

## Considerations after disconnecting DNS sharing
{: #considerations-after-disconnecting-dns-sharing}

When a hub VPC owner deletes a DNS resolution binding, it can result in a degraded health state for the DNS-shared VPC when the DNS-shared VPC delegates its DNS resolver to use the hub VPC's custom resolver. To restore the VPC's health from `Degraded` state, the DNS-shared VPC owner can update its DNS resolver type to `System`.

When a DNS-shared VPC owner deletes a DNS resolution binding, depending on whether the DNS-shared VPC is granted the [service-to-service IAM authorization policy](/docs/vpc?topic=vpc-vpe-dns-sharing-s2s-auth&interface=ui) on the hub VPC with the `DNSBindingConnector` role, the system either deletes the DNS resolution binding on the hub side (when authorized) or puts the hub's binding in `Faulted` state.

For more information, see [Monitoring a VPC for status and health](/docs/vpc?topic=vpc-vpc-states).
{: note}
