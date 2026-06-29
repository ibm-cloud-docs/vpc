---

copyright:
  years: 2022, 2026
lastupdated: "2026-06-29"

keywords: block storage, snapshots, planning, backups, consistency group

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-planning}

Plan your block storage snapshot strategy in {{site.data.keyword.vpc_short}}, including naming conventions, encryption, backup policies, and cross-regional replication.
{: shortdesc}

## Planning to create and use snapshots
{: #planning-for-data-encryption-snap}

Consider the following topics and prerequisites before you create snapshots.

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

When you create a cross-regional copy of a snapshot, the new snapshot is named `[copy]-[source-snapshot-name]`. For example, a cross-regional copy of the snapshot-my-volume-snapshot1_ is automatically named _copy-my-volume-snapshot1_ when it is placed in the target region. Cross-regional copies of snapshots are independent from the source snapshot and the source volume, and the copies can be managed like any other normal snapshot.

## Planning checklist
{: #planning-checklist}

| Item | Considerations |
|------|----------------|
| {{site.data.keyword.iamshort}} permissions | Confirm that you have the necessary role to create snapshots. For more information, see [IAM roles and actions for Block Storage Snapshots for VPC](/docs/iam?topic=iam-iam-service-roles-actions#is.snapshot-roles) and [IAM roles and actions for Multi Volume Snapshots for VPC](/docs/iam?topic=iam-iam-service-roles-actions#is.snapshot-consistency-group-roles). |
| Interface | Choose between the UI, CLI, API, or Terraform to create and manage your snapshots. |
| Volumes | - Evaluate which volumes are most important to snapshot. You can create a snapshot of boot and data volumes. \n - Evaluate the amount of change that you expect for the volumes that you intend to snapshot. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all snapshots for a volume can't exceed 10 TB. |
| Consistency groups | You can [create a consistency group](/docs/vpc?topic=vpc-snapshots-vpc-create-consistency-groups) to take snapshots of multiple Block Storage volumes that are attached to a single virtual server instance at the same time. The snapshots in the consistency group can be used later to restore a virtual server instance's boot and data volumes. Restoring an instance directly from snapshot consistency group identifier is not supported. |
| Naming conventions | Select a unique name for your snapshot or consistency group. For example, if you have a method for naming volumes, you might name snapshots by using similar conventions. It's easier to filter and search for them later. For more information, see [Naming snapshots](#snapshots-vpc-naming). \n When you create a consistency group, its name cannot exceed 64 characters. The snapshots in the consistency group are named by using the first 16 characters of the group name and 3-4 unique generated characters. |
| Snapshots retention | Evaluate how many snapshots to retain, how long you need to retain them, and when to delete snapshots. Review the [snapshots limitations](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots-vpc-limitations) before you perform these actions. |
| Restoring a volume | Consider when you might want to restore a volume from a snapshot. If you need to revert to an earlier version of the volume, plan which snapshot you want to use to create the volume. Evaluate the volumes that you need to restore and attach to an instance, and the volumes that you might restore as unattached volumes. |
| Fast restore | Evaluate when to enable snapshots for [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-use-fast-restore). This feature can be used in disaster recovery scenarios when you need to restore volumes in a different zone of the same region. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot. The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.|
| Cross-regional copy | You can create and store a copy of a snapshot in another region, and use it to create new volumes. By using a copy of a snapshot to create volumes in the target region, your data can be replicated across regions. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Think about whether you need to restore data in other regions. |
| Virtual server instances | To create a snapshot from a volume that is attached to an instance, verify that the instance is in a running state. \n To create an instance from a snapshot of a boot volume, use a snapshot in the same region. |
| Billing | Think about the number of snapshots that you want to take. Consider whether you want to create fast-restore clones in other zones of your region. Evaluate if you need cross-regional copies. You're charged for the storage consumption and the data transfer separately. Billing for the fast restore clones is set at a flat rate based on instance hours, and it is considerably more costly than regular snapshots. For more information, see the [FAQs](/docs/vpc?topic=vpc-snapshots-vpc-faqs#faq-snapshot-pricing). |
| Performance | Review the [performance impacts](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-performance-considerations) for restoring a volume from a snapshot. |
{: caption="Checklist for planning snapshots" caption-side="bottom"}

## Next Steps
{: #snapshots-vpc-planning-next-steps}

After you planned your snapshots, you can [create snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
