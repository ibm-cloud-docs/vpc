---

copyright:
  years: 2022, 2024
lastupdated: "2024-10-18"

keywords: backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Applying backup policies to resources with tags
{: #backup-use-policies}

Apply backup policies by adding user tags to new or existing volumes. When these tags match a backup policy tag, a backup is created.
{: shortdesc}

Up to 100 tags can be attached or detached in the same operation. Keeping the number of tags low can make it easier to track their usage and your backups.

1. [Create a backup policy and plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
1. Find the resources that you want to back up.
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui) in the console.{: ui}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=cli#add-user-tags-volumes-cli), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#creating-virtual-servers-cli)  from the CLI.{: cli}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=api#add-user-tags-volumes-api), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-api)  with the API.{: api}
1. Apply backup policy tags to your target [volumes](/docs/vpc?topic=vpc-managing-block-storage&interface=terraform#block-storage-add-tags-terraform), or [virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-terraform)  with Terraform.{: terraform} 
1. Verify that your selected resource is associated with a backup policy. For more information, see [View a list of resources that have a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-vol-backup-policies).

## Next steps
{: #backup-next-steps-use}

* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Create extra backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
