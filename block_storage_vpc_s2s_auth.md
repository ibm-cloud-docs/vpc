---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-02"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service-to-service authorizations for {{site.data.keyword.block_storage_is_short}}
{: #block-s2s-auth}

You can use the {{site.data.keyword.iamshort}} (IAM) to create or remove an authorization that grants one service access to another service. For {{site.data.keyword.block_storage_is_short}}, you need to create service-to-service authorization for configuring customer-managed encryption, and backups. You also need to specify [user roles](/docs/account?topic=account-iam-service-roles-actions#is.volume-roles).
{: shortdesc}

## Overview
{: #block-s2s-auth-overview}

In an authorization, the source service is the service that is granted access to the target service. The roles that you select define the level of access for the source service. The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign. Generally, a source service can be in the same account where the authorization is created or in another account. The authorization must be created in the account that owns the target service.

To be able to create an encrypted volume with customer-managed CRKs, you need to establish service-to-service authorization between the Block service and the Key Management Service of your choice.

If you want to create backup snapshots of your {{site.data.keyword.block_storage_is_short}} volumes, the Backup service needs to be authorized to work with {{site.data.keyword.block_storage_is_short}}, Snapshots for VPC, and Virtual Server for VPC services. For more information, see [Establishing service-to-service authorizations for the Backup service](/docs/vpc?topic=vpc-backup-s2s-auth).

For more information about authorizations, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).

## Creating service-to-service authorization for customer-managed encryption in the console
{: #block-s2s-auth-encryption-ui}
{: ui}

When the authorization is needed for cross-account encryption, the authorization must be created in the target account that owns the encryption key.
{: important}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**.
1. From the side panel, click **Manage access**, and select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. The source account is where the block storage volumes are to be created.
   - If the goal is to allow another account to use a CRK of the target account, select **Specific account** and enter the 32-character-long account ID. Then, click **Next**.
   - Otherwise, select **This account**. Then, click **Next**.
1. For the source service, select **Cloud Block Storage** from the list. Click **Next**.
   1. Select the scope by clicking **All resources**.
   1. Click **Next**.
1. For the target service, select **Hyper Protect Crypto Services** or **KeyProtect** from the list.
1. Select the role `Reader`.
1. Check the box to enable authorization to be delegated by source and dependent services.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for cross-account restore in the console
{: #block-s2s-auth-xaccountrestore-ui}
{: ui}

The following steps authorize the Block Storage service of one account to use a snapshot that is created by another account to restore volumes. The steps need to be performed by the account that owns the snapshot that is to be shared. The receiving account must ensure that their admin user has the `SnapshotRemoteAccountRestorer` role in IAM before they start a volume restoration with the CRN of the shared snapshot.

1. On the **Manage authorizations** page, click **Create**. 
1. On the **Grant a service authorization** page, select the source account. 
   1. Because the goal is to allow the use of a snapshot from another account, select a **Specific account**.
   1. Enter the 32-character account ID.
   1. Click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Select **Resource type**, and **Block Storage for VPC**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**.
   1. In the next field, select **Block Storage Snapshots for VPC**.
   1. If you want to restrict the authorization to a specific snapshot, click **Add a condition**. 
      1. Click **Select an attribute** and select **Snapshot ID**. 
      1. Enter the ID of the snapshot. Click **Next**
1. Select the role `Snapshot Remote Account Restorer`.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for customer-managed encryption from the CLI
{: #block-s2s-auth-encryption-cli}
{: cli}

Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the Block service to interact with one or both Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The source service is `server-protect` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the Block service and {{site.data.keyword.keymanagementserviceshort}}.

```sh
ibmcloud iam authorization-policy-create server-protect kms Reader
```
{: pre}

```sh
Creating authorization policy under account a1234567 as test.user@ibm.com...
OK
```
{: screen}

To list the service authorizations that are already in place for the account, run the `ibmcloud iam authorization-policies` command. The following example shows that the Block service can be encrypted with a CRK that is stored in {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.

```sh
ibmcloud iam authorization-policies
```
{: pre}

```sh
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

## Creating service-to-service authorization for cross-account encryption from the CLI
{: #block-s2s-xaccount-encryption-cli}
{: cli}

When the authorization is needed for cross-account encryption, the authorization must be created in the target account that owns the encryption key. Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the Block service of the source account to interact with one or both Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}) of the target account. The source service is `server-protect` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the Block service and {{site.data.keyword.keymanagementserviceshort}}.

1. Create a JSON file with the following information for the authorization policies in your local Documents folder.
   ```json
   '{
      "description":"Reader and Delegator role for KeyProtect service instance",
      "resources": [
         {"attributes":[
            {"name":"Account-A-CRK","operator":"stringEquals","value":"<CRK-Account-A-ID>"},
            {"name":"Hyper-Protect-Crypto-Services","operator":"stringEquals","value":"kms"}]}],
      "roles": [
         {"role_id":"crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},
         {"role_id":"crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
      "subjects": [
         {"attributes": [
            {"name":"Cloud Block Storage","value":"server-protect"},
            {"name":"Account-B","value":"<Account-B-ID>"}]}],
      "type":"authorization",
   }'
   ```
   {: codeblock}

1. Then, use the JSON files to run the following CLI command.
   ```sh
   ibmcloud iam authorization-policy-create --file ~/Documents/policy.json
   ```
   {: pre}
   
   The cross-account authorization is one-way and specific to key and service. When Account A authorizes their key to be used by Account B's file service, Account B can use Account A's CRK to encrypt Account B's shares. However, Account A cannot use Account B's root keys to encrypt Account A's shares.
   {: note}

## Creating service-to-service authorization for cross-account restore from the CLI
{: #block-s2s-auth-xaccountrestore-cli}
{: cli}

WHen the authorization is needed for cross-account data restoration, the authorization must be created in the target account that owns the snapshot. Run the `ibmcloud iam authorization-policy-create` command to authorize the Block Storage service of the source account to use a snapshot that was created by the target account to restore volumes. The receiving account must ensure that their admin user has the `SnapshotRemoteAccountRestorer` role in IAM before they start a volume restoration with the CRN of the shared snapshot.

1. Create a JSON file with the following information for the authorization policies in your local Documents folder.
   ```json
   '{
        "type":"authorization",
        "description":"Providing the Block Storage service access to restore from a Snapshot of another account ",
        "subjects": [{"attributes": [
                {"name":"Block Storage for VPC","value":"server-protect"},
                {"name":"Remote-restorer-account","value":"<Remote-Restorer-Account-ID>"}]}],
        "roles": [{"role_id":"crn:v1:bluemix:public:iam::::role:SnapshotRemoteAccountRestorer"}],
        "resources": [{"attributes": [
                {"name":"Snaphot-owner-account","operator":"stringEquals","value":"<Snapshot-Owner-Account-ID>"},
                {"name":"snapshotId","operator":"stringEquals","value":"*"}]}],
   }'
   ```
   {: codeblock}

1. Then, use the JSON files to run the following CLI command.
   ```sh
   ibmcloud iam authorization-policy-create --file ~/Documents/policy.json
   ```
   {: pre}

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
     "type":"access",
     "description":"Reader role for the Block service to interact with the KeyProtect service.",
     "subjects": [{"attributes":[{"name":"Cloud Block Storage","value":"server-protect"}]}],
     "roles":[{"role_id":"crn:v1:bluemix:public:iam::::role:Reader"}],
     "resources":[{"attributes": [{"name":"KeyProtect","value":"kms"}]}]
     }'
   ```
   {: pre}

* To create an authorization policy for {{site.data.keyword.hscrypto}}, replace `kms` with `hs-crypto` in the previous example.

## Creating service-to-service authorization for cross-account encryption with the API
{: #block-s2s-xaccount-encryption-api}
{: api}

Make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization for the source account's Block Storage service to interact with a Key Management Service instance ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}) of the target account. The request must be made from the account that owns the customer root key in their KMS.

* The following example shows how you can authorize the Block service `is.server-protect` of one account (source) to interact with the {{site.data.keyword.hscrypto}} service `hs-crypto` of another account (target) with the _Reader_ and _Authorization Delegator_ roles.

   ```sh
   curl -X POST "https://iam.cloud.ibm.com/v1/policies" \
     -H "Authorization: <Auth Token>" \
     -H 'Content-Type: application/json' \
     -d '{
        "type":"authorization",
        "description":"Reader and Delegator access to KeyProtect service instance",
        "subjects": [
          {"attributes": 
            {"name":"Cloud Block Storage","value":"server-protect"},
            {"name":"Account-B","value":"<Account-B-ID>"}]}],
        "roles": [{"role_id":"crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},{"role_id":"crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
        "resources": [
          {"attributes":[
            {"name":"Account-A-CRK","operator":"stringEquals","value":"<CRK-Account-A-ID>"},
            {"name":"Hyper-Protect-Crypto-Services","operator":"stringEquals","value":"kms"}]}],
        }'
    ```
    {: codeblock}

* To create an authorization policy for {{site.data.keyword.hscrypto}}, replace `kms` with `hs-crypto` in the previous example.

The cross-account authorization is one-way and specific to key and service. When Account A authorizes their key to be used by Account B's file service, Account B can use Account A's CRK to encrypt Account B's shares. However, Account A cannot use Account B's root keys to encrypt Account A's shares.
{: note}

## Creating service-to-service authorization for cross-account restore with the API
{: #block-s2s-auth-xaccountrestore-api}
{: api}

The following API request authorizes the Block Storage service of the source account to use a snapshot that was created by the target account to restore volumes. This call needs to be issued by the account that owns the snapshot. The receiving source account must ensure that their admin user has the `SnapshotRemoteAccountRestorer` role in IAM before they start a volume restoration with the CRN of the shared snapshot.

```sh
curl -X POST "https://iam.cloud.ibm.com/v1/policies"\
   -H 'Content-Type: application/json\
   -d '{
        "type":"authorization",
        "description":"Providing the Block Storage service access to restore from a Snapshot of another account ",
        "subjects": [{"attributes": [
                {"name":"BCloud Block Storage","value":"server-protect"},
                {"name":"Remote-restorer-account","value":"<Remote-Restorer-Account-ID>"}]}],
        "roles": [{"role_id":"crn:v1:bluemix:public:iam::::role:SnapshotRemoteAccountRestorer"}],
        "resources": [{"attributes": [
                {"name":"Snaphot-owner-account","operator":"stringEquals","value":"<Snapshot-Owner-Account-ID>"},
                {"name":"snapshotId","operator":"stringEquals","value":"*"}]}],
   }'
```
{: codeblock}
 
When you want to restrict the access to a specific snapshot, use the snapshot ID instead of `*` when you define your resources.

## Creating service-to-service authorization for customer-managed encryption with Terraform
{: #block-s2s-auth-encryption-terraform}
{: terraform}

Create an authorization policy between the Block service and the key management services by using the `ibm_iam_authorization_policy` resource argument in your `main.tf` file.

The following example creates an authorization policy between the Block service and {{site.data.keyword.keymanagementserviceshort}} when applied. 
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4keyprotect" {
  source_service_name  = "server-protect"
  target_service_name  = "kms"
  roles                = ["Reader"]
}
```
{: codeblock}

The following example creates an authorization policy between the Block service and {{site.data.keyword.hscrypto}} when applied.
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4HPCS" {
  source_service_name  = "server-protect"
  target_service_name  = "hs-crypto"
  roles                = ["Reader"]
}
```
{: codeblock}

For more information about the arguments and attributes, see the [Terraform documentation for authorization resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy){: external}.

## Creating service-to-service authorization for cross-account encryption with Terraform
{: #block-s2s-xaccount-encryption-terraform}
{: terraform}

1. Terraform supports configuring two different accounts for IBM provider. The provider without an alias is considered to be the default provider. See the following example, where two IBM accounts are specified, and the second account uses the alias `team_account`. That configuration must be referred to as `ibm.team_account` later. 

   ```terraform 
   terraform {
     required_providers {
       ibm = {
         source = "IBM-Cloud/ibm"
         version = ">= 1.12.0"
       }
     }
   }

   provider "ibm" {
     ibmcloud_api_key = var.ibmcloud_api_key
     region           = var.region
     ibmcloud_timeout = var.ibmcloud_timeout
   }

   provider "ibm" {
     alias = "team_account"
     ibmcloud_api_key = var.ibmcloud_api_key_second_account
     region           = var.region
     ibmcloud_timeout = var.ibmcloud_timeout
   } 
   ```
   {: screen}

   For more information about the arguments and attributes, see [IBM Cloud provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs#example-usage-of-provider){: external}.

1. To create the IAM authorization between the key management service from one account to the Block storage service in a different account, use the resource `ibm_iam_authorization_policy`. The following example creates an authorization between {{site.data.keyword.keymanagementserviceshort}} service and the file service of the two accounts. To create authorization to access {{site.data.keyword.hscrypto}}, specify `hs-crypto` as the value for `target_service_name`.

   ```Terraform
   resource "ibm_iam_authorization_policy" "policy" {
       source_service_name = "server-protect"
       source_service_account = "<volume-account-id>"
       target_service_name = "kms"   
       target_resource_instance_id = ibm_kms_key.key.instance_id
       roles               = ["Reader", "Authorization Delegator"]
       description         = "Authorization Policy" 
   }
   ```
   {: screen}

   This terraform resource must also include the provider alias (in our example, `ibm.team_account`) with the account `ibmcloud_api_key` where the encryption key belongs.

   For more information about the arguments and attributes, see [ibm_iam_authorization_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy#authorization-policy-between-two-specific-services){: external}.

## Creating service-to-service authorization for cross-account restore Terraform
{: #block-s2s-auth-xaccountrestore-terraform}
{: terraform}

1. Configure the two IBM accounts for IBM provider. See the example in the previous section.

1. To create the IAM authorization for the Block Storage service of one account to use a snapshot that was created by another account to restore volumes, use the resource `ibm_iam_authorization_policy`.

   ```Terraform
   resource "ibm_iam_authorization_policy" "policy" {
       source_service_name = "server-protect"
       source_service_account = "<volume-account-id>"
       target_service_name = "ibm_is_snapshot"   
       target_resource_instance_id = ibm_is_snapshot.snapshot.instance_id
       target_service_account = "<snapshot-account-id>"
       roles               = ["SnapshotRemoteAccountRestorer"]
       description         = "Authorization Policy" 
   }
   ```
   {: screen}

   This terraform resource must also include the provider alias (in our example, `ibm.team_account`) with the account `ibmcloud_api_key` where the encryption key belongs.

   For more information about the arguments and attributes, see [ibm_iam_authorization_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy#authorization-policy-between-two-specific-services){: external}.

## Next Steps
{: #block-s2s-next-steps}

- Review [Establishing service-to-service authorizations for the Backup service](/docs/vpc?topic=vpc-backup-s2s-auth).
- Review [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).
- Review [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
