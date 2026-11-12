---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot failure, backup job failed, backup troubleshooting, pending state, expired snapshot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot-in-pending-state' event type?
{: #baas-ts-snapshot-pending-state}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup jobs fail to delete an expired snapshot that is in a pending state. Wait until the snapshot is stable or failed, then retry the deletion.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot-in-pending-state'.
{: tsSymptoms}

The backup job failed to delete the snapshot that had an expired retention date because the snapshot was in a pending state.
{: tsCauses}

Wait until the snapshot's state becomes 'stable' or 'failed', and try deleting the snapshot again.
{: tsResolve}
