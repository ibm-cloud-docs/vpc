---

copyright:
  years: 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for snapshot creation and use
{: #snapshots-vpc-planning}

When you plan a snapshot strategy for your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful to set up and use the snapshot service.
{: shortdesc}


## Planning to create and use snapshots
{: #planning-for-data-encryption-snap}

Consider the following prerequisites before you set up Snapshots for VPC.

| Item | Considerations |
|------|----------------|
| {{site.data.keyword.iamshort}} permissions | Evaluate your [IAM access permissions](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam) to create snapshots. |
| Interface | Choose the UI, CLI, or API for creating and managing your snapshots. |
| Volumes | Evaluate which volumes are most important to snapshot. You can create a snapshot of boot and data volumes. \n  Evaluate the amount of change that you expect for volumes that you intend to snapshot. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all snapshots for a volume can't exceed 10 TB. |
| Snapshots retention | Evaluate how many snapshots to retain, how long you need to retain them, and when to delete snapshots. Review the [snapshots limitations](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots-vpc-limitations) before you perform these actions. |
| Restoring a volume | Evaluate when you might want to restore a volume from a snapshot. If you need to revert to an earlier version of the volume, plan which snapshot you use to create the volume. Evaluate which volumes you need to restore and attach to an instance, and which volumes you might restore as unattached volumes. |
| Conventions | Select a unique name for your snapshot. For example, if you have a method for naming volumes, you might name snapshots by using similar conventions. It's easier to filter and search for them later. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| Virtual server instances | To create a snapshot from a volume that is attached to an instance, verify that the instance is in a running state. \n To create an instance from a snapshot of a boot volume, use a snapshot in the same region. |
| Billing | Think about the number of snapshots that you want to take and the [billing considerations](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_considerations) as the number of snapshots grows. |
| Performance | Review the [performance considerations](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-perf) for restoring a volume from a snapshot. |
{: caption="Table 1. Checklist for planning snapshots" caption-side="bottom"}

## Next Steps
{: #snapshots-vpc-planning-next-steps}

[Create snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
