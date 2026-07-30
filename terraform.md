---

copyright:
  years: 2021, 2026
lastupdated: "2026-07-30"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Terraform for VPC
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the provisioning, update, and deletion of your VPC instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for VPC
{: #install-terraform}

Before you begin, make sure that you have the [required access](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls) to create and work with VPC resources.

1. Follow the [Terraform on {{site.data.keyword.cloud}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to install the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in for Terraform. The plug-in abstracts the {{site.data.keyword.cloud}} APIs that are used to provision, update, or delete VPC service instances and resources.
2. Create a Terraform configuration file that is named `main.tf`. In this file, you add the configuration to create an {{site.data.keyword.vpc_full}} service instance and to assign a user an access policy in Identity and Access Management (IAM) for that instance by using HashiCorp Configuration Language (HCL). For more information, see [Provisioning infrastructure with Terraform](/docs/solution-tutorials?topic=solution-tutorials-vpc-app-deploy#vpc-app-deploy-terraform) and [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.

   The {{site.data.keyword.vpc_full}} instance in the following example is named `myvpc` and is created with the tiered pricing plan in the `us-south` region. For other supported regions, see [Regions and endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).

   

   ```json
   resource "ibm_is_vpc" "my_vpc" {
     name = "myvpc"
   }

   resource "ibm_is_subnet" "my_subnet" {
     name            = "mysubnet"
     vpc             = ibm_is_vpc.my_vpc.id
     zone            = "us-south-1"
     total_ipv4_address_count = 256
   }

   resource "ibm_is_ssh_key" "my_sshkey" {
     name       = "myssh"
     public_key = "ssh-rsa
    AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
   }

   resource "ibm_is_instance" "my_instance" {
     name    = "myinstance"
     image   = "7eb4e35b-4257-56f8-d7da-326d85452591"
     profile = "bc1-2x8"

     boot_volume {
       encryption = "crn:v1:bluemix:public:kms:us-south:a/dffc98a0f1f0f95f6613b3b752286b87:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
     }

     primary_network_interface {
       subnet = ibm_is_subnet.my_subnet.id
       primary_ipv4_address = "10.240.0.6"
     }

     network_interfaces {
       name   = "eth1"
       subnet = ibm_is_subnet.my_subnet.id
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

   For more IBM VPC Terraform examples, see the [Terraform Registry for IBM VPC infrastructure](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external}

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

5. Create the {{site.data.keyword.vsi_is_short}} instance and IAM access policy in {{site.data.keyword.cloud_notm}}.

   ```sh
   terraform apply
   ```
   {: pre}

6. From the [{{site.data.keyword.cloud_notm}} resource list](/resources){: external}, select the {{site.data.keyword.vpc_full}} instance that you created and note the instance ID.
7. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/iam?topic=iam-assign-access-resources&interface=ui#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.vpc_short}} service instance with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks:
