---

copyright:
  years: 2022, 2025
lastupdated: "2025-09-04"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# What is causing my Backup jobs to indicate a failed state?
{: #baas-ts-2}
{: troubleshoot}

The backup failed, as indicated in the backup job details. When you make a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` call, the `status` property shows `failed`.
{: tsSymptoms}

Look at the `reason_codes` property in the response. It might be an internal error or the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) is reached.
{: tsCauses}

- If you see an internal error, contact IBM support. 
- If the backup snapshot limit is reached, delete the oldest snapshots so new ones can be created. You can create up to 100 backup snapshots of a volume.
- If the snapshot rate is too high, consider increasing the time between backups. That change allows the service to complete the creation of the backup snapshots and loading them into the regional storage repository before a new snapshot is triggered.
{: tsResolve}
