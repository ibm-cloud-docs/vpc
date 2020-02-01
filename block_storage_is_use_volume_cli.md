---

copyright:
  years: 2019, 2020
lastupdated: "2019-09-30"

keywords: vpc, cli, command line interface, block storage, IBM Cloud, virtual private cloud, block storage, volume
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

After you create a {{site.data.keyword.block_storage_is_short}} volume and attaching it to an instance, to use your
block storage volume as a filesystem, you'll need to partition the volume, format the volume, and then mount it as a filesystem.
{:shortdesc}

Follow this procedure to use your block storage volume on a Linux system.

**Note**: Depending on your Linux distribution, devices show up with different paths. For example, Ubuntu block devices show up as `xvda`, `xvdb`, and so on, as in the following examples.

## Step 1 - List all block storage volumes
{: #linux-procedure-list-volumes}

Run the following command to list all block storage volumes from your instance.

```
lsblk
```
{:pre}

You should see output like this:

```
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
├─xvda1 202:1    0  256M  0 part /boot
└─xvda2 202:2    0 99.8G  0 part /
xvdb    202:32   0  100G  0 disk
```
{:screen}

Volume `xvdb` is your new block storage data volume.

## Step 2 - Partition the volume
{: #linux-procedure-partition-volume}

Run the following command to partition the volume.

```
fdisk /dev/xvdb
```
{:pre}

Type the `n` command for a new partition, then `p` for primary partition.

```
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
```
{:pre}

Complete the prompts to define the partition's first cylinder number and last cylinder number.  After creating a new partition, run the `w` command to save changes to the partition table. Reboot your system to verify newly created partition.

## Step 3 - Format the volume partition
{: #linux-procedure-format-volume}

```
/sbin/mkfs -t ext3 /dev/xvdb
```
{:pre}

To check the size of the partition, run:

```
fdisk -s /dev/xvdb
```
{:pre}

## Step 4 - Create the directory and mount the volume as a filesystem
{: #linux-procedure-mount-volume}

```
mkdir /myvolumedir
mount /dev/xvdb /myvolumedir
```
{: codeblock}

## Step 5 - Access and use the new filesystem
{: #linux-procedure-use-filesystem}

To see your new filesystem, run the following command:

```
df -k
```
{:pre}

You should see output like this:

```
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4075344       0   4075344   0% /dev
tmpfs             816936    8844    808092   2% /run
/dev/xvda2     101330012 1261048 100052580   2% /
tmpfs            4084664       0   4084664   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4084664       0   4084664   0% /sys/fs/cgroup
/dev/xvda1        245679   64360    168212  28% /boot
tmpfs             817040       0    817040   0% /run/user/0
/dev/xvdb      103081248   61176  97777192   1% /myvolumedir
```
{:screen}

Go to the directory in your new filesystem and create a file:

```
cd /myvolumedir
touch myvolumefile1
```
{:codeblock}
