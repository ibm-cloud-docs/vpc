---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-27"

keywords: instance storage, local disk, storage, temporary storage, generation 2, gen 2

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing instance storage
{: #instance-storage-provisioning}

Instance storage provides fast, affordable, temporary storage that can improve the performance of some cloud native workloads (or apps or services). For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

If you want instance storage disk to be provisioned with your virtual server instance, select a profile that has instance storage included. The instance storage disk devices cannot be live attached or detached, as they are purchased as part of the instance profile itself. The general purpose profile families (Balanced, Compute, and Memory) all have profile options with instance storage. The amount of instance storage is proportionally allocated based on the number of vCPUs assigned to the profile. For more information about profiles, see [x86 instance profiles](/docs/vpc?topic=vpc-profiles).

## Provisioning a Virtual Server Instance with instance storage with the UI
{: ui}

Before you can create a virtual server instance with instance storage, you need to create an {{site.data.keyword.vpc_short}}. {: important}

To provision a with instance storage, complete the following steps:
1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Virtual server instances**.
2. On the Virtual server instances for VPC page click **Create** and enter the required information. For more information about specific fields, see [Creating virtual servers](/docs/vpc?topic=vpc-creating-virtual-servers).
3. To choose your instances storage profile, click **View all profiles**. The profiles that include instance storage are indicated with the letter "d" in the fourth position of the Profile name, for example bx2d-2x8. Use the checkboxes on the left to filter the list.
4. Choose the profile that you want to use and click **Save**. The instance storage disks are attached to the virtual server instance after the boot volume, and before remote block storage volumes, if any.
5. When you are ready to provision your instance, click **Create virtual server instance**.

### Next steps
{: #next-steps-creating-virtual-servers-ui}

<!---A series of emails is sent to your administrator: Acknowledgment of the virtual server instance order, order approval and processing, and a message that the instance is created.--->

After the instance is created, [associate a floating IP address to the instance](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#reserving-a-floating-ip-address). Then, you can connect to your instance. For more information, see [Connecting to your Linux instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux) or [Connecting to your Windows instance](/docs/vpc?topic=vpc-vsi_is_connecting_windows). If you have an existing instance with a floating IP address, then it is not necessary to assign a second floating IP to another instance. You can connect to the first with a floating IP, then SSH to the second instance by using the private subnet IP address that is automatically assigned to it.

## Provisioning a Virtual Server Instance with instance storage with the CLI
{: cli}

To provision a virtual server instance with instance storage, follow the instructions for [Creating virtual servers by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli) and be sure to choose a profile that includes an instance storage disk. For more information about profiles, see [x86 instance profiles](https://test.cloud.ibm.com/docs/vpc?topic=vpc-profiles).

## Provisioning a Virtual Server Instance with instance storage with the API
{: api}

To provision a virtual server instance with instance storage, follow the instructions for [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis#create-instance-api-tutorial) and be sure to choose a profile that includes an instance storage disk. For more information about profiles, see [x86 instance profiles](https://test.cloud.ibm.com/docs/vpc?topic=vpc-profiles).

## Using custom images with instance storage disk 
If you are using custom images, make sure that you load the correct device drivers in the image to use the disks. 

If your instance storage device has a connection type of virtio_blk (all General Purpose profiles with instance storage - Balanced, Compute and Memory - offer only virtio_blk connections), then the libvirt virtio driver must be installed. The libvirt virtio driver is automatically installed with all IBM provided operating systems.

## Using instance storage from your instance

### Listing the block devices on your instance
On Linux&reg;, you can use the `lsblk` command to list the block devices after logging in to your instance. In the following example, the instance that is provisioned has two 600 GB instance disks and two 50 GiB data volumes. A list of custom columns is specified to the lsblk command in order to highlight some differences between instance storage and remote block storage.

Instance Disks are measured in gigabytes (GB), but lsblk shows size in gibibyte (GiB). 
{: note}
```
# lsblk -o NAME,SIZE,MAJ:MIN,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT
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

In this example all of the block storage devices are virtio devices: 
* vda is the boot volume
* vdb and vdc are the 600 GB instance storage disks. Since the lsblk command is displaying “G” units as base-2 Gibibytes, the size displays as 558.8 GiBs
* vdd and vde are remote data volumes that are attached to the instance. Since these data volumes are requested in the same units as lsblk is displaying, their size remains as whole GiBs (50).
* Notice that the physical sector size and logical sector size are much larger for the instance storage disks. To identify the disk name for all instance storage, you can run the following command. In this example, the command would return `/dev/vdb and /dev/vdc`.

```
lsblk -p -o NAME,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT | grep disk | grep 4096 | awk 'NR>0{print $1}'
```
{: pre}

* vdf and vdg are small block volumes that are used for the cloud-init configuration of the instance

### Partitioning, formatting, and mounting instance storage disks
The instance storage disks can be partitioned, formatted with a file system, and mounted into the hierarchical file system in the same manner as the remote block volumes. 

* For instructions for Linux&reg;, see [Using your block storage data volume (CLI)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume) or [Configuring a single disk instance storage by using the cloud-config script](/docs/vpc?topic=vpc-user-data#configure-instance-storage-cloud-config).
* For Windows&reg;, use the Computer Management UI to bring a block volume online, partition, and format it.

In the previous example, after partitioning vdc into two parts, formatting them with separate file systems, and mounting them, lsblk now reports the following for the pair instance storage disks:
```
# lsblk -o NAME,SIZE,TYPE,PHY-SEC,LOG-SEC,MOUNTPOINT,FSTYPE | grep -E "8192|NAME"
NAME          SIZE TYPE PHY-SEC LOG-SEC MOUNTPOINT  FSTYPE
vdb         558.8G disk    8192    4096
vdc         558.8G disk    8192    4096
├─vdc1        200G part    8192    4096 /localdisk1 ext4
└─vdc2      358.8G part    8192    4096 /localdisk2 xfs
```
{: screen}

Due to the ephemeral nature of instance storage, it is recommended that you configure the mount entry in the Linux&reg; VM’s /etc/fstab file with the "nofail” option (or leave the entry out entirely) to avoid a mount failure during boot. Here is an example mount entry with the "nofail" option:
```
/dev/vdb1 /mnt/inststg1 ext4 defaults,nofail 0 0
```
{: pre}

### Looking up instance storage details

Look for the instance storage column in the web UI or CLI when filtering on instance profiles.

Here are some example column values and what they mean:

* 1 x 150 GB – A single 150 GB block device is attached to the virtual server instance for instance storage.
* 2 x 600 GB – Two 600 GB (1200 total) block devices are attached

In some contexts, you might see an Interface Type for an instance profile or for an attached instance disk. The following is an example of obtaining details through the CLI for a specific instance profile.

```
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

Currently, the only supported Interface Type is virtio_blk . This is the standard virtualization block device and the disk shows up as a virtio block device. For more information about virtio, see Virtio.

### Reporting instance storage information
You can report instance storage information for your virtual server instance by using the following CLI example.

```
$ ibmcloud is instance-disks 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488
Listing disks of instance 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 under account YYYYYY as user vpc-user@myco.com...
ID                                          Name                               Size   Interface Type
0716-6ebed02f-c421-4b06-a73b-f334413e84b3   clunky-refocus-tidings-underpaid   600    virtio_blk
0716-54ee0659-d92a-4d4c-ac6b-8f03cb178a07   linguini-epiphany-lush-preschool   600    virtio_blk
```
{: screen}

### Updating one of the default names
You can update the custom names of the instance storage disks by using the following is a CLI example. 
```
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
