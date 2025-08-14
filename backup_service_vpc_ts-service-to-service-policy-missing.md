---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'service-to-service-policy-missing' event type?
{: #baas-ts-s2s-policy-missing}
{: troubleshoot}
{: support} 

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'service-to-service-policy-missing'.
{: tsSymptoms}

When the service-to-service authorization is not set up correctly, the backup jobs fail to create or delete snapshots.
{: tsCauses}

Verify that the service-to-service authorizations are configured correctly, and try to create or delete the backup snapshot again. For more information about setting up authorizations for the backup service, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
{: tsResolve}
