---

copyright:
  years: 2023, 2024
lastupdated: "2024-06-14"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Encryption in transit - Securing mount connections between file share and virtual server instance
{: #file-storage-vpc-eit}

You can establish an encrypted mount connection between the virtual server instance and storage system by using the Internet Security Protocol (IPsec) security profile and X.509 certificates. By enabling encryption in transit, you create secure end-to-end encryption for your data.
{: shortdesc}

If you choose to use Encryption-in-transit, you need to balance your requirements between performance and enhanced security. Encrypting data in transit can have some performance impact due to the processing that is needed to encrypt and decrypt the data at the endpoints. The impact depends on the workload characteristics. Workloads that perform synchronous writes or bypass VSI caching, such as databases, might have a substantial performance impact when EIT is enabled. To determine EIT’s performance impact, benchmark your workload with and without EIT. 

Even without EIT, the data moves through a secure data center network. For more information about network security, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc) and [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).



## Overview
{: #file-storage-eit-overview}

With this feature, you can enable secure end-to-end encryption of your data when you use file shares with security-group-based access control mode and mount targets with virtual network interfaces. When such a mount target is attached and the share is mounted on a virtual server instance, the virtual network interface checks the security group policy to ensure that only authorized instances can communicate with the share. The traffic between the authorized virtual server instance and the file share can be IPsec encapsulated by the client. 

IPsec is a group of protocols that together set up encrypted connections between devices. It helps keep data sent over public networks secure. IPsec Encrypts IP packets, and authenticates the source where the packets come from. To configure IPsec on your virtual server instance, you can use [strongSwan](https://www.strongswan.org/){: external}, which is an open source IPsec-based VPN solution. For more information about how strongSwan works, see [Introduction to strongSwan](https://docs.strongswan.org/docs/5.9/howtos/introduction.html){: external} and [IPsec Protocol](https://docs.strongswan.org/docs/5.9/howtos/ipsecProtocol.html){: external}, too.

The IPsec connection requires that you have an X.509 certificate for authentication. X.509 is an international standard format for public key certificates, digital documents that securely associate cryptographic key pairs with identities such as websites, individuals, or organizations. The Instance metadata service is used to create the certificates.

A Certificate Signing Request (CSR) is a block of encoded texts that are forwarded to a certificate authority (CA) when users apply for a certificate. CSR is created on the server where the certificate is to be installed. CSR includes information such as domain name, organization name, locality, and country. The request also contains the public key, which is associated with the certificate that is generated, and the private key. The CA uses only the public key when the certificate is created. The private key must be saved and kept secret. As the private key is part of the key pair with the public key, and the certificate does not work if the private key is lost.

Encryption in transit is not supported on {{site.data.keyword.bm_is_short}}.
{: restriction}

## Configuring encryption in transit
{: #file-storage-eit-setup}

To use the feature, the following requirements need to be met:
- The file share must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#dp2-profile) and be configured with [Security Group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets). 
- The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). The virtual server instance and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups). For more information, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
- Data encryption in transit must be enabled. In the UI, you can toggle encryption in transit on when you create the mount target. The API `transit_encryption` property accepts the `user_managed` value to enable the feature.
- [Instance metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled for the virtual server instance.

If you want to connect a file share to instances that are running in different VPCs in a zone, you can create multiple mount targets. You can create one mount target for each VPC.
{: important}

### Configure the host and obtain a certificate
{: #file-storage-eit-manual-setup}

The {{site.data.keyword.cloud}} file service provides a [Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to automate the following tasks that are performed on the virtual server instance.
{: fast-path}

1. When you configure the virtual server instance to access the file share, you must configure [IPsec Transport Mode](https://docs.strongswan.org/docs/6.0/howtos/ipsecProtocol.html#_ipsec_transport_mode){: external} for the mount target address. [Install](https://docs.strongswan.org/docs/5.9/install/install.html){: external} and configure the strongSwan client.
2. Obtain the X.509 certificates that are needed for authentication. The same certificates cannot be used across multiple regions.
   1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl.
      ```sh
      openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
      ```
      {: pre}

      When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.

      OpenSSL is an open source command-line toolkit that you can use to work with X.509 certificates, certificate signing requests (CSRs), and cryptographic keys. For more information, see [OpenSSL Documentation](https://www.openssl.org/docs/){: external}.
      {: note}

      If you're using a different software to create the CSR, you might be prompted to enter information about your location such as country code (C), state (ST), locality (L), your organization name (O), and organization unit (OU). Any one of these naming attributes can be used. Any other naming attributes, such as common name, are rejected. CSRs with Common Name specified are rejected because when you make the request to the Metadata API, the system applies instance ID values to the subject Common Name for the instance identity certificates. CSRs with extensions are also rejected.
      {: important}

   2. Format the csr before you make an API call to the metadata service by using the following command.
      ```sh
      awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
      ```
      {: pre}

3. Then, use the [Instance metadata service](/docs/vpc?topic=vpc-imd-about) to create a client certificate. 
   1. Make a `PUT /instance_identity/v1/token` API request to get a token from metadata service to be used for subsequent calls. For more information, see [Acquiring an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-json-token).
   2. Make a `POST /instance_identity/v1/certificates` request and specify the instance identity token in the HTTP Authorization header, plus a Certificate Signing Request (as `csr` property) and a validity duration (as `expires_in` property). The call returns a new client certificate and intermediate certificate chain that allows the client to access file shares by using IPsec Encryption in Transit. For more information, see [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-acquire-certificate).    
