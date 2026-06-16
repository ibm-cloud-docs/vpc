---

copyright:
  years: 2022, 2026
lastupdated: "2026-06-16"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create more backup policies?
{: #baas-backup-policy-quota-exceeded}

{{site.data.keyword.cloud_notm}} limits accounts to 10 backup policies per region. When you reach this quota, you must delete an existing policy before creating a new one.
{: shortdesc}

You receive the message that the account reached its backup policy quota when you try to create another backup policy. The event type is 'snapshot-quota-reached'.
{: tsSymptoms}

You are limited to 10 backup policies per account in a region.
{: tsCauses}

The backup policy quota can't be increased. To create a new policy, you need to delete a previous one. For more information about backup strategies, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).
{: tsResolve}
