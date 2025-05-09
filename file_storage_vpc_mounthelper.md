---

copyright:
  years: 2023, 2025
lastupdated: "2025-05-09"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# IBM Cloud File Share Mount Helper utility
{: #fs-mount-helper-utility}

Mount Helper is an open source automation tool that configures and establishes secure IPsec communication between the compute host and the file share. It ensures that the communication between the server and the zonal file share is encrypted.
{: shortdesc}

The utility uses strongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html) to configure IPsec on the virtual server instance with Linux OS.

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires mutual authentication. The Mount Helper retrieves the instance identity token from the Metadata service, and with the instance identity token it requests the creation of the instance identity certificate.

The Mount Helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use the utility for encrypted or unencrypted connections. For encrypted connections, the Mount Helper uses the metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/apidocs/vpc/latest#create-instance).

## Requirements
{: #fs-eit-requirements}

* [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the virtual server instance. If it is not enabled yet, follow the instructions for [enabling instance metadata in the console.](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-service-ui){: ui}[enabling instance metadata from the CLI.](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable-cli){: cli}[enabling instance metadata from the API.](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-metadata-service-enable-api){: api}
* The file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which virtual server instances can mount the share.
* Data encryption in transit must be enabled for the mount target.
* The virtual server instance and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups).
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), so it has an IP address within the VPC that represents the virtual NFS server.

## Restrictions
{: #fs-eit-restrictions}

* The same certificates cannot be used across multiple regions.
* The Mount Helper is supported on Linux hosts only. See the table for the supported distributions:

   | Supported OS    | Supported OS | Supported OS                 |
   |-----------------|--------------|------------------------------|
   | UBUNTU_2204     | UBUNTU_2404  | SAP_SLES_15_SP3_HANA         |
   | RHEL_8          | RHEL_9       | SAP_SLES_15_SP3_APPLICATIONS |
   | CENTOS_STREAM_9 | CENTOS_STREAM_10 | SAP_SLES_15_SP4_HANA       |
   | DEBIAN_11       | DEBIAN_12    | SAP_SLES_15_SP4_APPLICATIONS |
   | ROCKYLINUX_8[8.9,8.10] | ROCKYLINUX_9[9.4,9.5]  |   |
   {: caption="This table shows the supported host OS distributions." caption-side="bottom"}

* Installing the Mount Helper on Red Hat Enterprise Linux CoreOS is not supported.
* Encryption in transit is not supported on {{site.data.keyword.bm_is_short}}.
   
## Installation and configuration of the Mount Helper
{: #fs-eit-installation}

[SSH into the Compute instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#next-steps-after-creating-virtual-servers-ui) where you want to mount the file share. Then, you can download the package directly from GitHub, or build the utility from the source code.

### Downloading the installation package
{: #download-from-github}

1. Download the Mount Helper package from GitHub. 
    ```sh
    curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz 
    ```
    {: codeblock}

1. Extract the compressed file.
   ```sh
   tar -xvf mount.ibmshare-latest.tar.gz
   ```
   {: pre}
   
   The file contains the following items: installation and uninstallation scripts, `rpm` and `deb` packages, root CA certificates, and the configuration file.

   Closed environments: To install Mount Helper on a virtual server instance without internet connection, create or update a local repository on the VSI based on the OS. Copy the Mount Helper package along with its dependencies to the local directory.
   {: note}

1. To install the Mount Helper and all the dependencies, use the following script and specify the region where the file share is going to be mounted.
   ```sh
   ./install.sh region=us-south
   ```
   {: pre}

    The `region` argument is used to copy region-specific root CA cert to the strongSwan certificate location. If no region is specified, then the utility copies all the root CA certs. The following table shows the values that you can use to specify the region.

   | Location            | New value | Previous Value |
   |---------------------|-------------|-------|
   | Australia - Sydney  | `au-syd`    | `syd` |
   | Brazil - Sao Paulo  | `br-sao`    | `sao` |
   | Canada - Montreal   | `ca-mon`    |       |
   | Canada - Toronto    | `ca-tor`    | `tor` |
   | Germany - Frankfurt | `eu-de`     | `fra` | 
   | Japan - Osaka       | `jp-osa`    | `osa` | 
   | Japan - Tokyo       | `jp-tok`    | `tok` |   
   | Spain - Madrid      | `eu-mad`    | `mad` |
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

1. Optional - By default, a certificate lasts 1 hour, and new certificates are fetched every 45 minutes. However, you can modify the `certificate_duration_seconds` option in the configuration file `/etc/ibmcloud/share.conf` to a different time interval. The new value must be between 5 minutes and 1 hour, and expressed in seconds.
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

- On RPM-based instances, run the following commands:
   ```sh
   yum update -y
   yum install git make python3 rpm-build -y
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
   ```sh
   mount -t ibmshare -o secure=true  <share-ip>:/<mount-point> /mnt/share-test
   ```
   {: pre}
   
   The `ibmshare` in the command is a script that creates the certificate signing request(csr) and calls the Metadata service to get the intermediate cert and end peer certificate. It parses the mount command-line arguments and creates `/etc/swanctl/conf.d/type_ibmshare_.conf`. The strongSwan service uses this configuration file to establish the IPsec connection. Then, the script loads the IPsec connection and calls the NFS `mount` command. A successful response looks like the following example.

   ```sh 
   [root@my-demo-eit-instance ~]# mount -t ibmshare -o secure=true 10.240.64.5:/0c937ac3_814e_4a7c_99b8_719ec3cad7fd  /mnt/share-test
   Info  - IpSec using StrongSwan(5.7.2)
   Debug - RunCmd: /usr/sbin/swanctl --list-conns 
   Debug - Config data unchanged:/etc/strongswan/swanctl/conf.d/type_ibmshare_10.240.64.5.conf
   Debug - StrongSwan cleanup config files Total(1) Mounted(0) Deleted(0) Recent(1)
   Debug - RunCmd: ReloadConfig (/usr/sbin/swanctl --load-all)
   Debug - File unlocked:/var/lock/ibm_mount_helper.lck
   Debug - RunCmd: MountCmd (mount -t nfs4 -o sec=sys,nfsvers=4.1,rw 10.240.64.5:/0c937ac3_814e_4a7c_99b8_719ec3cad7fd /mnt/share-test)
   Debug - RunCmd: LoadCert (openssl x509 -in /etc/strongswan/swanctl/x509ca/type_ibmshare_int.crt -noout -dates -subject -issuer)
   Debug - RunCmd: LoadCert (openssl x509 -in /etc/strongswan/swanctl/x509ca/type_ibmshare_root_dal.crt -noout -dates -subject -issuer)
   Share successfully mounted:
   ```
   {: screen}

Adding the mount details to the `/etc/fstab` is not recommended. The IPsec connection might not be established in time for the automated `fstab` mount requests.
{: note}

## Updating the Mount Helper
{: #fs-eit-mount-helper-update}

To update the installation package, run the `install.sh` script again.
```sh
./install.sh
```
{: pre} 

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

- To list active connections, use one of the following commands.
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

## Next steps
{: #next-steps-eit}

Learn about [Managing file shares](/docs/vpc?topic=vpc-file-storage-managing).
