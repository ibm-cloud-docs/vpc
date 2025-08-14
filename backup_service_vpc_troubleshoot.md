---

copyright:
  years: 2022, 2025
lastupdated: "2025-08-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my backup policies, backup policy plans, backup jobs not found?
{: #baas-troubleshoot}
{: troubleshoot}
{: support}

When you try to list a backup policy, backup policy plan or backup job with the API or the CLI, you receive a `ID is not found` error.
{: tsSymptoms}

The policy, plan or job that you are trying to find is either deleted or never existed. You might have a typo in your ID entry.
{: tsCauses}

To address this error, list the resources and see whether the ID that you're looking for is in the list.
{: tsResolve}

For more information, see [Viewing backup policies and plans](/docs/vpc?topic=vpc-backup-view-policies) and [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).
