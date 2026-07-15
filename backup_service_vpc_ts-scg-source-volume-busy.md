---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-15"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-source-volume-busy' event type?
{: #baas-ts-scg-volume-busy}
{: troubleshoot}
{: support}

Backup snapshots in a consistency group cannot be created because one or more parent volumes are performing operations that block snapshot creation.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-source-volume-busy'.
{: tsSymptoms}

Your backup snapshots in the consistency group were not created because one or more of the parent volumes are performing operations that block the locking that is needed to create the snapshots.
{: tsCauses}

Wait until the volume operation is complete, and allow the next scheduled backup job to create the backups.
{: tsResolve}
