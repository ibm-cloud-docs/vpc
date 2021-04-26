---

copyright:
  years: 2021
lastupdated: "2021-04-06"

keywords: file Storage, NFS, mounting file Storage, mounting file shares on Ubuntu,

subcollection: FileStorage

---

{:term: .term}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:beta: .beta}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Mounting file shares on Ubuntu (beta)
{: #file-storage-vpc-mount-ubuntu}

Use these instructions to connect an Ubuntu Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a Network file system (NFS) share.
{:shortdesc}

This service is available only to accounts with a special approval to preview this beta feature. 
{:beta}

## Before you begin - Create a virtual server instance
{: #fs-ubuntu-create-vsi}

Before you begin to mount File Storage for VPC file shares, you must create a [virtual server instance](/docs/vpc?topic=vpc-about-advanced-virtual-servers) in the same zone as the file share. After creating an instance, get the mount path of the file share from the mount target created. You need a mount path for mounting file shares. 

Mount path information can be obtained from the File share details page in the UI, or through an API or CLI call to get the mount target information.
{:tip}

## Mounting the file share
{: #fs-Ubuntu-mount}

Follow these steps to mount a file share on an Ubuntu host. Examples are based on Ubuntu 18.04. 

VPC File Storage service requires NFS versions v4.1 or higher.
{:note}

SSH into the virtual server instance where you want to mount the file share, then continue with these steps:

1. Install the required tools.

   ```
   apt-get install nfs-common
   ```
   {:pre}

2. Create a directory in your instance.

   ```
   mkdir /mnt/test
   ```
   {:pre}


3. Mount the remote file share.

   ```
   mount -t nfs4 -o <options> <host:/mount_target> /mnt
   ```
   {:pre}

   For example:

   ```
   mount -t nfs4 -o sec=sys,nfsvers=4.1 fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_mz0726_c391f0ba-50ed-4460-8704-a36032c96a4c /mnt/test
   ```
   {:pre}

4. Verify that the mount was successful by using the disk file system command.

   ```
   $ df -h
   Filesystem                                                                                    Size  Used Avail Use% Mounted on
   udev                                                                                          3.9G     0  3.9G   0% /dev
   tmpfs                                                                                         798M  660K  798M   1% /run
   /dev/vda2                                                                                      99G  1.6G   93G   2% /
   tmpfs                                                                                         3.9G     0  3.9G   0% /dev/shm
   tmpfs                                                                                         5.0M     0  5.0M   0% /run/lock
   tmpfs                                                                                         3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda1                                                                                     240M   73M  155M  33% /boot
   fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_mz0726_c391f0ba-50ed-4460-8704-a36032c96a4c  190G  384K  190G   1% /mnt/test
   tmpfs                                                                                         798M     0  798M   0% /run/user/0
   ```
   {:codeblock}


5. Go to the mount point and read/write files.

   ```
   touch /mnt/test/test.txt
   ls -al /mnt/test
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Apr 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Apr 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Apr 8 15:52 test.txt
   ```
   {:pre}

6. Mount the remote share on start. To complete the setup, you must edit the file systems table (`/etc/fstab`) and add the remote share to the list of entries that are automatically mounted on startup. Before creating an entry in the `fstab`, perform the following steps to add the mount path hostname to `/etc/hosts`. 

   a. Get the `hostname.com` portion of mount path, `for example: fsf-dal2433a-dz.adn.networklayer.com` and get the IP address. Run the following command from inside the instance to get the IP address.

      ```
      host hostname.com
      ```
      {:pre}

      For example:
      ```
      # host fsf-dal2433a-dz.adn.networklayer.com
      fsf-dal2433a-dz.adn.networklayer.com has address 203.0.113.0
      ```
      {:pre}

      If you get command not found error when you run `host` command, use `yum install bind-utils` to install it.
      {:tip}

   b. Edit `/etc/hosts` and add an IP to the hostname entry.

      ```
      <IP_ADDRESS> hostname.com
      ```
      {:pre}

      Examples:

      ```
      198.51.100.0 fsf-dal2433a-dz.adn.networklayer.com
      ```
      {:pre}

   c. Edit the file systems table (`/etc/fstab`) and add an entry

      ```
      (hostname):/(file_share_path) /mnt nfs_version options 0 0
      ```
      {:pre}

      For example:

      ```
      fsf-dal2433a-dz.adn.networklayer.com:/nxg_s_voll_246a9cb9-4679-4dc5-9522-4a7ed2575136 /mnt/test nfs4 nfsvers=4.1,sec=sys,_netdev 0 0
      ```
      {:pre}
  
7. Verify that the configuration file has no errors.

   ```
   mount -fav
   ```
   {:pre}

   If the command completes with no errors, your setup is complete.

   For NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues. Use `_netdev` to wait for the storage mounted until after all network components have started.
   {:tip}

## Unmounting the file system
{: #fs-Ubuntu-umount}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt/test
```
{:pre}
