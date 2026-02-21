---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-21"

keywords: Block Storage, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption, fihe share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Key rotation for VPC resources
{: #vpc-key-rotation}

For {{site.data.keyword.vpc_short}} resources such as volumes, file shares, and custom images that are protected by your customer root key (CRK), you can rotate the root keys for more security. When you rotate a root key by schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing resources but can't be used to encrypt new ones.
{: shortdesc}

## Key rotation overview
{: #vpc-key-rotation-overview}

Customer-managed encrypted resources such as volumes, file shares, and custom images use your root key (CRK) as the root-of-trust key. The root key's function is to encrypt a LUKS passphrase that encrypts a data encryption key (DEK) protecting the resource. You can import your CRK to a key management service (KMS) instance or have the KMS generate one for you. Supported key management services are {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}. Root keys are rotated in your KMS instance.

When you rotate a root key, a new version of the key is created by generating or importing new cryptographic key material. The old root key is retired, which means its key material remains available for decrypting existing resources but not for encrypting new ones.

You can have several root keys in each KMS instance available for key rotation. These root keys can be a combination of keys that you imported to the Cloud or ones that were automatically generated in the KMS instance.

You can set up a rotation policy to schedule automatic key rotation. The KMS generates a new root key at the rotation interval and automatically replaces the root key with new key material.

For any root keys that you import to the KMS instance because you're providing new key material that is not in the KMS, you can't set up automatic key rotation. Instead, you must manually rotate your keys by using the new cryptographic key material that you imported.

### How key rotation works
{: #vpc-key-rotation-function}

Key rotation works by securely transitioning root key material, shortening the cryptoperiod of the root keys that protect your resources. New resources are encrypted with the most recent root keys.

When you rotate a root key, the service identifies all resources that are protected by that key and automatically reencrypts the resource. This encryption process wraps (encrypts) a LUKS passphrase that protects a data encryption key (DEK). The system-generated DEK encrypts the data in the virtual disk. This process is called [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption), which creates a wrapped DEK, or WDEK. The WDEK _does not_ refer to the data encryption key that is protecting data in the virtual disk. Rather, it encrypts the LUKS passphrase.

The new version of the root key is used to unwrap the newly rewrapped LUKS passphrase the next time the resource is decrypted. For example, it is used when you decrypt a volume upon restarting an instance.

The service retires the old root key version but it remains available for decrypting existing resources. You can list the key versions to see how many times the key was rotated and see the most recent version. Older versions of the root key continue to work until they expire or until no resources are available to decrypt. If you manually [disable](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) or [delete](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys) the root key or restart the instance, the old key no longer functions.

After your root keys are rotated, the new root key version becomes available for encrypting new resources.

For more information about the key rotation process, see:

* {{site.data.keyword.keymanagementserviceshort}} - [Understanding the key rotation process](/docs/key-protect?topic=key-protect-key-rotation#understand-key-rotation-process)
* {{site.data.keyword.hscrypto}} - [Root key rotation](/docs/hs-crypto?topic=hs-crypto-root-key-rotation-intro)

### Benefits of key rotation
{: #vpc-key-rotation-benefits}

Rotating your root keys provides these security benefits:

* You can limit how long your data is protected by a single root key. By shortening the time that your resources are encrypted with a particular root key, you decrease the probability of a security breach.
* You limit the number of resources encrypted with any one version of the root key because resources created after key rotation are encrypted with different key material.
* If your organization detects a security threat, you can immediately rotate your keys to protect your resources.
* Rotating keys mitigates the costs that are associated with key compromises.

### Key rotation options
{: #vpc-key-rotation-options}

You can rotate your root keys in your KMS instance by setting a rotation policy or by manually rotating your keys. A rotation policy automatically rotates your keys based on a schedule; manual rotation rotates keys on demand.

You can also manually rotate your imported or KMS-generated root keys at any time. {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} allow one rotation per hour for each root key. The KMS replaces the key immediately upon your request. Use this option when you import a new root key to the KMS instance and want to rotate the key immediately.

For more information about these options, see:

* {{site.data.keyword.keymanagementserviceshort}} - [Managing rotation policies](/docs/key-protect?topic=key-protect-set-rotation-policy) and [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).
* {{site.data.keyword.hscrypto}} - [Setting a rotation policy for root key](/docs/hs-crypto?topic=hs-crypto-set-rotation-policy) and [Rotating root keys manually](/docs/hs-crypto?topic=hs-crypto-rotate-keys).

### Root key metadata
{: #vpc-key-rotation-metadata}

When you rotate your root key, the key retains the same metadata and key ID. The key rotation changed the root key ciphertext and the `keyVersion` data.
