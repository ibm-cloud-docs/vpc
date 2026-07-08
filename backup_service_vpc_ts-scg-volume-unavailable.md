---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-08"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot_consistency_group-volume-unavailable' event type?
{: #baas-ts-scg-volume-unavailable}
{: troubleshoot}
{: support}

Backup jobs fail when virtual server instances aren't running or volumes are detached from the instance. Learn how to resolve consistency group backup failures caused by unavailable volumes.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot_consistency_group-volume-unavailable'.
{: tsSymptoms}

Your backup snapshots in the consistency group were not created due to one of the following reasons:

- The virtual server instance is not running, so the attached volumes cannot be reached.
- One or more of the volumes in the consistency group is no longer attached to the instance.
- The ID of one or more volumes cannot be found.
{: tsCauses}

Review your consistency group and update it as necessary. Make sure that the virtual server instance is running when the next backup job is scheduled to run.
{: tsResolve}
