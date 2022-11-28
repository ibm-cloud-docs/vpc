---

copyright:
  years: 2021, 2022
lastupdated: "2022-11-08"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting file shares in CentOS
{: #file-storage-mount-centos}

Use these instructions to connect a CentOS Linux&reg;-based {{site.data.keyword.cloud}} Compute Instance to a Network File System (NFS) file share.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Before you begin - Create a VSI
{: #fs-centos-create-vsi}

Before you begin to mount {{site.data.keyword.filestorage_vpc_short}} file shares, you must create a [virtual server instance](/docs/vpc?topic=vpc-about-advanced-virtual-servers) in the same zone as the file share. After you created an instance, get the mount path of the file share from the mount target that was created. You need a mount path for mounting file shares.

Mount path information can be obtained from the File share details page in the UI, or through an API or CLI call.
{: tip}

## Mount the file share on CentOS
{: #fs-mount-CentOS}

Mount a file share on a CentOS host by following these steps. The examples are based on CentOS 8. The steps are similar to the ones that are described on [Mounting file shares on Red Hat Enterprise Linux&reg;](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL).

VPC File Storage service requires NFS versions v4.1 or higher.
{: important}

SSH into the virtual server instance where you want to mount the file share, then continue with these steps:

1. Install the required tools.

   ```zsh
   yum install nfs-utils
   ```
   {: pre}

2. Create a directory in your instance.

   ```zsh
   mkdir /mnt/test
   ```
   {: pre}

3. Mount the remote file share.

   ```zsh
   mount -t nfs4 -o <options> <host:/mount_target> /mnt
   ```
   {: pre}

   For example:

   ```zsh
   mount -t nfs4 -o sec=sys,nfsvers=4.1 fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_246a9cb9-4679-4dc5-9522-4a7ed2575136 /mnt/test
   ```
   {: pre}

4. Verify that the mount was successful with the disk file system command.

   ```text
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

   ```zsh
   touch /mnt/test/test.txt
   ls -al /mnt/test
   ```
   {: pre}

   ```zsh
   $ touch /mnt/test/test.txt
   ls -al /mnt/test
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Apr 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Apr 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Apr 8 15:52 test.txt
   ```
   {: screen}

   The files are created by root and have an ownership of `nobody:nobody`. To display the ownership correctly, update `idmapd.conf` with the correct domain settings. For more information, see [How to implement no_root_squash for NFS](#fs-CentOS-norootsquash).

6. Mount the remote file share on start. To complete the setup, you must edit the file systems table (`/etc/fstab`) and add the remote file share to the list of entries that are automatically mounted on startup. Before you create an entry in the `fstab`, perform the following steps to add the mount path hostname to `/etc/hosts`.
    1. Get the `hostname.com` portion of mount path, `for example: fsf-dal2433a-dz.adn.networklayer.com` and get the IP address. Run the following command from the instance to get the IP address.

        ```sh
        host hostname.com
        ```
        {: pre}

        For example:
        ```text
        host fsf-dal2433a-dz.adn.networklayer.com
        fsf-dal2433a-dz.adn.networklayer.com has address 203.0.113.0
        ```
        {: screen}

        If you get command not found error when you run `host` command, use `yum install bind-utils` to install it.
        {: tip}

    2. Edit `/etc/hosts` and add an IP to the hostname entry.

       ```sh
       <IP_Address> hostname.comhostname.com
       ```
       {: pre}

       For example:
       ```text
       198.51.100.0 fsf-dal2433a-dz.adn.networklayer.com
       ```
       {: screem}

    3. Edit the file systems table (`/etc/fstab`) and add an entry.

       ```sh
       (hostname):/(file_share_path) /mnt nfs_version options 0 0
       ```
       {: pre}

       For example:
       ```sh
       fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_246a9cb9-4679-4dc5-9522-4a7ed2575136 /mnt/test nfs4 nfsvers=4.1,sec=sys,_netdev 0 0
       ```
       {: screen}

7. Verify that the configuration file has no errors.

   ```zsh
   mount -fav
   ```
   {: pre}

   If the command completes without errors, your setup is complete.

   For NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues. Use `_netdev` to wait for the storage to get mounted until after all network components have started.
   {: tip}


## Implement `no_root_squash` for NFS (optional)
{: #fs-CentOS-norootsquash}

By default, NFS downgrades any files that were created with the root permissions to the `nobody` user. This security feature prevents privileges from being shared unless they are requested.

By configuring `no_root_squash`, root clients can retain root permissions on the remote NFS file share.

For NFSv4.1, set the nfsv4 domain to: `slnfsv4.com`, and start `rpcidmapd` or a similar service that is used by your OS.

For example:

1. From the host, set domain setting in `/etc/idmapd.conf`.

   ```sh
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

## Unmount the file system
{: #fs-CentOS-umount}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```zsh
umount /dev/sdb
```
{: pre}

```zsh
umount /mnt
```
{: pre}
