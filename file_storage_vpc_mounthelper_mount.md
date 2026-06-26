---

copyright:
  years: 2023, 2026
lastupdated: "2026-06-26"

keywords: file share, Mount Helper, mount, mounting, IPsec, stunnel, NFS

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mounting file shares with the Mount Helper utility
{: #fs-mount-helper-mount}

Use Mount Helper to mount zonal and regional file shares with encryption in transit. The utility automates IPsec configuration for zonal shares and stunnel setup for regional shares.
{: shortdesc}

## Before you begin
{: #fs-mount-helper-mount-prereqs}

Before you mount file shares with Mount Helper:
- Review the [Mount Helper requirements and limitations](/docs/vpc?topic=vpc-fs-mount-helper-utility#fs-mount-helper-requirements)
- [Install the Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-install) on your compute host


## Connecting to your compute host
{: #fs-mount-helper-mount-connect}

To mount file shares with Mount Helper, you must login to your compute host.
- [Connect to your virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui).
- [Connect to your bare metal server](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
- If you want to access the file shares from IBM {{site.data.keyword.powerSys_notm}} instances, you must use a network path through a load balancer. For more information, see the following tutorial: [Accessing File Storage for VPC shares from IBM Power Virtual Server instances](/docs/sap?topic=sap-ha-nlb-rt-rfs-intro).

## Creating a mount directory
{: #fs-mount-helper-create-dir}

Create a directory on your server where you want to mount the file share.

```sh
mkdir /mnt/share-test
```
{: pre}

## Mounting a zonal file share
{: #fs-eit-mount-share-ipsec}

Use the following command syntax to mount a zonal file share. Replace the mount path with the information that is specific to your file share.

```sh
mount -t ibmshare -o secure=true 10.0.0.1:/MOUNT_PATH /mnt/MOUNT_POINT
```
{: pre}

When the command is sent, the utility creates the certificate signing request(csr) and calls the Metadata service to get the intermediate cert and end peer certificate. It parses the mount command-line arguments and creates `/etc/swanctl/conf.d/type_ibmshare_.conf`. The strongSwan service uses this configuration file to establish the IPsec connection. Then, the script loads the IPsec connection and calls the NFS `mount` command. A successful response looks like the following example.

```sh
   [root@my-eit-instance ~]# mount -t ibmshare -o secure=true 10.240.64.5:/0c937ac3_814e_4a7c_99b8_719ec3cad7fd  /mnt/share-test
   Info  - IpSec using StrongSwan(5.7.2)
   Debug - RunCmd: /usr/sbin/swanctl --list-conns
   Debug - Config data unchanged:/etc/strongswan/swanctl/conf.d/type_ibmshare_10.240.64.5.conf
   Debug - StrongSwan cleanup config files Total(1) Mounted(0) Deleted(0) Recent(1)
   Debug - RunCmd: ReloadConfig (/usr/sbin/swanctl --load-all)
   Debug - File unlocked:/var/lock/ibm_mount_helper.lck
   Debug - RunCmd: MountCmd (mount -t nfs4 -o sec=sys,nfsvers=4.2,rw 10.240.64.5:/0c937ac3_814e_4a7c_99b8_719ec3cad7fd /mnt/share-test)
   Debug - RunCmd: LoadCert (openssl x509 -in /etc/strongswan/swanctl/x509ca/type_ibmshare_int.crt -noout -dates -subject -issuer)
   Debug - RunCmd: LoadCert (openssl x509 -in /etc/strongswan/swanctl/x509ca/type_ibmshare_root_dal.crt -noout -dates -subject -issuer)
   Share successfully mounted:
```
{: screen}

You're advised not to add the mount details to the `/etc/fstab` because it can cause the compute host to hang during boot.
{: important}

## Mounting a regional file share
{: #fs-eit-mount-share-stunnel}

Use the following command syntax to mount a regional file share. Replace the mount path with the information that is specific to your file share.

```sh
mount -t ibmshare -o stunnel 10.0.0.1:/MOUNT_PATH /mnt/MOUNT_POINT
```
{: pre}

When the command is sent, the utility initiates the stunnel connection and calls the NFS `mount` command. A successful response looks like the following example.

Adding the `-v` option to the mount command generates the debug output, which can help pinpoint any issues during mounting.
{: tip}

```sh
[root@my-eit-instance ~]#  mount -v -o stunnel -t ibmshare 10.240.64.24:/EAD9B8582BC84FDAB57B7A315BCA1210 /mnt/EAD9B8582BC84FDAB57B7A315BCA1210
Debug - Locked ok:/var/lock/ibm_mount_helper.lck
Debug - File unlocked:/var/lock/ibm_mount_helper.lck
Debug - Locked ok:/var/lock/ibm_mount_helper.lck
Debug - RunCmd: ListNfsMounts (mount -t nfs,nfs4)
Debug - Existing nfs/nfs4 mounts found:0
Debug - RunCmd: ListNfsMounts (mount -t nfs,nfs4)
Debug - Existing nfs/nfs4 mounts found:0
Debug - Local port 10001 will be used for setting up next stunnel
Debug - Starting stunnel for mounting /EAD9B8582BC84FDAB57B7A315BCA1210
Debug - Stunnel conf file created /etc/stunnel/ibmshare_EAD9B8582BC84FDAB57B7A315BCA1210.conf
Debug - Attempting to start stunnel using /etc/stunnel/ibmshare_EAD9B8582BC84FDAB57B7A315BCA1210.conf
Debug - Attempting mount of /EAD9B8582BC84FDAB57B7A315BCA1210 on the local host
Debug - RunCmd: Mount using stunnel  (mount -t nfs4 -o sec=sys,nfsvers=4.2,rw,port=10001 127.0.0.1:/EAD9B8582BC84FDAB57B7A315BCA1210 /mnt/EAD9B8582BC84FDAB57B7A315BCA1210 -v)
Debug - Stunnel mount was successful
Debug - File unlocked:/var/lock/ibm_mount_helper.lck
```
{: pre}

You're advised not to add the mount details to the `/etc/fstab` because it can cause the compute host to hang during boot.
{: important}

If you need to add the mount details to `/etc/fstab`, make sure that you use the `_netdev` and `nofail` options. The following examples show the correct format:

```sh
55.38.12.137:/9A81XXXXXX367497  /mnt/rfs   ibmshare  stunnel,rw,sec=sys,_netdev  0  0
55.38.12.141:/380BXXXXXXDCA870  /mnt/rfs2  ibmshare  stunnel,rw,sec=sys,_netdev  0  0
```
{: pre}

After you add the entries to `/etc/fstab`, run the following commands:

```sh
mount -a
systemctl daemon-reload
```
{: pre}

The file shares are mounted and automatically mount on system restart.

## Next steps
{: #fs-mount-helper-mount-next-steps}

By default, NFS downgrades any files that were created with the root permissions to the `nobody` user. This security feature prevents privileges from being shared unless they are requested. By configuring `no_root_squash`, root clients can retain root permissions on the remote NFS file share. For NFSv4.2, set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd` or a similar service that is used by your OS. For more information, see [Implementing `no_root_squash` for NFS (optional)](/docs/vpc?topic=vpc-file-storage-mount-RHEL&interface=ui#fs-RHEL-norootsquash).

Learn about [Managing file shares](/docs/vpc?topic=vpc-file-storage-managing).

If you encounter issues while mounting file shares, see [Troubleshooting Mount Helper](/docs/vpc?topic=vpc-fs-mount-helper-ts).
