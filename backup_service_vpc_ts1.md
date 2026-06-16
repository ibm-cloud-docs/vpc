---

copyright:
  years: 2022, 2026
lastupdated: "2026-06-16"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why aren't my scheduled backups running?
{: #baas-ts-1}
{: troubleshoot}
{: support}

Scheduled backups may fail when volumes are detached, tags are deleted, instances aren't running, or service-to-service authorizations are missing. Learn how to troubleshoot and resolve backup plan execution issues.
{: shortdesc}

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
