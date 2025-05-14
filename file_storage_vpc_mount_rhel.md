---

copyright:
  years: 2021, 2025
lastupdated: "2025-05-14"

keywords: file share, file storage, mount helper, mount target, mount path, secure connection, NFS

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting file shares on Red Hat Linux
{: #file-storage-mount-RHEL}

Use these instructions to connect a Red Hat Enterprise Linux&reg;-based {{site.data.keyword.cloud}} Compute Instance to a Network File System (NFS) file share.
{: shortdesc}

## Before you begin
{: #fs-rhel-create-vsi}

1. If the file share was set up with Security group access mode, verify that the compute host is part of the same [security group](/docs/vpc?topic=vpc-using-security-groups#sg-getting-started) as the share. If your file share was set up with VPC access mode, verify that the server where you want to mount the share is in the same zone as the file share. For more information, see [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-mount-access-mode).
2. Confirm that a mount target for the share exists for the VPC where the server is. If a new mount target is needed, follow the instructions in [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create). 
3. Get the mount path of the file share from the mount target. Mount path information can be obtained from the File share details page in the [console](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-get-mountpath-ui-vpc), from the [CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-get-mountpath-cli), with the [API](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-get-target-api), or [Terraform](/docs/vpc?topic=vpc-file-storage-view&interface=terraform#fs-view-mount-target-terraform).
4. If you want to use encryption in transit, you need to obtain an IPsec certificate from the Instance metadata service. Make sure that encryption in transit is enabled for the mount target. Plus, mount the file share with a secure connection. This feature is only available for file shares with `dp2` profiles and security group access mode. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).
   
   Install and run the [mount helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to mount file shares with encryption in transit or without an encrypted connection.
   {: fast-path}

{{site.data.keyword.filestorage_vpc_short}} service requires NFS versions v4.1 or higher.
{: requirement}

## Mounting the file share
{: #fs-RHEL-mount-share}

Follow these steps to mount a file share on an RHEL host. Examples are based on RHEL 8.

[SSH into the Compute instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui) where you want to mount the file share, then continue with these steps:

1. Install the required tools.

   ```sh
   yum install nfs-utils
   ```
   {: pre}


2. Create a directory in your instance.

   ```sh
   mkdir /mnt/test
   ```
   {: pre}

3. Mount the remote file share.

   ```sh
   mount -t nfs4 -o <options> <host:/mount_target> /mnt
   ```
   {: pre}

   See the following example.

   ```sh
   mount -t nfs4 -o sec=sys,nfsvers=4.1 10.240.64.11:/384f711c_0684_4643_b1c3_dc7acb36d04a /mnt/test
   ```

4. Verify that the mount was successful with the disk file system command.

   ```sh
   $ df -h
   Filesystem                                                                                    Size  Used Avail Use% Mounted on
   udev                                                                                          3.9G     0  3.9G   0% /dev
   tmpfs                                                                                         798M  660K  798M   1% /run
   /dev/vda2                                                                                      99G  1.6G   93G   2% /
   tmpfs                                                                                         3.9G     0  3.9G   0% /dev/shm
   tmpfs                                                                                         5.0M     0  5.0M   0% /run/lock
   tmpfs                                                                                         3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda1                                                                                     240M   73M  155M  33% /boot
   fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_246a9cb9-4679-4dc5-9522-4a7ed2575136  190G  384K  190G   1% /mnt/test
   tmpfs                                                                                         798M     0  798M   0% /run/user/0
   ```
   {: screen}

5. Go to the mount point to create a test file and list all files to verify that the share is mounted as read/write.

   ```sh
   touch /mnt/test/test.txt
   ls -al /mnt/test
   ```
   {: pre}

   ```sh
   $ touch /mnt/test/test.txt
   ls -al /mnt/test
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Apr 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Apr 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Apr 8 15:52 test.txt
   ```
   {: screen}

   The files are created by root and have an ownership of `nobody:nobody`. To display the ownership correctly, update `idmapd.conf` with the correct domain settings. For more information, see [How to implement no_root_squash for NFS](#fs-RHEL-norootsquash).

6. Mount the remote file share on start. To complete the setup, you must edit the file systems table (`/etc/fstab`) and add the remote file share to the list of entries that are automatically mounted on startup. Before you create an entry in the `fstab`, take the following steps to add the mount path hostname to `/etc/hosts`.

    1. Get the `hostname.com` portion of the mount path, for example `fsf-dal2433a-dz.adn.networklayer.com` and get the IP address. Run the following command from the instance to get the IP address.

       ```sh
       host hostname.com
       ```
       {: pre}

       See the following example.

       ```sh
       host fsf-dal2433a-dz.adn.networklayer.com
       fsf-dal2433a-dz.adn.networklayer.com has address 203.0.113.0
       ```

       If you get a command not found error when you run `host` command, use `yum install bind-utils` to install it.
       {: tip}

    2. Edit `/etc/hosts` and add an IP to the hostname entry.

       ```text
       <IP_Address> hostname.com
       ```
       {: pre}

       See the following example.

       ```sh
       198.51.100.0 fsf-dal2433a-dz.adn.networklayer.com
       ```
       {: screen}
      
    3. Edit the file systems table `/etc/fstab`, and add an entry.
       ```text
       (hostname):/(file_share_path) /mnt nfs_version options 0 0
       ```
       {: pre}

       See the following example.
       ```text
       fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_246a9cb9-4679-4dc5-9522-4a7ed2575136 /mnt/test nfs4 nfsvers=4.1,sec=sys,_netdev 0 0
       ```
       {: screen}

7. Verify that the configuration file has no errors.

   ```sh
   mount -fav
   ```
   {: pre}

   If the command completes without errors, your setup is complete.

   For NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues. Use `_netdev` to wait for the storage to be mounted after all network components are started.
   {: tip}

## Implementing `no_root_squash` for NFS (optional)
{: #fs-RHEL-norootsquash}

By default, NFS downgrades any files that were created with the root permissions to the `nobody` user. This security feature prevents privileges from being shared unless they are requested.

By configuring `no_root_squash`, root clients can retain root permissions on the remote NFS file share.

For NFSv4.1, set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd` or a similar service that is used by your OS.

1. From the host, set the domain setting in `/etc/idmapd.conf`.

   ```text
   $ vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```
   {: codeblock}

2. Run `nfsidmap -c`.

## Unmounting the file system
{: #fs-RHEL-umount}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```sh
umount /dev/sdb
```
{: pre}

```sh
umount /mnt
```
{: pre}
