---

Copyright:
  years: 2019, 2020
lastupdated: "2020-08-05"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Key rotation for VPC resources
{: #vpc-key-rotation}

For {{site.data.keyword.vpc_short}} resources such as volumes and encrypted images that are protected by your customer root key (CRK), you can rotate the root keys for additional security. When you rotate a root key by schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing resources but can't be used to encrypt new ones. This feature is available on Gen 2 Compute resources only.
{:shortdesc}

Encrypted custom image is a Beta feature available for testing and evaluation. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

## Key rotation overview
{: #vpc-key-rotation-overview}

Customer-managed encrypted resources such as volumes and custom images use your root key (CRK) as the root-of-trust key that encrypts a LUKS passphrase that encrypts a master key protecting the resource. You can import your CRK to a key management service (KMS) instance or instruct the KMS to generate one for you. Supported key management services are {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (HPCS). Root keys are rotated in your KMS instance.

When you rotate a root key, a new version of the key is created by generating or importing new cryptographic key material. The old root key is retired, which means its key material remains available for deecrypting existing resources, but not available for encrypting new ones. 

You can have several root keys in each KMS instance available for key rotation. These root keys can be a combination of keys that you imported to the cloud or ones that were automatically generated in the KMS instance. 

You can set up a [rotation policy](#vpc-key-rotation-policies) to schedule automatic key rotation. The KMS generates a new root key at the rotation interval and automatically replaces the root key with new key material.

For root keys that you import to the KMS instance, because you're providing new key material not already in the KMS, you can't set a rotation policy for automatic key rotation. Instead, you [manually rotate](#vpc-key-rotation-ui) your keys by using the new cryptographic key material you imported.

The key rotation feature is available for root keys and does not apply to standard encryption keys. Key rotation is available for encrypting resrouces on Gen 2 Compute resources only.
{:note}

### How key rotation works
{: #vpc-key-rotation-function}

Key rotation works by securely transitioning root key material, shortening the cryptoperiod of the root keys protecting your resources. New resources are encrypted using the latest root keys.

When you rotate a root key, the service identifies all resources protected by that key and automatically reenrypts the resource. This encryption process actually _wraps_ LUKS passphrases encrypting master keys that encrypt the resource. This process, called [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption), creates what the KMS calls a wrapped data encryption key, or WDEK. The new version of the root key is then used to unwrap (decrypt) the newly-rewrapped passphrase the next time the resource is decrypted (for example, decrypting a volume upon restarting an instance).

The service retires the old root key version but it remains availabe for decrypting existing resources. You can list the key versions to see how many times the key has been rotated and see the latest version. Older versions of the root key continue to work until they expire or until there aren't any resources to encrypt. If you manually disable the root key or reboot the instance, the old key no longer functions.

After your root keys are rotated, the new root key version becomes available for encrypting new resources. 

For more information about the key rotation process, see:

* {{site.data.keyword.keymanagementserviceshort}} - [Understanding the key rotation process](/docs/key-protect?topic=key-protect-key-rotation#understand-key-rotation-process)
* {{site.data.keyword.hscrypto}} - [Root key rotation](https://test.cloud.ibm.com/docs/hs-crypto?topic=hs-crypto-key-rotation#root-key-rotation-intro)

### Benefits of key rotation
{: #vpc-key-rotation-benefits}

Rotating your root keys provides these security benefits:

* You can limit how long your data is protected by a single root key. By shortening the time your resources are encrypted with a particular root key, you decrease the probability of a security breach.
* You limit the number of resources encrypted with any one version of the root key, because resources created after key rotation are encrypted with different key material.
* If you organization detects a security threat, you can immediately rotate your keys to protect your resources.  
* Rotating keys mitigates costs associated with key compromises.

### Key rotation options
{: #vpc-key-rotation-options}

You can rotate your root keys in your KMS instance by setting a rotation policy or by manually rotating your keys. A rotation policy automatically rotates your keys based on a schedule; manual rotation rotates keys on demand.

If you're using HPCS, you can also rotate master keys using [IBM Cloud TKE CLI plug-in](/docs/hs-crypto?topic=hs-crypto-rotate-master-key-cli). Information in this topic pertains to rotating root keys only.
{:note}

When you set a [rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy) for a root key, you define a rotation schedule to automatically rotate your key. In your KMS instance, you can set a rotation interval between 1 - 12 months.

You can also manually rotate your keys at any time. {{site.data.keyword.keymanagementserviceshort}} and HPCS allow one rotation per hour for each root key. The KMS replaces the key immediately upon your request. Use this option when you import a new root key to the KMS instance and want to rotate the key immediately.

For more information about these options, see [Comparing your key rotation options in Key Protect](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options). For more information about setting a rotation policy in your KMS, see either of these topics:

* Key Protect - [Managing rotation policies](/docs/key-protect?topic=key-protect-set-rotation-policy)
* HPCS - [Setting a rotation policy for root key](/docs/hs-crypto?topic=hs-crypto-set-rotation-policy) and [Rotating root keys manually](/docs/hs-crypto?topic=hs-crypto-rotate-keys).

### Root key metadata
{: #vpc-key-rotation-metadata}

When you rotate your root key, key retains the same metadata and key ID. All that's changed is the root key ciphertext and the `keyVersion` data.

## Prerequisites for key rotation
{: #prerequisites-byok-key-rotation}

To perform key rotation:

* Import one or more root keys to the KMS instance, or have the KMS generate one for you. See [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).
* Select one KMS for key rotation. Key rotation modifies key material within that instance.
* Any root key can be rotated and if the key is protecting a resource; the key will be re-wrapped.
* Keys for resources (such as block storage volumes) on Gen 1 Compute resources can't be rotated. Create separate root keys for resources on Gen 2 Compute resources that can be rotated.
* If you initially imported a root key, you must provide new base64-encoded key material to rotate the key.
* Locate the KMS instance ID. See [Retrieving your instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
* Locate the root key ID and CRN. Using the CLI, see [Step 1 - Obtain service instance and root key information](/docs/vpc?topic=vpc-creating-instances-byok#byok-cli-setup-prereqs). To view a list of available root keys, also see this information for [Key Protect](/docs/key-protect?topic=key-protect-view-keys) or [HPCS](/docs/hs-crypto?topic=hs-crypto-view-keys).
* Locate the API endpoint when using the API service. Using the UI:
  1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external}) **Resource List**, under **Services**, click on the KMS instance. (The panel also shows the instance CRN and GUID.)
  2. Click **View Key Details** for the API endpoint.

## Using the key management service UI to rotate keys
{: #vpc-key-rotation-ui}

This process rotates your root key to a new key version that you can use to reencrypt your data. These steps pertain to {{site.data.keyword.keymanagementserviceshort}}, but similar steps are available for [HPCS](/docs/hs-crypto?topic=hs-crypto-rotate-keys#rotate-root-key-gui). 

[After you create a root key](/docs/key-protect?topic=key-protect-create-root-keys), follow this procedure:

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Resource List** to view a list of your resources.
2. From your IBM Cloud resource list, select your provisioned {{site.data.keyword.keymanagementserviceshort}} instance.
3. On the application details page, use the **Keys** table to browse the keys in your service.
4. Click the overflow icon (hellipsis) to open a list of options for the key that you want to rotate. 
5. From the options menu, click **Rotate key**. 
6. Click **Rotate key** to confirm.

Verify that the key was rotated by viewing the root key's [registration information](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-verify-rotation). The root key version ID and key version date change, indicating that the key has been rotated. The root key retains its original name and ID.

You can also view Activity Tracker events for key rotation. For information, see [Important events for key rotation ](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-key-rotation-events)

## Using the key management service CLI to rotate keys
{: #vpc-key-rotation-cli}

This procedure describes how to rotate the root key protecting your data with a new root key using the CLI. This example uses a {{site.data.keyword.keymanagementserviceshort}} instance for rotating keys; HPCS steps are similar.  

### Before you begin
{: #byok-cli-rotate-prereqs}

Before you begin, you'll need:

* The CRN of the root key in your key management service.
* The instance ID for the {{site.data.keyword.keymanagementserviceshort}} service instance where your customer root keys are stored. 
* The list of available keys and their associated CRNs within your {{site.data.keyword.keymanagementserviceshort}} service instance.

You'll also need to install the the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in. 

For more information, see [Step 1 - Obtain service instance and root key information](/docs/vpc?topic=vpc-vpc-vsi-instances-byok#byok-cli-setup-prereqs).

### CLI procedure for imported keys
{: #CLI-vpc-imported-key-procedure}

1. To import a root key and key material, you must use the UI or API, then continue with CLI step  See the information about importing root keys for [Key Protect](/docs/key-protect?topic=key-protect-import-root-keys) instances or [HPCS](/docs/hs-crypto?topic=hs-crypto-import-root-keys) instances.

2. List the available keys and their associated CRNs for the {{site.data.keyword.keymanagementserviceshort}} service instance by specifying the instance ID:

   ```
   ibmcloud kp keys -c --instance-id 7mnxxxo8-91xx-23px-q4rs-xxtuv5w6xxx7
   ```
   {: pre}

3. Rotate the key.

  ```
  $ ibmcloud kp key rotate 1a6d5be8-287c-4eb3-9c44-cf0c2b0d67ad
  Rotating root key...
  SUCCESS
  ```
  {: codeblock}

4. Verify the key was rotated.

  ```
  $ ibmcloud kp key show 1a6d5be8-287c-4eb3-9c44-cf0c2b0d67ad
  Grabbing info for key id: 1a6d5be8-287c-4eb3-9c44-cf0c2b0d67ad...
  SUCCESS
  Key ID                                 Key Name      Description   Creation Date                   Expiration Date
  1a6d5be8-287c-4eb3-9c44-cf0c2b0d67ad   my-root-key                 2020-05-06 17:25:22 +0000 UTC   Key does not expire
  ```
  {: codeblock}

### CLI procedure for KMS-generated keys
{: #CLI-vpc-generated-key-procedure}

This procedure describes creating a new root key with base64 key material. It starts by creating the first key and then rotating it with new key material. 

1. Create a random, base64-encoded, 32-byte key material.

  ```
  $ KEY_MATERIAL=$(openssl rand -base64 32)
  ```
  {:pre}

2. Create a root key from the base64-encoded value.

  ```
  $ ibmcloud kp key create my-base64-root-key -k $KEY_MATERIAL
  Creating key: 'my-base64-root-key', in instance: '390086ac-76fa-4094-8cf3-c0829bd69526'...
  SUCCESS
  Key ID                                 Key Name
  e55f86ab-6984-4594-ad23-3024f6440a58   my-base64-root-key
  ```
  {: codeblock}

3. Create new key material.

  ```
  $ NEW_KEY_MATERIAL=$(openssl rand -base64 32)
  ```
  {:pre}

4. Rotate the key with the new key material.

  ```
  $ ibmcloud kp key rotate e55f86ab-6984-4594-ad23-3024f6440a58 -k $NEW_KEY_MATERIAL
  Rotating root key...
  SUCCESS
  ``` 
  {: codeblock}

## Using the key management service API to rotate keys
{: #vpc-key-rotation-api}

Using the API service, rotating a root key with new key material creates a new root key version that wraps (encrypts) the DEK used to rewrap (reencrypt) your data. The process for manually rotating a key using the {{site.data.keyword.keymanagementserviceshort}} API is described here, but the overall process is similar for HPCS.

If you initially imported a root key by using an import token, follow the steps in [Using an import token to rotate a key](/docs/key-protect?topic=key-protect-rotate-keys#rotate-keys-secure-api).
{:note}

### Before you begin
{: #vpc-key-rotation-api-prereqs}

[Set up the {{site.data.keyword.keymanagementserviceshort}} API service](/docs/key-protect?topic=key-protect-set-up-api). When you set up the service, you establish your service and authentication credentials. This step includes generating an IBM Cloud IAM access token used to authenticate your requests to the API service.  

Retrieve the instance ID that uniquely identifies your Key Protect service instance and the API endpoint. For more information, see [Prerequisites for key rotation](#prerequisites-byok-key-rotation).

When you make an API call to the service, structure your API request according to how you initially provisioned your KMS instance. For more information, see [Form your API request](/docs/key-protect?topic=key-protect-set-up-api#form-api-request).

### Root key registration
{: #api-byok-root-key-reg}

When you create a data volume that uses customer-managed encryption, your root keys are automatically registered. Registration creates the relationship between your root key and Cloud Block Storage. When you list the registrations for a key, you can see the resources it's registered with.

Make a `GET /keys/<key_id>/registrations` call to see which cloud resources are protected by the key that you specify. In this example, the IBM Cloud instance ID (`bluemix-instance`) identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.

```
curl -X GET \
  https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/registrations \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>'
```
{:codeblock}

The response will display information about the resource:

```
{
  "metadata": {
    "collectionType": "application/vnd.ibm.kms.registration+json",
    "collectionTotal": 2,
    "totalCount": 4
  },
  "resources":  {
      "keyId": "7f19bee1-4fdf-4646-866e-111c80f94478",
      "resourceCrn": "crn:v1:bluemix:public:iaas:global:a/<account_id>:volume:<volume_id>",
      "createdBy": "IBMid-203BRNKPR5",
      "creationDate": "2020-06-03T05:23:19+0000",
      "updatedBy": "IBMid-203BRNKPR5",
      "lastUpdated": "2020-06-03T05:23:19+0000",
      "description": "Registration between a bucket and root key",
      "preventKeyDeletion": true,
      "keyVersion": {
        "id": "4a0225e9-17a0-46c1-ace7-f25bcf4237d4",
        "creationDate": "2020-06-03T05:23:19+0000"
      }
    }
}
```
{:code-block}

### API procedure
{: #vpc-key-rotation-api-procedure}

Follow these steps to rotate a root key using the Key Protect API.

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api) to work with keys in the service.

2. Locate the root key ID in your KMS instance by making a `GET/keys` request to the following endpoint:

  ```
  https://<region>.kms.cloud.ibm.com/api/v2/keys
  ```
  {:pre}

  Use the `limit` and `offset` parameters to retrieve a subset of your keys, beginning with the offset value that you specify. For more information, see [Retrieving a subset of keys](https://test.cloud.ibm.com/docs/key-protect?topic=key-protect-view-keys#retrieve-subset-keys-api).
  {:tip}

3. Copy the ID of the root key that you want to rotate.

4. Replace the root key with new key material with a `POST/key/<key-ID> action=rotate ` request. If the root key was generated by the KMS, you don't need a POST request body. If you imported the key, supply the new key material (Base64 encoded) in the `payload` field of the JSON object in the POST request body.

  For example:

    ```cURL
      curl -X POST \
        https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=rotate \
        -H 'accept: application/vnd.ibm.kms.key_action+json' \
        -H 'authorization: Bearer <IAM_token>' \
        -H 'bluemix-instance: <instance_ID>' \
        -H 'content-type: application/vnd.ibm.kms.key_action+json' \
        -d '{
         'payload: <your_key_material>'
       }'
    ```
    {: codeblock}

  Replace the variables in the example request according to Table 1. 

5. Optionally, verify that the key was rotated by running the following call to browse the keys in your Key Protect service instance. 

  ```cURL
   curl -X GET \
     https://<region>.kms.cloud.ibm.com/api/v2/keys/metadata \
     -H 'accept: application/vnd.ibm.collection+json' \
     -H 'authorization: Bearer <IAM_token>' \
     -H 'bluemix-instance: <instance_ID>'
  ```
  {: codeblock}

  Review the `lastRotateDate` and `keyVersion` values in the response to inspect the date and time that your key was last rotated. The `keyVersion` attribute contains identifying information that describes the latest version of the root key.

| Variable | Description |
|----------|-------------|
| region | **Required.** The region abbreviation, such as _us-south_ or _eu-gb_, that represents the geographic area where your Key Protect instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| key_ID  | **Required.** The unique identifier for the root key that you want to rotate. |
| IAM_token | **Required.** Your IBM Cloud access token. Include the full contents of the IAM token, including the  Bearer value, in the cURL request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token) |
| instance_ID | **Required.** The unique identifier that is assigned to your Key Protect service instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID). |
| key_material | The new base64 encoded key material that you want to store and manage in the service. This value is required if you initially imported the key material when you added the key to the service. |
| | To rotate a key that was initially generated by Key Protect, omit the `payload` attribute and pass an empty request entity-body. To rotate an imported key, provide a key material that meets the following requirements: |
| | <ul><li>The key must be 128, 192, or 256 bits.</li><li>The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.</li></ul> |
{: caption="Table 1. Variables for key rotation" caption-side="top"}

When your root keys are successfully rotated, an HTTP `204 No Content` response is returned, which indicates that your root key was replaced by new key material.

When you unwrap (decrypt) a wrapped data encryption key (WDEK) by using a rotated root key, the service returns a new ciphertext in the response entity-body. Each ciphertext remains available for unwrap actions. If you unwrap a DEK with a previous ciphertext, the service also returns the latest ciphertext and latest key version in the response. Use the latest ciphertext for future unwrap operations.
{:note}

### Listing root key versions
{:#api-byok-list-root-key}

To see all versions of a root key that you rotated, make a `GET /keys/<key_ID>/versions` call. The older versions remain valid for decrypting existing resources but can't be used to encrypt new ones. They are removed from the list upon their expiration date or when there are no more resources encrypted by them. For an example request and response, see [List key versions](/apidocs/key-protect#list-key-versions). 

To find out more about programmatically managing your keys, see the [Key Protect API reference](/apidocs/key-protect).

## Next steps
{: #byok-key-rotation-next-steps}

[Track and manage your root keys](/docs/vpc?topic=vpc-vpc-encryption-managing).

