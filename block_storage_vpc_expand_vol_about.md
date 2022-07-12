---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: block storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# About increasing volume capacity

For {{site.data.keyword.block_storage_is_short}} boot and data volumes, you can increase volume capacity from its initial setting, within limits.
{: shortdesc}

## Expandable volume concepts
{: #expandable-volume-concepts}

You can increase capacity for boot and data volumes attached to a virtual server instance.

### Data volumes
{: #expand-data-vols}

For data volumes, when you create and attach a volume to a virtual server instance, you can later increase the size of the volume in GB increments up to 16,000 GB capacity, depending on your volume profile. This process doesn't require you to perform manual steps. For example, you don't need to migrate your data to a larger volume. There's no outage or lack of access to the storage while the volume is being resized.

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

To expand a volume, it must be in an _available_ state and the instance must be running. Your user authorization is verified before the volume is expanded. You can use the [UI](#expand-vpc-volumes-ui), [CLI](#expand-vpc-volumes-cli), or [API](#expand-vpc-volumes-api) to expand volume capacity. You can expand the volume multiple times, up to its [maximum capacity limit](#exp-vols-capacity-IOPs-limitations). After expanding the volume, you can't reduce the volume capacity.

Expanded capacity is determined by the maximum that is allowed by the volume's profile.

Volumes that were created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) can be expanded to the maximum size for its IOPS tier:

* A general-purpose, 3 IOPS/GB profile can be expanded up to 16,000 GB.
* A 5 IOPS/GB profile can be expanded up to 9,600 GB.
* A 10 IOPS/GB profile can be expanded up to 4,800 GB.

IOPS values are automatically adjusted for tiered profiles, based on the size of the volume. For example, if you expand a volume that was created by using a 5 IOPS/GB profile from the original size of 250 GB to the expanded size of 1,000 GB, it has a max IOPS of 5,000 IOPS (1,000 GB capacity _x_ 5 IOPS). Because a 5 IOPS/GB volume can potentially expand to 9,600 GB, the max IOPS would adjust to 48,000 IOPS. While the volume capacity is immediately changed, to realize increased IOPS, you must restart the instance.

Volumes that are created from a [Custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can be expanded within their custom IOPS range. Depending on the range you originally set, this can be up to 16,000 GB. IOPS remains constant at the level you set when you created the custom volume. You can later increase or decrease IOPS, based on the new size of the volume. For more information, see [Adjusting IOPS for block storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).

You can monitor the progress of your volume expansion from the UI or CLI. You can also use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to verify that the volume was expanded. After a volume is expanded, you can't reduce capacity.

You can resize a data volume that is attached to an IBM Cloud Hyper Protect Virtual Server for {{site.data.keyword.vpc_full}} instance, however you must restart the instance to use the resized volume.
{: note}

*z/OS - Experimental* When you expand block storage volume capacity on an existing z/OS virtual server instance, a new device address is broadcasted to the users with the additional storage size.
{: note}

### Boot volumes
{: #expand-boot-vols}

By default, when you create an instance from a stock image, a 100 GB, 3,000 IOPS boot volume is created and attached to the instance. Instances created from a custom image can have a boot volume capacity 10 GB to 250 GB.

For either stock or custom images, you can increase boot volume capacity from it's minimum provisioned size up to 250 GB, either when provisioning a new instance or by updating the boot volume from the list of block storage volumes. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

The expanded boot volume will take effect without a reboot. However, for the increased boot volume size to take effect, you must expand your operating system size so the increased boot volume capacity is recognized.
{: note}

## Requirements
{: #exp-vol-requirements}

### Data volume requirements
{: #exp-data-vol-reqs}

You must meet these requirements to increase a data volume's capacity.

* The volume is expanded based on their predefined IOPS profile or custom IOPS range.
* The volume must be attached to a virtual server instance.
* The volume must be in an _available_ state.
* The instance must be powered on and in a _running_ state.
* To realize maximum [volume bandwidth](/docs/vpc?topic=vpc-block-storage-bandwidth), you must detach and reattach the volume from the instance.

### Boot volume requirements
{: #exp-boot-vol-reqs}

You must meet these requirements to resize a boot volume:

* When provisioning a new instance, the size of the boot volume can be larger than the existing image size but not smaller.

* The maximum boot volume size is 250 GB.

* For an existing instance, you can increase the size of the boot volume up to the maximum size allowed during instance provisioning.
* There are no special requirements for encrypted boot volumes.
* Supported operating systems are x86 and Linux.

## Limitations
{: #expandable-volume-limitations}

Limitations for resizing boot and data volumes apply in this release.

### Data volume limitations

* You cannot expand a volume that is at its maximum capacity for its [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) or [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) volume range.

* Data volumes can expand to 16,000 GB, with the following limitations:
    * If the volume was created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta) that limits capacity to less than 16,000 GB, it can only expand to the allowed capacity for that tier.
    * If the volume is a [custom volume](/docs/vpc?topic=vpc-block-storage-profiles#custom) created in a lower range that doesn't allow expanding to 16,000 GB, it can only expand to its maximum capacity for that custom range.
    * Volumes can expand multiple times until maximum capacity is reached.
* IOPS can increase to the maximum that is allowed by the IOPS tier profile. While volume resizing does not incur downtime (that is, you don't need to restart the instance and reattach the volume), for the increased IOPS to take effect, you must restart the virtual server instance, or detach and reattach the volume from the instance.
* After you create a volume, can't change a volume's IOPS tier profile.
* You can't independently modify IOPS for a volume that is created from an IOPS tier profile. IOPS are adjusted when you expand a volume's capacity and then restart the instance, or attach and detach the volume from the instance.
* When you expand a volume that was created from a custom profile, the capacity is increased but the IOPS remains the same. You  can't independently increase the IOPS.
* Maximum IOPS for a volume is capped at [48,000 IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#tiers).
* After a volume is expanded, you can't reduce the size of the volume.
* When a volume is in transition, for example, the volume is detached while the volume expansion is in progress, the volume remains in an _updating_ state until you reattach it to an instance. The volume expansion the resumes and completes.
* When you delete an instance, volumes that are marked for auto-deletion are not deleted when volume expansion is underway. For more information, see [troubleshooting](/docs/vpc?topic=vpc-troubleshooting-block-storage#troubleshoot-topic-4).

### Boot volume limitations
{: #exp-vols-boot-limits}

* Boot volume capacity cannot be smaller than the size of the image. If the custom image is smaller than 10 GB, boot volume capacity is rounded up to 10 GB.

* You cannot increase boot volume capacity beyond the maximum image size, which is 250 GB.

* Boot volumes stored as unattached secondary volumes can't be expanded.

* Resizing takes place without a reboot. However, you must expand the size in your operating system so that the boot volume is recognized.

* Z/OS systems are not supported.

## Next steps
{: exp-vols-next-steps}

* [Expand block storage volumes that are attached to an instance](/docs/vpc?topic=vpc-expanding-block-storage-volumes).
* [Expand boot volumes attached to an instance](/docs/vpc?topic=vpc-resize-boot-volumes).
