---

copyright:
  years: 2022, 2023
lastupdated: "2022-06-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning snapshots
{: #snapshots-vpc-planning}

When you plan a snapshot strategy for your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful when you configure and use snapshots.
{: shortdesc}

## Planning to create and use snapshots
{: #planning-for-data-encryption-snap}

Consider the following prerequisites before you create snapshots.

| Item | Considerations |
|------|----------------|
| {{site.data.keyword.iamshort}} permissions | Confirm that you have the necessary [IAM access permissions](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam) to create snapshots. |
| Interface | Choose between the UI, CLI, API, or Terraform to create and manage your snapshots. |
| Volumes | - Evaluate which volumes are most important to snapshot. You can create a snapshot of boot and data volumes. \n - Evaluate the amount of change that you expect for the volumes that you intend to snapshot. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all snapshots for a volume can't exceed 10 TB. |
| Naming conventions | Select a unique name for your snapshot. For example, if you have a method for naming volumes, you might name snapshots by using similar conventions. It's easier to filter and search for them later. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| Snapshots retention | Evaluate how many snapshots to retain, how long you need to retain them, and when to delete snapshots. Review the [snapshots limitations](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots-vpc-limitations) before you perform these actions. |
| Restoring a volume | Consider when you might want to restore a volume from a snapshot. If you need to revert to an earlier version of the volume, plan which snapshot you want to use to create the volume. Evaluate the volumes that you need to restore and attach to an instance, and the volumes that you might restore as unattached volumes. |
| Fast restore | Evaluate when to enable snapshots for [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-use-fast-restore). This feature can be used in disaster recovery scenarios when you need to restore volumes in a different zone of the same region. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot. |
| Cross-regional copy | You can create and store a copy of a snapshot in another region, and use it to create volumes in the target region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Think about whether you need to restore data in other regions. |
| Virtual server instances | To create a snapshot from a volume that is attached to an instance, verify that the instance is in a running state. \n To create an instance from a snapshot of a boot volume, use a snapshot in the same region. |
| Billing | Think about the number of snapshots that you want to take. Consider whether you want to create fast-restore clones in other zones of your region. Evaluate if you need cross-regional copies. You're charged for the storage consumption and the data transfer separately. Billing for the fast restore feature is set at a flat rate based on instance hours. For more information, see the [FAQs](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=api#faq-snapshot-pricing). |
| Performance | Review the [performance impacts](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-performance-considerations) for restoring a volume from a snapshot. |
{: caption="Table 1. Checklist for planning snapshots" caption-side="bottom"}

## Next Steps
{: #snapshots-vpc-planning-next-steps}

After you planned your snapshots, you can [create snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
