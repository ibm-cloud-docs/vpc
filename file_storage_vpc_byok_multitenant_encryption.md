---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Cross-account encryption for file storage resources
{: #file-storage-byok-cross-acct}

{{site.data.keyword.filestorage_vpc_short}} supports cross-account customer-managed encryption. {{site.data.keyword.vpc_full}} customers can authorize access to a customer root key (CRK) for users of another account. Then, those users can use the CRK to encrypt a new file share in their own account.
{: shortdesc} 

## About cross-account key access and use
{: #byok-cross-acct-about}

Customer root keys in one account can be made available to users of another account to use when they create an encrypted file share. For more information, see [Encryption key management](/docs/solution-tutorials?topic=solution-tutorials-resource-sharing#resource-sharing-security-kms).

A privileged user from the account that owns the root key must [invite the user](/docs/account?topic=account-iamuserinv) of the second account, and set the IAM delegated policy to the root key. For more information, see [Granting access to keys with {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-grant-access-keys) and [Granting access to keys with {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-grant-access-keys).

If the invited user is already a member of {{site.data.keyword.cloud_notm}}, they receive an invitation link in their notifications and by email. The user in the account that's creating the file share can be the same user as the owner of the root key in the other account.

When the invited user decides to create the encrypted file share in their own account, they can specify the CRN of the root key of the first account in the `encryption_key` property.

## Restrictions
{: #byok-cross-key-acct-restrictions}

You must use the API to see the encryption keys that are used between accounts, and to create the IAM policy. You can use the UI to access your accounts and see the [IAM authorization](/docs/account?topic=account-serviceauth#serviceauth). However, the UI does not show the encryption keys that belong to another account even if they are used across accounts. Users of the account that owns the root keys can use the UI to verify the resources that are associated with their encryption keys.

The cross-account authorization is one-way and specific to key and service. When Account A authorizes their key to be used by Account B's file service, Account B can use Account A's CRK to encrypt Account B's shares. However, Account A cannot use Account B's root keys to encrypt Account A's shares. 

For Account A to use Account B's CRK to encrypt shares, Account B must invite Account A's user. Then, Account B must create an IAM policy that allows Account A's specific service to use Account B's CRK.

## Create an IAM policy to establish authorization between accounts with the API
{: #byok-cross-acct-iam-api}
{: api}

Before file share can be created and encrypted with a root key from another account, the key-owner account must create an IAM policy to authorize the second account to use the CRK.

To create the IAM policy, Account A must make a `POST /policies` call to the IAM API. In the following example, 
* Account A, which owns the CRK, grants `AuthorizationDelegator` and `Reader` roles to Account B's file service that is to create the encrypted file share.
* The `resources` section specifies information about Account A that owns the root key and the target key management service (KMS). The `kms` property indicates a {{site.data.keyword.keymanagementserviceshort}} instance. If you use a {{site.data.keyword.hscrypto}} instance, specify `hs-crypto`.
* The `subjects` section specifies Account B and the file share service that is to access Account A's {{site.data.keyword.keymanagementserviceshort}} instance and use the CRK.

```sh
curl -X POST "https://iam.cloud.ibm.com/v1/policies" \
     -H "Authorization: <Auth Token>" \
     -H 'Content-Type: application/json' \
     -d '{
        "roles": [{"role_id": "crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},{"role_id": "crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
        "resources": [{"attributes":[{"name":"Account-A","operator":"stringEquals","value":"<CRK-Account-A-ID>"},{"name":"Key-Protect","operator":"stringEquals","value":"kms"}]}],
        "type": "authorization",
        "description": "Reader and Delegator role for KeyProtect Service instance",
        "subjects": [{"attributes": [{"name": "serviceName","value": "is"},{"name": "resourceType","value": "share"},{"name": "Account-B","value": "<Account-B-ID>"}]}]
        }'
```
{: screen}

For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth&interface=api).

## Create an IAM policy to establish authorization between accounts with Terraform
{: #byok-cross-acct-iam-terraform}
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

   For more information about the arguments and attributes, see [IBM Cloud provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs#example-usage-of-provider){: external}. For more information about [Creating multiple provider configurations](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#multiple-providers), see [Terraform on IBM Cloud](/docs/ibm-cloud-provider-for-terraform).

1. To create the IAM authorization between the key management service from one account to the file share service in a different account, use the resource `ibm_iam_authorization_policy`. The following example creates an authorization between {{site.data.keyword.keymanagementserviceshort}} service and the file service of two accounts. To create authorization to access {{site.data.keyword.hscrypto}}, specify `hs-crypto` as the value for `target_service_name`.

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

   This terraform resource must also include the provider alias (in our example, `ibm.team_account`) with the account `ibmcloud_api_key` where the encryption key belongs.

   For more information about the arguments and attributes, see [ibm_iam_authorization_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_authorization_policy#authorization-policy-between-two-specific-services){: external}.

## Create a cross-account encrypted file share with the API
{: #byok-cross-key-acct-create-api}
{: api}

Before you can create an encrypted file share, you must authorize the file service to access the key management service. From {{site.data.keyword.iamshort}} (IAM), authorize access between Cloud File Storage (source service) and {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} (target service). Specify reader access for the role. For more information, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning&interface=api#byok-encryption-prereqs).

After the IAM authorizations are set, use the VPC API to [create a file share](/docs/vpc?topic=vpc-file-storage-byok-encryption&interface=api#fs-byok-api) with the root key that is owned by the other account. The API calls are the same as when you create an encrypted file share with a root key from your own KMS instance. When you make the `POST /shares` call, specify the CRN of the root key from the other account in the `encryption_key` property.

See the following example.

 ```curl
   curl -X POST \
   "$vpc_api_endpoint/v1/shares?version=2023-12-12&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
       "encryption_key": {"crn":"crn:bluemix:public:kms:us-south:a/a1234567:key:0cb88b98-9261-4d07-8329-8f594b6641b5"},
        "iops": 1000,
        "name": "my-encrypted-share",
        "profile": {"name": "dp2"},
        "resource_group": {"id": "678523bcbe2b4eada913d32640909956"},
        "size": 500,
        "zone": {"name": "us-south-2"}
       }'
   ```
   {: codeblock}

In the response, the CRN of the encryption key is from Account A that owns the key, while the file share is created in Account B.

```json
{
  "created_at": "2023-12-12T23:28:45Z",
  "crn": "crn:v1:bluemix:public:is:us-south-1:a/a7654321::share:r006-fe7219eb-c9a9-4aab-8636-9a57141f0cee",
  "encryption": "user_managed",
  "encryption_key": {"crn": "crn:bluemix:public:kms:us-south:a/a1234567:key:0cb88b98-9261-4d07-8329-8f594b6641b5"},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-fe7219eb-c9a9-4aab-8636-9a57141f0cee",
  "id": "r006-fe7219eb-c9a9-4aab-8636-9a57141f0cee",
  "initial_owner": {"gid": 0,"uid": 0},
  "iops": 1000,
  "lifecycle_state": "stable",
  "name": "my-test-us-south-1-er5s",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "name": "dp2",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:bluemix:public:resource-controller::a/a7654321::resource-group:5400b7d6ac9f0a183f70abbe8b8d54c6",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/5400b7d6ac9f0a183f70abbe8b8d54c6",
    "id": "5400b7d6ac9f0a183f70abbe8b8d54c6",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 500,
  "mount_targets": [],
  "user_tags": [
    "description:user-tags-test",
    "tag1:value1",
    "tag2:value2",
    "tag3:value with spaces"
  ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: screeen}

## Create a cross-account encrypted file share with Terraform
{: #byok-cross-key-acct-create-terraform}
{: terraform}

After the authorization is created, you can create the share in the default account and use the CRN of the encryption key from the account with the alias. To create a file share, use the `ibm_is_share` resource.

To create a file share, use the `ibm_is_share` resource. The following example creates a share with 800 GiB capacity and the `dp2` performance profile. The file share is encrypted by using a key that is identified by its CRN. The example also specifies a new mount target with a virtual network interface.

```terraform
resource "ibm_is_share" "share4" {
   zone                = "us-south-2"
   name                = "my-share4"
   size                = "800"
   iops                = 
   profile             = "dp2"
   encryption_key      = "crn:bluemix:public:kms:us-south:a/a1234567:key:0cb88b98-9261-4d07-8329-8f594b6641b5"
   access_control_mode = "security_group"
   mount_target {
       access_protocol = "nfs4"
       name            = "target"
       resource_group  = <resource_group_id>
       security_groups = [<security_group_ids>]
       virtual_network_interface {
        primary_ip {
           address     = "10.240.64.5"
           auto_delete = true
           name        = "<reserved_ip_name>
          }
       }  
    }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #next-steps-multiten-shares}

Learn more about [data encryption](/docs/vpc?topic=vpc-vpc-encryption-about) for VPC resources.
