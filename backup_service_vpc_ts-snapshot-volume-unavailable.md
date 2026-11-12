---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-12"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, IBM Cloud backup, snapshot failure, backup job failed, backup troubleshooting, volume unavailable

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-volume-unavailable' event type?
{: #baas-ts-snapshot-volume-unavailable}
{: troubleshoot}
{: support}

{{site.data.keyword.cloud_notm}} backup snapshots can't be created when the virtual server instance isn't running or the tagged volume is detached. Learn how to resolve it.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshots-volume-unavailable'.
{: tsSymptoms}

Your backup snapshot could not be created due to one of the following reasons. The virtual server instance is not running so the attached volume cannot be reached. The volume is no longer attached to the instance, or their ID cannot be found.
{: tsCauses}

Review your backup policy plan. Make sure all volumes that are tagged for backups are attached to running virtual server instances when the next backup job is scheduled to run.
{: tsResolve}
