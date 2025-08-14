---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-bad-state' event type?
{: #baas-ts-snapshot-bad-state}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-bad-state'.
{: tsSymptoms}

The backup job failed to delete a snapshot that has passed its retention period because the snapshot was not in a stable state.
{: tsCauses}

Check the snapshot to see when it reaches 'stable' or 'failed' status. Then, either wait for next backup job to delete the snapshot or delete the snapshot manually.
{: tsResolve}
