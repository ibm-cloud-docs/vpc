---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-source-volume-busy' event type?
{: #baas-ts-snapshot-volume-busy}
{: troubleshoot}
{: support}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-source-volume-busy'.
{: tsSymptoms}

Your backup snapshot could not be created because the parent volume is performing an operation that blocks the locking that's needed to create the snapshots.
{: tsCauses}

Wait until the volume operation is complete, and let the next scheduled backup job create the backup.
{: tsResolve}
