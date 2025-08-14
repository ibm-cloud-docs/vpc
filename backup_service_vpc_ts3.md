---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# What causes the service authorization error when I try to create a backup policy?
{: #baas-ts-3}
{: troubleshoot}

A service authorization error is generated when you try to create a backup policy.
{: tsSymptoms}

When you attempt to create a backup policy, an `s2s service not completed` error is generated, which indicates that incorrect service-to-service authorizations were set up. The backup policy creation fails. This behavior is expected. It prevents the backup policy from being created without proper authorization.
{: tsCauses}

Establish correct service-to-service authorizations and try creating a backup policy again. For more information about setting up authorizations for the backup service, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
{: tsResolve}

