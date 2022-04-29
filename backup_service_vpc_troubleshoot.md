---

copyright:
  years: 2022
lastupdated: "2022-04-29"

keywords:

subcollection: vpc


---

{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Troubleshooting Backup for VPC
{: baas-troubleshoot}

When you use the Backup for VPC service, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

This service is available only to accounts with special approval to preview this feature.
{: preview}

## Volume backups were not made according to backup plan
{: #baas-ts-1}
{: troubleshoot}
{: support}

Volumes are not backing up on all scheduled days.
{: tsSymptoms}

You created a schedule for backups in a backup plan (for example, to create daily backups). However, after creating backups for as defined in the plan, the backups stop. For example, daily backups might stop after a few days. Some causes might be:
{: tsCauses}

* Your volume might have been deattached after the successful backups were made.
* The volume tag matching the tag for target resources might have been deleted.
* The instance might not be running.

Verify that the volume wasn't detached from a virtual server instance. Search for the instance to which you last attached the volume from the list of all virtual server instances. Verify the instance is running. Also, look at the backup policy and verify the tags are present in the volume, in case the tag was inadvertantly deleted. Note that scheduled backups do not occur instantly; you might need to look again in a hour or so to verify the backup was made.
{: tsResolve}

## Backup jobs are indicating a failed state
{: #baas-ts-2}
{: troubleshoot}

The backup has failed, indicated in the backup job details. When you make a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` call, the `status` property shows `failed`. 
{: tsSymptoms}

Look at the `reason_codes` property in the response. There might be an internal error or the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) has been reached.
{: tsCauses}

Contact IBM support if you see an internal error. If the backup snapshot limit is reached, delete the oldest snapshots so new ones can be created. You can create up to 100 backup snapshots of a volume, so delete accordingly. For information about all backup status reason codes, see [Backup job statuses and reason codes](TBD#backup-jobs-status).
{: tsResolve}
