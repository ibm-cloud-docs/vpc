---

copyright:
  years: 2023, 2024
lastupdated: "2024-07-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Cross-account encryption for Block Storage resources
{: #vpc-block-storage-byok-cross-acct}

{{site.data.keyword.block_storage_is_short}} supports cross-account customer-managed encryption for stand-alone data volumes that you can attach to an instance, and boot volumes that are created during instance provisioning. With this feature, users can access a customer root key (CRK) from another account and use that CRK to encrypt the volume. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts.
{: shortdesc}

## About cross-account key access and use
{: #byok-vol-cross-acct-about}

Customer root keys (CRK) in one account can be made available to users of another account to use when they create an encrypted storage volume. For more information, see [Encryption key management](/docs/solution-tutorials?topic=solution-tutorials-resource-sharing#resource-sharing-security-kms).

Account A owns the CRK and the account administrator creates the IAM delegated policy for the root keys to be available to the Account B user. Account A controls all actions that are related to the key, such as [rotating, disabling, or deleting](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-manage-root-keys) them. If a key is deleted or disabled, volumes that are protected by that key in Account B become unusable. Owners of the root key need to inform users across accounts of any action on that root key.

The Account B user needs to set up the required service-to-service authorizations between the Block Storage service of Account B and the key management service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}) of Account B. When the Account B user creates the encrypted volume, they specify the CRN of the root key from Account A in the API request as the value of the `encryption_key` property.

## Before you begin
{: #byok-vol-cross-key-acct-prereqs}

From {{site.data.keyword.iamshort}} (IAM), authorize access between Cloud Block Storage (source service) and {{site.data.keyword.keymanagementserviceshort}} (target service). For custom images, authorize between Image Service for VPC (source service) and {{site.data.keyword.cos_full}} (target service). Specify reader access for the role. For more information, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning&interface=api#byok-encryption-prereqs).

You must use the API to see the encryption keys that are used between accounts, and to create the IAM policy. You can use the UI to access your accounts and see the [IAM authorization](/docs/account?topic=account-serviceauth#serviceauth). However, the UI does not show the encryption keys that are used across accounts. As account user where the root keys are, you can use the [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision) UI to verify the resources that are associated with the encryption keys.

## Create service-to-service authorization between accounts
{: #byok-storage-cross-acct-iam}

Before you can create an encrypted {{site.data.keyword.block_storage_is_short}} volume with a root key from another account, you must create an IAM authorization policy from the secondary account to use the primary account's root key.

In the following example:

* Account A, which owns the key, grants `AuthorizationDelegator` and `Reader` access to Account B, where the encrypted volume is to be created.
* Account B creates the policy and grants reader access for `server-protect` to the key management service (KMS). In this example, `kms` indicates a {{site.data.keyword.keymanagementserviceshort}} instance.

```sh
curl -X "POST" "https://iam.cloud.ibm.com/v1/policies" \
     -H "Authorization: <Auth Token>" \
     -H 'Content-Type: application/json' \
     -d '{
        "roles": [{"role_id": "crn:v1:bluemix:public:iam::::role:AuthorizationDelegator"},{"role_id": "crn:v1:bluemix:public:iam::::serviceRole:Reader"}],
        "resources": [{"attributes": [
                {"name": "Account-A","operator": "stringEquals","value": "<CRK-Account-A-ID>"},
                {"name": "Key-Protect","operator": "stringEquals","value": "kms"}]}],
        "type": "authorization",
        "description": "Reader and Delegator role for KeyProtect Service instance",
        "subjects": [{"attributes": [
                {"name": "Key-Protect","value": "server-protect"},
                {"name": "Account-B","value": "<Account-B-ID>"}]}]
     }'
```
{: codeblock}

## Create a cross-account encrypted volume with the API
{: #byok-cross-key-acct-create-vol-API}

With the VPC API, create an [encrypted {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-block-storage-vpc-encryption&interface=api#data-vol-encryption-api) in one account and specify the root key CRN from another account that owns the key. The API calls for creating a cross-account encrypted volume are the same as the calls that you use when you create an encrypted volume with a root key from the same account. Make a `POST /volumes` call and specify the CRN of the root key from the other account in the `encryption_key` property.

This example specifies the CRN of a customer-managed key from a different account:

```sh
curl -X POST \
"$vpc_api_endpoint/v1/volumes?version=2022-08-08&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-volume-1",
      "iops": 100,
      "capacity": 20,
      "zone": {
        "name": "us-south-3"
      },
      "profile": {
        "name": "general-purpose"
      },
      "encryption_key": {
          "crn":"crn:bluemix:public:kms:us-south:a/a1234567:key:0cb88b98-9261-4d07-8329-8f594b6641b5"
      },
      "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
      }
    }
```
{: codeblock}

In the response, the CRN of the encryption key is from the account that owns the root key, while the volume is created in a second account.

```json
{
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "crn": "crn:[...]",
    "name": "my-volume-1",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "capacity": 20,
    "iops": 100,
    "encryption_key": {
        "crn":"crn:bluemix:public:kms:us-south:a/a1234567:key:0cb88b98-9261-4d07-8329-8f594b6641b5"
    },
    "encryption": "user_managed",
    "status": "available",
    "zone":
        "name": "us-south-3",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/
         us-south-3"
    },
    "profile": {
        "name": "general-purpose",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/
       general-purpose"
    },
    "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/
         a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
        "name": "Default"
    },
    "volume_attachments": [],
    "created_at": "2022-08-08T16:03:22.000Z"
}
```
{: screen}

## Next steps
{: #next-steps-multiten-volumes}

Learn more about [data encryption](/docs/vpc?topic=vpc-vpc-encryption-about) for VPC resources.
 