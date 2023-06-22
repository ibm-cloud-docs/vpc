---

copyright:
  years: 2023
lastupdated: "2023-06-20"

keywords: file share, file storage, encryption in transit, mount helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Encryption in transit - Securing mount connections between file share and host
{: #file-storage-vpc-eit}

[New]{: tag-new}

You can establish an encrypted mount connection between the virtual server instance and storage system by using the Internet Security Protocol (IPsec) security profile and X.509 certificates. By enabling encryption in transit, you create secure end-to-end encryption for your data.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Overview
{: #file-storage-eit-overview}

With this feature, you can enable secure end-to-end encryption of your data when security-group-based access control on file shares and mount targets with virtual network interfaces are used. When such a mount target is attached and the share is mounted, the virtual network interface performs security group policy check to ensure only authorized instances can communicate with the share. The traffic between the authorized virtual server instance and the file share can be IPsec encapsulated by the client.

IPsec is a group of protocols that together set up encrypted connections between devices. It helps keep data sent over public networks secure. IPsec Encrypts IP packets, and authenticates the source where the packets come from. 

The mutual SSL connection requires that you have an X.509 certificate for authentication. X.509 is an international standard format for public key certificates, digital documents that securely associate cryptographic key pairs with identities such as websites, individuals, or organizations. The Instance Metadata service is used to create the certificates.

A Certificate Signing Request (CSR) is a block of encoded texts that are forwarded to a certificate authority (CA) when users apply for a certificate, such as a SSL/TLS certificate. CSR is created on the server where the certificate is to be installed. CSR includes information such as domain name, organization name, locality, and country. The request also contains the public key, which is associated with the certificate that is generated, and the private key. The CA uses only the public key when the certificate is created. The private key must be saved and kept secret. As the private key is part of the key pair with the public key, and the certificate does not work if the private key is lost.

## Configuring encryption in transit
{: #file-storage-eit-setup}

To use the feature, the following requirements need to be met:
- The file share must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#dp2-profile) and be configured with [Security Group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets). 
- The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). For more information, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
- Data encryption in transit must be enabled. In the UI, you can toggle encryption in transit on or off. The API `transit_encryption` property accepts the `user_managed` value to enable the feature.
- When you configure the virtual server instance to access the file share, you must configure IPsec Transport Mode for the mount target address. [Install](https://docs.strongswan.org/docs/5.9/install/install.html){: external} and configure the [StrongSwan](https://www.strongswan.org/download.html){: external} client.
- Obtain X.509 certificates are needed for authentication. The same certificates cannot be used across multiple regions.
   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl.
      ```sh
      openssl req -out sslcert.csr -newkey rsa:2048 -nodes -keyout private.key
      ```
      {: pre}

      OpenSSL is an open source command-line toolkit that you can use to work with X.509 certificates, certificate signing requests (CSRs), and cryptographic keys. For more information, see [OpenSSL Documentation](https://www.openssl.org/docs/){: external}.
      {: note}

   2. Format the csr before you make an API call to metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}
   
      The system rejects any certificate signing requests with Common Name, and the extensions `IsCA=true` and `KeyUsage.KeyUsageCertSign=True`.
      {: important}

- [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled for the virtual server instance. 
   - Make a `PUT /instance_identity/v1/token` API call to get a token from Metadata service to be used for subsequent calls. For more information, see [Acquiring an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-json-token).
   - Make a `POST /instance_identity/v1/certificates` call and specify the instance identity token in the HTTP Authorization header, as well as a Certificate Signing Request (as `csr` property) and a validity duration (as `expires_in` property). The call returns a new client certificate and intermediate certificate chain that allows the client to access file shares by using IPsec Encryption in Transit.   

The {{site.data.keyword.cloud}} file service provides a [mount helper utility](#fs-mount-helper-utility) to automate some of these tasks.

## IBM Cloud File Share Mount Helper utility
{: #fs-mount-helper-utility}

Mount helper is an open source automation tool that configures and establishes secure IPsec communication between customer virtual server instance and the file share. It ensures that the communication between the server instance and zonal file share service is encrypted by configuring the IPsec Transport Mode, obtaining region-specific certificates, and mounting the file share securely.

The utility uses StrongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html) to configure IPsec on the virtual server instance with Linux OS.

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires Mutual SSL. The Mount Helper makes the following calls to generate and configure the certificates.
- Mount helper retrieves the INSTANCE IDENTITY TOKEN:
   ```sh
   curl -iks -X PUT -H "Metadata-Flavor: ibm" https://169.254.169.254/instance_identity/v1/token?version=2023-06-20
   ```
   {: pre}

- With the instance identity token, the Mount helper generates the certificate request with the `csr`.
   ```sh
   curl -X POST "https://169.254.169.254/instance_identity/v1/certificates?version=2023-06-20" -H "Accept:application/json" \ -H "Authorization: Bearer $IAM_TOKEN" \ --data-raw '{ "csr": "$csr" }'
   ```
   {: pre}

The mount helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use the utility for encrypted or unencrypted connections. For encrypted connections, the mount helper uses the instance metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/apidocs/vpc/latest#create-instance).

For more information, see the [readme file](https://github.com/IBM/vpc-file-storage-mount-helper).

### Requirements and restrictions
{: #fs-eit-requirements}

* [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the virtual server instance.
* The file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which virtual server instances can mount the share. 
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), to create an IP address within the VPC that represents a virtual NFS server.
* Data encryption in transit must be enabled either in the UI or with the API.
* The same certificates cannot be used across multiple regions.
* The mount helper is supported for mounting on Linux hosts only.

### Procedure
{: #fs-eit-procedure}

1. Download the mount helper package:

   ```bash
   curl -L https://github.com/IBM/vpc-file-storage-mount-helper/releases/download/latest/mount.ibmshare-0.0.1.tar.gz --output mh.gz && tar -xf mh.gz
   ```
   {: pre}

2. Use the following script to install the mount helper and all the dependencies.

   ```bash
   ./install.sh region=aaa
   ```
   {: pre}

   The installation script accepts the command-line argument `region`. Example regions are `dal`, `fra`, `lon`, `osa`, `sao`, `syd`, `tok`, `tor`, `wdc`. This argument is used to copy region-specific root CA cert to the StrongSwan certificates location. If no region is specified, then the utility copies all the root CA certs.

3. Update the configuration file `/etc/ibmcloud/share.conf` with region information and with the peer certificate expiration time that you want. By default, certificates last 1 hour and new certificates are fetched every 45 minutes. However, you can modify the `certificate_duration_seconds` option to have a value between 5 minutes and 1 hour.

4. Run the `mount` command with the following syntax.

   ```bash
   mount -t ibmshare -o secure=true  <share-ip>:/<mount-point> /mnt/share-test

   ```
   {: pre}

   See the following example 
   ```bash 
   mount -t ibmshare -o secure=true 10.240.0.5:/acdaecff_a291_41bd_87c1_0dde05135d59 /mnt/nfs -v
   ```
   {: pre}
   
   The `ibmshare` in the command is a script that creates the certificate signing request(csr) and calls the Metadata service to get the intermediate cert and end peer certificate. It parses the mount command-line arguments and creates `/etc/swanctl/conf.d/type_ibmshare_.conf`. The strongSwan service uses this configuration file to establish the IPsec connection. Then, the script loads the IPsec connection and calls the NFS `mount` command. 
   
   You can also include options such as `vers=4.1`and `sec=sys`.

## Next steps
{: #next-steps-eit}

Learn about [Managing file shares](/docs/vpc?topic=vpc-file-storage-managing).
