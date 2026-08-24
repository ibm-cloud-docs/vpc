---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, snapshot status, lifecycle states, monitoring

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring {{site.data.keyword.block_storage_is_short}} snapshot lifecycle states
{: #snapshots-vpc-monitoring}

By using the UI, CLI, or API, you can check on the lifecycle status of your {{site.data.keyword.block_storage_is_short}} snapshots.
{: shortdesc}

## Snapshot lifecycle states
{: #snapshots-vpc-status}

The following table describes the snapshot states in the snapshot lifecycle. You can see these states in the console, the command outputs of the CLI, in the API responses, and in Terraform data sources.

| Snapshot status | Explanation |
|-----------------|-------------|
| Stable | The snapshot is available for restoring a volume. |
| Waiting | Snapshot information is being retrieved. |
| Pending | While the snapshot is being created, the percentage completed displays. |
| Failed | The snapshot failed to be created, the volume can't be restored from a snapshot. |
| Suspended | Snapshot is temporarily unavailable. |
| Updating | You changed something about the snapshot and it is being updated. |
| Deleting | The snapshot is being deleted. |
| Deleted | The snapshot was deleted and is not available to restore volumes. |
{: caption="Snapshot lifecycle states" caption-side="bottom"}

## Next steps
{: #snapshots-vpc-monitoring-next-steps}

* [View snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view)
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore)
