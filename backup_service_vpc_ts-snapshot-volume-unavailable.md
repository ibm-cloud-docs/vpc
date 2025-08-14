---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-volume-unavailable' event type?
{: #baas-ts-snapshot-volume-unavailable}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-volume-unavailable'.
{: tsSymptoms}

Your backup snapshot could not be created due to one of the following reasons. The virtual server instance is not running so the attached volume cannot be reached. The volume is no longer attached to the instance, or their ID cannot be found.
{: tsCauses}

Review your backup policy plan. Make sure all volumes that are tagged for backups are attached to running virtual server instances when the next backup job is scheduled to run.
{: tsResolve}
