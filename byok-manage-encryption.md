---

copyright:
  years: 2019, 2024
lastupdated: "2024-02-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing data encryption
{: #vpc-encryption-managing}

In {{site.data.keyword.cloud}}, you can secure your resources with your own root keys. You can rotate your root keys regularly or manually as needed. You can verify key rotation by viewing Activity Tracker events. You can disable or delete root keys that are compromised or that you no longer need. You can restore root keys that were deleted within 30 days of their deletion.
{: shortdesc}

## Manage root keys
{: #byok-manage-root-keys}

Manage your root keys by taking the following actions:

   * View the association of root keys to the resources that they protect by [viewing root key registrations](#byok-root-key-registration).
   * View key rotation information by looking at the [root key registration](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-root-key-verify-rotation) in the KMS instance.
   * [Rotate your root keys](/docs/vpc?topic=vpc-vpc-key-rotation) at regular intervals or manually rotate imported root keys. Shortening the crypto period of a key reduces the possibility of a security breach.
   * Decide whether importing your own HSM root key or generating a root key by the KMS is preferable. If you want to set up a rotation policy for automatic key rotation, you must use KMS-generated root keys.
   * See what happens to your [root key state](#byok-root-key-states) when you take certain actions, such as disabling the key.
   * Decide when the [disabling](#byok-disable-root-keys) or [deletion](#byok-delete-root-keys) of a root key is necessary. When you rotate the keys, the former key remains active, and is still used to decrypt existing resources. Take precautions when you disable or delete root keys.
   * Enable a key that was disabled or [restore a deleted key](#byok-restore-root-key).
   * Decide whether you might want to make your data temporarily inaccessible by [removing service authorization](#instance-byok-inaccessible-data).
   * Review the [Activity Tracker](#byok-activity-tracker-events) to verify events as you manage the lifecycle of your keys.

### View root key registrations
{: #byok-root-key-registration}

{{site.data.keyword.block_storage_is_short}} volumes, snapshots, {{site.data.keyword.filestorage_vpc_short}} shares, and custom images that are encrypted with your key are registered against the root key in the key management service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). By viewing the registration, you can map your resources to their associated encryption keys. You can quickly see which resources are protected by a root key. You can also assess the risk that is involved in disabling or deleting a key by viewing which keys are actively protecting data.

For more information, see the following topics.

* {{site.data.keyword.keymanagementserviceshort}} - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/key-protect?topic=key-protect-view-protected-resources)
* {{site.data.keyword.hscrypto}} - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/hs-crypto?topic=hs-crypto-view-protected-resources)

### Verify root key rotation in the UI
{: #byok-root-key-verify-rotation}
{: ui}

When you create volume, snapshot, file share, or custom image with customer-managed encryption, your root key is automatically registered in the KMS instance. You can view the registration to verify whether the key was rotated. The following procedure shows how to verify key rotation for a {{site.data.keyword.block_storage_is_short}} volume, but the steps are similar for other resources.

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**. The list shows all volumes and the Encryption column displays either "Provider Managed" or the name of the KMS that is used for custoner-managed encryption.
1. Click the name of a volume to see its details.
1. In the Encryption Instance field, click the link of the KMS instance. The KMS instance overview page is displayed.

   If you created your KMS instance by using a private endpoint, these instances and associated root keys do not appear in the UI. Use the {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} CLI or API to verify key rotation instead.
   {: note}

1. Click **KMS Keys** ({{site.data.keyword.hscrypto}}) or **Key** ({{site.data.keyword.keymanagementserviceshort}}). The list of all keys in the instance is displayed. The Last updated column shows the date when the key was created or rotated.
1. Click the Actions menu in the row of the key that you want to know more about. Select **View key details**. A side panel opens and information about the key is displayed. You can see the key name, key ID, CRN, creation date, version date, last updated date, and last rotated date. 

After you [rotate the key](/docs/vpc?topic=vpc-vpc-key-rotation), the version ID field and key version date are updated. The rotated key retains its original name and ID in the list of KMS instances.

### User actions that impact root key states and resource status
{: #byok-root-key-states}

Root keys move to various states as a result of the actions that you take, and different root key states impact resources differently. The following table describes what happens to a resource when you disable, enable, delete, and restore a root key.

| Resource type | Resource status | Result |
|---------------|-----------------|--------|
| Root key | _suspended_ [^tabletext1] |  |
| All | -- | The key can't be used to encrypt new resources or unwrap (decrypt) passphrases that are protecting existing resources until you enable the key. For more information, see [Disabling root keys](#byok-disable-root-keys). |
| Custom image | _unusable_ | Images cannot be used for creating boot volumes during instance provisioning. |
| Boot volume | _available_ | Boot volumes remain encrypted with the suspended key. If you stop the instance that uses that boot volume, it can't restart. |
| Data volume |  _available_ | Data volumes remain encrypted, attached, and available until you stop the instance. Stand-alone data volumes that are encrypted by the suspended key can't be attached to an instance. |
| Snapshot | _unusable_ | Snapshot cannot be used to restore a volume. |
| File shares | _stable_ | Data on the file share is accessible but you can't create a file Share with that key. |
| Instance |  _available_ | Instance workloads continue to run with an _available_ status in the CLI and API, and with a _running_ status in the UI. If you stop instances, they can't restart. |
{: class="simple-tab-table"}
{: caption="Table 1. Disable root key" caption-side="bottom"}
{: #keystatetable1}
{: tab-title="Disable root key"}
{: tab-group="Key-state-impact"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
| Root key | _active_ |  |
| All | -- | The root key is available to unwrap passphrases that are protecting existing resources, and to encrypt new resources. |
| Custom image | _active_ | Images can be used to create virtual server instances. |
| Boot volume | _available_ | Boot volumes are available to start instances. |
| Data volume | _available_ | Data volumes can be attached to instances. |
| Snapshot | _stable_ | The snapshot can be used to restore a volume. |
| File share | _stable_ | File share is available. You can add or remove mount targets. You can write and read data. |
| Instance | _available_ | Instances can be restarted. |
{: caption="Table 2. Enable root key" caption-side="bottom"}
{: #keystatetable2}
{: tab-title="Enable root key"}
{: tab-group="Key-state-impact"}
{: class="simple-tab-table"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
| Root key | _destroyed_ [^tabletext2] |  |
| All | _unusable_ | The key can't be used for any encryption actions. You have 30 days to restore a deleted root key that you imported. After 30 days, the status changes to _failed_ and the resources are no longer recoverable. (KMS-generated root keys can't be restored.) Review the Activity Tracker events to see when you deleted the key. For more information, see [Deleting root keys](#byok-delete-root-keys). |
| Custom image | _unusable_ | Images can't be used for creating boot volumes to provision a new virtual server instance. |
| Boot volume | _unusable_ | The associated virtual server instance is stopped. A stopped instance cannot be started while the instance boot volume is in an _unusable_ state. |
| Data volume | _unusable_ | If the data volume is attached to a running instance, the instance is stopped. Stand-alone data volumes cannot be attached to instances. You can delete the volume. |
| Snapshot | _unusable_ | The snapshot is inaccessible and can't be used to restore a volume. |
| File share | _suspended_ | The File Storage system is offline and data cannot be accessed. |
| Instance | _unusable_ | Instances with a deleted boot volume that were automatically stopped cannot restart. |
{: caption="Table 3. Delete root key" caption-side="bottom"}
{: #keystatetable3}
{: tab-title="Delete root key"}
{: tab-group="Key-state-impact"}
{: class="simple-tab-table"}

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
| Root key | _active_ |  |
| All | -- | The previously deleted key is moved from the _destroyed_ state back to the _active_ state where it can be used for all encryption actions. For more information, see [Restoring root keys](#byok-restore-root-key). |
| Custom image | _active_ | Images can be used for creating virtual server instances. |
| Boot volume | _available_ | Boot volumes are available for starting instances. |
| Data volume | _available_ | Data volumes can be attached to instances. |
| Snapshot | _stable_ | Snapshot that were encrypted with this root key can be used to restore a volume. |
| File shares | _stable_ | File share data is available. |
| Instance | _available_ | Instances can be restarted. |
{: caption="Table 4. Restore root key" caption-side="bottom"}
{: #keystatetable4}
{: tab-title="Restore root key"}
{: tab-group="Key-state-impact"}
{: class="simple-tab-table"}

[^tabletext1]: Volume and image status of _suspended_ is used for the fraud and abuse cases, where IBM makes the resource inaccessible.

[^tabletext2]: A root key can be deleted from three states, _active_, _suspended_, or _deactivated_. The _deactivated_ state occurs automatically when a key's expiration date is reached. Regardless of the state before deletion, keys can be restored.

For more information about root key states from a KMS perspective, see the following topics.

   * [{{site.data.keyword.keymanagementserviceshort}} - Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions)
   * [{{site.data.keyword.hscrypto}} - Key states and transitions](/docs/hs-crypto?topic=hs-crypto-key-states#key-transitions)

### Disable root keys
{: #byok-disable-root-keys}

When you disable a root key, you suspend its encryption and decryption operations. Disabling a root key in your KMS places the key in a _suspended_ state, which makes it unusable for protecting a resource. The resources become unusable for normal operations. Temporarily disabling a root key is good practice if you suspect possible security exposure, compromise, or data breach. You can enable a disabled root key when the security threat is no longer active.

When you disable a root key, workloads continue to run in virtual server instances, and boot volumes remain encrypted. Data volumes remain attached. File share data is encrypted. However, if you stop the virtual server instance, it can't be restarted. You also can't provision a resource by using a disabled key. Before you disable a key, it's best to verify which resources are being protected by it.

To see which root keys are disabled, look in the UI list of resources. The status of volume and snapshot resources are _unusable_. File shares in a _stable_ state remain so. The UI tooltip displays "key suspended" for the resource. In the API response, you can see an _encryption_key_disabled_ reason code.

You can [enable a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#enable-ui) that's in a _suspended_ state, which returns the key to an _active_ state. You can also [delete a suspended key](#byok-delete-root-keys) or restore the deleted key if necessary.

For more information about disabling root keys, the following topics.

   * [{{site.data.keyword.keymanagementserviceshort}} - Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys)
   * [{{site.data.keyword.hscrypto}} - Disabling root keys](/docs/hs-crypto?topic=hs-crypto-disable-keys)

### Deleting root keys
{: #byok-delete-root-keys}

When you delete a root key, the key is no longer available to decrypt passphrases that are used to protect your resources. Deleting a root key places it in a _destroyed_ or _deleted_ state in the KMS. Volume, snapshot, and image resources that are protected by the deleted root key have an _unusable_ status and can't be used for normal operations. {{site.data.keyword.filestorage_vpc_short}} shares show a _suspended_ status. The storage system is offline, and data cannot be accessed.

Your data still exists. You have a 30-day grace period to [restore the deleted key](#byok-restore-root-key). Otherwise, your encrypted resources become inaccessible. After 30 days, your root key can't be restored, and your resources are unrecoverable.
{: important}

By default, the KMS prevents you from deleting a root key that's actively protecting a resource. You can **force delete** a root key in {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}. When you force delete a root key, the following actions happen automatically:

   * If the deleted root key is protecting boot volumes, the associated virtual server instance is stopped.
   * If the deleted root key is protecting data volumes, the associated virtual server instance is stopped.
   * If the deleted root key is protecting file shares, the file share is suspended.
   * Deleting a root key purges usage of the key for all resources in the VPC.
   * Events are logged in the Activity Tracker.

To force the deletion of a root key in {{site.data.keyword.hscrypto}}, use the API. {{site.data.keyword.hscrypto}} requires that you delete all resources before you delete a root key that is protecting those resources. If you can't delete the key, see [troubleshooting key management service](/docs/hs-crypto?topic=hs-crypto-troubleshoot-unable-to-delete-keys).

{{site.data.keyword.block_storage_is_short}} volumes, snapshots, and custom images with a deleted root key appear in the list of resources with an _unusable_ status. {{site.data.keyword.filestorage_vpc_short}} shares show a _suspended_ status. The API reason code is _encryption_key_deleted_.

Deletion of the root key results in the following conditions.

   * All instances with an unusable boot volume do not restart.
   * You can't attach unusable data volumes to an instance.
   * You can't restore a volume from a snapshot.
   * You can't access a file share.
   * You can't provision instances from unusable images.
   * Billing continues for unusable resources until you delete them.

Before you force delete a root key, it's best to review all resources that are associated with that root key. Consider [temporarily disabling the key](#byok-disable-root-keys) instead of deleting it to suspend the use of that root key. Root keys can be restored within 30 days, but only if they are imported root keys, not KMS generated.
{: important}

For more information about deleting root keys, see the following topics.

   * [{{site.data.keyword.keymanagementserviceshort}} - Deleting keys](/docs/key-protect?topic=key-protect-delete-keys)
   * [{{site.data.keyword.hscrypto}} - Deleting keys](/docs/hs-crypto?topic=hs-crypto-delete-keys)

For more information about how to see the list of deleted root keys and possibly restore imported root keys, see [Restoring deleted root keys](#byok-restore-root-key).

### Restoring deleted root keys
{: #byok-restore-root-key}

Restoring an imported root key returns the key to an _active_ state, and reestablishes access to the key. (Root keys that were generated by the KMS can't be restored.) You can restore access to all resources that were previously protected by the root key. You can resume regular actions, such as restarting an instance and attaching data volumes.

To see a list of deleted root keys for a KMS instance, you can filter by key state. For example, to filter a list of deleted root keys in a {{site.data.keyword.keymanagementserviceshort}} instance in the UI:

1. Go to the [{{site.data.keyword.cloud_notm}} console](/vpc){: external} > **Resource List** > **Security**, 
1. Locate, and click the KMS instance.
1. On the **Keys** page, the list of root keys that are stored in this instance is displayed.
1. Click the **Filter icon![Filter icon](../icons/filter.svg)** to see more information.
1. Under **States**, click the down arrow to display all key states.
1. Select **Deleted**.

The list of root keys is refreshed, and shows all root keys in a _deleted_ key state. The most recently deleted key is at the first of the list.

To restore a root key:

1. Click **Actions**(![Actions icon](/images/overflow.png)).
1. Select the **Restore key**.

To complete the key restoration process, you need the key ID that was associated with the key and the original key material that you imported.

You also need to create volume attachments for your data volumes. The volume attachment IDs are different than before you deleted the root key.

For more information about restoring root keys in a KMS instance, see the following topics.

   * [{{site.data.keyword.keymanagementserviceshort}} - Restoring keys](/docs/key-protect?topic=key-protect-restore-keys)
   * [{{site.data.keyword.hscrypto}} - Restoring keys](/docs/hs-crypto?topic=hs-crypto-restore-keys)

### Manage root keys in the UI
{: #byok-ui-root-key}
{: ui}

You can use the UI to disable, enable, delete, or restore your root keys. Table 5 describes each action and links to detailed steps for {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}. For more information about the relationship of user actions and key states, see [Root key states and user actions](#byok-root-key-states).

| User action | {{site.data.keyword.keymanagementserviceshort}} UI procedure | {{site.data.keyword.hscrypto}} UI procedure |
|-------------|--------------------------|-------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#disable-ui). | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#disable-ui). |
| Enable key | [Enabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#enable-ui). | [Enabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#enable-ui). |
| Delete key | [Deleting keys in the console (single authorization)](/docs/key-protect?topic=key-protect-delete-keys&interface=ui). | [Deleting keys with the GUI (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys&interface=ui#delete-keys-gui). |
| | [Deleting a key with dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys&interface=api#delete-dual-auth-keys-api). | [Authorize deletion for a key with the GUI (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys&interface=ui#set-key-deletion-console). |
| Restore key | [Restoring a deleted key with the console](/docs/key-protect?topic=key-protect-restore-keys&interface=ui#restore-ui). | [Restoring a deleted key with the GUI](/docs/hs-crypto?topic=hs-crypto-restore-keys&interface=ui#restore-keys-ui). |
{: caption="Table 5. UI procedures for managing root keys" caption-side="bottom"}

### Manage root keys with the API
{: #byok-api-root-key}
{: api}

You can use the API to disable, enable, delete, or restore your root keys. Table 6 describes each action and links to detailed steps for the {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} API. For more information about the relationship of user actions and key states, see [Root key states and user actions](#byok-root-key-states).

Because deleting a root key makes all resources that are protected by it unusable (status = `unusable`), program your application to check the status of the resource (such as a volume, snapshot, or image) before it attempts to use it. If the root key is restorable, restore it for use, or create and use a different root key.
{: tip}

| User action | {{site.data.keyword.keymanagementserviceshort}} API procedure | {{site.data.keyword.hscrypto}} API procedure |
|-------------|---------------------------|--------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=api#disable-api) | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=api#disable-api) |
| Enable key | [Enabling a disabled root key](/docs/key-protect?topic=key-protect-disable-keys&interface=api#enable-api) | [Enabling a disabled root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=api#enable-api) |
| Delete key | [Deleting keys with the API (single authorization)](/docs/key-protect?topic=key-protect-delete-keys&interface=api#delete-keys-api) | [Deleting keys with the API (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys&interface=api#delete-keys-api) |
| | [Authorize deletion for a key with the API (dual authorization)](/docs/key-protect?topic=key-protect-delete-dual-auth-keys&interface=api#delete-dual-auth-keys-key-api) | [Authorize deletion for a key with the API (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys&interface=api#set-key-deletion-api) |
| Restore key | [Restoring a deleted key with the API](/docs/key-protect?topic=key-protect-restore-keys&interface=api#restore-api) | [Restoring a deleted key with the API](/docs/hs-crypto?topic=hs-crypto-restore-keys&interface=api#restore-keys-api) |
{: caption="Table 6. API procedures for managing root keys" caption-side="bottom"}

### Removing service authorization to a root key
{: #instance-byok-inaccessible-data}

You can make your data inaccessible but retain it on the cloud by removing IAM authorization to use that root key.

When you [authorize use](/docs/account?topic=account-serviceauth#serviceauth) of your root key, you grant permission for IBM to use the key to encrypt your resource. Authorization is done at the key management service level through IAM, when you authorize service between your service (for example, Cloud Block Storage) and the key management service.

You can remove any authorization between services in your account when you have the Administrator role on the target service (in this case, the key management service). If you remove an access policy that was created by the source service for its dependent services, the source service is unable to complete the workflow or access the target service.

Because the root keys are under your control, you don't need to contact IBM to remove authorization.

Do not remove IAM authorization between Cloud Block Storage and the KMS instance, and then delete a Block Storage volume, snapshot, or image resource. Such action causes the root key in the KMS instance to remain registered with the deleted resource. You must delete all BYOK volumes, snapshots, or images before you remove the IAM authorization.
{: important}

To make your data inaccessible, but retain it on the {{site.data.keyword.cloud_notm}}:

1. [Remove IAM authorization](/docs/account?topic=account-serviceauth&interface=ui#remove-auth) from the source Cloud Block Storage service to your target key management service instance.
1. [Stop all virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#stop-start-virtual-server-instances-ui) with attached encrypted volumes that are secured by that root key.

You can also [disable a root key](#byok-disable-root-keys), which suspends the key and temporarily revokes access to it.

## Viewing events in the {{site.data.keyword.at_full_notm}}
{: #byok-activity-tracker-events}

Use the Activity Tracker to verify user-initiated activities that change the state of your keys in the key management service.

Due to the sensitivity of the information, when an event is generated as a result of an API call to the KMS, the event doesn't include detailed information about the key. Instead, it includes a correlation ID that you can use to identify the key internally in your cloud environment. The correlation ID is a field that is returned as part of the `correlationId` field.

### Activity Tracker events for key rotation
{: #byok-key-rotation-events}

When you initiate an activity in the KMS to rotate and manage your root keys, Activity Tracker events are generated. These events are particular to {{site.data.keyword.keymanagementserviceshort}}, but similar events are generated for {{site.data.keyword.hscrypto}}, too.

* When you list keys, a `kms.secrets.list` event is generated.
* When you rotate a root key, a `kms.secrets.rotate` event is generated.
* When you manually rewrap a data encryption key (DEK) with new key material, a `kms.secrets.rewrap` event is generated.
* If you initially imported a root key by using an import token and use the import token to rotate a key, a `kms.secrets.ack-rotate` event is generated.
* When you retrieve a key, the `requestData.keyType` field includes the type of key that was retrieved.
* The `responseData.keyState` field includes the integer that correlates to the state of the key. The integers correlate with these key state values: Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5. For more information about key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
* When you authorize the deletion of a key, a `kms.secrets.setkeyfordeletion` event is generated. The `responseData.keyState` field includes the integer that correlates to the deleted state (5).
* The `responseData.totalResources` field includes the total number of key versions that are associated with the key.
* The `responseData.eventAckData.newKeyVersionId` field includes the unique identifier of the latest key version.

For more information about key rotation events that indicate a successful rotation, see these [key rotation events](/docs/key-protect?topic=key-protect-at-events#rotate-key-registrations-success). For more information about all Activity Tracker events in {{site.data.keyword.keymanagementserviceshort}}, see [{{site.data.keyword.at_full_notm}} events](/docs/vpc?topic=vpc-at-events).

### Example key rotation event
{: #byok-activity-tracker-key-rotation-example}

The following JSON example shows a `kms.secrets.rotate` event when a root key is rotated.

```json
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
{: screen}

This event shows the updated volume after a successful key rotation:

```json
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
      "id": "crn:v1:bluemix:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
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
      "crn": "crn:v1:bluemix:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
      "generation": "gc",
      "id": "cc6d2924-3cd2-459c-bb5e-9e84287fb530",
      "requestPath": "",
      "resourceGroupID": "696cb88e9039453f86f0802c92446a60"
    },
    "correlationId": "496cbc9b-3758-4eff-bc7d-f71a51be0e1d"
  },
  "logSourceCRN": "crn:v1:bluemix:public:is:us-south-1:a/82a90f2e-39e1-4a18-a8dc-6ebc17b61d7f::volume:cc6d2924-3cd2-459c-bb5e-9e84287fb530",
  "saveServiceCopy": true
}
```
{: screen}

### Activity Tracker events for key suspension and deletion
{: #byok-key-delete-suspend-events}

When you initiate activity in the KMS to disable or delete a root key, specific Activity Tracker events are generated. These events are particular to {{site.data.keyword.keymanagementserviceshort}}, but similar events are generated for {{site.data.keyword.hscrypto}}.

* When you disable a key (key state changes from _active_ to _suspended_), a `kms.secrets.disable` event is generated.
* When you enable a disabled key (key state changes from _suspended_ to _active_), a `kms.secrets.enable` event is generated.
* When you delete a key (key state changes to _destroyed_), a `kms.secrets.delete` event is generated.
* When you restore a deleted key (key state changes from _destroyed_ to _active_), a `kms.secrets.restore` event is generated.

For more information, see all [Activity Tracker key events](/docs/key-protect?topic=key-protect-at-events#key-actions).

## Next Steps
{: #next-steps-byok-manage}

* [Create volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create an instance with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
