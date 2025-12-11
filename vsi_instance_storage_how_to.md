---

copyright:
  years: 2020, 2025
lastupdated: "2024-03-28"

keywords: instance storage, local disk, storage, temporary storage, format, mount, metadata service, generation 2, gen 2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing instance storage
{: #instance-storage-provisioning}

Instance storage provides fast, affordable, temporary storage that can improve the performance of some cloud native workloads (or apps or services). For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

To provision an instance storage disk with your virtual server instance, select a profile that includes instance storage. The instance storage disk devices can't be live attached or detached because they are purchased as part of the instance profile itself. The general-purpose profile families (Balanced, Compute, and Memory) and some specialty profile families (Very High Memory and Storage Optimized) all have profile options with instance storage. All 3rd generation instance profiles include instance storage. For more information, see [3rd generation instance profiles](/docs/vpc?topic=vpc-profiles#next-gen-profiles). The amount of instance storage is proportionally allocated based on the number of vCPUs assigned to the profile. For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).

## Provisioning a Virtual Server Instance with instance storage in the console
{: #instance-storage-create-vsi-ui}
{: ui}

Before you can create a virtual server instance with instance storage, you need to create an {{site.data.keyword.vpc_short}}. {: important}

To provision a with instance storage, complete the following steps:
1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
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
      "href": "https://sample-region.iaas.cloud.ibm.com/v1/instances/vsi_aba03916-8804-46ae-9275-b7dc196e94b3/disks/c4e4d137-a44f-47b8-beb0-a22dc8f8c6ea",
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
vdb         558.8G 252:16  disk    4096    4096
vdc         558.8G 252:32  disk    4096    4096
vdd            50G 252:48  disk     512     512
vde            50G 252:64  disk     512     512
vdf           366K 252:80  disk     512     512
vdg            44K 252:96  disk     512     512
```
{: screen}

In this example all of the Block Storage devices are `virtio_blk` devices:

* `vda` is the boot volume.
* `vdb` and `vdc` are the 600 GB instance storage disks. In Linux&reg;, instance storage disks follow the boot volume with the first one being `/dev/vdb`. Since the lsblk command is displaying “G” units as base-2 gibibytes, the size is displayed as 558.8 GiBs. The sector size column values might differ depending on the operating system or the generation of profile. For example, `x3d` instance profile families might provision with 512 byte sector sizes for local disks.
* `vdd` and `vde` are remote data volumes that are attached to the instance. Since these data volumes are requested in the same units as `lsblk` is displaying, their size remains as whole GiBs (50).
* `vdf` and `vdg` are small block volumes that are used for the cloud-init configuration of the instance.

### Using the Metadata service to list instance storage
{: #instance-storage-metadata-service-list}

The metadata service can be used from within the instance to retrieve information about the instance, including instance storage disks. To retrieve metadata information, the metadata service capability must be enabled when the instance is provisioned or by updating the instance afterward. For more information on enabling and usage procedure, see [Accessing metadata from an instance](/docs/vpc?topic=vpc-imd-access-instance-metadata).

If you have an instance with the Metadata service enabled, the following steps demonstrate an example of utilizing instance storage metadata from a Linux virtual server.

1. Retrieve a short-lived instance identity token.

   ```bash
   instance_identity_token=`curl -X PUT "http://api.metadata.cloud.ibm.com/instance_identity/v1/token?version=2022-03-01" -H "Metadata-Flavor: ibm" -H "Accept: application/json" -d '{ "expires_in": 300}' | jq -r '(.access_token)'`
   ```
   {: codeblock}

   Use HTTPS instead of HTTP if the secure metadata service endpoint is enabled. You might need to install `curl` or `jq` packages before running the curl command. `jq` is a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/).
   {: note}

2. Use the token to obtain instance metadata and select the `disks` portion and store the json in a bash variable.

   ```bash
   disks=$(curl -sS -X GET "http://api.metadata.cloud.ibm.com/metadata/v1/instance?version=2024-02-20" -H "Authorization: Bearer $instance_identity_token" | jq -r '.disks')
   ```
   {: codeblock}

3. Loop over `virtio` devices in `/dev/disk/by-id` and correlate device paths to the instance disk information stored in the previous step.

   ```bash
   for ln in $(ls /dev/disk/by-id/virtio*); do b=$(basename $ln); s=${b#virtio-}; disk=$(echo $disks | jq -r ".[] | select(.id | startswith(\"$s\"))"); if [ -n "$disk" ]; then echo "$ln $(readlink -f $ln) $(echo $disk | jq -r '(.size|tostring) + " " + .name')";fi;done
   ```
   {: codeblock}

The following is example output of the prior command where the column order is (1) by-id path, (2) symlink device target, (3) size in GB, and (4) the Cloud UI/CLI/API disk name:

```text
/dev/disk/by-id/virtio-0736-35db69bb-d09b-4 /dev/vdc 600 palmtree-vacuumed-judiciary-wharf
/dev/disk/by-id/virtio-0736-874803ac-2593-4 /dev/vdb 600 lunacy-dismantle-dotted-fiddling
```
{: screen}

In this case, the disk names are the random four-word generated names, but these can be edited to have custom names. For more information, see [Updating one of the default names](#instance-storage-update-name).

### Partitioning, formatting, and mounting instance storage disks
{: #instance-storage-partition-formatting-mounting}

The instance storage disks can be partitioned, formatted with a file system, and mounted into the hierarchical file system in the same manner as the remote block volumes. 

* For instructions for Linux&reg;, see [Set up your {{site.data.keyword.block_storage_is_short}} data volume for use (Linux)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-lin) or [Configuring a single disk instance storage by using the cloud-config script](/docs/vpc?topic=vpc-user-data#configure-instance-storage-cloud-config).
* For Windows&reg;, use the Computer Management UI to bring a block volume online, partition, and format it. For more information, see [Set up your {{site.data.keyword.block_storage_is_short}} data volume for use (Windows)](/docs/vpc?topic=vpc-start-using-your-block-storage-data-volume-win).

The following `lsblk` example shows the output after logging into a `bx3d-48x240` instance and partitioning `vdc` into two primary partitions, formatting them with separate file systems, and mounting them. The `grep` command with regular expression is used to filter the output to just the vdb and vdc devices obtained through the Metadata service (refer to the prior section).

```sh
$ lsblk | grep -E "NAME|vd[b|c]"
NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
vdb     252:16   0 726.4G  0 disk
vdc     252:32   0 726.4G  0 disk
├─vdc1  252:33   0   200G  0 part /mnt/localdisk1
└─vdc2  252:34   0 526.4G  0 part /mnt/localdisk2
```
{: screen}

Due to the ephemeral nature of instance storage, configure the mount entry in the Linux&reg; VM’s `/etc/fstab` file with the `nofail` option, or leave the mount entry out entirely. This setting avoids a mount failure during boot.
{: important}

The following example shows a mount entry with the `nofail` option.

```sh
/dev/vdb1 /mnt/inststg1 ext4 defaults,nofail 0 0
```
{: pre}

### Looking up instance storage details of an instance profile
{: #instance-storage-lookup-details}

Look for the instance storage column in the web UI or CLI when you filter instance profiles. It may be called `Storage(GB)`.

The following examples show possible column values and what they mean:

* 1 x 150 GB – A single 150 GB block device is attached to the virtual server instance for instance storage.
* 2 x 600 GB – Two 600 GB (1200 total) instance storage block devices are attached.

In some contexts, you might see an Interface Type (usually `virtio_blk`) for an instance profile or for an attached instance disk. The following example shows details that were obtained from the CLI for a specific instance profile.

```sh
ibmcloud is in-pr bx2d-48x192
Getting instance profile bx2d-48x192 under account YYYYYY as user vpc-user@myco.com...

Name                     bx2d-48x192
Architecture             amd64
vCPU Manufacturer        intel
Family                   balanced
vCPUs                    48
Memory(GiB)              192
GPU                      Model   Manufacturer   Count   Memory(GiB)
                         -       -              -       -

Numa Count               -
Bandwidth(Mbps)          80000
Volume bandwidth(Mbps)   -
Instance Storage Disks   Quantity   Size   Supported interface types
                         2          900    virtio_blk

Min NIC Count            1
Max NIC Count            10
```
{: screen}

### Reporting instance storage information
{: #instance-storage-reporting}

You can report instance storage information for your virtual server instance by using the following CLI example. Substitute your own instance ID or name.

```sh
ibmcloud is instance-disks 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488
Listing disks of instance 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 under account YYYYYY as user vpc-user@myco.com...
ID                                          Name                               Size   Interface Type
0716-6ebed02f-c421-4b06-a73b-f334413e84b3   clunky-refocus-tidings-underpaid   600    virtio_blk
0716-54ee0659-d92a-4d4c-ac6b-8f03cb178a07   linguini-epiphany-lush-preschool   600    virtio_blk
```
{: screen}

### Updating one of the default names
{: #instance-storage-update-name}

You can update the custom names of the instance storage disks by using the following CLI command. Substitute your own instance ID and disk ID for the last two parameters, respectively.

```sh
ibmcloud is instance-disk-update 0716_f8c0fd3b-cada-46c3-91f6-c271937ef488 0716-6ebed02f-c421-4b06-a73b-f334413e84b3 --name my-instance-disk1
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
