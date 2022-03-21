---

copyright:
  years: 2022
lastupdated: "2022-03-21"

keywords: 

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
{:table: .aria-labeledby="caption"}

# Modifying a Linux OS for expanding boot volumes
{: #modifying-the-linux-os-expanded-boot-volume}

You can increase the disk capacity on linux systems after the boot volume has been expanded. This can be done with existing virtual server instances or while creating a new virtual server instance.

## Modifying an existing virtual server instance and increasing its boot volume
{: #modify-existing-vsi}

An Ubuntu system is used in this example to display increasing the disk capacity of an existing virtual server instance. The procedure and commands for other Linux distributions may vary.

**Ubuntu example**

1. Create an Ubuntu virtual server instance instance from the stock image with a capacity of 100 G (default value).

2. Assign FIP to the virtual server instance and SSH into the instance using FIP.

3. Validate the os-release.
   
   ```
   root@ubuntu-18:~# cat /etc/os-release
   NAME="Ubuntu"
   VERSION="18.04.5 LTS (Bionic Beaver)"
   ID=ubuntu
   ID_LIKE=debian
   PRETTY_NAME="Ubuntu 18.04.5 LTS"
   VERSION_ID="18.04"
   HOME_URL="https://www.ubuntu.com/"
   SUPPORT_URL="https://help.ubuntu.com/"
   BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
   PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
   VERSION_CODENAME=bionic
   UBUNTU_CODENAME=bionic
   ```
   
4. Patch the larger volume to the existing boot volume.
   ```
   curl -k -sS -X PATCH "$api_endpoint/v1/volumes/$1?generation=$generation&version=$api_version" \
       -H "Authorization: Bearer $iam_token" \
       -d '
   {
       "capacity": 200
   } ' | jq .
   ```
   
5. Validate if the latest capacity is 250 G.
   
   ```
   root@ubuntu-18-04:~# lsblk
   NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda     252:0    0  250G  0 disk
   ├─vda1  252:1    0 99.9G  0 part /
   ├─vda14 252:14   0    4M  0 part
   └─vda15 252:15   0  106M  0 part /boot/efi
   vdb     252:16   0  370K  0 disk
   vdc     252:32   0   44K  0 disk
   root@ubuntu-18-04:~# growpart /dev/vda 1
   ```
   
   If the capacity is not 250 G, execute commands to grow the partition size to cover maximum free space. <!--Growpart for Extended partition-->
   
   ```
   root@ubuntu-18-04:~# growpart /dev/vda 1
   CHANGED: partition=1 start=227328 old: size=209487839 end=209715167 new: size=524060639,end=524287967
   root@ubuntu-18-04:~# lsblk
   NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
   vda     252:0    0   250G  0 disk
   ├─vda1  252:1    0 249.9G  0 part /
   ├─vda14 252:14   0     4M  0 part
   └─vda15 252:15   0   106M  0 part /boot/efi
   vdb     252:16   0   370K  0 disk
   vdc     252:32   0    44K  0 disk
   ```
   
6. Test if the file system got resized (execute the command - "df -kh" ).  <!-- Test and validate the expansion of root partition. -->
   
   If it shows no changes, execute commands to resize the filesystem (xfs or ext4) along with modifications for physical volume, volume group, and logical volume.
   ```
   root@ubuntu-18-04:~# resize2fs /dev/vda1
   resize2fs 1.44.1 (24-Mar-2018)
   Filesystem at /dev/vda1 is mounted on /; on-line resizing required
   old_desc_blocks = 13, new_desc_blocks = 32
   The filesystem on /dev/vda1 is now 65507579 (4k) blocks long.
   ```
   
7. Test if the file system got resized. Now, the filesystem should have been resized to 250G as expected.
   ```
   root@ubuntu-18-04:~# df -kh
   Filesystem      Size  Used Avail Use% Mounted on
   udev            3.9G     0  3.9G   0% /dev
   tmpfs           798M  652K  798M   1% /run
   /dev/vda1       243G  1.1G  242G   1% /
   tmpfs           3.9G     0  3.9G   0% /dev/shm
   tmpfs           5.0M     0  5.0M   0% /run/lock
   tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
   /dev/vda15      105M  6.6M   98M   7% /boot/efi
   tmpfs           798M     0  798M   0% /run/user/0
   ```


<!-- ubutnu -->

## Creating a new virtual server instance from a custom image or stock image and increasing the boot volume
{: #modify-new-vsi}

A CentOS system is used in this example to display increasing the disk capacity of a virtual server instance being created. The procedure and commands for other Linux distributions may vary.

**Centos example**

1. Create a Centos virtual server instance and assign FIP to it. 

2. SSH into the virtual server instance using FIP.

3. Validate the os-release.

   ```
   [root@centos-8 ~]# cat /etc/os-release
   NAME="CentOS Linux"
   VERSION="8"
   ID="centos"
   ID_LIKE="rhel fedora"
   VERSION_ID="8"
   PLATFORM_ID="platform:el8"
   PRETTY_NAME="CentOS Linux 8"
   ANSI_COLOR="0;31"
   CPE_NAME="cpe:/o:centos:centos:8"
   HOME_URL="https://centos.org/"
   BUG_REPORT_URL="https://bugs.centos.org/"
   CENTOS_MANTISBT_PROJECT="CentOS-8"
   CENTOS_MANTISBT_PROJECT_VERSION="8"
   ```
4. Validate if the latest capacity is 150 G.
   
   ```
   [root@centos-8 ~]# lsblk
   NAME                MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda                 252:0    0  150G  0 disk
   |-vda1              252:1    0    1G  0 part /boot
   `-vda2              252:2    0   99G  0 part
     |-vg_virt-lv_root 253:0    0   98G  0 lvm  /
     `-vg_virt-lv_swap 253:1    0    1G  0 lvm 
   vdb                 252:16   0  366K  0 disk
   vdc                 252:32   0   44K  0 disk
   ```
   
5. Install Cloud Utils.
   ```
   [root@centos-8 ~]# yum install cloud-utils-growpart -y
   ```
   
6. Growpart for Extended partition.
   
   ```
   [root@centos-8 ~]# growpart /dev/vda 2
   CHANGED: partition=2 start=2099200 old: size=207616000 end=209715200 new: size=312473567 end=314572767
   [root@centos-8 ~]# lsblk
   NAME                MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   vda                 252:0    0  150G  0 disk
   |-vda1              252:1    0    1G  0 part /boot
   `-vda2              252:2    0  149G  0 part
     |-vg_virt-lv_root 253:0    0   98G  0 lvm  /
     `-vg_virt-lv_swap 253:1    0    1G  0 lvm 
   vdb                 252:16   0  366K  0 disk
   vdc                 252:32   0   44K  0 disk
   ```
   
7. Test if the file system got resized. It shows no changes.

   ```
   [root@centos-8 ~]# df -kh
   Filesystem                   Size  Used Avail Use% Mounted on
   devtmpfs                     3.8G     0  3.8G   0% /dev
   tmpfs                        3.8G     0  3.8G   0% /dev/shm
   tmpfs                        3.8G   17M  3.8G   1% /run
   tmpfs                        3.8G     0  3.8G   0% /sys/fs/cgroup
   /dev/mapper/vg_virt-lv_root   98G  1.6G   97G   2% /
   /dev/vda1                   1014M  195M  820M  20% /boot
   tmpfs                        777M     0  777M   0% /run/user/0
   ```
8. Execute commands to resize the filesystem - ext4.

   Ubuntu automatically resizes at reboot.
   {: note}
   
   ```
   [root@centos-8 ~]# pvresize /dev/vda2
   Physical volume "/dev/vda2" changed
   1 physical volume(s) resized or updated / 0 physical volume(s) not resized
 
   [root@centos-8 ~]# vgs
   VG      #PV #LV #SN Attr   VSize    VFree
   vg_virt   1   2   0 wz--n- <149.00g 50.00g
    
   [root@centos-8 ~]# lvextend -r -l +100%FREE /dev/mapper/vg_virt-lv_root
   Size of logical volume vg_virt/lv_root changed from <98.00 GiB (25087 extents) to <148.00 GiB (37887 extents).
   Logical volume vg_virt/lv_root successfully resized.
   meta-data=/dev/mapper/vg_virt-lv_root isize=512    agcount=4, agsize=6422272 blks
            =                       sectsz=512   attr=2, projid32bit=1
            =                       crc=1        finobt=1, sparse=1, rmapbt=0
            =                       reflink=1
   data     =                       bsize=4096   blocks=25689088, imaxpct=25
            =                       sunit=0      swidth=0 blks
   naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
   log      =internal log           bsize=4096   blocks=12543, version=2
            =                       sectsz=512   sunit=0 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 25689088 to 38796288
   ```
   
9. Test if the file system got resized. Now, the filesystem (ext4) should have been resized to 150G as expected.
   
   ```
   [root@centos-8 ~]# df -kh
   Filesystem                   Size  Used Avail Use% Mounted on
   devtmpfs                     3.8G     0  3.8G   0% /dev
   tmpfs                        3.8G     0  3.8G   0% /dev/shm
   tmpfs                        3.8G   17M  3.8G   1% /run
   tmpfs                        3.8G     0  3.8G   0% /sys/fs/cgroup
   /dev/mapper/vg_virt-lv_root  148G  2.0G  147G   2% /
   /dev/vda1                   1014M  195M  820M  20% /boot
   tmpfs                        777M     0  777M   0% /run/user/0
   ```
   

1. Create a Debian/Redhat/Centos virtual server instance Instance from the stock image with a capacity of 150 G (Sample value considered for this set up is 150G. Ideally any value greater than the default value of 100 G would work and lesser than the cut-off)

2. Execute commands to grow the partition size to cover maximum free space. 

3. Execute commands to resize the filesystem (xfs or ext4) along with modifications for physical volume, volume group, and logical volume. 

4. Test and validate the expansion of root partition.
