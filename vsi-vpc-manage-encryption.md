---

Copyright:
  years: 2019, 2021
lastupdated: "2021-01-28"

keywords: block storage, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# Managing data encryption
{: #vpc-encryption-managing}

Take actions to manage your customer-managed encryption that secures your resources with your own root keys. View Activity Tracker events to verify key rotation. Disable or delete root keys that have been compromised or that you no longer need. Optionally, make your data inaccessible after setting up customer-managed encryption.
{:shortdesc}

## Managing root keys
{: #byok-manage-root-keys}

Manage your root keys by taking the following actions:

* View the association of root keys to the resources they protect by [viewing root key registrations](#byok-root-key-registration).
* View information showing your root keys have been rotated by looking at [root key registration](#byok-root-key-verify-rotation) in the KMS instance.
* [Rotate your root keys](/docs/vpc?topic=vpc-vpc-key-rotation) at regular intervals or manually rotate imported root keys. Shortening the crypto period of a key reduces the possibility of a security breach.
* Decide whether importing your own root key or using a KMS-generated root key is preferable. If you want to set up a rotation policy for automatic key rotation, you must use KMS-generated root keys.
* See what happens to your [root key state](#byok-root-key-states) when you take certain actions, such as disabling the key.
* Decide when [disabling](#byok-disable-root-keys) or [deleting](#byok-delete-root-keys) a root key is necessary. When you rotate keys, the former key remains active and is still used to decrypt existing resources. Take precautions when disabling or deleting root keys.
* Enable a key that's been disabled or [restore a deleted key](#byok-restore-root-key).
* Decide whether you might want to make your data temporarily inaccessible by [removing service authorization](#instance-byok-inaccessible-data).
* Review the [Activity Tracker](#byok-activity-tracker-events) to verify events as you manage the lifecycle of your keys.

### Viewing root key registrations
{: #byok-root-key-registration}

Block storage volumes and custom images that are encrypted with your key are registered against the root key in the key management service (Key Protect or HPCS). Registration lets you map your resources to their associated encryption keys. You can quickly see which resources are protected by a root key. You can also access the risk involved in disabling or deleting a key by viewing which keys are actively protecting data.

For more information, see:

* Key Protect - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/key-protect?topic=key-protect-view-protected-resources)
* HPCS - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/hs-crypto?topic=hs-crypto-view-protected-resources)

### Verifying root key rotation by using the UI
{: #byok-root-key-verify-rotation}

When you create a customer-managed encryption volume or custom image, your root key is automatically registered in the KMS instance. You can view this information to verify the key has been rotated by using the UI. The following procedure shows how  to verify key rotation for a block storage volume, but steps are similar for other resources.

1. From the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes** and click on the name of a volume to see its details.
1. For Encryption, you'll see the name of the KMS and **customer-managed**, for example _Key Protect - Customer-managed_.
1. In the Encryption Instance field, click the link of the KMS instance you provisioned for the root key protecting this volume. Information about that KMS instance shows, which includes the name and ID of the root key.

  If you created your KMS instance using a private endpoint, these instances and associated root keys do not appear in the UI. Use the Key Protect or HPCS CLI or API to verify key rotation instead.
  {:note}

1. Click  **Associated Resources**. You'll see the following information for the root key in the KMS instance:
  * Key Name
  * Key ID
  * Cloud Resource Name (CRN) of the associated resource (i.e., volume or custom image). If you have more than one resource using this root key, they appear in the list.
1. In the Details column, click the arrow to expand the information. You'll see:
  * Description of the resource, if you provided one.
  * Key version ID
  * Key version date
  * Retention policy

After you [rotate the key](/docs/vpc?topic=vpc-vpc-key-rotation), the version ID field and key version date change, indicating the key has been rotated. The rotated key retains its original name and ID in the list of KMS instances.

### User actions that impact root key states and resource status
{: #byok-root-key-states}

Root keys move to various states depending on the action you take and impacts resources differently. The following describes what happens to a resource when you disable, enable, delete, and restore a root key.

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
|<td colspan=4>Root key state = _suspended_<sup>1</sup></td> | | |
| All | -- | The key can't be used to encrypt new resources or unwrap (decrypt) passphrases protecting existing resources, until you enable the key. For more information, see [Disabling root keys](#byok-disable-root-keys). |
| Custom image | _unusable_ | Images cannot be used for creating boot volumes during instance provisioning. |
| Boot volume | _available_ | Boot volumes remain encrypted with the suspended key. If you stop the instance using that boot volume, it won't restart. |
| Data volume |  _available_ | Data volumes remain encrypted, attached, and available until you stop the instance. Standalone data volumes that are encrypted by the suspended key can't be attached to an instance. |
| Instance |  _available_ | Instance workloads remain running with an _available_ status in the CLI and API, and with a _running_ status in the UI. If you stop instances, they won't restart. |
{: class="simple-tab-table"}
{: caption="Table 1. Disable root key" caption-side="top"}
{: #keystatetable1}
{: tab-title="Disable root key"}
{: tab-group="IAM-simple"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
|<td colspan=4>Root key state = _active_</td> | | |
| All | -- | The root key is available to unwrap passphrases protecting existing resources and for encrypting new resources. |
| Custom image | _active_ | Images can be used for creating virtual server instances. |
| Boot volume | _available_ | Boot volumes are available for startng instances. |
| Data volume | _available_ | Data volumes can be attached to instances. |
| Instance | _available_ | Instances can be restarted. |
{: caption="Table 2. Enable root key" caption-side="top"}
{: #keystatetable2}
{: tab-title="Enable root key"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
|<td colspan=4>Root key state = _destroyed_<sup>2</sup></td> | | |
| All | _unusable_ | The key can't be used for any encryption actions. You have 30 days to restore a deleted root key that you imported, afterwhich the status changes to _failed_ and the resources are no longer recoverable. (KMS-generated root keys can't be restored.) Review the Activity Tracker events to see when you deleted the key. For more information, see [Deleting root keys](#byok-delete-root-keys) |
| Custom image | _unusable_ | Images can't be used for creating boot volumes to provision a new virtual server instance. |
| Boot volume | _unusable_ | Associated virtual server instances are stopped. The boot volume can't be used to boot up instances. |
| Data volume | _unusable_ | Instances are stopped and data volumes are detached. Standalone data volumes can't be attached to instances. You can delete the volume. |
| Instance | _unusable_ | Instances with a deleted boot volume that were automatically stopped will not restart. |
{: caption="Table 3. Delete root key" caption-side="top"}
{: #keystatetable3}
{: tab-title="Delete root key"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
|<td colspan=4>Root key state = _active_</td> | | |
| All | -- | The previously-deleted key is moved from the _destroyed_ state back to the _active_ state where it can be used for all encryption actions. For more information, see [Restoring root keys](#byok-restore-root-key). |
| Custom image | _active_ | Images can be used for creating virtual server instances. |
| Boot volume | _available_ | Boot volumes are available for starting instances. |
| Data volume | _available_ | Data volumes can be attached to instances. |
| Instance | _available_ | Instances can be restarted. |
{: caption="Table 4. Restore root key" caption-side="top"}
{: #keystatetable4}
{: tab-title="Restore root key"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

<sup>1</sup> Volume and image status of _suspended_ is used for the fraud and abuse cases, where IBM has taken action to make the account resource inaccessible.

<sup>2</sup> A root key can be deleted from three states, _active_, _suspended_, or _deactivated_. (The _deactivated_ state occurs automatically when a key's expiration date is reached.) Regardless of the state prior to deletion, keys can be restored.

For more information about root key states from a KMS perspective see:

* [Key Protect - Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions)
* [HPCS - Key states and transitions](/docs/hs-crypto?topic=hs-crypto-key-states#key-transitions)

### Disabling root keys
{: #byok-disable-root-keys}

When you disable a root key, you suspend its encryption and decryption operations. Disabling a root key in your KMS places the key in a _suspended_ state in the KMS, which makes it unusable for protecting a resource. The [statuses of your resources](#byok-root-key-states) change accordingly; the resources become unusable for normal operations. Temporarily disabling a root key is good practice if you suspect possible security exposure, compromise, or breach of your data. You can enable a disabled root key when the security risk is no longer active.

When you disable a root key, workloads remain running in virtual server instances and boot volume volumes remain encrypted. Data volumes remain attached. However, if you stop the virtual server instance and then restart it, it won't restart. You also can't provision a new resource using a disabled key. As best practice, verify which resources are being protected by that root key before disabling it.

To see which root keys are disabled, look in the UI list of resources. The status of the resource will be _unusable_. The UI tooltip displays "key suspended". In the API, you'll see an *encryption_key_disabled* reason code.

You can enable a root key that's in a _suspended_ state from [Key Protect](/docs/key-protect?topic=key-protect-disable-keys#enable-ui) or [HPCS](/docs/hs-crypto?topic=hs-crypto-disable-keys#enable-ui). Enabling a root key returns the key to an _active_ state. You can also [delete a suspended key](#byok-delete-root-keys) or [restore the deleted key](#byok-restore-root-key) if necessary.

For more information about disabling root keys, see:

* [Key Protect - Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys)
* [HPCS - Disabling root keys](/docs/hs-crypto?topic=hs-crypto-disable-keys)

### Deleting root keys
{: #byok-delete-root-keys}

When you delete a root key, the key is no longer available to decrypt passphrases used to protect your resources. Deleting a root key places it in a _destroyed_ state in the KMS. All resources protected by the deleted root key have an _unusable_ status and can't be used for normal operations.

Your data still exists. You have a 30-day grace period to [restore the deleted key](#byok-restore-root-key). Otherwise, your encrypted resources become inaccessible. After the 30-day grace period, your root key can't be restored and your resources are unrecoverable.
{:important}

By default, the KMS prevents you from deleting a root key that's actively protecting a resource. Key Protect and HPCS let you **force delete** a root key. To force delete in HPCS, use the API only. Also, see [this troubleshooting issue](/docs/hs-crypto?topic=hs-crypto-troubleshoot-unable-to-delete-keys) for deleting root keys in HPCS. HPCS requires that you delete all resources before you delete a root key protecting those resources.

When you force delete a root key, the following actions happen automatically:

* If the deleted root key is protecting boot volumes, the associated virtual server instance is stopped.
* If the deleted root key is protecting data volumes, the associated virtual server instance is stopped and the volume is detached.
* Deleting a root key purges usage of the key for all resources in the VPC.
* Events are logged in the Activity Tracker.

Block storage volumes and custom images with a deleted root key appear in the list of resources with an _unusable_ status. The API reason code is *encryption_key_deleted*.

The following conditions result:

* All instances with an unusable boot volume will not restart.
* You can't attach unusable data volumes to an instance.
* You can't provision new instances from unusable images.
* Billing continues for unusable resources until you delete them.

As a best practice, before you force delete a root key, review all resources that are associated with that root key. Consider [temporarily disabling the key](#byok-disable-root-keys) instead of deleting it to suspend using that the root key. Root keys can be restored within 30 days, but they must be imported root keys, not KMS generated.
{:important}

For more information about deleting root keys, see:

* [Key Protect - Deleting keys](/docs/key-protect?topic=key-protect-delete-keys)
* [HPCS - Deleting keys](/docs/hs-crypto?topic=hs-crypto-delete-keys)

To see a list of deleted root keys and optionally take action to restore imported root keys, see [Restoring deleted root keys](#byok-restore-root-key).

### Restoring deleted root keys
{: #byok-restore-root-key}

Restoring a previously-deleted root key that you imported to the KMS returns the key to an _active_ state from a _destroyed_ state and re-establishes access the key. (Root keys that were KMS generated can't be restored.) You can restore access to all resources previously protected by the root key. You can resume regular actions, such as restarting an instance and attaching data volumes.

To see a list of deleted root keys for a KMS instance, you can filter by key state. For example, to filter a list of deleted root keys in Key Protect by using the UI:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external} **Resource List**, under **Services**, locate and click on the KMS instance.
1. Click **Manage keys**. A list of root keys for this instance displays.
1. Click the **Filter icon![Filter icon](/images/filter-icon.png)** to see more information.
1. Under **Key states**, the default is _Show all_. Click the down arrow for more key states.
1. Select **Destroyed**.

The list of root keys is refreshed, showing all root keys in a _destroyed_ key state, most recently deleted key at the top of the list.

To restore a root key:

1. Click the **overflow menu** (![Filter icon](/images/overflow.png)).
1. Select **Restore key**.

To complete the key restoration process, you need the key ID that was associated with the key and original key material that you imported.

You also need to create new volume attachments for your data volumes. The volume attachment IDs will be different than before you deleted the root key.

For more information about restoring root keys in a KMS instance, see:

* [Key protect - Restoring keys](/docs/key-protect?topic=key-protect-restore-keys)
* [HPCS - Restoring keys](/docs/hs-crypto?topic=hs-crypto-restore-keys)

### Using the UI to manage root keys
{: #byok-ui-root-key}

You can use the UI to disable, enable, delete, or restore your root keys. Table 5 describes each action and links to detailed steps for performing the action using the UI in Key Protect and HPCS. For information about the relationship of user actons to key states, see [Root key states and user actions](#byok-root-key-states).

| User action | Key Protect UI procedure | HPCS UI procedure |
|-------------|--------------------------|-------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys#disable-ui) | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys#disable-ui) |
| Enable key | [Enabling a root key](/docs/key-protect?topic=key-protect-disable-keys#enable-ui) | [Enabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys#enable-ui) |
| Delete key | [Deleting keys in the console (single authorization)](/docs/key-protect?topic=key-protect-delete-keys#delete-key-gui) | [Deleting keys with the GUI (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys#delete-keys-gui) |
| | [Deleting a key using dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys#delete-dual-auth-keys-api) | [Authorize deletion for a key with the GUI (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys#set-key-deletion-console) |
| Restore key | [Restoring a deleted key with the console](/docs/key-protect?topic=key-protect-restore-keys#restore-ui) | [Restoring a deleted key with the GUI](/docs/hs-crypto?topic=hs-crypto-restore-keys#restore-ui) |
{: caption="Table 5. UI procedures for managing root keys" caption-side="top"}

### Using the API to manage root keys
{: #byok-api-root-key}

You can use the API to disable, enable, delete, or restore your root keys. Table 6 describes each action and links to detailed steps for performing the action using the Key Protect or HPCS API. For information about the relationship of user actons to key states, see [Root key states and user actions](#byok-root-key-states).

Because deleting a root key makes all resources protected by it unusable (status = `unusable`), program your apps to check the status of the resource such as a volume or image before using it. If the root key is restorable, restore it for use or create and use a new root key.
{: tip}

| User action | Key Protect API procedure | HPCS API procedure |
|-------------|---------------------------|--------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys#disable-api) | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys#disable-api) |
| Enable key | [Enabling a disabled root key](/docs/key-protect?topic=key-protect-disable-keys#enable-api) | [Enabling a disabled root key](/docs/hs-crypto?topic=hs-crypto-disable-keys#enable-api) |
| Delete key | [Deleting keys with the API (single authorization)](/docs/key-protect?topic=key-protect-delete-keys#delete-key-api) | [Deleting keys with the API (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys#delete-keys-api) |
| | [Authorize deletion for a key with the API (dual authorization)](/docs/key-protect?topic=key-protect-delete-dual-auth-keys#set-key-deletion-api) | [Authorize deletion for a key with the API (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys#set-key-deletion-api) |
| Restore key | [Restoring a deleted key with the API](/docs/key-protect?topic=key-protect-restore-keys#restore-api) | [Restoring a deleted key with the API](/docs/hs-crypto?topic=hs-crypto-restore-keys#restore-api) |
{: caption="Table 6. API procedures for managing root keys" caption-side="top"}

### Removing service authorization to a root key
{: #instance-byok-inaccessible-data}

You can make your data inaccessible but retain it on the cloud by removing authorization to use that root key.

When you [authorize use](/docs/account?topic=account-serviceauth#serviceauth) of your root key, you grant permission for IBM to use the key to encrypt your resource. Authorization is done at the key management service level through IAM, when you authorize service between your service (for example, Cloud Block Storage) and the key management service you set up. (for example, {{site.data.keyword.keymanagementserviceshort}}).

You can remove any authorization between services in your account when you have the Administrator role on the target service (in this case, the key management service). If you remove any access policies created by the source service for its dependent services, the source service is unable to complete the workflow or access the target service.

Because the root keys are under your control, you don't have to contact IBM to remove authorization.

Do not remove IAM authorization between Cloud Block Storage and the KMS instance and then delete a block storage or image resource. This causes the root key in the KMS instance to remain registered with the deleted resource. You must delete all BYOK volumes and images before removing IAM authorization.
{: important}

To make your data inaccessible, but retain it on the IBM Cloud:

1. [Remove IAM authorization](/docs/account?topic=account-serviceauth#remove-auth) from the source Cloud Block Storage service to your target key management service instance.
2. [Stop all virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances#stop-and-start) that have attached encrypted volumes secured by that root key.

You can also [disable a root key](#byok-disable-root-keys), which suspends the key and temporarily revokes access to it.

## Viewing events in the Activity Tracker
{: #byok-activity-tracker-events}

Use the Activity Tracker to verify user-initiated activities that change the state of your key management service.

Due to the sensitivity of the information for an encryption key, when an event is generated as a result of an API call to the key management service, the event that is generated does not include detailed information about the key. Instead, it includes a correlation ID that you can use to identify the key internally in your cloud environment. The correlation ID is a field that is returned as part of the `correlationId` field.

### Activity Tracker events for key rotation
{: #byok-key-rotation-events}

When you initiate activity in the KMS to rotate and manage your root keys, Activity Tracker events are generated. These events are particular to {{site.data.keyword.keymanagementserviceshort}}, but similar events are generated for HPCS.

* When you list keys, a `kms.secrets.list` event is generated.
* When you rotate a root key, a `kms.secrets.rotate` event is generated.
* When you manually rewrap a data encryption key (DEK) using the new key material, a `kms.secrets.rewrap` event is generated.
* If you initially imported a root key using an import token and use the import token to rotate a key, a `kms.secrets.ack-rotate` event is generated.
* When you retrieve a key, the `requestData.keyType` field includes the type of key that was retrieved.
* The `responseData.keyState` field includes the integer that correlates to the state of the key, which include these key state values: Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5. For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
* When you authorize that a key be deleted, a `kms.secrets.setkeyfordeletion` event is generated. The `responseData.keyState` field includes the integer that correlates to deleted state (5).
* The `responseData.totalResources` field includes the total amount of key versions associated with the key.
* The `responseData.eventAckData.newKeyVersionId ` field includes the unique identifier of the latest key version.

For additional key rotation events that indicate a successful rotation, see these [key rotation events](/docs/key-protect?topic=key-protect-at-events#rotate-key-registrations-success). For information about all Activity Tracker events in {{site.data.keyword.keymanagementserviceshort}}, see [Activity Tracker events](/docs/vpc?topic=vpc-at-events).

### Example key rotation event
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

### Activity Tracker events for key suspension and deletion
{: #byok-key-delete-suspend-events}

When you initiate activity in the KMS to disable or delete a root key, specific Activity Tracker events are generated. These events are particular to {{site.data.keyword.keymanagementserviceshort}}, but similar events are generated for HPCS.

* When you disable a key (key state changes from _active_ to _suspended_), a `kms.secrets.disable` event is generated.
* When you enable a disabled key (key state changes from _suspended_ to _active_), a `kms.secrets.enable` event is generated.
* When you delete a key (key state changes to _destroyed_), a `kms.secrets.delete` event is generated.
* When you restore a deleted key (key state changes from _destroyed_ to _active_), a `kms.secrets.restore` event is generated.

For more information, see all [Activity Tracker key events](/docs/key-protect?topic=key-protect-at-events#key-actions)

## Next Steps
{: #next-steps-byok-manage}

* [Create volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Create an instance with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok)
