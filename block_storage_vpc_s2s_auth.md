---

copyright:
  years: 2022, 2024
lastupdated: "2024-07-02"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service-to-service authorizations for {{site.data.keyword.block_storage_is_short}}
{: #block-s2s-auth}

You can use the {{site.data.keyword.iamshort}} (IAM) to create or remove an authorization that grants one service access to another service. For {{site.data.keyword.block_storage_is_short}}, you need to create service-to-service authorization for configuring customer-managed encryption, and backups.
{: shortdesc}

## Overview
{: #block-s2s-auth-overview}

In an authorization, the source service is the service that is granted access to the target service. The roles that you select define the level of access for the source service. The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign. Generally, a source service can be in the same account where the authorization is created or in another account. The target service is always in the account where the authorization is created. 

To be able to create an encrypted volume with customer-managed CRKs, you need to establish service-to-service authorization between the Block service and the Key Management Service of your choice.

If you want to create backup snapshots of your {{site.data.keyword.block_storage_is_short}} volumes, the Backup service needs to be authorized to work with {{site.data.keyword.block_storage_is_short}}, Snapshots for VPC, and Virtual Server for VPC services. For more information, see [Establishing service-to-service authorizations for the Backup service](/docs/vpc?topic=vpc-backup-s2s-auth).

For more information about authorizations, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).

## Creating service-to-service authorization for customer-managed encryption in the console
{: #block-s2s-auth-encryption-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. 
   - If the goal is to allow the use of a CRK from another account, select **Specific account** and enter the 32-character-long account ID. Then, click **Next**.
   - Otherwise, select **This account**. Then, click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **Block Storage for VPC**.
   1. Click **Next**.
1. For the target service, select **Hyper Protect Crypto Services** or **KeyProtect** from the list.
1. Select the role `Reader`.
1. Check the box to enable authorization to be delegated by source and dependent services.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for customer-managed encryption from the CLI
{: #block-s2s-auth-encryption-cli}
{: cli}

Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the Block service to interact with one or both Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The source service is `server-protect` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the Block service and {{site.data.keyword.keymanagementserviceshort}}.

```sh
$ ibmcloud iam authorization-policy-create server-protect kms Reader
Creating authorization policy under account a1234567 as test.user@ibm.com...
OK
```
{: screen}

To list the service authorizations that are already in place for the account, run the `ibmcloud iam authorization-policies` command. The following example shows that the Block service could be encrypted with a CRK that is stored in {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.

```sh
$ ibmcloud iam authorization-policies
Getting authorization policies under account a1234567 as test.user@ibm.com...
OK
                           
ID:                        1f722de4-c3e6-4765-b0d3-482ec77a04f8
Source service name:       server-protect
Source service instance:   All instances
Target service name:       kms
Target service instance:   51042d7f-f0df-4915-bd39-6a49957c9175
Roles:                     Reader
                           
ID:                        605cb9b9-ba0d-456b-8c22-180abee66c47
Source service name:       server-protect
Source service instance:   All instances
Target service name:       hs-crypto
Target service instance:   All instances
Roles:                     Reader 
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

## Creating service-to-service authorization for customer-managed encryption with the API
{: #block-s2s-auth-encryption-api}
{: api}

Make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization for the source volume's Block service to interact with a Key Management Service instance ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}).

* The following example shows how you can authorize the Block service `is.server-protect` (source) to interact with the {{site.data.keyword.keymanagementserviceshort}} service `kms` (target) with the _Reader_ role.

   ```sh
   curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
   '{
     "type": "access",
     "description": "Reader role for the Block service to interact with the KeyProtect service.",
     "subjects": [{"attributes": [{"name": "serviceName","value": "server-protect"}]}],
     "roles":[{"role_id": "crn:v1:bluemix:public:iam::::role:Reader"}],
     "resources":[{"attributes": [{"name": "serviceName","value": "kms"}]}]
     }'
     ```
     {: pre}

* To create an authorization policy for {{site.data.keyword.hscrypto}}, replace `kms` with `hs-crypto` in the previous example.

## Creating service-to-service authorization for customer-managed encryption with Terraform
{: #block-s2s-auth-encryption-terraform}
{: terraform}

Create an authorization policy between the Block service and the key management services by using the `ibm_iam_authorization_policy` resource argument in your `main.tf` file.

The following example creates an authorization policy between the Block service and {{site.data.keyword.keymanagementserviceshort}} when applied. 
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4keyprotect" {
  source_service_name  = "is"
  source_resource_type = "server-protect"
  target_service_name  = "kms"
  roles                = ["Reader"]
}
```
{: codeblock}

The following example creates an authorization policy between the Block service and {{site.data.keyword.hscrypto}} when applied.
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4HPCS" {
  source_service_name  = "is"
  source_resource_type = "server-protect"
  target_service_name  = "hs-crypto"
  roles                = ["Reader"]
}
```
{: codeblock}

For more information about the arguments and attributes, see the [Terraform documentation for authorization resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy){: external}.

## Next Steps
{: #block-s2s-next-steps}

- [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

