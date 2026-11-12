---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot failure, backup job failed, backup troubleshooting, source volume busy

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-source-volume-busy' event type?
{: #baas-ts-snapshot-volume-busy}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup snapshots fail when the source volume is busy with an operation that blocks snapshot creation. Wait for it to finish before the next backup job.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-source-volume-busy'.
{: tsSymptoms}

Your backup snapshot wasn't created because the parent volume is performing an operation that blocks the locking that is needed to create the snapshots.
{: tsCauses}

Wait until the volume operation is complete, and allow the next scheduled backup job to create the backup.
{: tsResolve}
