---

copyright:
  years: 2020, 2023
lastupdated: "2023-06-30"

keywords: Block Storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About increasing volume capacity
{: #about-increasing-volume-capacity}

For {{site.data.keyword.block_storage_is_short}} boot and data volumes, you can increase volume capacity from its initial setting, within limits.
{: shortdesc}

## Expandable volume concepts
{: #expandable-volume-concepts}

You can increase capacity for boot and data volumes that are attached to a virtual server instance.

### Data volumes
{: #expand-data-vols}

After you provisioned and attached a data volume to a virtual server instance, you can increase its volume size in GB increments up to 16,000 GB capacity, depending on your volume profile. This process doesn't require you to perform manual steps. For example, you don't need to migrate your data to a larger volume. The resizing operation causes no outage or lack of access to the storage.

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

To expand a volume, it must be in an _available_ state and the instance must be running. Your user authorization is verified before the volume is expanded. You can use the UI, CLI, API, or Terraform to expand volume capacity. You can expand the volume multiple times, up to its [maximum capacity limit](#exp-vols-capacity-IOPs-limitations). After the volume is expanded, you can't reduce the volume capacity.

Expanded capacity is determined by the maximum that is allowed by the volume's profile.

Volumes that were created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles) can be expanded to the maximum size for its IOPS tier:

* A general-purpose, 3 IOPS/GB profile can be expanded up to 16,000 GB.
* A 5 IOPS/GB profile can be expanded up to 9,600 GB.
* A 10 IOPS/GB profile can be expanded up to 4,800 GB.

IOPS values are automatically adjusted for tiered profiles, based on the size of the volume. For example, if you expand a volume that was created by using a 5 IOPS/GB profile from the original size of 250 GB to the expanded size of 1,000 GB, it has a max IOPS of 5,000 IOPS (1,000 GB capacity _x_ 5 IOPS). Because a 5 IOPS/GB volume can potentially expand to 9,600 GB, the max IOPS would adjust to 48,000 IOPS. While the volume capacity is immediately changed, to realize increased IOPS, you must restart the instance.

Volumes that are created from a [Custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can be expanded within their custom IOPS range. Depending on the range you originally set, this range can be up to 16,000 GB. IOPS remains constant at the level that you set when you created the custom volume. You can later increase or decrease IOPS, based on the new size of the volume. For more information, see [Adjusting IOPS for Block Storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).

You can monitor the progress of your volume expansion from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the volume was expanded. After a volume is expanded, you can't reduce its capacity.

You can resize a data volume that is attached to an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, however you must restart the instance to use the resized volume.
{: note}

_z/OS - Experimental_ When you expand Block Storage volume capacity on an existing z/OS virtual server instance, a new device address is broadcasted to the users with the additional storage size.
{: note}

### Boot volumes
{: #expand-boot-vols}

By default, when you create an instance from a stock image, a 100 GB, 3,000 IOPS boot volume is created and attached to the instance. Instances that are created from a custom image can have a boot volume capacity of 10 GB to 250 GB.

Regardless of the image type, you can increase boot volume capacity from its minimum provisioned size up to 250 GB. You can increase the capacity either when you provision an instance or later by updating the boot volume. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

The boot volume expansion takes effect without a reboot. However, to use the increased boot volume space, you must expand your operating system so the increased boot volume capacity is recognized. For more information, see [Modifying a Linux OS for expanding boot volumes](/docs/vpc?topic=vpc-modifying-the-linux-os-expanded-boot-volume).
{: note}

## Requirements
{: #exp-vol-requirements}

### Data volume requirements
{: #exp-data-vol-reqs}

You must meet the following requirements to increase a data volume's capacity.

* The volume is expanded based on their predefined IOPS profile or custom IOPS range.
* The volume must be attached to a virtual server instance.
* The volume must be in an _available_ state.
* The instance must be powered on and in a _running_ state.
* To realize maximum [volume bandwidth](/docs/vpc?topic=vpc-block-storage-bandwidth), you must detach and reattach the volume from the instance.

### Boot volume requirements
{: #exp-boot-vol-reqs}

You must meet these requirements to resize a boot volume:

* When an instance is provisioned, the size of the boot volume can be larger than the existing image size but not smaller. The maximum boot volume size is 250 GB.
* For an existing instance, you can increase the size of the boot volume up to the maximum size that was allowed during instance provisioning.
* Encrypted boot volumes have no special requirements.
* Supported operating systems are x86 and Linux.

## Limitations
{: #expandable-volume-limitations}

Limitations for resizing boot and data volumes apply in this release.

### Data volume limitations
{: #expand-vol-limitations}

* You cannot expand a volume that is at its maximum capacity for its [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles) or [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) volume range.

* Data volumes can expand to 16,000 GB, with the following limitations:
    * If the volume was created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles) that limits capacity to less than 16,000 GB, it can expand only to the allowed capacity for that tier.
    * If the volume was provisioned with a [custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) that is created in a range that doesn't allow expanding to 16,000 GB, it can expand only to its maximum capacity for that custom range.
    * Volumes can expand multiple times until maximum capacity is reached.
* IOPS can increase to the maximum that is allowed by the IOPS tier profile. For the increased IOPS to take effect, you must restart the virtual server instance, or detach and reattach the volume from the instance. This behavior is unlike a capacity increase that doesn't incur any downtime.
* You can't independently modify IOPS for a volume that is created from an IOPS tier profile. IOPS is adjusted when you expand a volume's capacity and then restart the instance, or attach and detach the volume from the instance.
* When you expand a volume that was created from a custom profile, the capacity is increased but the IOPS remains the same. You can't independently increase the IOPS.
* Maximum IOPS for a volume is capped at [48,000 IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#tiers).
* After a volume is expanded, you can't reduce the size of the volume.
* When a volume is in transition, its state is _updating_. If the volume is detached while the volume expansion is in progress, the volume remains in an _updating_ state until you reattach it to an instance. After reattachment, the volume expansion resumes and completes.
* When you delete an instance, volumes that are marked for auto-deletion are not deleted when volume expansion is underway. For more information, see [troubleshooting](/docs/vpc?topic=vpc-troubleshooting-block-storage#troubleshoot-topic-4).

### Boot volume limitations
{: #exp-vols-boot-limits}

* Boot volume capacity cannot be smaller than the size of the image. If the custom image is smaller than 10 GB, the boot volume capacity is rounded up to 10 GB.
* You cannot increase boot volume capacity beyond the maximum image size, which is 250 GB.
* Boot volumes stored as unattached secondary volumes can't be expanded.
* Resizing takes place without a reboot. However, you must expand the size in your operating system so that the boot volume is recognized.
* Z/OS systems are not supported.

## Next steps
{: #exp-vols-next-steps}

* [Expand Block Storage volumes that are attached to an instance](/docs/vpc?topic=vpc-expanding-block-storage-volumes).
* [Expand boot volumes that are attached to an instance](/docs/vpc?topic=vpc-resize-boot-volumes).
