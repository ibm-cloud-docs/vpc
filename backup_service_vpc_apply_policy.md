---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-18"

keywords: backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Applying backup policies to resources
{: #backup-use-policies}

Apply backup policies by adding user tags to new or existing volumes, virtual server instances, or shares. When these tags match a backup policy tag, a backup is created.
{: shortdesc}

Up to 100 tags can be attached or detached in the same operation. Maintaining a limited quantity of tags can simplify monitoring their usage and your backups.

1. [Create a backup policy and plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan), and specify the tags for the target resources.
1. Find the resources that you want to back up.
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui), or [file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags) in the console.{: ui}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=cli#add-user-tags-volumes-cli), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#creating-virtual-servers-cli), or [file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=cli#fs-add-user-tags) from the CLI.{: cli}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=api#add-user-tags-volumes-api), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-api), or [file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags) with the API.{: api}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=terraform#block-storage-add-tags-terraform), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-terraform), or [file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=terraform#file-storage-manage-terraform) with Terraform.{: terraform} 
1. Verify that your selected resource is associated with a backup policy. For more information, see [View a list of resources that have a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies).

Scheduling snapshots of a replica share is not supported. Backup schedules for file shares can be set up only on the source side of a replication pair. When you choose to failover operations to the replica share, the source and replica shares switch roles. After a failover is performed, backup policies need to be removed from what was previously the source and applied to the current source share. Make sure that you update the tags on the affected shares.
{: important}

## Next steps
{: #backup-next-steps-use}

* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Create extra backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
