---

copyright:
  years: 2024, 2025
lastupdated: "2025-10-09"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-volume-too-large' event type?
{: #baas-ts-scg-volume-too-large}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-volume-too-large'.
{: tsSymptoms}

Your backup snapshots in the consistency group could not be created because one or more parent volumes exceed 10 TB. This is the limit for Gen 1 volumes that have the `custom` or one of the `tiered` volume profiles.
{: tsCauses}

Snapshots are not supported for Generation 1 volumes that exceed 10 TB. No workaround exists for this issue.
{: tsResolve}
