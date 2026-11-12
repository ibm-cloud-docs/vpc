---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot failure, backup job failed, backup troubleshooting, volume too large, 10 TB limit

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-volume-too-large' event type?
{: #baas-ts-snapshot-volume-too-large}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup snapshots fail for Generation 1 volumes that exceed 10 TB. Snapshots aren't supported for these volumes.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the error message 'snapshots-volume-too-large'.
{: tsSymptoms}

Your backup snapshot could not be created because the parent volume has a Generation 1 volume profile from the `custom` or `tiered` profile family, and it exceeds 10 TB.
{: tsCauses}

Snapshots of Generation 1 volumes that exceed 10 TB are not supported. No workaround exists for this issue.
{: tsResolve}
