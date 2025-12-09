---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating and deleting a DNS resolution binding
{: #vpe-dns-sharing-resolution-bindings}

Create a DNS resolution binding between a VPC and a DNS hub VPC so that the DNS hub VPC can resolve the DNS hostnames of Virtual Private Endpoint (VPE) gateways residing on the DNS-shared VPC. Keep in mind that VPE gateways bound to the DNS-shared VPCs must also be enabled for resolution binding (the default setting).
{: shortdesc}

## Before you begin
{: #vpe-before-you-begin-prerequisites}

Before you create a DNS resolution binding, review the following prerequisites:

* Review [DNS sharing planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations).
* Ensure a [VPC enabled as a DNS hub](/docs/vpc?topic=vpc-vpe-dns-sharing-configure-hub) already exists.
* The hub VPC administrator must create an IAM service-to-service authorization policy that allows this DNS-shared VPC to have `DNSBindingConnector` permission on the hub VPC. This `DNSBindingConnector` role on the hub VPC is required regardless of whether the DNS-shared VPC is in the same or a different account. For more information, see [Establishing service-to-service authorization](/docs/vpc?topic=vpc-vpe-dns-sharing-s2s-auth&interface=api).

You can create a DNS resolution binding with the console, CLI, API, or Terraform.

## Creating a DNS resolution binding in the console
{: #create-dns-resolution-binding-ui}
{: ui}

To create a DNS resolution binding in the IBM Cloud console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPCs**.
1. Click the Virtual Private Cloud in which you want to share DNS records with the DNS hub VPC.
1. Scroll to the Optional DNS settings section, then expand the DNS resolution binding section and click **Create**.
1. In the Create side panel, enter a name for the resolution binding.
1. Enter the cloud resource name (CRN) of the DNS hub VPC, or use the menu to select a hub VPC within the same account.
1. Click **Create** to create the DNS resolution binding.

   The VPC with the DNS resolution binding now shows a `DNS-Shared` tag next to its name.

Next, [Configure a DNS custom resolver](/docs/dns-svcs?topic=dns-svcs-ui-create-cr&interface=ui) on the DNS hub VPC to be responsible for resolving DNS queries from hub and DNS-shared VPCs, as well as those from on-prem networks.

## Deleting a DNS resolution binding in the console
{: #delete-dns-resolution-binding-ui}
{: ui}

To delete a DNS resolution binding in the IBM Cloud console, follow these steps:

To delete a resolution binding, you need the appropriate DNS Services IAM role (Manager or Administrator). Also, keep in mind that all DNS resolution bindings must be deleted before you can delete a custom resolver.
{: note}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPCs**.
1. Click the Virtual Private Cloud in which you want to delete the DNS resolution binding.
1. Scroll to the Optional DNS settings section, then expand the DNS resolution binding section.
1. Click **Delete**. If there is more than one binding, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg), then click **Delete**.

Kindly note that to delete , you need appropriate IAM permissions for DNS Services : Manager, Administrator. 
Also, you need to delete DNS resolution bindings before deleting the Custom Resolver. 
Refer here: https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-cr-delete&interface=ui#ui-cr-delete

## Creating a DNS resolution binding from the CLI
{: #create-dns-resolution-binding-cli}
{: cli}

To create a DNS resolution binding with the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

To create a DNS resolution binding, enter the following command:

   ```bash
   ibmcloud is vpc-dns-resolution-binding-create VPC --target-vpc TARGET_VPC [--name NAME] [--output JSON] [-q, --quiet]
   ```
   

   Where:

   `VPC`
   :    Specifies the ID or name of the VPC.

   `--target-vpc TARGET_VPC`
   :    Specifies the ID or name of another VPC to bind this VPC to, for DNS resolution.

   `-name NAME`
   :    Specifies the name for this DNS resolution binding.

   `--output JSON`
   :    Specifies the output format, only JSON is supported. One of: `JSON`.

    `--q, --quiet`
   :    Suppresses verbose output.
   

### Command examples
{: #command-examples-resolution-binding1}
{: cli}

* `ibmcloud is vpc-dns-resolution-binding-create my-vpc --name my-dns-res-binding --target-vpc my-dns-binding-vpc --output JSON`
* `ibmcloud is vpc-dns-resolution-binding-create 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name my-dns-res-binding --target-vpc my-dns-binding-vpc --output JSON` 



## Deleting a DNS resolution binding from the CLI
{: #delete-dns-resolution-binding-cli}
{: cli}

To delete one or more resolution bindings with the CLI, follow these steps:

```bash
ibmcloud is vpc-dns-resolution-binding-delete VPC (DNS_RESOLUTION_BINDING1 DNS_RESOLUTION_BINDING2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```


Where:

`VPC`
:    Specifies the ID or name of the VPC.

`DNS_RESOLUTION_BINDING1`
:    Specifies the ID or name of the DNS resolution binding.

`DNS_RESOLUTION_BINDING2`
:    Specifies the ID or name of the DNS resolution binding.

`--output JSON`
:    Specifies the output format, only JSON is supported. One of: `JSON`.

`--f, --force`
:    Forces the operation without confirmation.

`--q, --quiet`
:    Suppresses verbose output.


### Command example
{: #command-examples-delete-binding2}
{: cli}

`ibmcloud is vpc-dns-resolution-binding-delete r006-e5b9726b-c975-46bd-b713-c8aea55d51d8 r006-75ccea7b-c705-4b50-934d-2152f9eab4ec --name my-dns-resolution --output JSON`



##  Creating a DNS resolution binding with the API
{: #create-dns-resolution-binding-api}
{: api}

To create a resolution binding with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API commands:

   ```sh
   export hub_vpc_id=<hub_vpc_id>
   export spoke_vpc_id=<spoke_vpc_id>
    ```
    {: pre}

1. To create a DNS resolution binding, run the following command:

   ```sh
   curl -sX POST "$vpc_api_endpoint/v1/vpcs/$spoke_vpc_id/dns_resolution_bindings?version=$version&generation=2" -H "Authorization: Bearer ${iam_token}" -d '{"vpc": {"id": "'$hub_vpc_id'"}}'
   ```
   {: codeblock}

Sample output:

```sh
{
  "created_at": "2023-08-22T23:23:37Z",
  "endpoint_gateways": [],
  "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-955c4b01-006f-4630-8a6a-f57e30354974/dns_resolution_bindings/r006-f95df674-74af-47ea-91d9-0aec39972ae7",
  "id": "r006-f95df674-74af-47ea-91d9-0aec39972ae7",
  "lifecycle_state": "pending",
  "name": "my-binding",
  "resource_type": "vpc_dns_resolution_binding",
  "vpc": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/vpcs/r006-c6c9d533-a43b-488c-a4f5-b532f4ddd3e0",
    "id": "r006-c6c9d533-a43b-488c-a4f5-b532f4ddd3e0",
    "name": "hub-vpc",
    "remote": {},
    "resource_type": "vpc"
  }
}
```
{: screen}

##  Deleting a DNS resolution binding with the API
{: #delete-dns-resolution-binding-api}
{: api}

To delete a DNS resolution binding with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API commands:

   ```sh
   export dns_binding_id=<spoke_vpc_dns_binding_id>
    ```
    {: pre}

1. To delete a DNS resolution binding, run the following command:

   ```sh
   curl -sX DELETE "$vpc_api_endpoint/v1/vpcs/$spoke_vpc_id/dns_resolution_bindings/$dns_binding_id?version=$version&generation=2" -H "Authorization: Bearer ${iam_token}"
   ```
   {: codeblock}

##  Creating a DNS resolution binding with Terraform
{: #create-dns-resolution-binding-terraform}
{: terraform}

You can use Terraform to create a DNS resolution binding.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started){: external}.
{: requirement}

VPC infrastructure services use a regional specific endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: codeblock}

To create a DNS resolution binding:

```terraform
resource "ibm_is_vpc_dns_resolution_binding" "my_dns_resolution_binding" {
	name = "example-dns"
	vpc_id = "vpc_id"
	vpc {
		id = "<target_vpc_id">
	}
}
resource "ibm_is_vpc_dns_resolution_binding" "my_dns_resolution_binding" {
	name = "example-dns"
	vpc_id = "vpc_id"
	vpc {
		crn = "<target_vpc_crn">
	}
}
resource "ibm_is_vpc_dns_resolution_binding" "my_dns_resolution_binding" {
	name = "example-dns"
	vpc_id = "vpc_id"
	vpc {
		href = "<target_vpc_href">
	}
}
```
{: codeblock}

##  Deleting a DNS resolution binding with Terraform
{: #delete-dns-resolution-binding-terraform}
{: terraform}

To delete a DNS resolution binding with Terraform, see the following example:

```terraform
terraform destroy --target ibm_is_vpc_dns_resolution_binding.my_dns_resolution_binding
```
{: codeblock}
