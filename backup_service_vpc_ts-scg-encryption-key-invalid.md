---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-encryption-key-invalid' event type?
{: #baas-ts-scg-encryption-key-invalid}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-encryption-key-invalid'.
{: tsSymptoms}

The snapshots in the consistency group could not be created because the provided encryption key was found to be not valid, or the key might be unaccessible.
{: tsCauses}

Verify the encryption key in your key management system and update your backup plan with the valid key's CRN. If you are using another account's encryption key, contact the key owner to confirm that you have the necessary authorization to the encryption key and that the key is in the 'active' state. If you're using your own encryption key make sure that your key is in the 'active' state. For more information, see (Customer root key states and resource statuses)[/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-root-key-states].
{: tsResolve}
