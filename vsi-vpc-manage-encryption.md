---

Copyright:
  years: 2019, 2020
lastupdated: "2020-08-04"

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

# Managing customer-managed encryption
{: #vpc-encryption-managing}

Take actions to manage your customer-managed encryption that secures your resources with your own root keys. View Activity Tracker events to verify key rotation. Disable or delete root keys that have been compromised or that you no longer need. Optionally, make your data inaccessible after setting up customer-managed encryption.
{:shortdesc}

## Managing root keys
{: #byok-manage-root-keys}

Manage your root keys by taking the following actions: 

* View information showing your root keys have been rotated by looking at [root key registration](#byok-root-key-registration) in the KMS instance. 
* [Rotate your root keys](/docs/vpc?topic=vpc-vpc-key-rotation) at regular intervals, including imported keys that you rotate manually. Shortening the crypto period of a key reduces the possibility of a security breach.
* Decide whether importing your own root key or using a KMS-generated root key is preferable. If you want to set up a rotation policy for automatic key rotation, you must use KMS-generated root keys.
* Review the [Activity Tracker](#byok-key-rotation-activity-tracker-events) to verify events as you manage the lifecycle of your keys.
* Decide when you might want to make your data [temporarily inaccessible](#instance-byok-inaccessible-data) but retain it on the cloud.
* Decide when [disabling](#byok-disable-root-keys) or [deleting](#byok-delete-root-keys) a root key is necessary. When you rotate keys, the former key remains active and is still used to decrypt existing resourcs. Take precautions when disabling or deleting root keys.

### Verifying root key rotation by using the UI
{: #byok-root-key-registration}

When you [create a customer-managed encryption volume](/docs/vpc?topic=vpc-block-storage-vpc-encryption#data-vol-encryption-ui), your root key is automatically registered in the KMS instance. You can view this information to verify the key has been rotated by using the UI:

1. From the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes** and click on the name of a volume to see its details.
1. For Encryption, you'll see the name of the KMS and **customer-managed**, for example _Key Protect - Customer-managed_. 
1. In the Encryption Instance field, click the link of the KMS instance you provisioned for the root key protecting this volume. Information about that KMS instance shows, which includes the name and ID of the root key.
1. Click the overflow menu (hellipsis) and select **Key Associated Resource**. On this page, you'll see the following information for the root key in the KMS instance:
  * Name
  * Key ID
  * Key version ID
  * Key version date
  
   <br>You'll also see the CRN of the key associated resource of the volume.</br> 

1. Click the expansion icon for the volume CRN. You can see the key version ID and root key name, which match the preceding information.

When a root key is [rotated](/docs/vpc?topic=vpc-vpc-key-rotation) manually or automatically based on a schedule, the version ID field and key version date change, indicating the key has been rotated. The rotated key retains its original name and ID in the list of KMS instances. 

### Disabling root keys
{: #byok-disable-root-keys}

[Disabling a key](/docs/key-protect?topic=key-protect-disable-keys) places it in a _suspended_ state, which revokes access to the key's associated data on the cloud. When you disable a root key, you suspend its encryption and decryption operations. Temporarily disabling a root key is good practice if you suspect a possible security exposure, compromise, or breach with your data. You can enable a disabled root key when the security risk is no longer active.

When you suspend a root key in your key management service, your workloads remain running in your virtual server instance and volumes remain encrypted. However, if you power down the VM and then power it back on, any instances with encrypted boot volumes will not start. You can cancel the deletion if your key is in a suspended state. Look at the list of resources for the status of the key.

### Deleting root keys
{: #byok-delete-root-keys}

When you delete a root key, the key is no longer available to decrypt passphrases used to encrypt your resources. Therefore, your encrypted resources become unavailable.

Deleting a root key places it in a _destoyed_ key state. The root key can't be recovered. 

The KMS blocks the deletion of any key that's actively protecting a resource. Before you delete a key, review the resources that are associated with the key.

As best practice, make sure the volume data is no longer needed before deleting any root key. Consider [temporarily disabling the key](#byok-disable-root-keys) instead of deleting it to suspend access to the key's associated data.
{:important}

For more information about deleting root keys, see the [Key Protect](/docs/key-protect?topic=key-protect-delete-keys) or [HPCS](/docs/hs-crypto?topic=hs-crypto-delete-keys) documentation.

### Making your data inaccessible after setting up customer-managed encryption
{: #instance-byok-inaccessible-data}

You can make your data inaccessible but retain it on the cloud by removing authorization to use that root key. 

When you [authorize use](/docs/iam?topic=iam-serviceauth#serviceauth) of your root key, you grant permission for IBM to use the key to encrypt your resource. Authorization is done at the key management service level through IAM, when you authorize service between **Cloud Block Storage** and the key management service you set up (for example, {{site.data.keyword.keymanagementserviceshort}}).

You can remove any authorization between services in your account when you have the Administrator role on the target service (in this case, the key management service). If you remove any access policies created by the source service for its dependent services, the source service is unable to complete the workflow or access the target service.

Because they root keys are under your control, you don't have to contact IBM to remove authorization.

To make your data inaccessible, but retain it on the IBM Cloud:

1. [Remove IAM authorization](/docs/iam?topic=iam-serviceauth#remove-auth) from the source Cloud Block Storage service to your target key management service instance.
2. [Power off all virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances#stop-and-start) that have attached encrypted volumes secured by that root key.

You can also disable a root key, which suspends the key and temporarily revokes access to the key's associated data on the cloud. For more information, see [Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys)

## Viewing events in the Activity Tracker 
{: #byok-activity-tracker-events}

Use the Activity Tracker to verify user-initiated activities that change the state of your key management service. 

Due to the sensitivity of the information for an encryption key, when an event is generated as a result of an API call to the key management service, the event that is generated does not include detailed information about the key. Instead, it includes a correlation ID that you can use to identify the key internally in your cloud environment. The correlation ID is a field that is returned as part of the `correlationId` field.

### Important events for key rotation
{: #byok-key-rotation-events}

When you initiate activity in the KMS to rotate and manage your root keys, specific Activity Tracker events are generated. These events are particular to {{site.data.keyword.keymanagementserviceshort}}, but similar events are generated for HPCS.

* When you list keys, a `kms.secrets.list` event is generated.
* When you rotate a root key, a `kms.secrets.rotate` event is generated.
* When you manually rewrap a data encryption key (DEK) using the new key material, a `kms.secrets.rewrap` event is generated.
* If you initially imported a root key using an import token, and use the import token to rotate a key, a `kms.secrets.ack-rotate` event is generated.
* When you retrieve a key, the `requestData.keyType` field includes the type of key that was retrieved.
* The `responseData.keyState` field includes the integer that correlates to the state of the key, which include these key state values: Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5. For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
* When you authorize that a key be deleted, a `kms.secrets.setkeyfordeletion` event is generated. The `responseData.keyState` field includes the integer that correlates to deleted state (5).
* The `responseData.totalResources` field includes the total amount of key versions associated with the key.
* The `responseData.eventAckData.newKeyVersionId ` field includes the unique identifier of the latest key version. 

For additional key rotation events that indicate a successful rotation, see these [key rotation events](/docs/key-protect?topic=key-protect-at-events#rotate-key-registrations-success). For information about all Activity Tracker events in {{site.data.keyword.keymanagementserviceshort}}, see [Activity Tracker events](/docs/key-protecttopic=key-protect-at-events#rotate-key-registrations-success).

### Example
{: #byok-activity-tracker-key-rotation-example}

The following JSON example shows a `kms.secrets.rotate` event when a root key is rotated.

```
{
   "eventTime":"2020-06-22T15:36:16.7+0000",
   "outcome":"success",
   "message":"Key Protect: rotate secret my-secret-key1",
   "action":"kms.secrets.rotate",
   "initiator":{
      "id":"MYid-111100P9W2",
      "name":"john.doe@mycompany.com",
      "typeURI":"service/security/account/user",
      "credential":{
         "type":"token"
      },
      "host":{
         "address": "192.0.2.0"
      }
   },
   "target":{
      "id":"crn:v1:bluemix:public:kms:us-south:a/a12333e9bd28461a8c92385628efac9f:fd692647-43d0-4699-9f83-f39a54b1327b:key:76c2c9cb-9095-5a24-811b-0ef4b24ac4d5",
      "name":"my-secret-key1",
      "typeURI":"kms/secrets"
   },
   "observer":{
      "name":"ActivityTracker"
   },
   "reason":{
      "reasonCode":204,
      "reasonType":"No Content"
   },
   "severity":"warning",
   "requestData":{
      "requestURI":"/api/v2/keys/76c2c9cb-9095-5a24-811b-0ef4b24ac4d5?action=rotate\u0026include_resource=true",
      "instanceId":"fd692647-43d0-4699-9f83-f39a54b1327b"
   },
   "dataEvent":true,
   "saveServiceCopy":true,
   "correlationId":"ef56e7ed-abf5-4da4-bbf1-ac0d381891fd",
   "logSourceCRN":"crn:v1:bluemix:public:kms:us-south:a/a12333e9bd28461a8c92385628efac9f:fd692647-43d0-4699-9f83-f39a54b1327b::"
}
```
{:codeblock}

This event shows the updated volume after a successful key rotation:

```
{
  "payload": {
    "id": "496cbc9b-3758-4eff-bc7d-f71a51be0e1d",
    "eventTime": "2020-06-10T17:27:24.81+0000",
    "action": "is.volume.volume.update",
    "outcome": "success",
    "message": "Block Storage for VPC: update volume my-encrypted-volume1",
    "initiator": {
      "id": "keyreact",
      "typeURI": "service/security/account/user",
      "name": "",
      "host": {
        "address": "192.0.2.0/24172",
        "agent": "grpc-go/1.22.1"
      },
      "credential": {
        "type": "token"
      }
    },
    "target": {
      "id": "crn:v1:staging:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
      "typeURI": "is.volume/volume",
      "name": "my-at-rewrap-encrypted-volume1",
      "host": {}
    },
    "observer": {
      "name": "ActivityTracker"
    },
    "reason": {
      "reasonCode": 200,
      "reasonType": "OK"
    },
    "severity": "normal",
    "requestData": {
      "account_id": "82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f",
      "action": "is.volume.volume.update",
      "crn": "crn:v1:staging:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
      "generation": "gc",
      "id": "cc6d2924-3cd2-459c-bb5e-9e84287fb530",
      "requestPath": "",
      "resourceGroupID": "696cb88e9039453f86f0802c92446a60"
    },
    "correlationId": "496cbc9b-3758-4eff-bc7d-f71a51be0e1d"
  },
  "logSourceCRN": "crn:v1:staging:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
  "saveServiceCopy": true
}
```
{:codeblock}

## Next Steps
{: #next-steps-byok-manage}

* [Create volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Create an instance with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok)