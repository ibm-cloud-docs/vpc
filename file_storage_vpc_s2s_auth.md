---

copyright:
  years: 2022, 2024
lastupdated: "2024-08-08"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service-to-service authorizations for {{site.data.keyword.filestorage_vpc_short}}
{: #file-s2s-auth}

You can use the {{site.data.keyword.iamshort}} (IAM) to create or remove an authorization that grants one service access to another service. For {{site.data.keyword.filestorage_vpc_short}}, you need to create service-to-service authorization for configuring customer-managed encryption and setting up cross-region replication.
{: shortdesc}

## Overview
{: #file-s2s-auth-overview}

In an authorization, the source service is the service that is granted access to the target service. The roles that you select define the level of access for the source service. The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign. Generally, a source service can be in the same account where the authorization is created or in another account. The target service is always in the account where the authorization is created. 

To be able to create an encrypted file share with customer-managed CRKs, you need to establish service-to-service authorization between the File service and the Key Management Service of your choice. The instructions to create the service-to-service authorization are provided in this topic.

For cross-region replication, you need to establish service-to-service authorizations and specify [user roles](/docs/account?topic=account-iam-service-roles-actions#is.share-roles). for the various File service instances in different VPCs. This authorization enables the File service in one VPC to interact with the File service of another VPC. Both VPCs must belong to the same account. Cross-account replication is not supported. For more information, see [Replication overview](/docs/vpc?topic=vpc-file-storage-replication).

For more information about authorizations, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).

## Creating service-to-service authorization for customer-managed encryption in the UI
{: #file-s2s-auth-encryption-ui}
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
   1. In the next field, select **File Storage for VPC**.
   1. Click **Next**.
1. For the target service, select **Hyper Protect Crypto Services** or **KeyProtect** from the list. Click **Next**.
1. Select the role `Reader`.
1. Check the box to enable authorization to be delegated by source and dependent services.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for cross-region replication in the UI
{: #file-s2s-auth-replication-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. Select **This account**, and click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **File Storage for VPC**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **File Storage for VPC**.
   1. Click **Next**.
1. Select the role `Editor`.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for cross-account access in the UI
{: #file-s2s-auth-xaccount-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. 
   - If you want to allow another account to access data on your file share, select **Specific account** and enter the 32-character-long account ID. Then, click **Next**.
   - If you want to create accessor shares in the same account, select **This account**. Then, click **Next**.
1. For the source service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **File Storage for VPC**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **File Storage for VPC**.
   1. Click **Add a condition**. 
   1. Click **Select an attribute** and select **Share ID**. 
   1. Select the share from the list.
   1. Click **Next**.
1. Select the role `Share Broker`.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for Watson Studio in the UI
{: #file-s2s-auth-watsonstudio-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. On the **Grant a service authorization** page, select **This account**.
1. For the source service, select **Watson Studio** from the list.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Source service instance**. 
   1. In the next field, select **all instances**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**. 
   1. In the next field, select **File Storage for VPC**.
   1. Click **Add a condition**. 
   1. Click **Select an attribute** and select **Share ID**. 
   1. Leve the **string equals** field as it is.
   1. In the next field, select the origin share from the list.
   1. Click **Next**.
1. Select the role `Share Broker`.
1. Check the box to enable authorization to be delegated by source and dependent services.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating service-to-service authorization for customer-managed encryption from the CLI
{: #file-s2s-auth-encryption-cli}
{: cli}

Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the File service to interact with one or both Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The source service is `is` with the `--source-resource-type share` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the File service and {{site.data.keyword.keymanagementserviceshort}}.

```sh
$ ibmcloud iam authorization-policy-create is kms Reader --source-resource-type share
Creating authorization policy under account a1234567 as test.user@ibm.com...
OK
```
{: screen}

To list the service authorizations that are already in place for the account, run the `ibmcloud iam authorization-policies` command. The following example shows that the file shares can be encrypted with a CRK that is stored in {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.

```sh
$ ibmcloud iam authorization-policies
Getting authorization policies under account a1234567 as test.user@ibm.com...
OK
                           
ID:                        8a2ef8a5-2e4c-46ea-b2e7-d4b0d0a0e1a5
Source service name:       is
Source service instance:   All instances
Source resource type:      share
Target service name:       hs-crypto
Target service instance:   All instances
Roles:                     Reader
                           
ID:                        d2df60ea-5575-4bd1-9cd6-f35c52576577
Source service name:       is
Source service instance:   All instances
Source resource type:      share
Target service name:       kms
Target service instance:   All instances
Roles:                     Authorization Delegator, Reader
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

## Creating service-to-service authorization for cross-region replication from the CLI
{: #file-s2s-auth-replication-cli}
{: cli}

Run the `ibmcloud iam authorization-policy-create` command to authorize the regional file services to work together.

```sh
ibmcloud iam authorization-policy-create is is Editor --source-resource-type share --target-resource-type share
```
{: pre}

See the following example.

```sh 
$ ibmcloud iam authorization-policy-create is is Editor --source-resource-type share --target-resource-type share 
Creating authorization policy under account a1234567 as test.user@ibm.com...
OK
Authorization policy f311f255-d20e-49d0-aa94-316feaba981e was created.
                           
ID:                        f311f255-d20e-49d0-aa94-316feaba981e
Source service name:       is
Source service instance:   All instances
Source resource type:      share
Target service name:       is
Target service instance:   All instances
Target resource type:      share
Roles:                     Editor
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud iam authorization-policy-create](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_authorization_policy_create).

## Creating service-to-service authorization for cross-account access from the CLI
{: #file-s2s-auth-xaccount-cli}
{: cli}

As the share owner, create a JSON file and use it with the `ibmcloud iam authorization-policy-create` command to create the service-to-service authorization for the {{site.data.keyword.filestorage_vpc_short}} service of the share owner account (source) to access a share in accessor account (target).

1. Create a JSON file with the following content. In this example, the file is created as the policy.json in the Documents folder.

   ```json
   {
        "roles": [{"role_id": "crn:v1:bluemix:public:iam::::role:ShareBroker","display_name": "Share Broker",
                 "description": "As a share broker, you can create and delete share bindings"}],
        "resources": [{"attributes": [
            {"name": "accountId","value": "<origin share owner Account ID>","operator": "stringEquals"},
            {"name": "serviceName","value": "is","operator": "share"},
            {"name": "shareId","value": "<OriginShareId>","operator": "stringEquals"}]}],
        "type": "authorization",
        "description": "ShareBroker role for File Storage for VPC cross-account share access",
        "subjects": [{"attributes": [
            {"name": "serviceName","value": "is"},
            {"name": "resourceType","value": "share"},
            {"name": "accountId","value": "<accessor share Account ID>"}]}]
        }
   ```
   {: pre}

1. Then, run the CLI command with the JSON file to create the authorization policy.
   ```sh
   ibmcloud iam authorization-policy-create --file ~/Documents/policy.json
   ```
   {: pre}

## Creating service-to-service authorization for customer-managed encryption with the API
{: #file-s2s-auth-encryption-api}
{: api}

Make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization for the source share's file service to interact with a Key Management Service instance ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}).

* The following example shows how you can authorize the File service `is.share` (source) to interact with the {{site.data.keyword.keymanagementserviceshort}} service `kms` (target) with the _Reader_ role.

   ```sh
   curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
   'Authorization: Bearer $TOKEN' -H 
   'Content-Type: application/json' -d 
   '{
     "type": "access",
     "description": "Reader role for the file service to interact with the KeyProtect service.",
     "subjects": [{"attributes": [{"name": "serviceName","value": "is"},{"name": "resourceType","value": "share"}]}],
     "roles":[{"role_id": "crn:v1:bluemix:public:iam::::role:Reader"}],
     "resources":[{"attributes": [{"name": "serviceName","value": "kms"}]}]
     }'
     ```
     {: pre}

* To create an authorization policy for {{site.data.keyword.hscrypto}}, replace `kms` with `hs-crypto` in the previous example.

## Creating service-to-service authorization for cross-region replication with the API
{: #file-s2s-auth-replication-api}
{: api}

Make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization for the source share's regional file service to interact with the replica share's regional file service.

   * Authorize `is.share` (source) to interact with `is.share` (target) with the _Editor_ role.

     ```sh
     curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 
     'Authorization: Bearer $TOKEN' -H 
     'Content-Type: application/json' -d 
     '{
       "type": "access",
       "description": "Editor role for the source share's regional file service to interact with replica share's regional file service.",
       "subjects": [{"attributes": [{"name": "serviceName","value": "is"},{"name": "resourceType","value": "share"}]}],
       "roles":[{"role_id": "crn:v1:bluemix:public:iam::::role:Editor"}],
       "resources":[{"attributes": [{"name": "serviceName","value": "is"},{"name": "resourceType","value": "share"}]}]
       }'
     ```
     {: pre}

For more information, see the api spec for [IAM Policy Management](/apidocs/iam-policy-management#create-policy).

## Creating service-to-service authorization for cross-account access with the API
{: #file-s2s-auth-xaccount-api}
{: api}

As the share owner, make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization for the {{site.data.keyword.filestorage_vpc_short}} service of the origin share account (source) to access a share in accessor account (target).

```json
curl -X "POST" "https://iam.cloud.ibm.com/v1/policies" \
     -H "Authorization: <Auth Token>" \
     -H 'Content-Type: application/json' \
     -d '{
        "roles": [{"role_id": "crn:v1:bluemix:public:iam::::role:ShareBroker","display_name": "Share Broker",
                 "description": "As a share broker, you can create and delete share bindings"}],
        "resources": [{"attributes": [
            {"name": "accountId","value": "<origin share owner Account ID>","operator": "stringEquals"},
            {"name": "serviceName","value": "is","operator": "share"},
            {"name": "shareId","value": "<OriginShareId>","operator": "stringEquals"}]}],
        "type": "authorization",
        "description": "ShareBroker role for File Storage for VPC cross-account share access",
        "subjects": [{"attributes": [
            {"name": "serviceName","value": "is"},
            {"name": "resourceType","value": "share"},
            {"name": "accountId","value": "<accessor share Account ID>"}]}]
        }'
```
{: screen}

## Creating service-to-service authorization for customer-managed encryption with Terraform
{: #file-s2s-auth-encryption-terraform}
{: terraform}

Create an authorization policy between the file service and the key management services by using the `ibm_iam_authorization_policy` resource argument in your `main.tf` file.

The following example creates an authorization policy between the file service and {{site.data.keyword.keymanagementserviceshort}} when applied. 
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4keyprotect" {
  source_service_name  = "is"
  source_resource_type = "share"
  target_service_name  = "kms"
  roles                = ["Reader"]
}
```
{: codeblock}

The following example creates an authorization policy between the file service and {{site.data.keyword.hscrypto}} when applied.
```terraform 
resource "ibm_iam_authorization_policy" "mypolicy4HPCS" {
  source_service_name  = "is"
  source_resource_type = "share"
  target_service_name  = "hs-crypto"
  roles                = ["Reader"]
}
```
{: codeblock}

For more information about the arguments and attributes, see the [Terraform documentation for authorization resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy){: external}.

## Creating service-to-service authorization for cross-region replication with Terraform
{: #file-s2s-auth-replication-terraform}
{: terraform}

Create an authorization policy between the file services in the different regions by using the `ibm_iam_authorization_policy` resource argument in your `main.tf` file.

```terraform 
resource "ibm_iam_authorization_policy" "mypolicy" {
  source_service_name  = "is"
  source_resource_type = "share"
  target_service_name  = "is"
  target_resource_type = "share"
  roles                = ["Editor"]
}
```
{: codeblock}

For more information about the arguments and attributes, see the [Terraform documentation for authorization resources](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy){: external}.

## Creating service-to-service authorization for cross-account access with Terraform
{: #file-s2s-auth-xaccount-terraform}
{: terraform}

1. Terraform supports configuring two different accounts for IBM provider. The provider without an alias is considered the default provider. See the following example, where two IBM accounts are specified, and the second is given the alias `team_account`. That configuration must be referred to as `ibm.team_account` later. 

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

1. To create the IAM authorization between the file share service from one account to the file share service in a different account, use the resource `ibm_iam_authorization_policy`. The following example creates an authorization between the file services of two accounts.

   ```Terraform
   resource "ibm_iam_authorization_policy" "policy" {
       source_service_name = "is"
       source_resource_type = "share"
       source_service_account = "<ShareOwner-Account-id>"
       target_service_name = "is"   
       target_resource_type = "share"
       target_service_account = "<Accessor-Account-id>"
       roles               = ["ShareBroker"]
       description         = "Authorization Policy" 
   }
   ```
   {: codeblock}

   Or

   ```terraform
   resource "ibm_iam_authorization_policy" "policy1" {
    subject_attributes {
      name  = "accountId"
      value = data.ibm_iam_account_settings.iam.account_id
    }
    subject_attributes {
      name  = "serviceName"
      value = "is"
    }
    subject_attributes {
      name  = "resourceType"
      value = "share"
    }
    resource_attributes {
      name  = "shareId"
      operator = "stringEquals"
      value = data.ibm_share_id
    }
    resource_attributes {
      name  = "serviceName"
      operator = "stringEquals"
      value = "is"
    }
    resource_attributes {
      name  = "volumeId"
      operator = "stringExists"
      value = "true"
    }
    roles   = ["ShareBroker"]
    }
   ```
   {: codeblock}

   This terraform resource must also include the provider alias (in our example, `ibm.team_account`) with the account `ibmcloud_api_key` where the origin share is.

   For more information about the arguments and attributes, see [ibm_iam_authorization_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy#authorization-policy-between-two-specific-services){: external}.

## Next Steps
{: #file-s2s-next-steps}

- [Create file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption).
- [Create cross-region replica shares](/docs/vpc?topic=vpc-file-storage-create-replication).
