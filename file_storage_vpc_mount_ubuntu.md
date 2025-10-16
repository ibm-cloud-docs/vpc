---

copyright:
  years: 2021, 2025
lastupdated: "2025-10-16"

keywords: file share, file storage, mount helper, mount target, mount path, secure connection, NFS, mounting share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting file shares on Ubuntu
{: #file-storage-mount-ubuntu}

Use these instructions to connect a Network File System (NFS) file share to an Ubuntu Linux&reg;-based {{site.data.keyword.cloud}} Compute instance.
{: shortdesc}

## Before you begin
{: #fs-ubuntu-create-vsi}

1. If the file share was set up with Security group access mode, verify that the compute host is part of the same [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) as the share. If your file share was set up with VPC access mode, verify that the server where you want to mount the share is in the same zone as the file share. For more information, see [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-mount-access-mode).
2. Confirm that a mount target for the share exists for the VPC where the server is. If a new mount target is needed, follow the instructions in [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create). 
3. Get the mount path of the file share from the mount target. Mount path information can be obtained from the File share details page in the [console](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-get-mountpath-ui-vpc), from the [CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-get-mountpath-cli), with the [API](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-get-target-api), or [Terraform](/docs/vpc?topic=vpc-file-storage-view&interface=terraform#fs-view-mount-target-terraform).
4. If you want to use encryption in transit, you need to obtain an IPsec certificate from the metadata service. Make sure that encryption in transit is enabled for the mount target. Plus, mount the file share with a secure connection. This feature is only available for file shares with `dp2` profiles and security group access mode. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).
   
   Install and run the [mount helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to mount file shares with encryption in transit or without an encrypted connection.
   {: fast-path}

{{site.data.keyword.filestorage_vpc_short}} service requires NFS versions v4.1 or higher.
{: requirement}

For more information about how NFS works on Ubuntu, see the [Ubuntu Server Documentation](https://documentation.ubuntu.com/server/how-to/networking/install-nfs/){: external}.

## Mounting the file share
{: #fs-Ubuntu-mount}

Follow these steps to mount a file share on an Ubuntu host. 

1. [Log in to the Compute host through an SSH connection](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui).

1. Create a `/mnt/nfs` directory.

    ```sh
    mkdir -p /mnt/nfs
    ```
    {: pre}

1. Install `nfs-common`:

    ```sh
    apt install nfs-common
    ```
    {: pre}

1. Restart your instance:

    ```sh
    reboot
    ```
    {: pre}

1. Mount the remote file share:

   ```sh
   mount -t nfs4 -o <options> <host:/mount_target> /mnt/nfs
   ```
   {: pre}

   See the following example.

   ```sh
   mount -t nfs4 -o sec=sys 10.240.64.11:/384f711c_0684_4643_b1c3_dc7acb36d04a /mnt/nfs
   ```
   {: pre}

1. Verify that the mount was successful by using the disk file system command `df -h`:

    ```sh
    $ df -h
    Filesystem                                                                                    Size  Used Avail Use% Mounted on
    /dev/root                                                                                      97G  1.6G   96G   2% /
    devtmpfs                                                                                      3.9G     0  3.9G   0% /dev
    tmpfs                                                                                         3.9G     0  3.9G   0% /dev/shm
    tmpfs                                                                                         798M  508K  797M   1% /run
    tmpfs                                                                                         5.0M     0  5.0M   0% /run/lock
    tmpfs                                                                                         3.9G     0  3.9G   0% /sys/fs/cgroup
    /dev/vda15                                                                                    105M  9.2M   96M   9% /boot/efi
    /dev/loop0                                                                                     56M   56M     0 100% /snap/core18/1885
    /dev/loop1                                                                                     71M    71M     0 100% /snap/lxd/16922
    /dev/loop2                                                                                     31M   31M     0 100% /snap/snapd/9279
    tmpfs                                                                                         798M     0  798M   0% /run/user/0
    fsf-dal1099a-fz.adn.networklayer.com:/voll_58fd55a_685c_4ccd_b42e_25d5b61129e2   95G  256K   95G   1% /mnt/nfs
    ```
    {: screen}

1. Go to the mount point to create a test file and list all files to verify that the share is mounted as read/write.

   ```sh
   touch /mnt/nfs/test.txt
   ```
   {: pre}

   ```sh
   ls -al /mnt/nfs
   ```
   {: pre}

   ```sh
   touch /mnt/nfs/test.txt
   ls -al /mnt/nfs
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Apr 28 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Apr 28 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Apr 28 15:52 test.txt
   ```

1. Make the configuration persistent by editing the file systems table (`/etc/fstab`). Add the remote share to the list of entries that are automatically mounted on startup:

   ```sh
   sudo nano /etc/fstab
   ```
   {: pre}

   Add a line with the following syntax to the end of the file.

   ```sh
   (hostname):/(mount_point) /mnt/nfs nfs_version defaults 0 0
   ```

   Example

   ```sh
   fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_mz0726_c391f0ba-50ed-4460-8704-a36032c96a4c /mnt/nfs nfsvers=4.1 defaults 0 0
   ```

1. Verify that the configuration file has no errors.

   ```sh
   mount -fav
   ```
   {: pre}

   If the command completes with no errors, your setup is complete.

   For NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues. Use `_netdev` to wait for the storage to be mounted until after all network components are started.
   {: tip}

## Unmounting the file system
{: #fs-Ubuntu-umount}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```sh
umount /dev/sdb
```
{: pre}

```sh
umount /mnt/nfs
```
{: pre}
