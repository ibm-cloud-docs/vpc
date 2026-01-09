---

copyright:
  years: 2026
lastupdated: "2026-01-09"

keywords: VPE, virtual private endpoint, endpoint gateway, resource binding

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating resource bindings
{: #vpe-create-resource-binding} 

You can create a resource binding to enable granular access control to specific Object Storage buckets from a specific VPC. Creating a resource binding is optional, but it lets you define security group and Context-Based Restriction (CBR) rules that precisely control which buckets are accessible through a VPE in a DNS-sharing VPC topology.
{: shortdesc}

A resource binding associates one or more Object Storage buckets with a VPE, ensuring (together with CBR rules on the buckets) that only traffic originating from the associated VPC can access those buckets. 

## Before you begin
{: #resource-bindings-before-you-begin}

Before you create a resource binding, review the following considerations:

* Ensure that you have an existing DNS-hub and DNS-shared topology configured for your VPCs.  
* For Cloud Object Storage, a VPE in a DNS-shared VPC must be created using **Per-resource binding** mode before any resource bindings can be added. VPEs can be created in either hub or DNS-shared VPCs.
* To allow access to Cloud Object Storage resources only from the DNS-shared VPC, you must create CBR rules that explicitly allow the DNS-shared VPC to access the Cloud Object Storage resources.  

   You must create the CBR rules before you create the resource binding and before you restrict access from the DNS hub VPC.
   {: note}

## Creating resource bindings in the console 
{: #creating-resource-binding-console}
{: ui}
 
To create resource bindings for the Cloud Object Storage service in the IBM Cloud console, follow these steps:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Network > Virtual private endpoint gateways**.  
1. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the VPE created for the **cloud-object-storage** service. 

   Alternatively, you can click the VPE name in the list and click the **Resource bindings** tab to create or manage existing resource bindings.  

   If DNS sharing is set to **Primary** mode for this VPE, the **Resource bindings** tab doesn't show as you can't create resource bindings in this mode. 
   {: note} 

1. Select **Create resource binding** from the menu.
1. Enter a **resource binding name prefix**.  
   - The system automatically appends a number to the prefix (for example, `vpe-gtw-resource-01`, `vpe-gtw-resource-02`).
   - You can add up to 10 buckets at one time.

1. In the **Select within same account** view, select the Object Storage instance and one or more Object Storage bucket locations, or click **Enter CRN** to add them manually.
1. Select an Object Storage bucket from the list.  Then, review your configuration and click **Save**.

After the resource binding is created, Cloud Object Storage traffic for the specified buckets will egress through the corresponding tenant-isolated VPE in the DNS-shared VPC.  

You can modify or remove bindings at any time to adjust access.

## Creating resource bindings from the CLI
{: #creating-resource-binding-cli}
{: cli} 

To create resource bindings for the Cloud Object Storage service from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts which account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre} 

To create a DNS resource binding, enter the following command: 

```bash
ibmcloud is endpoint-gateway-resource-binding-create ENDPOINT_GATEWAY --name NAME --target CRN
```
   
Where:

`ENDPOINT_GATEWAY`
:    Is the name of the VPE where you plan to bind this resource to. 

`-name NAME`
:    Is the name of this DNS resource binding.

`--target CRN`
:    Is the CRN of the Object Storage bucket.

### Command example
{: #resource-binding-command-example}

To create resource binding `my-resource-binding` to the endpoint gateway `cli-egw-1`:

```sh
ibmcloud is endpoint-gateway-resource-binding-create cli-egw-1 --name my-resource-binding --target crn:v1:bluemix:public:cloud-object-storage:global:a/efe5afc483594adaa8325e2b4d1290df:504bde7e-28c0-447f-8f75-484c55b5f222:bucket:test-cli 
```

## Creating resource bindings with the API 
{: #creating-resource-binding-api}
{: api}
 
To create resource bindings for the Cloud Object Storage with the API, follow these steps:
 
1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

1. To create a DNS resource binding:
   ```sh
   curl -X POST \
              "__ENDPOINT__/__BASE_PATH__/endpoint_gateways/$endpoint_gateway_id/resource_bindings?__QUERY__" \
              __AUTHORIZATION__ \
              -d '{
                    "name": "my-resource-binding",
                    "target": {
                      "crn":"crn:v1:bluemix:public:cloud-object-storage:global:a/aa2432b1fa4d4ace891e9b80fc104e34:1a0ec336-f391-4091-a6fb-5e084a4c56f4:bucket:bucket-27200-lwx4cfvcue"
                    }
                  }'
   ```
   {: codeblock}

## Creating resource bindings with Terraform
{: #creating-resource-binding-terraform}
{: terraform}

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

To create resource bindings for the Cloud Object Storage service using Terraform:

```terraform
resource "ibm_is_virtual_endpoint_gateway" "iam_vpe" {
  name = "${var.name}-vpe-iam"
  vpc  = ibm_is_vpc.testacc_vpc.id

  target {
    resource_type = var.vpe_target_type
    crn           = var.vpe_target_crn
  }

  dns_resolution_binding_mode = "disabled"
}

resource "ibm_is_virtual_endpoint_gateway_resource_binding" "iam_vpe_rb" {
  name                = "${var.name}-vpe-rb"
  endpoint_gateway_id = ibm_is_virtual_endpoint_gateway.iam_vpe.id

  target {
    crn = var.vpe_rb_target
  }
}

data "ibm_is_virtual_endpoint_gateway" "iam_vpe" {
  name = ibm_is_virtual_endpoint_gateway.iam_vpe.name
}

data "ibm_is_virtual_endpoint_gateways" "all" {}

data "ibm_is_virtual_endpoint_gateway_resource_bindings" "iam_vpe_rbs" {
  endpoint_gateway_id = ibm_is_virtual_endpoint_gateway.iam_vpe.id
}

data "ibm_is_virtual_endpoint_gateway_resource_binding" "iam_vpe_rb" {
  endpoint_gateway_id                     = ibm_is_virtual_endpoint_gateway.iam_vpe.id
  endpoint_gateway_resource_binding_id    = ibm_is_virtual_endpoint_gateway_resource_binding.iam_vpe_rb.endpoint_gateway_resource_binding_id
}
```
{: codeblock}
