---

copyright:
  years: 2022, 2025
lastupdated: "2025-03-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting Backup for VPC
{: #baas-troubleshoot}

When you use the Backup for VPC service, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Volume backups were not made according to the backup plan
{: #baas-ts-1}
{: troubleshoot}
{: support}

Volumes are not getting backed up on all scheduled times. For example, you created a schedule for backups in a backup plan to create daily backups. However, after the service created backups for a while as defined in the plan, the backups stop.
{: tsSymptoms}

The issue can have one of the following causes.
{: tsCauses}

* Your volume might be deattached after the successful backups were made.
* The volume tag that matched the tag for target resources might be deleted.
* The instance might not be running.
* Correct service-to-service authorizations might not be in place.

Verify that the volume wasn't detached from a virtual server instance. Search for the instance to which you last attached the volume from the list of all virtual server instances. Verify that the instance is running. Also, look at the backup policy and verify that the tags are present in the volume, in case the tag was inadvertently deleted. Scheduled backups do not occur instantly, so you might need to look again in an hour or so to verify that the backup was made.
{: tsResolve}

## Backup jobs are indicating a failed state
{: #baas-ts-2}
{: troubleshoot}

The backup failed, as indicated in the backup job details. When you make a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` call, the `status` property shows `failed`.
{: tsSymptoms}

Look at the `reason_codes` property in the response. It might be an internal error or the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) is reached.
{: tsCauses}

- If you see an internal error, contact IBM support. 
- If the backup snapshot limit is reached, delete the oldest snapshots so new ones can be created. You can create up to 100 backup snapshots of a volume.
- If the snapshot rate is too high, consider increasing the time between backups. That change allows the service to complete the creation of the backup snapshots and loading them into {{site.data.keyword.cos_full_notm}} before a new snapshot is triggered.
{: tsResolve}

## Backup policy is not created due to incorrect authorizations
{: #baas-ts-3}
{: troubleshoot}

A service authorization error is generated when you try to create a backup policy.
{: tsSymptoms}

When you attempt to create a backup policy, an `s2s service not completed` error is generated, which indicates that incorrect service-to-service authorizations were set up. The backup policy creation fails. This behavior is expected. It prevents the backup policy from being created without proper authorization.
{: tsCauses}

Establish correct service-to-service authorizations and try creating a backup policy again. For more information about setting up authorizations for the backup service, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
{: tsResolve}

## Backup policies are not found
{: #baas-backup_policies_not_found}

You receive a `backup_policies_not_found` error with the following message: _Backup policy with ID is not found_.
{: tsSymptoms}

The backup policy that you are trying to find is either deleted or never existed.
{: tsCauses}

To address this error, list the policies and see whether the backup policy ID that you're looking for is in the list. If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).
{: tsResolve}

For more information about viewing your backup policies, see [Viewing backup policies](/docs/vpc?topic=vpc-backup-view-policies).

## Backup policy plan is not found
{: #baas-backup_policy_plan_not_found}

You receive a `backup_policy_plan_not_found` error with the following message: `Backup policy plan with ID <policy_plan_uuid> is not found`.
{: tsSymptoms}

The backup policy plan that you are trying to find is either deleted or never existed.
{: tsCauses}

To address this error, list the backup policy plans and see whether the plan ID that you're looking for is in the list. If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).
{: tsResolve}

For more information about viewing your backup policies and plans, see [Viewing backup policies](/docs/vpc?topic=vpc-backup-view-policies).

## Backup job is not found
{: #baas-backup_jobs_not_found}

You receive a `backup_jobs_not_found` error with the following message: `Job with ID <backup_job_uuid> is not found`.
{: tsSymptoms}

The job that you are trying to find is either deleted or never existed.
{: tsCauses}

To address this error, list the jobs and see whether the job ID that you're looking for is in the list. If you still need help with the error, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).
{: tsResolve}

For more information about viewing your backup jobs list, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Backup policy quota exceeded
{: #baas-backup-policy-quota-exceeded}

You receive a message that the account reached its backup policy quota, when you try to create another backup policy.
{: tsSymptoms}

You are limited to 10 backup policies per account in a region.
{: tsCauses}

The backup policy quota can't be increased. To create a new policy, you need to delete a previous one. For more information about backup strategies, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).
{: tsResolve}
