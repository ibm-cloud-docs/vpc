---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my backup job fail with 'service-to-service-policy-missing' event type?
{: #baas-ts-s2s-policy-missing}
{: troubleshoot}
{: support}

Backup jobs fail to create or delete snapshots when the service-to-service authorization for {{site.data.keyword.cloud_notm}} Backup for VPC is not configured correctly.
{: shortdesc}

You receive an [event notification](/docs/vpc?topic=vpc-event-notifications-events#event-notifications-list) with the event type 'service-to-service-policy-missing'.
{: tsSymptoms}

Backup jobs fail to create or delete snapshots when service-to-service authorization is missing or scoped too narrowly. Common causes include:

- No authorization policy exists between the Backup service and the target service.
- Authorization exists but is scoped to specific resources that don't include all target instances.
- Authorization was deleted after the backup policy was created.
{: tsCauses}

1. Verify that service-to-service authorizations are configured correctly. Go to **Manage > Access (IAM) > Authorizations** and confirm that authorization policies exist for all required services.

2. Check the authorization scope. The Backup service needs authorization to access all virtual server instances whose volumes require backup.

3. If backups work for some instances but not others, see [Why are backups failing for some volumes but not others?](/docs/vpc?topic=vpc-baas-ts-authorization-scope) for detailed troubleshooting steps.

For more information about setting up authorizations with the correct scope, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
{: tsResolve}
