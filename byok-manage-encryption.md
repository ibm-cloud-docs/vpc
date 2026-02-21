---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-21"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing data encryption
{: #vpc-encryption-managing}

In {{site.data.keyword.cloud}}, you can secure your resources with your own root keys. You can rotate your root keys regularly or manually as needed. You can disable or delete root keys that are compromised or that you no longer need. You can restore root keys that were deleted within 30 days of their deletion.
{: shortdesc}

Manage your root keys by taking the following actions:

   * Decide whether importing your own HSM root key or generating a root key by the KMS is preferable. If you want to set up a rotation policy for automatic key rotation, you must use KMS-generated root keys.
   * View the association of root keys to the resources that they protect by [viewing root key registrations](#byok-root-key-registration).
   * Decide when the [disabling](#byok-disable-root-keys) or [deletion](#byok-delete-root-keys) of a root key is necessary. Use precautions when you disable or delete root keys.
   * Enable a key that was disabled or [restore a deleted key](#byok-restore-root-key).
   * [Rotate your root keys](/docs/vpc?topic=vpc-vpc-key-rotation) at regular intervals or manually rotate imported root keys. Shortening the crypto period of a key reduces the possibility of a security breach.
   * Decide whether you might want to make your data temporarily inaccessible by [removing IAM service authorization](#instance-byok-inaccessible-data).

## Viewing root key registrations
{: #byok-root-key-registration}

{{site.data.keyword.block_storage_is_short}} volumes, snapshots, {{site.data.keyword.filestorage_vpc_short}} shares, and custom images that are encrypted with your key are registered against the root key in the key management service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). By viewing the registration, you can map your resources to their associated encryption keys. You can quickly see which resources are protected by a root key. You can also assess the risk that is involved in disabling or deleting a key by viewing which keys are actively protecting data.

For more information, see the following topics.

* {{site.data.keyword.keymanagementserviceshort}} - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/key-protect?topic=key-protect-view-protected-resources)
* {{site.data.keyword.hscrypto}} - [Viewing associations between root keys and encrypted IBM Cloud resources](/docs/hs-crypto?topic=hs-crypto-view-protected-resources).

## Managing root keys in the console
{: #byok-ui-root-key}
{: ui}

You can use the UI to disable, enable, delete, restore, or rotate your root keys. The following table describes each action and links to detailed steps for {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}.

| User action | {{site.data.keyword.keymanagementserviceshort}} UI procedure | {{site.data.keyword.hscrypto}} UI procedure |
|-------------|--------------------------|-------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#disable-ui). | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#disable-ui). |
| Enable key | [Enabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#enable-ui). | [Enabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#enable-ui). |
| Delete key | [Deleting keys in the console (single authorization)](/docs/key-protect?topic=key-protect-delete-keys&interface=ui). | [Deleting keys with the GUI (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys&interface=ui#delete-keys-gui). |
| | [Deleting keys that have a dual auth policy](/docs/key-protect?topic=key-protect-manage-dual-auth&interface=ui#delete-dual-auth-keys). | [Authorize deletion for a key with the GUI (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys&interface=ui#set-key-deletion-console). |
| Restore key | [Restoring a deleted key with the console](/docs/key-protect?topic=key-protect-restore-keys&interface=ui#restore-ui). | [Restoring a deleted key with the GUI](/docs/hs-crypto?topic=hs-crypto-restore-keys&interface=ui#restore-keys-ui). |
| Manually rotate a key | [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys&interface=ui) | [Rotating root keys manually](/docs/hs-crypto?topic=hs-crypto-rotate-keys) |
{: caption="UI procedures for managing root keys" caption-side="bottom"}

## Managing root keys with the API
{: #byok-api-root-key}
{: api}

You can use the API to disable, enable, delete, restore, or rotate your root keys. The following table describes each action and links to detailed steps for the {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} API.

| User action | {{site.data.keyword.keymanagementserviceshort}} API procedure | {{site.data.keyword.hscrypto}} API procedure |
|-------------|---------------------------|--------------------|
| Disable key | [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=api#disable-api) | [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=api#disable-api) |
| Enable key | [Enabling a disabled root key](/docs/key-protect?topic=key-protect-disable-keys&interface=api#enable-api) | [Enabling a disabled root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=api#enable-api) |
| Delete key | [Deleting keys with the API (single authorization)](/docs/key-protect?topic=key-protect-delete-keys&interface=api#delete-keys-api) | [Deleting keys with the API (single authorization)](/docs/hs-crypto?topic=hs-crypto-delete-keys&interface=api#delete-keys-api) |
| | [Deleting keys that have a dual auth policy)](/docs/key-protect?topic=key-protect-manage-dual-auth&interface=api#delete-dual-auth-keys) | [Authorize deletion for a key with the API (dual authorization)](/docs/hs-crypto?topic=hs-crypto-delete-dual-auth-keys&interface=api#set-key-deletion-api) |
| Restore key | [Restoring a deleted key with the API](/docs/key-protect?topic=key-protect-restore-keys&interface=api#restore-api) | [Restoring a deleted key with the API](/docs/hs-crypto?topic=hs-crypto-restore-keys&interface=api#restore-keys-api) |
| Manually rotate a key | [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys&interface=ui) | [Rotating root keys manually](/docs/hs-crypto?topic=hs-crypto-rotate-keys) |
{: caption="API procedures for managing root keys" caption-side="bottom"}

Because deleting a root key makes all resources that are protected by it unusable (status = `unusable`), program your application to check the status of the resource (such as an image, volume, snapshot, or file share) before it attempts to use it. If the root key is restorable, restore it for use, or create and use a different root key.
{: tip}

## Customer root key states and resource statuses
{: #byok-root-key-states}

Root keys move to various states as a result of the actions that you take, and different root key states impact resources differently. The following sections describe how user actions affect the root key state, and how the root key state changes the statuses of the resources that are encrypted with that key.

For more information about root key states from a KMS perspective, see the following topics.

   * {{site.data.keyword.keymanagementserviceshort}} - [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
   * {{site.data.keyword.hscrypto}} - [Key states and transitions](/docs/hs-crypto?topic=hs-crypto-key-states#key-transitions).

### Disabling root keys
{: #byok-disable-root-keys}

When you disable a root key, you suspend its encryption and decryption operations. The encrypted resources become unusable for normal operations. Temporarily disabling a root key is good practice if you suspect possible security exposure, compromise, or data breach. You can enable a disabled root key when the security threat is no longer active.

When you disable a root key, workloads continue to run in virtual server instances, and boot volumes remain encrypted. Data volumes remain attached. File share data is encrypted. However, if you stop the virtual server instance, it can't be restarted. You also can't provision a resource by using a disabled key. Before you disable a key, it's best to verify which resources are being protected by it.

When the CRK is suspended, it can't be used to encrypt new resources or unwrap (decrypt) passphrases that are protecting existing resources.

| Resource type | Resource status | Result |
|---------------|-----------------|--------|
| Custom image | _unusable_ | Images cannot be used for creating boot volumes during instance provisioning. |
| Boot volume | _available_ | Boot volumes remain encrypted with the suspended key. If you stop the instance that uses that boot volume, it can't restart. |
| Data volume |  _available_ | Data volumes remain encrypted, attached, and available until you stop the instance. Stand-alone data volumes that are encrypted by the suspended key can't be attached to an instance. |
| Snapshot | _unusable_ | Snapshot cannot be used to restore a volume or file share. |
| File share | _stable_ | Data on the file share is accessible but you can't create a file share with that key. |
| Instance |  _available_ | Instance workloads continue to run with an _available_ status in the CLI and API, and with a _running_ status in the console. If you stop instances, they can't restart. |
{: caption="Disabled root key" caption-side="bottom"}

To see which root keys are disabled, look in the console list of resources. The status of volume and snapshot resources are _unusable_. File shares in a _stable_ state remain so. The UI tooltip displays "key suspended" for the resource. In the API response, you can see an _encryption_key_disabled_ reason code.

For more information about disabling a root key, see the following topics.

* {{site.data.keyword.keymanagementserviceshort}} - [Disabling a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#disable-ui).
* {{site.data.keyword.hscrypto}} - [Disabling a root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=ui#disable-ui).

### Enabling root keys
{: #byok-enable-root-keys}

You can enable a root key that's in a _suspended_ state, which returns the key to an _active_ state. 

The following table shows the statuses of the encrypted resources when the root key is active. The CRK is available to unwrap passphrases that are protecting existing resources, and to encrypt new resources.

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
| Custom image | _active_ | Images can be used to create virtual server instances. |
| Boot volume | _available_ | Boot volumes are available to start instances. |
| Data volume | _available_ | Data volumes can be attached to instances. |
| Snapshot | _stable_ | The snapshot can be used to restore a volume or share. |
| File share | _stable_ | File share is available. You can add or remove mount targets. You can write and read data. |
| Instance | _available_ | Instances can be restarted. |
{: caption="Enabled root key" caption-side="bottom"}

For more information about enabling a disabled root key, see the following topics.

* {{site.data.keyword.keymanagementserviceshort}} - [Enabling a disabled root key](/docs/key-protect?topic=key-protect-disable-keys&interface=api#enable-api).
* {{site.data.keyword.hscrypto}} - [Enabling a disabled root key](/docs/hs-crypto?topic=hs-crypto-disable-keys&interface=api#enable-api).

### Deleting root keys
{: #byok-delete-root-keys}

When you delete a root key, the key is no longer available to decrypt passphrases that are used to protect your resources. Volume, snapshot, and image resources that are protected by the deleted root key have an _unusable_ status and can't be used for normal operations. {{site.data.keyword.filestorage_vpc_short}} shares show a _suspended_ status. The storage system is offline, and data cannot be accessed. Your data still exists. 

| Resource type | Resource status | Result |
|---------------|-----------------| -------|
| Custom image | _unusable_ | Images can't be used for creating boot volumes to provision a new virtual server instance. |
| Boot volume | _unusable_ | The associated virtual server instance is stopped. A stopped instance cannot be started while the instance boot volume is in an _unusable_ state. |
| Data volume | _unusable_ | If the data volume is attached to a running instance, the instance is stopped. Stand-alone data volumes cannot be attached to instances. You can delete the volume. |
| Snapshot | _unusable_ | The snapshot is inaccessible and can't be used to restore a volume or file share. |
| File share | _suspended_ | The File Storage system is offline and data cannot be accessed. |
| Instance | _unusable_ | Instances with a deleted boot volume that were automatically stopped cannot restart. |
{: caption="Deleted root key" caption-side="bottom"}

A root key can be deleted from three states: _active_, _suspended_, or _deactivated_. The _deactivated_ state occurs automatically when a key's expiration date is reached. Regardless of the state before deletion, keys can be restored. 

You have a 30-day grace period to [restore the deleted key](#byok-restore-root-key). Otherwise, your encrypted resources become inaccessible. After 30 days, your root key can't be restored, and your resources are unrecoverable.
{: important}

By default, the KMS prevents you from deleting a root key that's actively protecting a resource. You can **force delete** a root key in {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} with the API. When you force delete a root key, the following actions happen automatically:

   * If the deleted root key is protecting volumes, the associated virtual server instance is stopped.
   * If the deleted root key is protecting file shares, the file share is suspended.
   * Deleting a root key purges usage of the key for all resources in the VPC.

{{site.data.keyword.hscrypto}} requires that you delete all resources before you delete a root key that is protecting those resources. If you can't delete the key, see the [troubleshooting key management service](/docs/hs-crypto?topic=hs-crypto-troubleshoot-unable-to-delete-keys) topic.

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

* {{site.data.keyword.keymanagementserviceshort}} - [Deleting keys](/docs/key-protect?topic=key-protect-delete-keys).
* {{site.data.keyword.hscrypto}} - [Deleting keys](/docs/hs-crypto?topic=hs-crypto-delete-keys).

### Restoring deleted root keys
{: #byok-restore-root-key}

Restoring an imported root key returns the key to an _active_ state, and reestablishes access to the key. You can restore access to all resources that were previously protected by the root key. You can resume regular actions, such as restarting an instance and reattaching data volumes. The volume attachment IDs are different than they were before you deleted the original customer root key.

Root keys that were generated by the KMS can't be restored.
{: important}

For more information about restoring root keys, see the following topics.

* {{site.data.keyword.keymanagementserviceshort}} - [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys).
* {{site.data.keyword.hscrypto}} - [Restoring keys](/docs/hs-crypto?topic=hs-crypto-restore-keys)

### Rotating customer root keys
{: #byok-rotate-root-key}

Rotating the customer root keys does not change the status of the encrypted resources. When you rotate the keys, the former key remains active, and is still used to decrypt existing resources.

You can rotate your root keys in your KMS instance by setting a rotation policy or by manually rotating your keys. A rotation policy automatically rotates your keys based on a schedule; manual rotation rotates keys on demand.

You can also manually rotate your imported or KMS-generated root keys at any time. {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} allow one rotation per hour for each root key. The KMS replaces the key immediately upon your request. Use this option when you import a new root key to the KMS instance and want to rotate the key immediately.

For more information about these options, see the following topics: 

* {{site.data.keyword.keymanagementserviceshort}} - [Managing rotation policies](/docs/key-protect?topic=key-protect-set-rotation-policy) and [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).
* {{site.data.keyword.hscrypto}} - [Setting a rotation policy for root key](/docs/hs-crypto?topic=hs-crypto-set-rotation-policy) and [Rotating root keys manually](/docs/hs-crypto?topic=hs-crypto-rotate-keys).

## Removing IAM service authorization to a root key
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

## Viewing events in the {{site.data.keyword.logs_full_notm}}
{: #byok-auditing-events}

For audit purposes, you can monitor the activity trail for a key by integrating your key management service with [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs){: external}. After both services are provisioned and running, events are generated and automatically collected in {{site.data.keyword.logs_full_notm}} when you perform actions on keys.

For more information, see the following topics:

* [{{site.data.keyword.keymanagementserviceshort}} - Monitoring the lifecycle of encryption keys](/docs/key-protect?topic=key-protect-key-states#monitor-lifecycle-changes).
* [Auditing events for {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-at-events).

## Next Steps
{: #next-steps-byok-manage}

* [Create volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create an instance with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create file share with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption).
