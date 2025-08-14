---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshots-volume-too-large' event type?
{: #baas-ts-snapshot-volume-too-large}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the error message 'snapshots-volume-too-large'.
{: tsSymptoms}

Your backup snapshot could not be created because the parent volume is larger than 10 TB.
{: tsCauses}

Snapshots of volumes that are larger than 10 TB are currently not supported. No workaround exists for this issue.
{: tsResolve}

