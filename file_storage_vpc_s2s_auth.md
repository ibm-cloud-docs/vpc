---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-22"

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

To be able to create an encrypted file share with a Customer Root Key (CRK), you must first have an instance of a Key Management Service (KMS) to hold your CRK. You can choose between {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. Then, you need to establish service-to-service authorization between the file service and the KMS instance. The authorization must be created in the account that owns and hosts the customer root key. The account that holds the CRK is the source, and the account where the file share is to be created is the target. 

For cross-region replication, you need to establish service-to-service authorizations and specify [user roles](/docs/account?topic=account-iam-service-roles-actions#is.share-roles) for the various File Storage Service instances in different VPCs. This authorization enables the File Storage service in one VPC to interact with the File Storage Service of another VPC. Both VPCs must belong to the same account. Cross-account replication is not supported. For more information, see [Replication overview](/docs/vpc?topic=vpc-file-storage-replication).

For cross-account access, you need to establish service-to-service authorizations between the File Storage service of two different accounts. The authorization must be created in the account that owns and hosts the file share. You must also specify [user roles](/docs/account?topic=account-iam-service-roles-actions#is.share-roles) in both accounts to allow the users to create and manage accessor shares and share bindings. For more information, see [Sharing and mounting a file share from another account](/docs/vpc?topic=vpc-file-storage-accessor-create).

For more information about authorizations, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth).

## Creating authorization for customer-managed encryption in the console
{: #file-s2s-auth-encryption-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, log in to the account where the Key Management Service (KMS) and the Customer Root Key (CRK) are located.
1. Go to **Manage > Access (IAM)**.
1. From the side panel, select **Authorizations**.
1. On the **Manage authorizations** page, click **Create**. 
1. In the **Source** section, select the **Source account**. This account is where the CRK is to be used to create a file share with customer-managed encryption.
   - To allow the use of CRKs from this account to encrypt file shares in another account, select **Specific account** and enter the 32-character-long account ID of the account of the File service. Then, click **Next**.
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

## Creating authorization for cross-region replication in the console
{: #file-s2s-auth-replication-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, log in to the account where your file share is located.
1. Go to **Manage > Access (IAM)**.
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

## Creating authorization for cross-account access in the console
{: #file-s2s-auth-xaccount-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, log in to the account where your file share is.
1. Go to **Manage > Access (IAM)**.
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

## Creating authorization for Watson Studio in the console
{: #file-s2s-auth-watsonstudio-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, log in to the account where your file share is.
1. Go to **Manage > Access (IAM)**.
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
   1. Leave the **string equals** field as it is.
   1. In the next field, select the origin share from the list.
   1. Click **Next**.
1. Select the role `Share Broker`.
1. Check the box to enable authorization to be delegated by source and dependent services.
1. Click **Review** and inspect your choices.
1. Click **Authorize**.

## Creating authorization for customer-managed encryption from the CLI
{: #file-s2s-auth-encryption-cli}
{: cli}

Log in to your account. Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the File Storage Service to interact with your instance of one or both Key Management Services ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). The source service is `is` with the `--source-resource-type share` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the File Storage Service and {{site.data.keyword.keymanagementserviceshort}}. 

```sh
ibmcloud iam authorization-policy-create is kms Reader --source-resource-type share
```
{: pre}

```sh
Creating authorization policy under account a1234567 as test.user@ibm.com...
OK
```
{: screen}

To list the service authorizations that are already in place for the account, run the `ibmcloud iam authorization-policies` command. The following example shows that the file shares can be encrypted with a CRK that is stored in {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.

```sh
ibmcloud iam authorization-policies
```
{: pre}

```sh
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

## Creating authorization for cross-account encryption from the CLI
{: #file-s2s-xaccount-encryption-cli}
{: cli}

Log in to the account that holds the instance of a Key Management Service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). Run the `ibmcloud iam authorization-policy-create` command to create authorization policies for the File service to interact with the KMS. The source service is `is` with the `--source-resource-type share` and the target service is either `kms` or `hs-crypto`. The role that you need to assign is `Reader`. The following example creates an authorization policy between the File service of the `KeyUserAccount` and the {{site.data.keyword.keymanagementserviceshort}} instance of the `KeyOwnerAccount`.

1. Create a JSON file with the following information for the authorization policies in your local Documents folder.
   ```json
   '{
      "description":"Reader and Delegator role for HPCS service instance",
      "resources": [{"attributes": [
            {"name": "KeyOwnerAccountID","value": "a/a1234567","operator": "stringEquals"},
            {"name":"Hyper-Protect-Crypto-Services","operator":"stringEquals","value":"hs-crypto"}]}],   
      "roles": [
         {"role_id":"crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},
         {"role_id":"crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
      "subjects": [
         {"name": "serviceName","value": "is"},
            {"name": "resourceType","value": "share"},
            {"name": "KeyUserAccountID","value": "a/a7654321>"}],
      "type":"authorization"
   }'
   ```
   {: codeblock}

1. Then, run the following CLI command with the JSON file to create the authorization policy.
   ```sh
   ibmcloud iam authorization-policy-create --file ~/Documents/policy.json
   ```
   {: pre}
   
   The cross-account authorization is one-way and specific to key and service. When Account A authorizes their key to be used by Account B's file service, Account B can use Account A's CRK to encrypt Account B's shares. However, Account A cannot use Account B's root keys to encrypt Account A's shares.
   {: note}

## Creating authorization for cross-region replication from the CLI
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

## Creating authorization for cross-account access from the CLI
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

## Creating authorization for customer-managed encryption with the API
{: #file-s2s-auth-encryption-api}
{: api}

To authorize the file service to access your Key Management Service instance ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}), make an API request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization.

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

## Creating authorization for cross-account encryption with the API
{: #file-s2s-xaccount-encryption-api}
{: api}

To authorize the file service of another account to access your Key Management Service instance ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}), make a request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create the service-to-service authorization. The following example shows how to create an authorization for the {{site.data.keyword.filestorage_vpc_short}} service of the source account to access the key management service of your account (target) with Reader permission.

```sh
curl -X POST "https://iam.cloud.ibm.com/v1/policies" \
     -H "Authorization: <Auth Token>" \
     -H 'Content-Type: application/json' \
     -d '{
      "description":"Reader and Delegator role for HPCS service instance",
      "resources": [{"attributes": [
            {"name": "KeyOwnerAccountID","value": "a/a1234567","operator": "stringEquals"},
            {"name":"Hyper-Protect-Crypto-Services","operator":"stringEquals","value":"hs-crypto"}]}],   
      "roles": [
         {"role_id":"crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},
         {"role_id":"crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
      "subjects": [
         {"name": "serviceName","value": "is"},
            {"name": "resourceType","value": "share"},
            {"name": "KeyUserAccountID","value": "a/a7654321>"}],
      "type":"authorization"
   }'
```
{: screen}

## Creating authorization for cross-region replication with the API
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

## Creating authorization for cross-account access with the API
{: #file-s2s-auth-xaccount-api}
{: api}

As the share owner, make an API request to the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy) to create an authorization that allows the accessor account to access the origin share. The following example shows how to create the service-to-service authorization between the origin share account (source) and the accessor account (target).

```sh
curl -X POST "https://iam.cloud.ibm.com/v1/policies" \
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

## Creating authorization for customer-managed encryption with Terraform
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

## Creating authorization for cross-account encryption with Terraform
{: #file-s2s-xaccount-encryption-terraform}
{: terraform}

1. Terraform supports configuring two different accounts for IBM provider. The provider without an alias is considered the default provider. See the following example, where two IBM accounts are specified, and the second uses the alias `team_account`. That configuration is referred to as `ibm.team_account` later. 

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

1. To create the IAM authorization between the file share service from one account to the key management service in another account, use the resource `ibm_iam_authorization_policy`. The following example creates an authorization between the file service of one account and the {{site.data.keyword.hscrypto}} service of another account.

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
      name  = "accountId"
      value = data.ibm_iam_account_settings.iam.ibm.team_account_id
    }
    resource_attributes {
      name  = "Hyper-Protect-Crypto-Services"
      operator = "stringEquals"
      value = "hs-crypto"
    }
    roles   = ["Reader"]
    }
   ```
   {: codeblock}

   The following example creates an authorization between the file service of one account and the {{site.data.keyword.keymanagementserviceshort}} service of another account.

   ```Terraform
   resource "ibm_iam_authorization_policy" "policy" {
       source_service_name = "is"
       source_resource_type = "share"
       source_service_account = "<fileshare-account-id>"
       target_service_name = "kms"   
       target_resource_instance_id = ibm_kms_key.key.instance_id
       roles               = ["Reader"]
       description         = "Authorization Policy" 
   }
   ```
   {: screen}

   This terraform resource must also include the provider alias (in our example, `ibm.team_account`) with the account `ibmcloud_api_key` where the encryption key is stored.

## Creating authorization for cross-region replication with Terraform
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

## Creating authorization for cross-account access with Terraform
{: #file-s2s-auth-xaccount-terraform}
{: terraform}

1. Terraform supports configuring two different accounts for IBM provider. The provider without an alias is considered the default provider. See the following example, where two IBM accounts are specified, and the second uses the alias `team_account`. That configuration must be referred to as `ibm.team_account` later. 

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

- Review [Establishing service-to-service authorizations for the Backup service](/docs/vpc?topic=vpc-backup-s2s-auth).
- Review [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).
- [Create file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption).
- [Create cross-region replica shares](/docs/vpc?topic=vpc-file-storage-create-replication).
