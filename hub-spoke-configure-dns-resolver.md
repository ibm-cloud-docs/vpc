---

copyright:
  years: 2023, 2025
lastupdated: "2025-06-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting the DNS resolver type
{: #configure-dns-resolver}

After you create a DNS-shared VPC by creating a resolution binding to the DNS hub VPC, you must set the shared VPC's DNS resolver type to Delegated or Manual so that the VPC can use the custom resolver in the DNS hub VPC.
{: shortdesc}

## About resolver types
{: #about-resolver-types}

There are three DNS resolver types:

* **System** is the IBM default setting for DNS servers. With this setting, DNS server addresses are provided by DNS Services, and the DNS hub VPC is set to this resolver type.
* Use **Delegated** for DNS-shared VPCs so that these VPCs can use the custom resolver in the hub VPC. DNS server addresses are provided by the resolver for the specified DNS-shared VPC. This option can be set only on a VPC where the DNS hub is disabled.
* Use **Manual** if you want a DNS-shared VPC to use a DNS resolver other than the hub VPC's custom resolver. This option is for DNS server addresses that are manually specified, replacing any existing servers. You can use this option to configure any DNS server (public or on-premises).

## Before you begin
{: #dns-resolver-prerequisites}

Before you set the DNS resolver type, review [Planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations) and [Known issues and limitations](/docs/vpc?topic=vpc-vpe-dns-sharing-limitations).
{: important}

You can set the DNS resolver type with the console, CLI, API, or Terraform.

## Setting the DNS resolver type in the console
{: #vpe-dns-sharing-resolver-ui}
{: ui}

To set the DNS resolver type on a DNS-shared VPC, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPCs**.
1. Click the DNS-shared VPC whose DNS resolver type you want to set.
1. Scroll to the Optional DNS settings section, then expand the DNS resolver settings and click **Edit**.
1. In the Edit DNS resolver settings side panel, disable the DNS hub.
1. Select **Delegate** as the DNS resolver type, then click **Save**.

## Setting the DNS resolver type from the CLI
{: #dns-resolver-cli}
{: cli}

To set the DNS resolver type for a DNS-shared VPC with the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

1. Log in to your account with the CLI. After you enter the password, the system prompts you for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Disable the hub VPC and specify the DNS resolver type for your DNS-shared VPC:

   ```bash
   ibmcloud is vpc-create my-cli-vpc [--dns-enable-hub false] [--dns-resolver-type delegate] [--dns-resolver-manual-servers  MANUAL_SERVERS | @MANUAL_SERVERS]
   ```

   Where:

   `--dns-enable-hub`
   :    Specifies whether to enable a hub for the VPC. The DNS hub must be disabled before using the `delegate` option.

   `--dns-resolver-type`
   :    Specifies the resolver type for the VPC. Values are `system` (default), `delegate`, and `manual`.

   `--dns-resolver-manual-servers`
   :    Configures manual DNS server addresses for the VPC. Valid when the resolver type is `manual`.

## CLI examples
{: #cli-examples-resolver}
{: cli}

To configure `manual` servers:

```sh
ibmcloud is vpcu my-vpc --dns-resolver-manual-servers '[
    {
        "address": "190.20.3.2"
    },
    {
        "address": "190.20.3.3"
    }
]'
```

To configure a `delegated` resolver type:

```sh
ibmcloud is vpcu r006-55007c36-fd61-48db-ae22-710b0277e78e --dns-resolver-type delegated --delegate-to-vpc r006-4691ccef-498c-4d8e-9064-bcf13972b1aa
```

To configure a `system` resolver type:

```sh
ibmcloud is vpcu my-vpc --dns-resolver-type system
```

## Setting the DNS resolver type with the API
{: #vpe-dns-sharing-resolver-api}
{: api}

To set the DNS resolver type with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. To set a DNS-shared VPC to use the hub VPC's DNS using the `delegated` resolver type:

   ```curl
   curl -X PATCH -sH "Authorization:${iam_token}" "${vpc_api_endpoint}/v1/vpcs/${vpc_id}?version=${api_version}&generation=2" -d '
   {
     "dns": {
       "resolver": {
         "type":"delegated",
         "vpc": {
           "id": "${hub_vpc_id}"
         }
       }
     }
   }'
   ```
   {: codeblock}

1. To set a VPC to use `manual` DNS server addresses:

   ```curl
   curl -X PATCH -sH "Authorization:${iam_token}" "${vpc_api_endpoint}/v1/vpcs/${vpc_id}?version=${api_version}&generation=2" -d '
   {
     "dns": {
       "resolver": {
         "type": "manual",
         "manual_servers": [
           {
             "address": "10.240.0.10",
             "zone_affinity": {
               "name": "us-south-1"
             }
           },
            {
             "address": "10.240.64.10",
             "zone_affinity": {
               "name": "us-south-2"
             }
           },
           {
             "address": "10.240.128.10",
             "zone_affinity": {
               "name": "us-south-3"
             }
           }
         ]
       }
     }
   }'
   ```
   {: codeblock}

1. To restore a VPC to the default `system` configuration:

   ```curl
   curl -X PATCH -sH "Authorization:${iam_token}" "${vpc_api_endpoint}/v1/vpcs/${vpc_id}?version=${api_version}&generation=2" -d '
   {
     "dns": {
       "resolver": {
         "type":"system",
         "vpc": null
       }
     }
   }'
   ```
   {: codeblock}

## Setting the DNS resolver type with Terraform
{: #vpe-dns-sharing-resolver-terraform}
{: terraform}

You can use Terraform to set the DNS resolver type.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started){: external}.
{: requirement}

VPC infrastructure services use a regional specific endpoint, which targets `us-south` by default. If you create your VPC in another region, make sure to target the appropriate region in the provider block of the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: codeblock}

To enable the DNS hub with `manual` resolver type:

```terraform
resource ibm_is_vpc my-vpc {
	name = "my-vpc"
	dns {
		enable_hub = true
		resolver {
			manual_servers     {
				address ="192.168.0.4"
				zone_affinity= "au-syd-1"
			}
			manual_servers{
				address =  "192.168.64.4"
				zone_affinity = "au-syd-2"
			}
			manual_servers{
				address= "192.168.128.4"
				zone_affinity ="au-syd-3"
			}
		}
	}
}
```
{: codeblock}

To patch the DNS hub to the `delegate` resolver type:

```terraform
resource ibm_is_vpc my-vpc {
	name = "my-vpc"
	dns {
		enable_hub = false

	    resolver {
			type = "delegated"
			vpc_id = ibm_is_vpc.hub_true.id
		}
	}
}
```
{: codeblock}

To patch a VPC to the `system` resolver type:

```terraform
resource ibm_is_vpc my-vpc {
	name = "my-vpc"
	dns {
		enable_hub = false

		resolver {
			type = "system"
			vpc_id = "null"
	    }
	}
}
```
{: codeblock}

## Next step
{: #next-step-create-vpe-gateways}

The DNS-shared VPC user [creates VPE gateways](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for single-tenant services in the DNS-shared VPC.
