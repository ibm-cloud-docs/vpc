---

copyright:
  years: 2019, 2026
lastupdated: "2026-01-29"

keywords: Block Storage, boot volume, data volume, status, health state, monitoring, performance 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring {{site.data.keyword.block_storage_is_short}} health states, volume status, and metrics
{: #block-storage-vpc-monitoring}

By using the UI, CLI, or API, you can check on the status and health states of your {{site.data.keyword.block_storage_is_short}} volumes. You can also view the cumulative number of read and write operations for a specific volume that is attached to a virtual server instance.
{: shortdesc}

## Monitoring {{site.data.keyword.block_storage_is_short}} performance
{: #block-storage-monitor}

You can monitor certain {{site.data.keyword.block_storage_is_short}} volume performance from the VPC virtual server instance metrics. These metrics include:

* Cumulative number of [bytes read for a volume](/docs/vpc?topic=vpc-vpc-monitoring-metrics#bytes-read-for-volume-gen2) since the virtual server instance was started.

* Cumulative number of [bytes written for a volume](/docs/vpc?topic=vpc-vpc-monitoring-metrics#bytes-written-for-volume-gen2) since the virtual server instance was started.

To see these metrics in the console, do the following.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Infrastructure ![VPC icon](../icons/vpc.svg) > Compute > Virtual server instances**.
2. Click the instance name to go to the instance details.
3. Click the **Monitoring** tab and scroll to the **Volume** metrics.
4. Select a volume. The read and write metrics are displayed. Figure 1 shows this view.

![Figure showing volume metrics.](/images/volume-read-write-metrics.png "Figure showing volume read/write metrics."){: caption="Read/write metrics for {{site.data.keyword.block_storage_is_short}} volumes." caption-side="bottom"}

## Block Storage volume status
{: #block-storage-vpc-status}

The following table shows the statuses that you might see when you create, view, or manage your {{site.data.keyword.block_storage_is_short}} volumes, and the associated health state.

| Status | Meaning | Health state |
|--------|---------|--------------|
| Available | A volume is available and can be attached to an instance. \n An attached data volume is available. \n A boot volume is available. | OK |
| Failed  | A volume creation failed. | Inapplicable |
| Pending | A volume is being created. | Inapplicable |
| Pending deletion | A volume is being deleted. When you're unsure if the volume is deleted, verify this state. Attempting to delete a volume again while in this state results in a conflict error. | Inapplicable |
| Updating | A volume's capacity is [expanding](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or the volume IOPS is getting [adjusted](/docs/vpc?topic=vpc-adjusting-volume-iops).  | OK |
| Unusable | A volume is unusable because the customer root key (CRK) was [deleted](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys) or [disabled](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys). | Inapplicable |
{: caption="Block Storage statuses" caption-side="bottom"}

## Block Storage volume health states
{: #block-storage-vpc-health-states}

Volume health states correspond with volume statuses. Table 2 describes the health states and health reasons.

| Health State | Reason |
|--------|-------------|
| OK | A volume is performing at the expected I/O performance and capacity is sufficient. No network connection issues are present. Or the volume is restored from a snapshot (and hydration is completed). |
| Degraded | A volume is experiencing degraded performance for any of the following reasons: \n  - Volume data is being restored (hydrated) and the volume shows as degraded until hydration is completed. \n - Volume initialization from a snapshot failed, and the volume hydration failed. \n - Volume hydration is not started. \n - Volume hydration is paused. \n - The snapshot is in an unusable state. |
| Inapplicable | The volume is being created. The volume creation failed. The volume is pending, pending deletion, or unusable. No health reasons are reported. |
| Faulted | The volume is unreachable, inoperative, or entirely incapacitated. |
{: caption="Block Storage health states and reasons" caption-side="bottom"}

For more information about the health states and reason codes in the API, see the [API reference](/apidocs/vpc/latest#create-volume) for creating, listing, and updating volumes.

## Volume performance when you restore from a snapshot
{: #block-vol-restore-snap}

Boot and data volume performance is initially degraded when you restore them from a snapshot. During the restoration, your data is copied from the regional storage repository to {{site.data.keyword.block_storage_is_short}}. After the restoration process is complete, full IOPS can be realized on the new volume.

Creating a boot volume from a "bootable" snapshot and then provisioning an instance with it results in slower performance. The slower performance is caused by the ongoing hydration process. The virtual server instance's performance is temporarily slower than it would be if you created the instance with a regular boot volume.
