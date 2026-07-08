---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-08"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-encryption-key-invalid' event type?
{: #baas-ts-snapshots-encryption-key-invalid}
{: troubleshoot}
{: support}

Backup snapshots cannot be created because the encryption key is invalid or inaccessible.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-encryption-key-invalid'.
{: tsSymptoms}

The backup snapshot was not be created because the provided encryption key is not valid or is inaccessible.
{: tsCauses}

Verify the encryption key in your key management system and update your backup plan with the valid key's CRN. Then, take one of the following actions based on your situation:

- If you are using another account's encryption key, contact the key owner to confirm that you have the necessary authorization and that the key is in the `active` state.
- If you are using your own encryption key, verify that your key is in the `active` state.

For more information, see [Customer root key states and resource statuses](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-root-key-states).
{: tsResolve}
