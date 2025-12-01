---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-01"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

ai-gen-assist: granite

---

{{site.data.keyword.attribute-definition-list}}

# IBM Cloud File Share Mount Helper utility
{: #fs-mount-helper-utility}

Mount Helper is an open source automation tool that configures and establishes secure communication between the compute host and the file share. It ensures that the communication between the server and the zonal or regional file share is encrypted.
{: shortdesc}

## In-transit encryption types
{: #fs-eit-types}

### IPsec encapsulated connection for zonal shares
{: #fs-eit-ipsec-requirements}

The utility uses strongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html) to configure IPsec on the compute host that's running a Linux OS.

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires mutual authentication. The Mount Helper retrieves the instance or bare metal server identity token from the Metadata service. Then, it requests the creation of the instance or bare metal server identity certificate by using the identity token.

The Mount Helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use the utility for encrypted or unencrypted connections. For encrypted connections, the Mount Helper uses the metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/apidocs/vpc/latest#create-instance) and [bare metal server provisioning](/apidocs/vpc/latest#create-bare-metal-server).

### Stunnel secure connection for regional shares
{: #fs-eit-stunnel-requirements}

The Mount Helper utility installs stunnel on the compute host that's running a Linux OS. Stunnel is an application that creates encrypted TLS tunnels between clients and servers for secure communication. In client mode, Stunnel initiates a connection from your virtual server instance of bare metal server to the file share, and tunnels data over a secure connection. Stunnel requires a PEM file, which typically contains a private key and a certificate. When stunnel operates in client mode, it relies on the system-wide SSL/TLS configuration and certificates. It can use the default PEM file provided by the Linux distribution rather than generating a custom certificate. The PEM file is often located in `/etc/ssl/private` or `/etc/pki/tls/private` folder.

## Requirements
{: #fs-eit-requirements}

* For setting up a secure connection with **zonal** file share, the [Metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the virtual server instance. If it is not enabled yet, follow the instructions for [enabling metadata in the console](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-service-ui){: ui}[enabling metadata from the CLI](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable-cli){: cli}[enabling metadata from the API](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-metadata-service-enable-api){: api} for virtual server instances or [enabling metadata in the console](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-enable-service-ui-bare-metal){: ui}[enabling metadata from the CLI](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=cli#metadata-service-enable-cli-bare-metal){: cli}[enabling metadata from the API](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-service-enable-api-bare-metal){: api} for bare metal servers.
* The zonal or regional file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which compute host can mount the share.
* Data encryption in transit must be enabled for the mount target.
* The compute host and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups).
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), so it has an IP address within the VPC that represents the virtual NFS server.

## Restrictions
{: #fs-eit-restrictions}

* For IPsec encapsulation, the same certificates cannot be used across multiple regions.
* The Mount Helper is supported on Linux hosts only. See the table for the supported distributions:

   | Supported OS    | Supported OS | Supported OS                 |
   |-----------------|--------------|------------------------------|
   | UBUNTU_2204     | UBUNTU_2404  | SAP_SLES_15_SP3_HANA         |
   | RHEL_8 [8.4, 8.6, 8.8, 8.10] | RHEL_9 [9.0, 9.2, 9.4] | SAP_SLES_15_SP3_APPLICATIONS |
   | CENTOS_STREAM_9 | CENTOS_STREAM_10 | SAP_SLES_15_SP4_HANA       |
   | DEBIAN_11       | DEBIAN_12    | SAP_SLES_15_SP4_APPLICATIONS |
   | ROCKYLINUX_8 [8.9, 8.10] | ROCKYLINUX_9 [9.4, 9.5]  |   |
   {: caption="This table shows the supported host OS distributions." caption-side="bottom"}

* Installing the Mount Helper on Red Hat Enterprise Linux CoreOS is not supported.
   
## Installation and configuration of the Mount Helper
{: #fs-eit-installation}

1. Log in to the compute host where you want to mount the file share.
   - [Connect to your virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui).
   - [Connect to your bare metal server](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers).
   - If you want to access the file shares from IBM {{site.data.keyword.powerSys_notm}} instances, you must use a network path through a load balancer. For more information, see the following tutorial: [Accessing File Storage for VPC shares from IBM Power Virtual Server instances](/docs/sap?group=file-storage-shares-for-vpc) and make sure that you completed Steps 1-3. Step 4 can be performed by the Mount Helper after it is installed on the Power VSI.
   
1. Then, you can download the package directly from GitHub, or build the utility from the source code.
   
   If you want to mount a regional file share on an IBM Power VSI, download the installation package, and follow the steps of [Installing the Mount Helper to mount regional file shares](#install-MH-for-regional).
   {: note}

### Downloading the installation package
{: #download-from-github}

1. Download the Mount Helper package from GitHub. 
    ```sh
    curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz 
    ```
    {: codeblock}

    To establish an encrypted connection between a bare metal server and a file share, download Mount Helper version 0.2.1.
    {: important}

1. Extract the compressed file.
   ```sh
   tar -xvf mount.ibmshare-latest.tar.gz
   ```
   {: pre}
   
   The file contains the following items: installation and uninstallation scripts, `rpm` and `deb` packages, root CA certificates, and the configuration file.

   Closed environments: To install Mount Helper on a virtual server instance without internet connection, create or update a local repository on the VSI based on the OS. Copy the Mount Helper package along with its dependencies to the local directory.
   {: note}

### Installing the Mount Helper to mount zonal file shares
{: #install-MH-for-zonal}

1. To install the Mount Helper and all the dependencies, use the following script and specify the region where the file share is going to be mounted.
   ```sh
   ./install.sh region=us-south
   ```
   {: pre}

   The `region` argument is used to copy region-specific root CA cert. If no region is specified, then the utility copies all the root CA certs. The following table shows the values that you can use to specify the region.

   | Location            | New value | Previous Value |
   |---------------------|-------------|-------|
   | Australia - Sydney  | `au-syd`    | `syd` |
   | Brazil - Sao Paulo  | `br-sao`    | `sao` |
   | Canada - Montreal   | `ca-mon`    |       |
   | Canada - Toronto    | `ca-tor`    | `tor` |
   | Germany - Frankfurt | `eu-de`     | `fra` |
   | India - Chennai     | `in-che`    |       | 
   | Japan - Osaka       | `jp-osa`    | `osa` | 
   | Japan - Tokyo       | `jp-tok`    | `tok` |   
   | Spain - Madrid      | `eu-es`    | `mad` |
   | United Kingdom - London | `eu-gb` | `lon` | 
   | United States - Washington, DC | `us-east`| `wdc` |
   | United States - Dallas, TX | `us-south`  | `dal` |
   {: caption="This table shows the region values that the script accepts." caption-side="bottom"}

1. Optional - Every installation image is accompanied by a file that contains the checksum value for the image file. For example, the image file ibmshare-0.0.1.tar.gz is accompanied by the ibmshare-0.0.1.tar.gz.sha256 file that contains the checksum value. To verify the integrity of the downloaded package, use the following commands.
   ```sh
   curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}
   
   ```sh
   sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   A successful response shows "OK". The output looks like the following example.
   ```text
   # sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ./mount.ibmshare-latest.tar.gz: OK
   ```
   {: screen}

1. Optional - By default, a certificate lasts 1 hour, and new certificates are fetched in every 45 minutes. However, you can modify the `certificate_duration_seconds` option in the configuration file `/etc/ibmcloud/share.conf` to a different time interval. The new value must be between 5 minutes and 1 hour, and expressed in seconds.
   ```sh
   certificate_duration_seconds = 600
   ```
   {: pre}
   
   The valid range for `certificate_duration_seconds` value is 300 - 3600 seconds. The certificates are renewed when the current certs reach 70% of their lifetime.
   {: note}

1. Optional - If you want to renew the certs immediately with the new expiration time, then run the following command.
   ```sh
   /sbin/mount.ibmshare -RENEW_CERTIFICATE_NOW
   ```
   {: pre}

### Installing the Mount Helper to mount regional file shares
{: #install-MH-for-regional}

1. To install the Mount Helper and all the dependencies, use the following script and specify the `--stunnel` option.
   ```sh
   ./install.sh --stunnel
   ```
   {: pre}

1. Optional - Every installation image is accompanied by a file that contains the checksum value for the image file. For example, the image file ibmshare-0.0.1.tar.gz is accompanied by the ibmshare-0.0.1.tar.gz.sha256 file that contains the checksum value. To verify the integrity of the downloaded package, use the following commands.
   ```sh
   curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}
   
   ```sh
   sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ```
   {: pre}

   A successful response shows "OK". The output looks like the following example.
   ```text
   # sha256sum -c mount.ibmshare-latest.tar.gz.sha256
   ./mount.ibmshare-latest.tar.gz: OK
   ```
   {: screen}

### Building the Mount Helper utility from the source code
{: #build-from-source-code}

- On Debian-based instances, run the following commands:
    ```sh
    apt-get update -y
    apt-get install git make python3 -y
    git clone https://github.com/IBM/vpc-file-storage-mount-helper.git
    cd vpc-file-storage-mount-helper
    make build-deb
    ```

- On rpm-based instances, run the following commands:
   ```sh
   yum update -y
   dnf install git make python3 rpm-build -y
   git clone https://github.com/IBM/vpc-file-storage-mount-helper.git
   cd vpc-file-storage-mount-helper
   make build-rpm
   ```

## Mounting a file share with the Mount Helper
{: #fs-eit-mount-share}

1. Create a directory on your server.
   ```sh
   mkdir /mnt/share-test
   ```
   {: pre}
   
1. Run the `mount` command with the following syntax. 

### Mounting zonal file share
{: #fs-eit-mount-share-ipsec} 

Use the following command syntax to mount the share. Replace the mount path with the information that is specific to your file share.

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

### Mounting regional file share
{: #fs-eit-mount-share-stunnel}

Use the following command syntax to mount the share. Replace the mount path with the information that is specific to your file share.

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

If you need to add the mount details to `/etc/fstab`, make sure that you use the `_netdev` and `nofail` options. See the following example:

```sh
10.241.128.16:/1F8943459151408E83CB4AC35FE22734  /mnt/1F8943459151408E83CB4AC35FE22734 ibmshare  _netdev,stunnel,nofail  0  0
```
{: pre}

Then, after the virtual server is started, you can mount the file share with the `mount /mnt/MOUNT_POINT` command.

## Updating the Mount Helper
{: #fs-eit-mount-helper-update}

To update the installation package, run the `install.sh` script again.
```sh
./install.sh
```
{: pre} 

Use the `--stunnel` option when you want to upgrade the stunnel package, too.

## Uninstalling the Mount Helper
{: #fs-eit-mount-helper-uninstall}

The following command uninstalls the utility.
```sh
./uninstall.sh
```
{: pre}

## Troubleshooting Tips
{: #fs-eit-mount-helper-ts}

- If the IPsec installation fails, then check the mount-helper installation script logs that are displayed on the standard output.

- If the IPsec is installed properly but failed to start the IPsec service, then check the `charon` logs. The `charon` logs can be found in one of the following log files based on OS image: `/var/log/syslog` or `/var/log/messages`.

- You can run the following `swanctl` command on another terminal to check the IPsec connection negotiation logs before you attempt the mount command.
   ```sh
   swanctl -T
   ```
   {: pre}

- To manually start or disconnect the IPsec connection, use one of the following commands.
   ```sh
   ipsec up <connection-name>
   ipsec down <connection-name>
   ```
   {: codeblock}

- If IPsec command is not available, use the following `swanctl` command.
   ```sh
   swanctl -i --ike <connection-name>
   swanctl -i --child <connection-name>
   swanctl -t --ike <connection-name>
   ```
   {: codeblock}

- To list active connections with IPsec encryption, use one of the following commands.
   ```sh
   ipsec status
   ```
   {: pre}
   
   Or
   ```sh
   swanctl -l
   ```
   {: pre}

- Command to restart the strongSwan service.
   ```sh
   systemctl restart strongswan
   ```
   {: pre}

- Location of the mount helper log.
   ```sh
   /opt/ibm/mount-ibmshare/mount-ibmshare.log
   ```
   {: pre} 

- Location of the stunnel logs:
   ```sh
   /var/log/stunnel/ibmshare_[MOUNT_PATH].log
   ```
   {: pre} 

## Next steps
{: #next-steps-eit}

By default, NFS downgrades any files that were created with the root permissions to the `nobody` user. This security feature prevents privileges from being shared unless they are requested. By configuring `no_root_squash`, root clients can retain root permissions on the remote NFS file share. For NFSv4.2, set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd` or a similar service that is used by your OS. For more information, see [Implementing `no_root_squash` for NFS (optional)](/docs/vpc?topic=vpc-file-storage-mount-RHEL&interface=ui#fs-RHEL-norootsquash).

Learn about [Managing file shares](/docs/vpc?topic=vpc-file-storage-managing).
