---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-source-volume-busy' event type?
{: #baas-ts-scg-volume-busy}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-source-volume-busy'.
{: tsSymptoms}

Your backup snapshots in the consistency group could not be created because one or more of the parent volumes are performing operations that block the locking that's needed to create the snapshots.
{: tsCauses}

Wait until the volume operation is complete, and let the next scheduled backup job create the backups.
{: tsResolve}
 
