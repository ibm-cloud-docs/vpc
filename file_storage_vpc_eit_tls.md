---

copyright:
  years: 2025, 2025
lastupdated: "2025-11-04"

keywords: file share, file storage, encryption in transit, Mount Helper, TLS, NFS over TLS, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing encryption in transit for regional file shares
{: #file-storage-vpc-eit-tls}

You can establish an encrypted mount connection between the compute host and a regional file share by creating a Transport Layer Security (TLS) 1.2+ connection with stunnel. You can enable secure end-to-end encryption when you use file shares with security-group-based access control mode and mount targets with virtual network interfaces (VNI). When such a mount target is attached and the share is mounted, the VNI checks the security group policy to make sure that only authorized instances can communicate with the share.
{: shortdesc}

Stunnel is an open source, multipurpose, multiplatform application that provides a universal TLS/SSL tunneling service. It relies on the OpenSSL library to implement the underlying TLS protocol. Stunnel uses public-key cryptography with X.509 digital certificates to secure the connection, and clients can optionally be authenticated through a certificate.

The cipher for the stunnel connection is AES-128-GCM (TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256) if established through the Mount Helper utility. For more information acceptable cipher suites, see the [IBM Cloud Framework for Financial Services - Data encryption in transit](/docs/framework-financial-services?topic=framework-financial-services-shared-encryption-in-transit) topic.

Installation and maintenance of the stunnel client application is your responsibility. For more information, see [Stunnel - How-to](https://www.stunnel.org/howto.html){: external}, [Using stunnel - Red Hat Enterprise Linux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/sec-using_stunnel#sec-Using_stunnel){: external}.
{: important}



## Before you begin using encryption in transit with Stunnel
{: #file-storage-vpc-eit-stunnel-setup}

To use the feature, the following requirements need to be met:

- The file share must be based on the [`rfs` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#rfs-profile) and be configured with [Security Group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

- The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). The compute host and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups). For more information, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).

- Data encryption in transit must be enabled. In the console, you can toggle encryption in transit on when you create the mount target. The API `transit_encryption` property accepts the `stunnel` value to enable the feature.

- Stunnel must be installed on the server. The {{site.data.keyword.cloud}} file service provides the [Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to automate the tasks that are required on the compute host, such as downloading, configuring and initiating the stunnel application.



## Mounting the file share with stunnel
{: #file-storage-vpc-eit-stunnel-manual-setup-mount}

1. Use the following command syntax to mount the share. Replace the mount path with the information that is specific to your file share.

   ```sh
   mount -t ibmshare -o stunnel 10.0.0.1:/MOUNT_PATH /mnt/MOUNT_PATH
   ```
   {: screen}

   See the following example.

   ```sh
   ++ mount -v -o stunnel -t ibmshare 10.240.64.24:/EAD9B8582BC84FDAB57B7A315BCA1210 /mnt/EAD9B8582BC84FDAB57B7A315BCA1210
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
   Debug - RunCmd: Mount using stunnel  (mount -t nfs4 -o sec=sys,nfsvers=4.1,rw,port=10001 127.0.0.1:/EAD9B8582BC84FDAB57B7A315BCA1210 /mnt/EAD9B8582BC84FDAB57B7A315BCA1210 -v)
   Debug - Stunnel mount was successful
   Debug - File unlocked:/var/lock/ibm_mount_helper.lck
   ```
   {: screen}

1. Verify the connection by checking the system logs to confirm that stunnel is running and the connection is established successfully. You can also run the following command to view the attached file system:

   ```sh
   df -h
   ```
   {: pre}



## Implementing `no_root_squash` for NFS (optional)
{: #fs-eit-norootsquash}

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
