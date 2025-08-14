---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I receive an error message about a quota when I want to create a new backup policy?
{: #baas-backup-policy-quota-exceeded}

You receive the message that the account reached its backup policy quota when you try to create another backup policy.
{: tsSymptoms}

You are limited to 10 backup policies per account in a region.
{: tsCauses}

The backup policy quota can't be increased. To create a new policy, you need to delete a previous one. For more information about backup strategies, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).
{: tsResolve}

