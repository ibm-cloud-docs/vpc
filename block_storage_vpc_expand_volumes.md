---

copyright:
  years: 2020, 2026
lastupdated: "2026-03-12"

keywords: Block Storage, boot volume, data volume, volume, data storage, virtual server instance, instance, expandable volume

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Expanding {{site.data.keyword.block_storage_is_short}} volume capacity
{: #expanding-block-storage-volumes}

For {{site.data.keyword.block_storage_is_short}} boot and data volumes, you can increase volume capacity from its initial setting, within limits. You can expand volumes in the console, from the CLI, with the API, or Terraform.
{: shortdesc}

## Overview
{: #expand-volumes-overview}

You can increase the capacity of second-generation boot and data volumes at any time, regardless if they are attached to a virtual server or not. First-generation boot and data volumes must be attached to a running server when you attempt to increase the capacity.

After you expand a volume, you can't reduce its capacity. However, if your requirements change, you can expand the same volume again up to the maximum capacity that's available for its profile.
{: important}

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

You can use the UI, CLI, API, or Terraform to expand volume capacity. Your user authorization is verified before the volume is expanded. You can monitor the progress of your volume expansion from the UI or CLI. You can also use [Activity tracking](/docs/vpc?topic=vpc-at_events) to verify that the volume was expanded.

## Expandable volume concepts
{: #expandable-volume-concepts}

### Data volumes
{: #expand-data-vols-concepts}

After you provisioned your data volume with the `sdp` profile, you can increase its volume capacity in 1 GB increments up to 32,000 GB. If you provisioned data volumes with the first-generation profiles (tiered or custom), attach the volume to a running instance and then you can increase its volume size in GB increments up to 16,000 GB capacity, depending on your volume profile. The resizing operation causes no outage or lack of access to the storage.

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

To expand a first-generation volume, it must be in an _available_ state and attached to an instance that must be running. To expand a second-generation volume, it must be in an _available_ state.

You can use the UI, CLI, API, or Terraform to expand volume capacity. Your user authorization is verified before the volume is expanded.

You can expand the volume multiple times, up to its maximum capacity limit. After the volume is expanded, you can't reduce the volume capacity.
{: important}

Expanded capacity is determined by the maximum that is allowed by the volume's profile.

- Volumes that were created with the `sdp` profile can be expanded up to 32,000 GB.

- Volumes that were created with one of the volume profiles from the [tiered family](/docs/vpc?topic=vpc-block-storage-profiles) can be expanded to the maximum size for its tier:

   - A general-purpose, 3 IOPS/GB profile can be expanded up to 16,000 GB.
   - A 5 IOPS/GB profile can be expanded up to 9,600 GB.
   - A 10 IOPS/GB profile can be expanded up to 4,800 GB.

IOPS values are automatically adjusted for tiered profiles, based on the size of the volume. For example, if you expand a volume that was created with the 5 IOPS/GB profile from 250 GB to 1,000 GB, its maximum IOPS becomes 5,000 IOPS. Because a 5 IOPS/GB volume can potentially expand to 9,600 GB, the max IOPS would adjust to 48,000 IOPS. While the volume capacity is immediately changed, to realize increased IOPS, you must restart the instance.

Volumes that are created from a [custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) can be expanded within their custom IOPS range. Depending on the range you originally set, this range can be up to 16,000 GB. IOPS remains constant at the level that you set when you created the custom volume. You can later increase or decrease IOPS, based on the new size of the volume. For more information, see [Adjusting IOPS for Block Storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).

You can monitor the progress of your volume expansion from the UI or CLI. You can also use the [Activity tracking and](/docs/vpc?topic=vpc-at_events) to verify that the volume was expanded. After a volume is expanded, you can't reduce its capacity.

You can resize a data volume that is attached to an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, however you must restart the instance to use the resized volume.
{: note}

_z/OS_ When you expand Block Storage volume capacity on an existing z/OS virtual server instance, a new device address is broadcasted to the users with the additional storage size.
{: tip}

### Boot volumes
{: #expand-boot-vols}

By default, when you create an instance from a stock image and use the general-purpose profile, a 100 GB, 3,000 IOPS boot volume is created and attached to the instance. Instances that are created with a custom image or snapshot from the CLI, with the API or Terraform, can have a specified boot volume capacity in the range of 10 GB to 250 GB. In the console, the default minimum capacity of a boot volume is always 100 GB.

Regardless of the image type, you can increase boot volume capacity from its minimum provisioned size up to 250 GB. You can increase the capacity either when you provision an instance or later by updating the boot volume.

The boot volume expansion takes effect without a restart of the virtual server. However, to use the increased boot volume space, you must expand your operating system so the increased boot volume capacity is recognized.

When the sdp profile is used for the boot volume, a 100 GB, 3,000 IOPS boot volume is also the default.  With the SDP profile the max initial boot volume size is 250 GB with a max IOPS of 64000. You can increase sdp profile boot volume capacity from its initial provisioned size up to 32 TB post provision without a restart of the virtual server. However, to use the increased boot volume space, you must expand your operating system so the increased boot volume capacity is recognized.

Both general-purpose and sdp profile can be used as boot volumes.  The difference being that general-propose profile has a max size of 250 GB and max IOPS of 48000.  sdp profiles have a max size of 32 TB and max IOPS of 64000 IOPS.


{: note}

## Requirements
{: #exp-vol-requirements}

### Data volume requirements
{: #exp-data-vol-reqs}

You must meet the following requirements to increase a first-generation data volume's capacity.

* The volume is expanded based on their predefined volume profile or custom range.
* The volume must be in an _available_ state.
* The volume must be attached to a virtual server instance.
* The instance must be powered on and in a _running_ state.

Second-generation data volumes' capacity can be increased when the volumes are in _available_ state. They do not need to be attached to a virtual server instance.

No matter what generation your data volume is, you must detach and reattach the volume to the instance to adjust the available [volume bandwidth](/docs/vpc?topic=vpc-block-storage-bandwidth).

### Boot volume requirements
{: #exp-boot-vol-reqs}

You must meet these requirements to resize a boot volume:

* When an instance is provisioned, the size of the boot volume can be larger than the existing image size but not smaller. The maximum boot volume capacity is 250 GB.
* For an existing instance, you can increase the size of the boot volume up to the maximum size that was allowed during instance provisioning.
* For more information about supported Operating Systems, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images).

## Limitations
{: #expandable-volume-limitations}

Limitations for resizing boot and data volumes apply in this release.

### Data volume limitations
{: #expand-vol-limitations}

* You cannot expand a volume that is at its maximum capacity for its [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles) or [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) volume range.
* Data volumes can expand to 16,000 GB, with the following limitations:
    * If the volume was created by using an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles) that limits capacity to less than 16,000 GB, it can expand only to the allowed capacity for that tier.
    * If the volume was provisioned with the [custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) in a range that doesn't allow expanding to 16,000 GB, it can expand only to its maximum capacity for that custom range.
    * Volumes can expand multiple times until maximum capacity is reached.
* IOPS can increase to the maximum that is allowed by the IOPS tier profile. For the increased IOPS to take effect, you must restart the virtual server instance, or detach and reattach the volume from the instance. This behavior is unlike a capacity increase that doesn't incur any downtime.
* You can't independently modify IOPS for a volume that is created from an IOPS tier profile. IOPS is adjusted when you expand a volume's capacity and then restart the instance, or attach and detach the volume from the instance.
* When you expand a volume that was created from a custom profile, the capacity is increased but the IOPS remains the same. You can't independently increase the IOPS.
* Maximum IOPS for a first-generation volume is capped at [48,000 IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#tiers).
* After a volume is expanded, you can't reduce the size of the volume.
* When a volume is in transition, its state is _updating_. If the volume is detached while the volume expansion is in progress, the volume remains in an _updating_ state until you reattach it to an instance. After reattachment, the volume expansion resumes and completes.
* When you delete an instance, volumes that are marked for auto-deletion are not deleted when volume expansion is underway. For more information, see [troubleshooting block storage](/docs/vpc?group=tbs-block-storage).

### Boot volume limitations
{: #exp-vols-boot-limits}

* Boot volume capacity cannot be smaller than the size of the image. If the custom image is smaller than 10 GB, the boot volume capacity is rounded up to 10 GB.
* First-generation boot volumes that are stored as unattached volumes can't be expanded.
* The use of second-generation boot volumes with Z/OS systems are not supported.

## Expanding data volumes
{: #expanding-data-volumes}

You can increase the capacity of data volumes after you provisioned them in the console, from the CLI, with the API, or Terraform. First-generation data volumes must be attached to a running virtual server instance before you increase the capacity. Capacity of second-generation block volumes can be increased even when they are unattached.

You can't decrease volume capacity. However, if your requirements change, you can expand the same volume again up to the maximum capacity that's available for its profile.

The maximum size that you can expand to is based on the selected profile. You can increase the capacity of a second-generation volume in 1 GB increments up to 32,000 GB. The maximum capacity of first-generation volumes can be increased up to 16,000 GB. For a custom profile, you can expand the volume based on [sizing limits](#expandable-volume-limitations).

### Expand data volumes in the console
{: #expand-vpc-data-volumes-ui}
{: ui}

Follow these steps to expand volume capacity:

1. Go to the list of Block Storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**. By default, Block Storage volumes display for all resource groups in your region.

1. In the list of all **Block Storage for VPC volumes**, click the name of the volume you want to expand to see the volume details.

   The volume that you select must be attached to a virtual server instance. In the list of volumes, its attachment type is _data_.
   {: note}

1. On the volume details page, locate **Size**.

1. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). Alternatively, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), and select **Expand Block Storage volume**.

1. In the panel, you can increase the volume size in GB increments. The maximum size that you can expand to is based on the selected profile. The UI indicates the maximum capacity for the selected profile. When you increase the size of the volume, max IOPS and throughput are calculated for the expanded volume.

1. Review the estimated monthly order summary and new pricing.

1. If you're satisfied, click **Save and continue**. Your new block storage allocation is available in a few minutes.

Alternatively, you can locate the virtual server instance that the volume is attached to. Select the volume from the list of attached volumes to display its volume details. Then, follow Steps 3-7 to increase the volume capacity.

### Expand data volumes from the CLI
{: #expand-vpc-data-volumes-cli}
{: cli}

#### Before you begin
{: #expand-vpc-data-volumes-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

#### Expand data volume capacity from the CLI
{: #expand-data-vol-capacity-cli}

From the CLI, use the `ibmcloud is volume-update` command with the `--capacity` option to indicate the new size of the volume in GBs.

```sh
ibmcloud is volume-update VOLUME_ID --capacity CAPACITY_GB
```
{: pre}

The following example expands the capacity of a `general-purpose` volume to 8,000 MB.

```sh
ibmcloud is volume-update demo-volume-update --capacity 8000
```
{: pre}

When the update operation completes, run the `ibmcloud is volume` command to see the updated properties of the volume.

For more information about available command options, see [`ibmcloud is volume-update`](/docs/cli?topic=cli-vpc-reference#volume-update).

### Expand data volumes with the API
{: #expand-vpc-data-volumes-api}
{: api}

You can expand existing data volumes by calling the Virtual Private Cloud (VPC) API. Make a `PATCH /volumes` request to increase the capacity of a volume that is attached to an instance.

You can't update the name of the volume and expand capacity in the same `PATCH /volumes` request. Make two separate `PATCH/volumes` requests.
{: note}

This example call expands a volume with a capacity of 50 GB to 250 GB.

```sh
curl -X PATCH \
 "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-02-25&generation=2" \
 -H "Authorization: $iam_token" \
 -d '{
      "capacity": 250
    }'
```
{: codeblock}

The volume status shows `updating` while the volume is being expanded. The current capacity is shown.

```json
{
	"capacity": 50,
	"created_at": "2022-02-25T09:46:43.000Z",
	"crn": "crn:v1:bluemix:public:is:us-south-1:a/<Acc id>::volume:<Volume ID>",
    .
    .
    .
	"status": "updating",
    .
    .
    .
}
```
{: screen}

When the volume expansion completes, the new value displays, and the volume status is `available`.

### Expand data volumes with Terraform
{: #expand-vpc-data-volumes-terraform}
{: terraform}

To increase the capacity of a volume, use the `ibm_is_volume` resource. When applied, the following example updates the capacity to 8000 GB.

```terraform
resource "ibm_is_volume" "storage" {
  name    = "demo-volume-update"
  size    = 8000
  profile = "general-purpose"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Expanding boot volumes
{: #resize-boot-volumes}

You can increase the capacity of your boot volumes up to 250 GB during and after instance provisioning in the console, from the CLI, with the API, or Terraform. For first-generation boot volumes, you can increase the capacity when the volumes are attached to a running virtual server instance. The capacity of second-generation boot volumes can be increased even if the volumes are not attached to a running instance. The steps for increasing the capacity are the same for all volume profiles.

### Expand boot volume capacity in the console
{: #resize-vpc-boot-volumes-ui}
{: ui}

Increase boot volume capacity for new or existing instances in the console. For existing instances, you can increase the boot volume capacity by selecting a boot volume from the list of Block Storage volumes.

#### Expand boot volume capacity during instance provisioning in the console
{: #resize-boot-vol-new-instance-ui}

When you create an instance with a stock or custom image, the boot volume capacity must be equal to or larger than the minimum provisioned size of the image. For example, a stock image would show 100 GB by default. You can increase the boot volume size up to 250 GB while using that image. For more information about creating virtual server instances, see [Creating virtual server instances in the console](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).

You can also specify a larger boot volume capacity when you create an instance template. For more information, see [Creating an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#creating-instance-template).

#### Expand boot volume capacity from the list of Block Storage volumes in the console
{: #resize-boot-vol-list-ui}

For an existing instance, you can increase its boot volume capacity by selecting it from the list of Block Storage volumes.

1. Go to the list of Block Storage volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

2. Select a boot volume from the list of volumes. The attachment type is _boot_.

3. In the boot volume details, click the **Size** pencil icon. Alternatively, select **Expand volume** from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions").

4. In the side panel, increase the boot volume size in the **Create size** field. The size must be more than the current size up to 250 GB.

5. Click **Expand boot volume size**.

### Expand boot volume capacity from the CLI
{: #expand-boot-vols-cli}
{: cli}

#### Before you begin
{: #expand-boot-vols-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

#### Expand boot volume capacity when you create an instance from the CLI
{: #expand-new-boot-vols-cli}

Run the `ibmcloud is instance-create` command and specify a boot volume capacity in GBs.

The following example creates an instance with a boot volume of 190 GB.

```sh
ibmcloud is instance-create vsi-1 vpc-1 us-south-1 bx2-2x8  subnet-1 --image ibm-ubuntu-20-04-3-minimal-amd64-1 --boot-volume '{"name": "my-boot-vol-1", "volume": {"capacity": 190, "profile": {"name": "general-purpose"}}}'
```
{: pre}

You can also specify a larger boot volume capacity when you create an instance template from an image or a snapshot. See the following example.

```sh
ibmcloud is instance-template-create tpl-1 vpc-1 us-south-1 bx2-2x8  cli-subnet-1 --image ubuntu-20-04-3-minimal-amd64-1 --boot-volume '{"name": "my-boot-vol1", "volume": {"capacity": 190, "profile": {"name": "general-purpose"}}}'
```
{: screen}

For more information about creating virtual server instances from the CLI, see [Creating virtual server instances from the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli). For more information about the commands that are used for increasing boot volume size, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).

#### Expand the capacity of an existing boot volume from the CLI
{: #expand-existing-boot-vol-cli}

From the CLI, locate the boot volume that you want to expand. You can use the `ibmcloud is volumes` command filter the results by specifying the resource group. Also, if you know the name or ID of the instance, you can view instance details and get information about the boot volume.

After you located the volume, use the `volume-update` command and provide the ID or name of the boot volume. Use the `--capacity` parameter to indicate the new size of the boot volume in GBs.

For example, this example increases the capacity of my-boot-vol-1 to 200 GB. The existing capacity displays as the boot volume capacity is being expanded. Run the `ibmcloud is volume update` command and specify the volume name to see the new capacity.

```sh
ibmcloud is volume-update my-boot-vol-1 --capacity 200
```
{: pre}

When the update operation completes, run the `ibmcloud is volume` command to see the updated properties of the volume.

### Expand boot volume capacity with the API
{: #increase-vpc-boot-volumes-api}
{: api}

#### Expand boot volume capacity when you create an instance with the API
{: #expand-new-boot-vol-api}

When you create an instance by making a `POST \instances` request, you can specify larger boot volume capacity for any of these contexts: when you create the instance from an image, a source boot volume, or an instance template. Specify a boot volume name and capacity in the `boot-volume-attachment` property. The capacity for the boot volume must be at least the image's minimum provisioned size, which is the default if you don't specify capacity.

The following example creates a virtual server instance from an image, with a boot volume that has 250 GB capacity.

```sh
curl -X POST "$vpc_api_endpoint/v1/instances?version=2022-02-01&generation=2"\
-H "Authorization: $iam_token"\
-d '{
      "boot_volume_attachment": {
         "volume": {
           "capacity": 250",
           "encryption_key": {
             "crn": "crn:[...]"
           },
           "name": "my-boot-volume",
           "profile": {"name": "general-purpose"}
         }
       },
      "image": {"id": "9aaf3bcb-dcd7-4de7-bb60-24e39ff9d366"},
       .
       .
       .
   }'
```
{: codeblock}

For more information, see [Create an instance](/apidocs/vpc#create-instance) in the VPC API reference.

#### Expand the capacity of an existing boot volume with the API
{: #expand-existing-boot-vol-api}

With the API, locate the boot volume that you want to expand by making a `GET \volumes` call. Then, make a `PATCH \volumes` call with the ID of the boot volume and specify a new value for capacity.

For example, this call increases the capacity of a boot volume to 250 GB.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/volumes/$volume_id/?version=2022-02-12&generation=2"\
-H "Authorization: $iam_token" \
-d '{
      "capacity": 250,
   }'
```
{: codeblock}

### Expand the capacity of an existing boot volume with Terraform
{: #expand-existing-boot-vol-terraform}
{: terraform}

To increase the capacity of a boot volume, use the `ibm_is_volume` resource. When it's applied, the following example updates the capacity of the volume to 250 GB.

```terraform
resource "ibm_is_volume" "boot-volume-example" {
  name    = "my-boot-volume"
  size    = 250
  profile = "general-purpose"
  zone    = "us-south-2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Modifying the OS to use increased capacity
{: #modifying-os-expanded-volume}

After you expand the volume capacity, you have to make your OS recognize the capacity increase. You must independently grow the disk partition, and then increase the file system into the partition.

For more information about expanding the file system, see your OS Documentation. For example,
- [RHEL 9 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_logical_volumes/basic-logical-volume-management_configuring-and-managing-logical-volumes#resizing-logical-volumes_basic-logical-volume-management){: external}
- [Microsoft&reg; - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.

### Modifying the expanded boot volume in Linux
{: #modifying-the-linux-os-expanded-boot-volume}

The following example is based on CentOS Linux 7. Instructions for other Linux distributions can vary. After you increased the volume capacity from 100 GB to 250 GB, you can log in to the virtual server instance to validate the increase. Then, increase the partition and then expand the file system on the volume.

Extending a file system is a moderately risky operation. Consider taking a snapshot of the volume to prevent data loss.
{: tip}

1. Establish an SSH connection to your virtual server instance by using the floating IP address that is assigned to the instance. For more information, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
1. Run the `lsblk` command to see the list of attached storage volumes. In the following example, `vda` is the expanded boot volume, and `vdc` is the attached {{site.data.keyword.block_storage_is_short}} data volume. The `vdb` disk is an instance storage volume. You can see that the partitions on the `vda` disk remained unchanged, although the overall size is increased to 250G.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda    253:0    0  250G  0 disk
   ├─vda1 253:1    0  200M  0 part /boot/efi
   └─vda2 253:2    0 99.8G  0 part /
   vdb    253:16   0 69.9G  0 disk
   vdc    253:32   0  1.2T  0 disk /myvolumedir
   vdd    253:48   0  370K  0 disk
   vde    253:64   0   44K  0 disk
   ```
   {: screen}

1. Issue the `growpart` command to grow the partition size to cover the maximum available space.
   1. By using the `--dry-run` option, you can preview the changes before you perform the partition update.
      ```sh
      [root@docs-demo-instance ~]# growpart /dev/vda 1 --dry-run
       NOCHANGE: partition 1 is size 409600. it cannot be grown
      [root@docs-demo-instance ~]# growpart /dev/vda 2 --dry-run
       CHANGE: partition=2 start=411648 old: size=209303552 end=209715200 new: size=523876319 end=524287967
       # === old sfdisk -d ===
       # partition table of /dev/vda
        unit: sectors
       /dev/vda1 : start=     2048, size=   409600, Id=ef
       /dev/vda2 : start=   411648, size=209303552, Id=83, bootable
       /dev/vda3 : start=        0, size=        0, Id= 0
       /dev/vda4 : start=        0, size=        0, Id= 0
       # === new sfdisk -d ===
       # partition table of /dev/vda
       unit: sectors
       /dev/vda1 : start=     2048, size=   409600, Id=ef
       /dev/vda2 : start=   411648, size=523876319, Id=83, bootable
       /dev/vda3 : start=        0, size=        0, Id= 0
       /dev/vda4 : start=        0, size=        0, Id= 0
      ```
      {: screen}

   1. Update the partition size of the boot volume as shown in the following example.
      ```sh
      [root@docs-demo-instance ~]# growpart /dev/vda 2
      CHANGED: partition=2 start=411648 old: size=209303552 end=209715200 new: size=523876319 end=524287967
      ```
      {: screen}

1. Issue the command `lsblk` to verify that the partition is resized. The following example shows that the `vda2` partition is successfully increased in size.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
   vda    253:0    0   250G  0 disk
   ├─vda1 253:1    0   200M  0 part /boot/efi
   └─vda2 253:2    0 249.8G  0 part /
   vdb    253:16   0  69.9G  0 disk
   vdc    253:32   0   1.2T  0 disk /myvolumedir
   vdd    253:48   0   370K  0 disk
   vde    253:64   0    44K  0 disk
   ```
   {: screen}

   However, the file system still sees the `vda2` partition as 99G instead of 249G.

   ```sh
   [root@docs-demo-instance ~]# df -kh
   Filesystem      Size  Used Avail Use% Mounted on
   devtmpfs        3.9G     0  3.9G   0% /dev
   tmpfs           3.9G     0  3.9G   0% /dev/shm
   tmpfs           3.9G  385M  3.5G  10% /run
   tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda2        99G  1.3G   92G   2% /
   /dev/vda1       200M   12M  189M   6% /boot/efi
   /dev/vdc        1.2T   71M  1.2T   1% /myvolumedir
   tmpfs           783M     0  783M   0% /run/user/0
   ```
   {: screen}

1. Resize the file system on the partition with the `resize2fs` command.
   ```sh
   [root@docs-demo-instance ~]# resize2fs /dev/vda2
   resize2fs 1.42.9 (28-Dec-2013)
   Filesystem at /dev/vda2 is mounted on /; on-line resizing required
   old_desc_blocks = 13, new_desc_blocks = 32
   The filesystem on /dev/vda2 is now 65484539 blocks long.
   ```
   {: screen}

1. Verify that the file system is expanded. In the example, you can see that the size of `vda2` increased.

   ```sh
   [root@docs-demo-instance ~]# df -kh
   Filesystem      Size  Used Avail Use% Mounted on
   devtmpfs        3.9G     0  3.9G   0% /dev
   tmpfs           3.9G     0  3.9G   0% /dev/shm
   tmpfs           3.9G  385M  3.5G  10% /run
   tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda2       246G  1.3G  234G   1% /
   /dev/vda1       200M   12M  189M   6% /boot/efi
   /dev/vdc        1.2T   71M  1.2T   1% /myvolumedir
   tmpfs           783M     0  783M   0% /run/user/0
   ```
   {: screen}

### Modifying the expanded boot volume in Windows
{: #modifying-the-windows-os-expanded-boot-volume}

You can extend your boot partition with the DiskPart utility from the Command Prompt or PowerShell as an administrator.

If you used a stock image of Windows Server 2022 Standard Edition or Windows Server 2025 Standard Edition to create your virtual server, you might find that the Recovery partition (D: drive) is preventing you from extending the C: drive. To resolve this issue, see the troubleshooting topic: [Why can't I expand my C: drive on Windows Server?](/docs/vpc?topic=vpc-troubleshooting-unexpandable-c-drive)

### Modifying the expanded data volume in Linux
{: #modifying-the-linux-os-expanded-data-volume}

The following example is based on CentOS Linux 7. After you increased the volume capacity from 600 GB to 700 GB, you can log in to the virtual server instance to validate the increase. Then, increase the file system on the volume.

Extending a file system is a moderately risky operation. Consider taking a snapshot of the volume to prevent data loss.
{: tip}

1. Establish the SSH connection to your virtual server instance by using the floating IP address that is assigned to the instance. For more information, see [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux).
1. Run the `lsblk` command to see the updated capacity. In the following example, `vdc` is the attached Block Storage volume.
   ```sh
   [root@docs-demo-instance ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda    253:0    0  100G  0 disk
   ├─vda1 253:1    0  200M  0 part /boot/efi
   └─vda2 253:2    0 99.8G  0 part /
   vdb    253:16   0 69.9G  0 disk
   vdc    253:32   0  700G  0 disk /myvolumedir
   vdd    253:48   0  370K  0 disk
   vde    253:64   0   44K  0 disk
   ```
   {: screen}

1. The volume is resized to 700G, but the file system still shows the previous size, 619140256 blocks.
   ```sh
   [root@docs-demo-instance ~]# df -hk
   Filesystem     1K-blocks    Used Available Use% Mounted on
   devtmpfs         3993976       0   3993976   0% /dev
   tmpfs            4004356       0   4004356   0% /dev/shm
   tmpfs            4004356   25092   3979264   1% /run
   tmpfs            4004356       0   4004356   0% /sys/fs/cgroup
   /dev/vda2      102877120 1178920  96449228   2% /
   /dev/vda1         204580   11468    193112   6% /boot/efi
   tmpfs             800872       0    800872   0% /run/user/0
   /dev/vdc       619140256   73752 587592840   1% /myvolumedir
   ```
   {: screen}

1. Run the `resize2fs`command to increase the file system.
   ```sh
   [root@docs-demo-instance ~]# resize2fs /dev/vdc
   resize2fs 1.42.9 (28-Dec-2013)
   Filesystem at /dev/vdc is mounted on /myvolumedir; on-line resizing required
   old_desc_blocks = 75, new_desc_blocks = 88
   The filesystem on /dev/vdc is now 183500800 blocks long.
   ```
   {: screen}

   If the command returns `pvresize: command not found`, install the logical volume manager by running the command `dnf install lvm2`.
   {: tip}

1. Confirm the new file system size. The example shows 722352120 blocks.
   ```sh
   [root@docs-demo-instance ~]# df -hk
   Filesystem     1K-blocks    Used Available Use% Mounted on
   devtmpfs         3993976       0   3993976   0% /dev
   tmpfs            4004356       0   4004356   0% /dev/shm
   tmpfs            4004356   25092   3979264   1% /run
   tmpfs            4004356       0   4004356   0% /sys/fs/cgroup
   /dev/vda2      102877120 1178920  96449228   2% /
   /dev/vda1         204580   11468    193112   6% /boot/efi
   tmpfs             800872       0    800872   0% /run/user/0
   /dev/vdc       722352120   72816 686590468   1% /myvolumedir
   ```
   {: screen}

## Next steps
{: #exp-vols-next-steps}

Create more volumes or manage your existing Block Storage volumes.

* [Creating Block Storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Managing Block Storage volumes](/docs/vpc?topic=vpc-managing-block-storage).
* [Adjusting IOPS for Block Storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops).
