---

copyright:
  years: 2022, 2026
lastupdated: "2026-07-08"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my backup policies, backup policy plans, backup jobs not found?
{: #baas-troubleshoot}
{: troubleshoot}
{: support}

Backup policies, plans, or jobs return an `ID is not found` error when listed with the API or CLI because the resource was deleted or never existed.
{: shortdesc}

When you try to list a backup policy, backup policy plan, or backup job with the API or the CLI, you receive an `ID is not found` error.
{: tsSymptoms}

The policy, plan, or job that you are trying to find is either deleted or never existed. You might have a typographical error in your ID entry.
{: tsCauses}

To resolve this error, list the resources and verify that the ID you are looking for appears in the list.
{: tsResolve}

For more information, see [Viewing backup policies and plans](/docs/vpc?topic=vpc-backup-view-policies) and [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).
