---

copyright:
  years: 2024, 2026
lastupdated: "2026-06-16"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-volume-too-large' event type?
{: #baas-ts-snapshot-volume-too-large}
{: troubleshoot}
{: support}

Backup snapshots fail for Generation 1 volumes that exceed 10 TB. Learn about snapshot size limitations and why no workaround exists for oversized Generation 1 volumes.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the error message 'snapshots-volume-too-large'.
{: tsSymptoms}

Your backup snapshot could not be created because the parent volume has a Generation 1 volume profile from the `custom` or `tiered` profile family, and it exceeds 10 TB.
{: tsCauses}

Snapshots of Generation 1 volumes that exceed 10 TB are not supported. No workaround exists for this issue.
{: tsResolve}
