---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-volume-unavailable' event type?
{: #baas-ts-scg-volume-unavailable}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-volume-unavailable'.
{: tsSymptoms}

Your backup snapshots in the consistency group could not be created due to one of the following reasons. The virtual server instance is not running so the attached volumes cannot be reached. One or more of the volumes in the consistency group is no longer attached to the instance, or their ID cannot be found.
{: tsCauses}

Review your consistency group and update as necessary. Make sure the virtual server instance is powered up and running when the next backup job is scheduled to run.
{: tsResolve}
