---

copyright:
  years: 2023
lastupdated: "2023-11-10"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# IBM Cloud File Share Mount Helper utility
{: #fs-mount-helper-utility}

Mount Helper is an open source automation tool that configures and establishes secure IPsec communication between customer virtual server instance and the file share. It ensures that the communication between the virtual server instance and zonal file share service is encrypted.
{: shortdesc}

The utility uses strongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html) to configure IPsec on the virtual server instance with Linux OS.

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires mutual authentication. The Mount Helper retrieves the instance identity token from the Metadata service, and with the instance identity token it requests the creation of the instance identity certificate.

The Mount Helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use the utility for encrypted or unencrypted connections. For encrypted connections, the Mount Helper uses the instance metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/apidocs/vpc/latest#create-instance).

For more information, see the [readme file](https://github.com/IBM/vpc-file-storage-mount-helper){: external}.

## Requirements
{: #fs-eit-requirements}

* [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the virtual server instance. If it's not enabled yet, follow the instructions for [enabling instance metadata in the UI.](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-service-ui){: ui}[enabling instance metadata from the CLI.](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable-cli){: cli}[enabling instance metadata from the API.](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-metadata-service-enable-api){: api}
* The file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which virtual server instances can mount the share.
* The virtual server instance and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups).
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), so it has an IP address within the VPC that represents the virtual NFS server.
* Data encryption in transit must be enabled for the mount target either in the UI or with the API.

## Restrictions
{: #fs-eit-restrictions}

* The same certificates cannot be used across multiple regions.
* The Mount Helper is supported for mounting on Linux hosts only. The utility is supported on the following distributions:

   | Supported OS | Supported OS | Supported OS                 |
   |--------------|--------------|------------------------------|
   | UBUNTU_2004  | RHEL_7       | SAP_SLES_15_SP3_HANA         |
   | UBUNTU_2204  | RHEL_8       | SAP_SLES_15_SP3_APPLICATIONS |
   | CENTOS_7     | RHEL_8_6     | SAP_SLES_15_SP4_HANA         |
   | DEBIAN_10    | RHEL_9       | SAP_SLES_15_SP4_APPLICATIONS |
   | ROCKYLINUX_8 |              | |
   {: caption="Table 1 - This table shows the supported host OS distributions." caption-side="bottom"}
   
## Installation and configuration of the Mount Helper
{: #fs-eit-installation}

1. You can either build the mount helper package from source code or download the package directly from GitHub.

    - Build from source code.

       - On Debian based instances, run the following commands:
       
         ```sh
         apt-get update -y
         apt-get install git make python3 -y
         git clone git@github.com:IBM/vpc-file-storage-mount-helper.git
         cd vpc-file-storage-mount-helper
         make build-deb
         ```
         {: pre}

       - On RPM based instances, run the following commands:
  
         ```sh
         yum update -y
         yum install git make python3 rpm-build -y
         git clone git@github.com:IBM/vpc-file-storage-mount-helper.git
         cd vpc-file-storage-mount-helper
         make build-rpm
         ```
         {: pre}

    - Download the Mount Helper package from GitHub. 

      ```sh
       curl -LO https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-latest.tar.gz 
       ```
       {: codeblock}

1. Extract the tar file.
   ```sh
   tar -xvf mount.ibmshare-latest.tar.gz
   ```
   {: pre}
   
   The tar file contains the following contents: installation and uninstallation scripts, `mount helper rpm` and `deb` packages, root CA certificates, and configuration file.

   Closed environments: To install Mount Helper on a virtual server instance without internet connection, create or update a local repository on the VSI based on the OS. Copy the Mount Helper package along with its dependencies to the local directory.
   {: note}

1. Every installation image is accompanied by a file that contains the checksum value for the image file. For example, the image file ibmshare-0.0.1.tar.gz is accompanied by the ibmshare-0.0.1.tar.gz.sha256 file that contains checksum value. To verify the integrity of the downloaded package, use the following command to verify the file `mount.ibmshare-latest.tar.gz`.
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


1. To install the Mount Helper and all the dependencies, use the following script.

   ```sh
   ./install.sh region=dal
   ```
   {: pre}

   The installation script accepts the command-line argument `region`. Example regions are `dal`, `fra`, `lon`, `osa`, `sao`, `syd`, `tok`, `tor`, `wdc`. This argument is used to copy region-specific root CA cert to the strongSwan certificates location. If no region is specified, then the utility copies all the root CA certs.

1. Update the configuration file `/etc/ibmcloud/share.conf` with region information and with the peer certificate expiration time that you want. By default, certificates last 1 hour and new certificates are fetched every 45 minutes. However, you can modify the `certificate_duration_seconds` option to have a value between 5 minutes and 1 hour.
   ```sh
   certificate_duration_seconds = 600
   ```
   {: pre}
   
   This option is specified in seconds. The valid range for its value is 300 - 3600 seconds. The certificates are renewed when the current certs reach 70% of their lifetime.

1. If you want to renew the certs immediately with the new expiration time, then run the following command.
   ```sh
   /sbin/mount.ibmshare -RENEW_CERTIFICATE_NOW
   ```
   {: pre}


## Mounting a file share with the Mount Helper
{: #fs-eit-mount-share}

1. Run the `mount` command with the following syntax.

   ```sh
   mount -t ibmshare -o secure=true  <share-ip>:/<mount-point> /mnt/share-test

   ```
   {: pre}

   See the following example.
   ```sh 
   mount -t ibmshare -o secure=true 10.240.0.5:/acdaecff_a291_41bd_87c1_0dde05135d59 /mnt/nfs -v
   ```
   {: pre}
   
   The `ibmshare` in the command is a script that creates the certificate signing request(csr) and calls the Metadata service to get the intermediate cert and end peer certificate. It parses the mount command-line arguments and creates `/etc/swanctl/conf.d/type_ibmshare_.conf`. The strongSwan service uses this configuration file to establish the IPsec connection. Then, the script loads the IPsec connection and calls the NFS `mount` command. 

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
