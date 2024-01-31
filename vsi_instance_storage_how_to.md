---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: instance storage, local disk, storage, temporary storage, generation 2, gen 2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing instance storage
{: #instance-storage-provisioning}

Instance storage provides fast, affordable, temporary storage that can improve the performance of some cloud native workloads (or apps or services). For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

To provision an instance storage disk with your virtual server instance, select a profile that includes instance storage. The instance storage disk devices cannot be live attached or detached, as they are purchased as part of the instance profile itself. The general-purpose profile families (Balanced, Compute, and Memory) and some specialty profile families (Very High Memory and Storage Optimized) all have profile options with instance storage. The amount of instance storage is proportionally allocated based on the number of vCPUs assigned to the profile. For more information about profiles, see [Instance profiles](/docs/vpc?topic=vpc-profiles).

## Provisioning a Virtual Server Instance with instance storage in the UI
{: #instance-storage-create-vsi-ui}
{: ui}

Before you can create a virtual server instance with instance storage, you need to create an {{site.data.keyword.vpc_short}}. {: important}

To provision a with instance storage, complete the following steps:
1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Virtual server instances**.
2. On the Virtual server instances for VPC page, click **Create** and enter the required information. For more information about specific fields, see [Creating virtual servers](/docs/vpc?topic=vpc-creating-virtual-servers).
3. To choose your instance storage profile, click **View all profiles**. All ox2 profiles and profiles with the letter "d" in the fourth position of the Profile name include instance storage. Examples of profiles that include instance storage are bx2d-2x8 and ox2-2x16. Use the checkboxes to filter the list. For a list of instance profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).
4. Choose the profile that you want to use and click **Save**. The instance storage disks are attached to the virtual server instance after the boot volume, and before remote Block Storage volumes, if any.
5. When you are ready to provision your instance, click **Create virtual server instance**.

### Next steps
{: #next-steps-creating-virtual-servers-ui}

After the instance is created, [associate a floating IP address to the instance](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#reserving-a-floating-ip-address). Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows). If you have an existing instance with a floating IP address, then it is not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance by using the private subnet IP address that is automatically assigned to it.

## Provisioning a Virtual Server Instance with instance storage from the CLI
{: #instance-storage-create-vsi-cli}
{: cli}

To provision a virtual server instance with instance storage, follow the instructions for [Creating virtual servers by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli). Choose a profile that includes an instance storage disk. For more information about profiles, see [Instance profiles](/docs/vpc?topic=vpc-profiles).

## Provisioning a Virtual Server Instance with instance storage with the API
{: #instance-storage-create-vsi-api}
{: api}

To provision a virtual server instance with instance storage, follow the instructions for [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=api#create-instance-api-tutorial). Choose a profile that includes an instance storage disk. For more information about profiles, see [Instance profiles](/docs/vpc?topic=vpc-profiles).

The API (or CLI) is useful for determining the interface type that is used for attaching the instance storage disks. The instance object that is returned from a create operation (or get) reports the instance disks in the `disks` array as shown in the following example partial output.

```json
  "disks": [
    {
      "created_at": "2022-04-19T17:13:28Z",
      "href": "https://sample-region.iaasdev.cloud.ibm.com/v1/instances/vsi_aba03916-8804-46ae-9275-b7dc196e94b3/disks/c4e4d137-a44f-47b8-beb0-a22dc8f8c6ea",
      "id": "c4e4d137-a44f-47b8-beb0-a22dc8f8c6ea",
      "interface_type": "virtio_blk",
      "name": "babbling-drove-grove-catnap",
      "resource_type": "instance_disk",
      "size": 600
    }]
```

This example shows an interface type of `virtio_blk`.

## Using custom images with instance storage disk
{: #instance-storage-custom-images}

If you are using custom images, make sure that you load the correct device drivers in the image to use the disks. 

If your instance storage device has an interface type of `virtio_blk`, then the `libvirt virtio` driver must be installed. The `libvirt virtio` driver is automatically installed with all IBM provided operating systems. All general-purpose profiles with instance storage offer only `virtio_blk` connections.

## Using instance storage from your instance
{: #instance-storage-use-from-instance}

### Listing the block devices on your instance
{: #instance-storage-list-block-device}

On Linux&reg;, you can use the `lsblk` command to list the block devices after you logged in to your instance. In the following example, the instance that is provisioned has two 600 GB instance disks and two 50 GiB data volumes. A list of custom columns is specified to the `lsblk` command to highlight some differences between instance storage and remote Block Storage.

Instance Disks are measured in gigabytes (GB), but `lsblk` shows size in gibibyte (GiB). 
{: note}

```sh
$ lsblk -o NAME,SIZE,MAJ:MIN,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT
NAME          SIZE MAJ:MIN TYPE PHY-SEC LOG-SEC MOUNTPOINT
vda           100G 252:0   disk     512     512
├─vda1          1G 252:1   part     512     512 /boot
└─vda2         99G 252:2   part     512     512
  ├─cl-root    97G 253:0   lvm      512     512 /
  └─cl-swap   2.1G 253:1   lvm      512     512
vdb         558.8G 252:16  disk    8192    4096
vdc         558.8G 252:32  disk    8192    4096
vdd            50G 252:48  disk     512     512
vde            50G 252:64  disk     512     512
vdf           366K 252:80  disk     512     512
vdg            44K 252:96  disk     512     512
```
{: screen}

In this example all of the Block Storage devices are `virtio_blk` devices:

* `vda` is the boot volume.
* `vdb` and `vdc` are the 600 GB instance storage disks. Since the lsblk command is displaying “G” units as base-2 gibibytes, the size is displayed as 558.8 GiBs.
* `vdd` and `vde` are remote data volumes that are attached to the instance. Since these data volumes are requested in the same units as `lsblk` is displaying, their size remains as whole GiBs (50).
* Notice that the physical sector size and logical sector size are much larger for the instance storage disks. To identify the disk name for all instance storage, you can run the following command. In this example, the command returns `/dev/vdb` and `/dev/vdc`.

```sh
lsblk -p -o NAME,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT | grep disk | grep 4096 | awk 'NR>0{print $1}'
```
{: pre}

* `vdf` and `vdg` are small block volumes that are used for the cloud-init configuration of the instance.

### Partitioning, formatting, and mounting instance storage disks
{: #instance-storage-partition-formatting-mounting}

The instance storage disks can be partitioned, formatted with a file system, and mounted into the hierarchical file system in the same manner as the remote block volumes. 

* For instructions for Linux&reg;, see [Set up your Block Storage for VPC data volume for use (Linux)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-lin) or [Configuring a single disk instance storage by using the cloud-config script](/docs/vpc?topic=vpc-user-data#configure-instance-storage-cloud-config).
* For Windows&reg;, use the Computer Management UI to bring a block volume online, partition, and format it. For more information, see [Set up your Block Storage for VPC data volume for use (Windows)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-win).

In the previous example, after partitioning `vdc` into two parts, formatting them with separate file systems, and mounting them, `lsblk` now reports the following for the pair instance storage disks.

```sh
$ lsblk -o NAME,SIZE,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT,FSTYPE | grep -E "4096|NAME"
NAME          SIZE TYPE PHY-SEC LOG-SEC MOUNTPOINT  FSTYPE
vdb         558.8G disk    8192    4096
vdc         558.8G disk    8192    4096
├─vdc1        200G part    8192    4096 /localdisk1 ext4
└─vdc2      358.8G part    8192    4096 /localdisk2 xfs
```
{: screen}

Due to the ephemeral nature of instance storage, configure the mount entry in the Linux&reg; VM’s `/etc/fstab` file with the `nofail` option, or leave the mount entry out entirely. This setting avoids a mount failure during boot.
{: important}

The following example shows a mount entry with the `nofail` option.

```sh
/dev/vdb1 /mnt/inststg1 ext4 defaults,nofail 0 0
```
{: pre}

### Looking up instance storage details
{: #instance-storage-lookup-details}

Look for the instance storage column in the web UI or CLI when you filter instance profiles.

The following examples show possible column values and what they mean:

* 1 x 150 GB – A single 150 GB block device is attached to the virtual server instance for instance storage.
* 2 x 600 GB – Two 600 GB (1200 total) block devices are attached.

In some contexts, you might see an Interface Type for an instance profile or for an attached instance disk. The following example shows details that were obtained from the CLI for a specific instance profile.

```sh
$ ibmcloud is in-pr bx2d-48x192
Getting instance profile bx2d-48x192 under account YYYYYY as user vpc-user@myco.com...

Name                     bx2d-48x192
Architecture             amd64
Family                   balanced
vCPUs                    48
Memory(G)                192
GPUs                     -
Network(Gbps)            80
Instance Storage Disks   Quantity   Size   Supported Interface Types
                         2          900    virtio_blk
```
{: screen}

Currently, the only supported Interface Type is `virtio_blk`. It's the standard virtualization block device, and the disk shows up as a Virtio block device. For more information about Virtio, see Virtio.

### Reporting instance storage information
{: #instance-storage-reporting}

You can report instance storage information for your virtual server instance by using the following CLI example.

```sh
$ ibmcloud is instance-disks 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488
Listing disks of instance 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 under account YYYYYY as user vpc-user@myco.com...
ID                                          Name                               Size   Interface Type
0716-6ebed02f-c421-4b06-a73b-f334413e84b3   clunky-refocus-tidings-underpaid   600    virtio_blk
0716-54ee0659-d92a-4d4c-ac6b-8f03cb178a07   linguini-epiphany-lush-preschool   600    virtio_blk
```
{: screen}

### Updating one of the default names
{: #instance-storage-update-name}

You can update the custom names of the instance storage disks by using the following CLI command. 

```sh
$ ibmcloud is instance-disk-update 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 0716-6ebed02f-c421-4b06-a73b-f334413e84b3 --name my-instance-disk1
Updating instance disk 0716-6ebed02f-c421-4b06-a73b-f334413e84b3 of instance 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 under account YYYYYY as user vpc-user@myco.com...

ID               0716-6ebed02f-c421-4b06-a73b-f334413e84b3
Name             my-instance-disk1
Size             600
Interface Type   virtio_blk
Created          2020-10-29T22:57:15-05:00

$ ibmcloud is instance-disks 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488
Listing disks of instance 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 under account YYYYYY as user vpc-user@myco.com...
ID                                          Name                               Size   Interface Type
0716-6ebed02f-c421-4b06-a73b-f334413e84b3   my-instance-disk1                  600    virtio_blk
0716-54ee0659-d92a-4d4c-ac6b-8f03cb178a07   linguini-epiphany-lush-preschool   600    virtio_blk
```
{: screen}
