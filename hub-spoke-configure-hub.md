---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Enabling a VPC as a DNS hub
{: #vpe-dns-sharing-configure-hub}

You can enable a VPC as the DNS hub so that other VPCs can create DNS resolution bindings with this VPC, as well as centralize the DNS resolution for VPE gateways with DNS resolution bindings enabled.

## Before you begin
{: #hub-prerequisites}

* Before you enable a VPC as a DNS hub, review [Planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations) and [Known issues and limitations](/docs/vpc?topic=vpc-vpe-dns-sharing-limitations).
* Ensure that the VPC you choose to enable as a hub has its DNS resolver type set to System (the default) or Manual. For more information, see [Setting the DNS resolver type](/docs/vpc?topic=vpc-configure-dns-resolver&interface=ui).

You can enable a VPC as a DNS hub with the UI, CLI, API, or Terraform.

## Enabling a VPC as a DNS hub in the UI
{: #vpe-dns-sharing-ui}
{: ui}

To enable a VPC as a DNS hub in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** ![menu icon](../../icons/icon_hamburger.svg), then click  **Infrastructure > Network > VPCs**.
1. Click the link of the Virtual Private Cloud that you want to designate as the DNS hub VPC. The Overview page displays.
1. Scroll to the Optional DNS settings section at the end of the page, then expand the DNS hub section and enable the DNS hub switch.

   The Virtual private clouds table now show a `DNS-hub` tag next to the name of the designated hub VPC.

## Enabling a VPC as a DNS hub from the CLI
{: #vpe-dns-sharing-cli}
{: cli}

To enable a VPC as a DNS hub with the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system asks which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_VPC_DNS_SHARING=true
   ```
   {: pre}

1. Enable a DNS hub for the VPC and specify the resolver type:

   ```bash
   ibmcloud is vpc-create my-cli-vpc [--dns-enable-hub true] [--dns-resolver-type manual] [--dns-resolver-manual-servers  MANUAL_SERVERS | @MANUAL_SERVERS]
   ```
   {: pre}

   Where:

   `--dns-enable-hub`
   :    Indicates whether this VPC is enabled as a DNS name resolution hub. One of: `false`, `true`.

   `--dns-resolver-type`
   :    Indicates the type of the DNS resolver to use. One of: `system` (default), `manual`.

   `--dns-resolver-manual-servers`
   :    Valid when the resolver type is `manual`. Configure manual DNS server addresses for the VPC. `MANUAL_SERVERS|@MANUAL_SERVERS` manual servers in JSON or a JSON file.

## Enabling a VPC as a DNS hub with the API
{: #vpe-dns-sharing-api}
{: api}

To enable a VPC as a DNS hub with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. To enable an existing VPC as a DNS hub, enter the following command:

   ```curl
   curl -X PATCH -sH "Authorization:${iam_token}" "${vpc_api_endpoint}/v1/vpcs/${vpc_id}?version=${api_version}&generation=2" -d '{"dns": {"enable_hub": true}}'
   ```
   {: codeblock}

1. To create a hub VPC with default DNS servers, enter the following command:

   ```curl
   curl -X POST -sH "Authorization:${iam_token}" "${vpc_api_endpoint}/v1/vpcs?version=$api_version&generation=2" -d '{"name": "test-hub", "dns": {"enable_hub": true} ,"resource_group": {"id": "'$ResourceGroupId'"}}'
   ```
   {: codeblock}

For more information, reference the [Create a VPC](/apidocs/vpc-beta#create-vpc) API.

Sample output:

```sh

  "classic_access": false,
  "created_at": "2019-01-27T14:39:40Z",
  "crn": "crn:[...]",
  "default_network_acl": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/network_acls/dcfd9b64-9119-48ea-8697-23f9f5fb6bd6",
    "id": "dcfd9b64-9119-48ea-8697-23f9f5fb6bd6",
    "name": "my-network-acl"
  },
  "default_routing_table": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/2a842e76-1302-45ad-8b1f-cc7eb0f3868b/routing_tables/f4e65793-1b51-4548-b0c5-769e18d64775",
    "id": "f4e65793-1b51-4548-b0c5-769e18d64775",
    "name": "my-routing-table",
    "resource_type": "routing_table"
  },
  "default_security_group": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/security_groups/95770f87-81f3-4164-8f6d-9f54a3f3a459",
    "id": "95770f87-81f3-4164-8f6d-9f54a3f3a459",
    "name": "my-security-group"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/f64efe74-a5a2-45c7-b37d-5071d2dd6339",
  "id": "2a842e76-1302-45ad-8b1f-cc7eb0f3868b",
  "name": "my-vpc-2",
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "resource_type": "vpc",
  "status": "available"
}
```
{: screen}

## Enabling a VPC as a DNS hub with Terraform
{: #vpe-dns-sharing-terraform}
{: terraform}

You can use Terraform to enable a VPC as a DNS hub.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a regional specific endpoint, which targets `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block of the `provider.tf` file.

The following example details targeting a region other than the default `us-south`:

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: codeblock}

To enable a VPC as a DNS hub:

```terraform
resource "ibm_is_vpc" "example" {
  name = "example-vpc"
  dns {
		enable_hub = true
		resolver {
			manual_servers {
				type = "manual"
				address = "192.168.3.4"
			}
		}
	}
}
```
{: codeblock}

## Next steps
{: #vpe-dns-sharing-next-step-dns-resolution-binding}

* [Create VPE gateways](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for multi-tenant services in the hub VPC and enable DNS sharing for each endpoint gateway.
* [Create a DNS resolution binding](/docs/vpc?topic=vpc-vpe-dns-sharing-resolution-bindings) to allow VPE gateway DNS records between a DNS-shared VPC and the DNS hub VPC.
* If you have different accounts for the hub VPC and a DNS-shared VPC, the hub VPC administrator must [create a service-to-service (s2s) authorization policy](/docs/vpc?topic=vpc-vpe-dns-sharing-s2s-auth) to give Read access to the DNS-shared VPC account.
* Ensure that DNS resolution binding is disabled on redundant or conflicting VPEs on the DNS-shared VPCs. By default, the DNS resolution binding switch is enabled on VPEs.
