---

copyright:
  years: 2022, 2026
lastupdated: "2026-07-08"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create more backup policies?
{: #baas-backup-policy-quota-exceeded}

{{site.data.keyword.cloud_notm}} limits accounts to 10 backup policies per region. When you reach this quota, you must delete an existing policy before you can create another one.
{: shortdesc}

When you try to create a backup policy, you receive a message that states that the account reached its backup policy quota. The event type is `snapshot-quota-reached`.
{: tsSymptoms}

Accounts are limited to 10 backup policies per region.
{: tsCauses}

The backup policy quota cannot be increased. To create a new policy, you must delete an existing one. For more information about backup strategies, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).
{: tsResolve}
