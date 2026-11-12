---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot consistency group, backup job failed, backup troubleshooting, bad state, expired snapshot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-bad-state' event type?
{: #baas-ts-scg-bad-state}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup jobs fail to delete an expired snapshot when its consistency group is in a bad state. Wait for a stable state, then retry.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-bad-state.
{: tsSymptoms}

The backup job failed to delete the snapshot that had an expired retention date because the snapshot consistency group was in a bad state.
{: tsCauses}

Wait until the consistency group's and snapshot's state becomes 'stable' or 'failed', and try deleting the snapshot again.
{: tsResolve}
