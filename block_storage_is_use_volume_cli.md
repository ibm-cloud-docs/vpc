---

copyright:
  years: 2019, 2022
lastupdated: "2022-04-12"

keywords:
subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:download: .download}

# Using your block storage data volume (CLI)
{: #start-using-your-block-storage-data-volume}

If you want to use your {{site.data.keyword.block_storage_is_short}} volume as a file system, you need to partition the volume, format the volume, and then mount it as a file system. You can perform this operation after you created a block storage volume and attached it to an instance.
{: shortdesc}

Follow this procedure to use your block storage volume on a Linux&reg; system.

Depending on your Linux&reg; distribution, devices show up with different paths. For example, Ubuntu block devices show up as `vda`, `vdb`, and so on, as in the following examples.
{: note}

## Step 1 - List all block storage volumes
{: #linux-procedure-list-volumes}

Run the following command to list all block storage volumes from your instance.

```bash
lsblk
```
{: pre}

You will see output like this:

```bash
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    202:0    0  100G  0 disk
├─vda1 202:1    0  256M  0 part /boot
└─vda2 202:2    0 99.8G  0 part /
vdb    202:32   0  100G  0 disk
```
{: screen}

Volume `vdb` is your block storage data volume.

## Step 2 - Partition the volume
{: #linux-procedure-partition-volume}

Run the following command to partition the volume.

```bash
fdisk /dev/vdb
```
{: pre}

Type the `n` command for a new partition, then `p` for primary partition.

```bash
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
```
{: pre}

Complete the prompts to define the partition's first cylinder number and last cylinder number. After creating a new partition, run the `w` command to save changes to the partition table. Reboot your system to verify the newly created partition.

## Step 3 - Format the volume partition
{: #linux-procedure-format-volume}

```bash
/sbin/mkfs -t ext4 /dev/vdb1
```
{: pre}

To check the size of the partition, run:

```bash
fdisk -s /dev/vdb1
```
{: pre}

## Step 4 - Update the file systems table
{: #linux-procedure-update-fstab}

Update `/etc/fstab`.

```bash
fstab /dev/vdb1
```
{: pre}

```
disk_partition=/dev/vdb1
 uuid=$(blkid -sUUID -ovalue $disk_partition)
 mount_point=$mount_parent/$uuid
 echo "UUID=$uuid $mount_point ext4 defaults,relatime 0 0" >> /etc/fstab
```
{: codeblock}

## Step 5 - Create a directory
{: #linux-procedure-mkdir}

```bash
mkdir /myvolumedir
mount /dev/vda /myvolumedir
```
{: pre}

## Step 6 - Mount the volume as a file system
{: #linux-procedure-mount-volume}

```bash
mount /dev/vdb1 /myvolumedir
```
{: pre}

## Step 7 - Access and use the new file system
{: #linux-procedure-use-file system}

To see your new file system, run the following command:

```bash
df -k
```
{: pre}

You will see output like this:

```bash
file system     1K-blocks    Used Available Use% Mounted on
udev             4075344       0   4075344   0% /dev
tmpfs             816936    8844    808092   2% /run
/dev/vda2     101330012 1261048 100052580   2% /
tmpfs            4084664       0   4084664   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4084664       0   4084664   0% /sys/fs/cgroup
/dev/vda1        245679   64360    168212  28% /boot
tmpfs             817040       0    817040   0% /run/user/0
/dev/vdb      103081248   61176  97777192   1% /myvolumedir
```
{: screen}

Go to the directory in your new file system and create a file:

```bash
cd /myvolumedir
touch myvolumefile1
```
{: pre}
