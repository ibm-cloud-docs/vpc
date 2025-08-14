---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-in-pending-state' event type?
{: #baas-ts-scg-in-pending-state}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-in-pending-state'.
{: tsSymptoms}

The backup job failed to delete the snapshot that had an expired retention date because the snapshot was in a pending state.
{: tsCauses}

Wait until the consistency group's and snapshot's state becomes 'stable' or 'failed', and try deleting the snapshot again.
{: tsResolve}
