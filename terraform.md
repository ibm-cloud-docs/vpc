---

copyright:
  years: 2021, 2026
lastupdated: "2026-07-30"

keywords: Terraform, VPC, infrastructure as code, IaC, HashiCorp

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Terraform for VPC
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multitier cloud environments by following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the provisioning, update, and deletion of your VPC instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use Terraform without setting up or maintaining the CLI and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for VPC
{: #install-terraform}

Before you begin, make sure that you have the [required access](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) to create and work with VPC resources.

1. Follow the [Terraform on {{site.data.keyword.cloud}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to install the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in for Terraform. The plug-in abstracts the {{site.data.keyword.cloud}} APIs that are used to provision, update, or delete VPC service instances and resources.
2. Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to create a {{site.data.keyword.vpc_full}} and associated resources by using HashiCorp Configuration Language (HCL). For more information, see [Provisioning infrastructure with Terraform](/docs/solution-tutorials?topic=solution-tutorials-vpc-app-deploy#vpc-app-deploy-terraform) and [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.

   The following example creates a VPC named `myvpc`, a subnet in the `us-south-1` zone with a reserved IP, an SSH key, and a virtual server instance with boot volume encryption and two network interfaces. For other supported regions, see [Regions and endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).

   ```terraform
   resource "ibm_is_vpc" "my_vpc" {
     name = "myvpc"
   }

   resource "ibm_is_subnet" "my_subnet" {
     name                     = "mysubnet"
     vpc                      = ibm_is_vpc.my_vpc.id
     zone                     = "us-south-1"
     total_ipv4_address_count = 256
   }

   resource "ibm_is_subnet_reserved_ip" "my_reserved_ip" {
     subnet  = ibm_is_subnet.my_subnet.id
     name    = "myreservedip1"
     address = "${replace(ibm_is_subnet.my_subnet.ipv4_cidr_block, "0/24", "13")}"
   }

   resource "ibm_is_ssh_key" "my_sshkey" {
     name       = "myssh"
     public_key = "<your_ssh_public_key>"
   }

   resource "ibm_is_instance" "my_instance" {
     name    = "myinstance"
     image   = ibm_is_image.my_image.id
     profile = "bc1-2x8"

     boot_volume {
       encryption = "crn:v1:bluemix:public:kms:us-south:a/<account_id>:<kms_instance_id>:key:<key_id>"
     }

     primary_network_interface {
       name   = "eth0"
       subnet = ibm_is_subnet.my_subnet.id
       primary_ip {
         reserved_ip = ibm_is_subnet_reserved_ip.my_reserved_ip.reserved_ip
       }
     }

     network_interfaces {
       name   = "eth1"
       subnet = ibm_is_subnet.my_subnet.id
       primary_ip {
         name        = "myreservedip1"
         auto_delete = true
         address     = "${replace(ibm_is_subnet.my_subnet.ipv4_cidr_block, "0/24", "14")}"
       }
     }

     vpc  = ibm_is_vpc.my_vpc.id
     zone = "us-south-1"
     keys = [ibm_is_ssh_key.my_sshkey.id]

     //User can configure timeouts
     timeouts {
       create = "15m"
       update = "15m"
       delete = "15m"
     }
   }
   ```
   {: codeblock}

   For more IBM VPC Terraform examples, see the [Terraform Registry for IBM VPC infrastructure](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external}.

3. Initialize the Terraform CLI.

   ```sh
   terraform init
   ```
   {: pre}

4. Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that need to be run to create the {{site.data.keyword.vsi_is_short}} instance in your account.

   ```sh
   terraform plan
   ```
   {: pre}

5. Create the {{site.data.keyword.vsi_is_short}} instance and associated resources in {{site.data.keyword.cloud_notm}}.

   ```sh
   terraform apply
   ```
   {: pre}

6. From the [{{site.data.keyword.cloud_notm}} resource list](/resources){: external}, select the {{site.data.keyword.vpc_full}} instance that you created and note the instance ID.
7. Verify that the instance is running. For more information, see [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.vpc_short}} service instance with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks:

- [Managing virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances#viewing-virtual-server-instances-terraform)
- [Creating {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-creating-block-storage#creating-vol-terraform)
- [Managing {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-managing-block-storage#managing-block-storage-terraform)
- [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create#file-storage-create-terraform)
- [Managing file shares, accessor share bindings, and mount targets](/docs/vpc?topic=vpc-file-storage-managing#file-storage-manage-terraform)
