---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-27"

keywords: Block storage for VPC, iscsi for VPC, SAN for VPC

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Set up your {{site.data.keyword.block_storage_is_short}} data volume for use (Linux)
{: #start-using-your-block-storage-data-volume-lin}

If you want to use your {{site.data.keyword.block_storage_is_full}} volume as a file system, you need to partition the volume, format it, and then mount it as a file system. You can perform this operation after you created a {{site.data.keyword.block_storage_is_short}} volume and attached it to an instance.
{: shortdesc}

Follow this procedure to use your block storage volume on a Linux&reg; system.

## Step 1 - List all storage volumes
{: #linux-procedure-list-volumes}

Run the following command to list all {{site.data.keyword.block_storage_is_short}} volumes from your instance.

```sh
lsblk
```
{: pre}

The output looks similar to this example.

```sh
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

1. Run the following command to partition the volume.

   ```sh
   fdisk /dev/vdb
   ```
   {: pre}

2. Type the `n` command for a new partition, then `p` for primary partition.

   ```sh
   Partition type:
      p   primary (0 primary, 0 extended, 4 free)
      e   extended
   Select (default p): p
   ```
   {: pre}

3. Complete the prompts to define the partition's first cylinder number and last cylinder number. You can use the default value for the first cylinder number. For the last cylinder, you can either define an absolute value for the last sector or you can define a relative value to the start sector. To define a relative value, use the + symbol followed by the partition size. The size can be specified in kibibytes (K), mebibytes (M), gibibytes (G), tebibytes (T), or pebibytes (P). For example, to set the partition size to 100 GiB, enter +100G.

4. After the partition is created, run the `w` command to save changes to the partition table. Restart your system to verify the newly created partition.

## Step 3 - Format the volume partition
{: #linux-procedure-format-volume}

```sh
/sbin/mkfs -t ext4 /dev/vdb1
```
{: pre}

To check the size of the partition, run the following command.

```sh
fdisk -s /dev/vdb1
```
{: pre}

## Step 4 - Update the file systems table
{: #linux-procedure-update-fstab}

Update `/etc/fstab`.

```sh
fstab /dev/vdb1
```
{: pre}

```sh
disk_partition=/dev/vdb1
 uuid=$(blkid -sUUID -ovalue $disk_partition)
 mount_point=$mount_parent/$uuid
 echo "UUID=$uuid $mount_point ext4 defaults,relatime 0 0" >> /etc/fstab
```
{: codeblock}

## Step 5 - Create a directory
{: #linux-procedure-mkdir}

```sh
mkdir /myvolumedir
mount /dev/vda /myvolumedir
```
{: pre}

## Step 6 - Mount the volume as a file system
{: #linux-procedure-mount-volume}

```sh
mount /dev/vdb1 /myvolumedir
```
{: pre}

## Step 7 - Access and use the new file system
{: #linux-procedure-use-file system}

To see your new file system, run the following command.

```sh
df -k
```
{: pre}

The command produces an output like the following example.

```sh
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

Go to the directory in your new file system and create a file.

```sh
cd /myvolumedir
touch myvolumefile1
```
{: pre}
