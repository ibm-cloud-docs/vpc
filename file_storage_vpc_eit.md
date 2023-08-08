---

copyright:
  years: 2023
lastupdated: "2023-08-08"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Encryption in transit - Securing mount connections between file share and virtual server instance
{: #file-storage-vpc-eit}

You can establish an encrypted mount connection between the virtual server instance and storage system by using the Internet Security Protocol (IPsec) security profile and X.509 certificates. By enabling encryption in transit, you create secure end-to-end encryption for your data.
{: shortdesc}

Encryption in transit is available in most regions. Support for EIT is not available in the `eu-es` region yet.
{: restriction}

If you choose to use Encryption-in-transit, you need to balance your requirements between performance and enhanced security. Encrypting data in transit can have some performance impact due to the processing that is needed to encrypt and decrypt the data at the endpoints. The impact depends on the workload characteristics. Workloads that perform synchronous writes or bypass VSI caching, such as databases, might have a substantial performance impact when EIT is enabled. To determine EIT’s performance impact, benchmark your workload with and without EIT. Also, even without EIT, the data is moving through a secure data center network.

For more information about network security, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc) and [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

## Overview
{: #file-storage-eit-overview}

With this feature, you can enable secure end-to-end encryption of your data when you use file shares with security-group-based access control mode and mount targets with virtual network interfaces. When such a mount target is attached and the share is mounted on a virtual server instance, the virtual network interface checks the security group policy to ensure only authorized instances can communicate with the share. The traffic between the authorized virtual server instance and the file share can be IPsec encapsulated by the client. 

IPsec is a group of protocols that together set up encrypted connections between devices. It helps keep data sent over public networks secure. IPsec Encrypts IP packets, and authenticates the source where the packets come from. To configure IPsec on your virtual server instance, you can use [strongSwan](https://www.strongswan.org/){: external}, which is an open source IPsec-based VPN solution. For more information about how strongSwan works, see [Introduction to strongSwan](https://docs.strongswan.org/docs/5.9/howtos/introduction.html){: external} and [IPsec Protocol](https://docs.strongswan.org/docs/5.9/howtos/ipsecProtocol.html){: external}, too.

The IPsec connection requires that you have an X.509 certificate for authentication. X.509 is an international standard format for public key certificates, digital documents that securely associate cryptographic key pairs with identities such as websites, individuals, or organizations. The Instance Metadata service is used to create the certificates.

A Certificate Signing Request (CSR) is a block of encoded texts that are forwarded to a certificate authority (CA) when users apply for a certificate. CSR is created on the server where the certificate is to be installed. CSR includes information such as domain name, organization name, locality, and country. The request also contains the public key, which is associated with the certificate that is generated, and the private key. The CA uses only the public key when the certificate is created. The private key must be saved and kept secret. As the private key is part of the key pair with the public key, and the certificate does not work if the private key is lost.

Encryption in transit is not supported on {{site.data.keyword.bm_is_short}}.
{: restriction}

## Configuring encryption in transit
{: #file-storage-eit-setup}

To use the feature, the following requirements need to be met:
- The file share must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#dp2-profile) and be configured with [Security Group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets). 
- The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). For more information, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
- Data encryption in transit must be enabled. In the UI, you can toggle encryption in transit on when you create the mount target. The API `transit_encryption` property accepts the `user_managed` value to enable the feature.
- When you configure the virtual server instance to access the file share, you must configure IPsec Transport Mode for the mount target address. [Install](https://docs.strongswan.org/docs/5.9/install/install.html){: external} and configure the strongSwan client.
- Obtain X.509 certificates are needed for authentication. The same certificates cannot be used across multiple regions.
   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl.
      ```sh
      openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
      ```
      {: pre}

      When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.

      OpenSSL is an open source command-line toolkit that you can use to work with X.509 certificates, certificate signing requests (CSRs), and cryptographic keys. For more information, see [OpenSSL Documentation](https://www.openssl.org/docs/){: external}.
      {: note}

      If you're using a different software to create the CSR, you might be prompted to enter information about your location (country code, state, locality), your organization name, organization unit, an email address, and a common name. Only country code and email address are required. Do not enter a common name. When you make the request to the Metadata API, the system applies instance ID values to the subject Common Name for instance identity certificates. Thus CSRs with Common Name specified are rejected. CSRs with `IsCA=true` or `KeyUsage.KeyUsageCertSign=True` extensions are also rejected.
      {: important}

   2. Format the csr before you make an API call to metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}

- [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled for the virtual server instance. 
   - Make a `PUT /instance_identity/v1/token` API call to get a token from Metadata service to be used for subsequent calls. For more information, see [Acquiring an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-json-token).
   - Make a `POST /instance_identity/v1/certificates` call and specify the instance identity token in the HTTP Authorization header, plus a Certificate Signing Request (as `csr` property) and a validity duration (as `expires_in` property). The call returns a new client certificate and intermediate certificate chain that allows the client to access file shares by using IPsec Encryption in Transit. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-acquire-certificate).    

The {{site.data.keyword.cloud}} file service provides a [Mount Helper utility](#fs-mount-helper-utility) to automate these tasks.

## IBM Cloud File Share Mount Helper utility
{: #fs-mount-helper-utility}

Mount Helper is an open source automation tool that configures and establishes secure IPsec communication between customer virtual server instance and the file share. It ensures that the communication between the virtual server instance and zonal file share service is encrypted.

The utility uses strongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html) to configure IPsec on the virtual server instance with Linux OS. The utility is supported on the following distributions:

- UBUNTU_2004
- UBUNTU_2204
- CENTOS_7
- DEBIAN_10
- ROCKYLINUX_8
- SAP_SLES_15_SP3_HANA
- SAP_SLES_15_SP3_APPLICATIONS
- SAP_SLES_15_SP4_HANA
- SAP_SLES_15_SP4_APPLICATIONS
- RHEL_7
- RHEL_8
- RHEL_8_6
- RHEL_9

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires mutual authentication. The Mount Helper retrieves the instance identity token from the Metadata service, and with the instance identity token.

The Mount Helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use the utility for encrypted or unencrypted connections. For encrypted connections, the Mount Helper uses the instance metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/apidocs/vpc/latest#create-instance).

For more information, see the [readme file](https://github.com/IBM/vpc-file-storage-mount-helper).

### Requirements and restrictions
{: #fs-eit-requirements}

* [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the virtual server instance.
* The file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which virtual server instances can mount the share. 
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to create an IP address within the VPC that represents a virtual NFS server.
* Data encryption in transit must be enabled either in the UI or with the API.
* The same certificates cannot be used across multiple regions.
* The Mount Helper is supported for mounting on Linux hosts only.

### Installation and configuration of the Mount Helper
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


### Mounting a file share with the Mount Helper
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
   
   You can also include options such as `vers=4.1` and `sec=sys`.

Adding the mount details to the `/etc/fstab` is not recommended. The IPsec connection might not be established in time for the automated `fstab` mount requests.
{: note}

### Updating the Mount Helper
{: #fs-eit-mount-helper-update}

To update the installation package, run the `install.sh` script again.
```sh
./install.sh
```
{: pre} 

### Uninstalling the Mount Helper
{: #fs-eit-mount-helper-uninstall}

The following command uninstalls the utility.
```sh
./uninstall.sh
```
{: pre}

### Troubleshooting Tips
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
