---

Copyright:
  years: 2019, 2020
lastupdated: "2020-07-16"

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

Take actions to manage your customer-managed encryption that secures your boot and data volumes using your own root keys.
{:shortdesc}

## Managing root keys
{: #byok-manage-root-keys}

Manage your root keys by taking the following actions: 

* Decide whether importing your own root key or using a KMS-generated root key is preferable. If you want to set up a rotation policy for automatic key rotation, you must use KMS-generated root keys.
* Review the [Activity Tracker](#byok-key-rotation-activity-tracker-events) to verify events as you manage the lifecycle of your keys.
* Decide when you might want to make your data [temporarily inaccessible](#instance-byok-inaccessible-data) but retain it on the cloud.
* Decide when [disabling](#byok-disable-root-keys) or [deleting](#byok-delete-root-keys) a root key is necessary. 

### Disabling root keys
{: #byok-disable-root-keys}

[Disabling a key](/docs/key-protect?topic=key-protect-disable-keys) places it in a _suspended_ state, which revokes access to the key's associated data on the cloud. When you disable a root key, you suspend its encryption and decryption operations. Temporarily disabling a root key is good practice if you suspect a possible security exposure, compromise, or breach with your data. You can restore a disabled root key when the security risk is no longer active.

When you suspend a root key in your key management service, your workloads remain running in your virtual server instance and volumes remain encrypted. However, if you power down the VM and then power it back on, any instances with encrypted boot volumes will not start. You can cancel the deletion if your key is in a suspended state. Look at the list of resources for the status of the key.

### Deleting root keys
{: #byok-delete-root-keys}

When you delete a root key, the KMS decrypts (unwraps) the passphrase encrypting a master key that encrypts the volume. The passphrase becomes unavailable and the volume is therefore inaccessible. 

Deleting a root key places it in a _destoyed_ key state. The root key can't be recovered. 

The KMS blocks the deletion of any key that's actively protecting a volume. Before you delete a key, review the volumes that are associated with the key.

As best practice, make sure the volume data is not longer needed before deleting any root key. Consider [temporarily disabling the key](#byok-disable-root-keys) instead of deleting it to suspend access to the key's associated data.
{:important}

For more information about deleting root keys, see the [Key Protect](/docs/key-protect?topic=key-protect-delete-keys) or [HPCS](/docs/hs-crypto?topic=hs-crypto-delete-keys) documentation.

### Making your data inaccessible after setting up customer-managed encryption
{: #instance-byok-inaccessible-data}

You can make your data inaccessible but retain it on the cloud by removing authorization to use that root key. 

When you [authorize use](/docs/iam?topic=iam-serviceauth#serviceauth) of your root key, you grant permission for IBM to use the key to encrypt your volume. Authorization is done at the key management service level through IAM, when you authorize service between **Cloud Block Storage** and the key management service you set up (for example, {{site.data.keyword.keymanagementserviceshort}}).

You can remove any authorization between services in your account when you have the Administrator role on the target service (in this case, the key management service). If you remove any access policies created by the source service for its dependent services, the source service is unable to complete the workflow or access the target service.

Because they keys are under your control, you don't have to contact IBM to remove authorization.

To make your data inaccessible, but retain it on the IBM Cloud:

1. [Remove IAM authorization](/docs/iam?topic=iam-serviceauth#remove-auth) from the source Cloud Block Storage service to your target key management service instance.
2. [Power off all virtual server instances](/docs/vpc?topic=vpc-managing-virtual-server-instances#stop-and-start) that have attached encrypted volumes secured by that root key.

You can also disable a root key, which suspends the key and temporarily revokes access to the key's associated data on the cloud. For more information, see [Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys)

## Viewing events in the Activity Tracker 
{: #byok-activity-tracker-events}

Use the Activity Tracker to verify user-initiated activities that change the state of your key management service. 

Due to the sensitivity of the information for an encryption key, when an event is generated as a result of an API call to the {{site.data.keyword.keymanagementserviceshort}} service, the event that is generated does not include detailed information about the key. Instead, it includes a correlation ID that you can use to identify the key internally in your cloud environment. The correlation ID is a field that is returned as part of the `correlationId` field.

For information about all Activity Tracker events in {{site.data.keyword.keymanagementserviceshort}}, see [Activity Tracker events](/docs/key-protecttopic=key-protect-at-events#rotate-key-registrations-success).

## Next Steps
{: #next-steps-byok-manage}

* [Create volumes that use customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Create an instance with volumes that use customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok)