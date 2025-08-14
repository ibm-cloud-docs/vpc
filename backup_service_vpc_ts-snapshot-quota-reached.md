---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'snapshot-quota-reached' event type?
{: #baas-ts-snapshot-quota-reached}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'snapshot-quota-reached'.
{: tsSymptoms}

Your backup snapshot could not be created because you already have as many snapshots as you can. You can have 750 snapshots of first-generation storage volumes and 512 snapshots of second-generation storage volumes.
{: tsCauses}

Delete unnecessary snapshots. Review and update your retention policy in the backup plan to make sure that you keep only the snapshots that you need.
{: tsResolve}
