---

copyright:
  years: 2019, 2025
lastupdated: "2025-07-03"

keywords: Block Storage for VPC, iscsi for VPC, SAN for VPC

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Setting up your {{site.data.keyword.block_storage_is_short}} data volume for use (Linux)
{: #start-using-your-block-storage-data-volume-lin}

You can create a {{site.data.keyword.block_storage_is_short}} volume and attached it to an instance in the IBM Cloud console, with the CLI, API or Terraform. If you want to use your {{site.data.keyword.block_storage_is_full}} volume as a file system, your next steps are to partition the volume, format it, and then mount it as a file system. 
{: shortdesc}

First, [connect to your instance](/docs/vpc?topic=vpc-vsi_is_connecting_linux). Then, follow this procedure from the shell.

## Listing all storage volumes
{: #linux-procedure-list-volumes}

Run the following command to list all {{site.data.keyword.block_storage_is_short}} volumes that are attached to your instance.

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

Volume `vdb` is your data volume.

## Partitioning the volume
{: #linux-procedure-partition-volume}

1. Run the following command to partition the data volume.

   ```sh
   fdisk /dev/vdb
   ```
   {: pre}

2. Type the `n` command for a new partition, then type `p` for primary partition.

   ```sh
   Partition type:
      p   primary (0 primary, 0 extended, 4 free)
      e   extended
   Select (default p): p
   ```
   {: pre}

3. Complete the prompts to define the partition's first cylinder number and last cylinder number. You can use the default value for the first cylinder number. For the last cylinder, you can either define an absolute value for the last sector or you can define a relative value to the start sector. To define a relative value, use the + symbol followed by the partition size. The size can be specified in kibibytes (K), mebibytes (M), gibibytes (G), tebibytes (T), or pebibytes (P). For example, to set the partition size to 100 GiB, enter +100G.

4. After the partition is created, run the `w` command to save changes to the partition table. Restart your system to verify the newly created partition.

## Formatting the volume partition
{: #linux-procedure-format-volume}

Create a file system on the new partition.

```sh
mkfs.ext4 /dev/vdb1
```
{: pre}

To check the size of the partition, run the following command.

```sh
fdisk -s /dev/vdb1
```
{: pre}

## Creating a mount point
{: #linux-procedure-mkdir}

```sh
mkdir /myvolumedir
```
{: pre}

## Mounting the volume
{: #linux-procedure-mount-volume}

```sh
mount /dev/vdb1 /myvolumedir
```
{: pre}

## Accessing the new file system
{: #linux-procedure-use-file-system}

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
/dev/vdb1      103081248   61176  97777192   1% /myvolumedir
```
{: screen}

Go to the directory and create a file.

```sh
cd /myvolumedir
touch myvolumefile1
```
{: pre}

## Updating the file systems table
{: #linux-procedure-update-fstab}

Update the configuration file `/etc/fstab` so the data volume is automatically mounted upon boot. Root privileges are needed to update this file with a text editor, like nano, vim, or emacs. It is recommended that you back up the file before you make any changes.

```sh
sudo cp /etc/fstab /etc/fstab.orig
```
{: pre}

The following command starts nano to edit the configuration file.

```sh
sudo nano /etc/fstab
```
{: pre}

Add a line for the newly attached data volume that looks similar to the following example.
```text
/dev/vdb1    /myvolumedir    ext4    defaults,_netdev    0    1
```

Instead of the device name `/dev/vdb1`, you can also use the UUID. To get the UUID of the data volume, use the `blkid` command.

```sh
blkid /dev/vdb1
```
{: pre}

When you're done editing, you can use `sudo mount -a` to remount the file systems that are listed in the updated `/etc/fstab` file without restarting your virtual server instance.
