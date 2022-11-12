---

copyright:
  years: 2022
lastupdated: "2022-11-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting Backup for VPC
{: #baas-troubleshoot}

When you use the Backup for VPC service, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Volume backups were not made according to backup plan
{: #baas-ts-1}
{: troubleshoot}
{: support}

Volumes are not getting backed up on all scheduled times. For example, you created a schedule for backups in a backup plan to create daily backups. However, after the service created backups for a while as defined in the plan, the backups stop.
{: tsSymptoms}

Some causes might be:
{: tsCauses}

* Your volume might be deattached after the successful backups were made.
* The volume tag that matched the tag for target resources might be deleted.
* The instance might not be running.
* Correct service-to-service authorizations may not be in place.

Verify that the volume wasn't detached from a virtual server instance. Search for the instance to which you last attached the volume from the list of all virtual server instances. Verify that the instance is running. Also, look at the backup policy and verify that the tags are present in the volume, in case the tag was inadvertently deleted. Scheduled backups do not occur instantly, so you might need to look again in an hour or so to verify that the backup was made.
{: tsResolve}

## Backup jobs are indicating a failed state
{: #baas-ts-2}
{: troubleshoot}

The backup failed, as indicated in the backup job details. When you make a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` call, the `status` property shows `failed`.
{: tsSymptoms}

Look at the `reason_codes` property in the response. It could be an internal error or the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) is reached.
{: tsCauses}

Contact IBM support if you see an internal error. If the backup snapshot limit is reached, delete the oldest snapshots so new ones can be created. You can create up to 100 backup snapshots of a volume, so delete accordingly.
{: tsResolve}

## Backup policy not created due to incorrect authorizations
{: #baas-ts-3}
{: troubleshoot}

A service authorization error is generated when you try to create a backup policy.
{: tsSymptoms}

When you attempt to create a backup policy, an `s2s service not completed` error is generated, which indicates that incorrect service-to-service authorizations were set up. The backup policy creation fails. This behavior is expected and right, it prevents the backup policy from being created without proper authorization.
{: tsCauses}

Establish correct service-to-service authorizations and try creating a backup policy again. For more information about setting up authorizations for the backup service, see [Establishing service to service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
{: tsResolve}
