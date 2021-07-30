---

Copyright:
  years:  2021
lastupdated: "2021-07-29"

keywords: block storage, virtual private cloud, volume, data storage, virtual server instance, instance, snapshots

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Planning for snapshot creation and use
{: #snapshots-vpc-planning}

When you plan a snapshot strategy for your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful to set up and use the snapshot service.
{:shortdesc}

## Planning to create and use snapshots
{: #planning-for-data-encryption}

Consider the following prerequisites before you set up Snapshots for VPC.

| Considerations |
|-------------------|
| __ Evaluate your [IAM access permissions](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam) to create snapshots. |
| __ Evaluate which volumes are most important to snapshot. You can create a snapshot of boot and data volumes. |
| __ Choose the UI, CLI, or API for creating and managing your snapshots. |
| __ Evaluate the volumes that you intend to snapshot. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all snapshots for a volume can't exceed 10 TB. |
| __ Evaluate how many snapshots to retain, how long you need to retain them, and when to delete snapshots. Review the [snapshots limitations](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots-vpc-limitations) before you perform these actions. |
| __ Evaluate when you might want to restore a volume from a snapshot. If you need to revert to an earlier version of the volume, plan which snapshot you use to create the volume. |
| __ Make sure you have a unique name for your snapshots. For example, if you have a method for naming volumes, you might name snapshots by using similar conventions. It is easier to filter and search for them later. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| __ To create a snapshot, verify that the volume is attached to a virtual server instance and that the instance is in a running state.|
| __ To create an instance from a snapshot of a boot volume, use a snapshot in the same region. |

{: caption="Table 1. Checklist for planning snapshots" caption-side="top"}


## Next Steps
{: #snapshots-vpc-planning-next-steps}

[Create snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
