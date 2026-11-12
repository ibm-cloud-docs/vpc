---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot consistency group, backup job failed, backup troubleshooting, volume too large, 10 TB limit

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-volume-too-large' event type?
{: #baas-ts-scg-volume-too-large}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup snapshots in a consistency group can't be created when Gen 1 parent volumes exceed the 10 TB limit. Snapshots aren't supported for these volumes.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-volume-too-large'.
{: tsSymptoms}

Your backup snapshots in the consistency group could not be created because one or more parent volumes exceed 10 TB. This is the limit for Gen 1 volumes that have the `custom` or one of the `tiered` volume profiles.
{: tsCauses}

Snapshots are not supported for Generation 1 volumes that exceed 10 TB. No workaround exists for this issue.
{: tsResolve}
