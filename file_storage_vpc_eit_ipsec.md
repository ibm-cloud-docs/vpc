---

copyright:
  years: 2023, 2025
lastupdated: "2025-11-20"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing encryption in transit for zonal file shares
{: #file-storage-vpc-eit-ipsec}

You can establish an encrypted mount connection between the virtual server instance and a zonal file share by using the Internet Security Protocol (IPsec) security profile and X.509 certificate. By enabling encryption in transit, you create secure end-to-end encryption for your data.
{: shortdesc}

IPsec is a group of protocols that together set up encrypted connections between devices. It helps keep data sent over public networks secure. IPsec Encrypts IP packets, and authenticates the source where the packets come from. To configure IPsec on your virtual server instance, you can use [strongSwan](https://www.strongswan.org/){: external}, which is an open source IPsec-based VPN solution. For more information about how strongSwan works, see [Introduction to strongSwan](https://docs.strongswan.org/docs/5.9/howtos/introduction.html){: external} and [IPsec Protocol](https://docs.strongswan.org/docs/5.9/howtos/ipsecProtocol.html){: external}, too.

The IPsec connection requires that you have an X.509 certificate for authentication. X.509 is an international standard format for public key certificates, digital documents that securely associate cryptographic key pairs with identities such as websites, individuals, or organizations. The metadata service is used to create the certificates.

A Certificate Signing Request (CSR) is a block of encoded texts that are forwarded to a certificate authority (CA) when users apply for a certificate. CSR is created on the server where the certificate is to be installed. CSR includes information such as domain name, organization name, locality, and country. The request also contains the public key, which is associated with the certificate that is generated, and the private key. The CA uses only the public key when the certificate is created. The private key must be saved and kept secret. As the private key is part of the key pair with the public key, and the certificate does not work if the private key is lost.

## Before you begin configuring encryption in transit with IPsec
{: #file-storage-eit-ipsec-prereq}

To use the feature, the following requirements need to be met:
- The file share must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#dp2-profile) and be configured with [Security Group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets). 
- The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about). The virtual server instance and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups). For more information, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
- Data encryption in transit must be enabled. In the console, you can toggle encryption in transit on when you create the mount target. The API `transit_encryption` property accepts the `ipsec` value to enable the feature.
- The metadata service must be enabled on the compute host. For more information, see [Metadata service on virtual server instances](/docs/vpc?topic=vpc-imd-about) and [Metadata service on bare metal servers](/docs/vpc?topic=vpc-bare-metal-server-metadata-about).

The {{site.data.keyword.cloud}} file service provides a [Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-utility) to automate the following tasks that are performed on the compute host.
{: fast-path}

If you want to connect a file share to instances that are running in different VPCs in a zone, you can create multiple mount targets. You can create one mount target for each VPC.

## Obtaining the instance identity certificate
{: #file-storage-eit-ipsec-cert-manual}

Obtain the X.509 certificates that are needed for authentication. The same certificates cannot be used across multiple regions.

1. The following command generates a *Certificate Signing Request* (CSR) and *RSA Key Pair* by using openssl.
   ```sh
   openssl req -sha256 -newkey rsa:4096 -subj '/C=US' -out ./sslcert.csr -keyout file.key -nodes
   ```
   {: pre}

   When you run the command, replace the country code `US` with your two-digit country code in `'/C=US'`.

   OpenSSL is an open source command-line toolkit that you can use to work with X.509 certificates, certificate signing requests (CSRs), and cryptographic keys. For more information, see [OpenSSL Documentation](https://docs.openssl.org/){: external}.
   {: note}

   If you're using a different software to create the CSR, you might be prompted to enter information about your location. Your location information can include country code (C), state (ST), locality (L), your organization name (O), and organization unit (OU). Any one of these naming attributes can be used. Any other naming attributes, such as common name, are rejected. CSRs with Common Name specified are rejected because when you make the request, the system automatically applies instance ID values to the subject Common Name for the instance identity certificates. CSRs with extensions are also rejected.
   {: important}

2. Format the csr before you make an API call to the metadata service by using the following command.
   ```sh
   awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' sslcert.csr
   ```
   {: pre}

3. Then, use the metadata service on the [virtual server instance](/docs/vpc?topic=vpc-imd-identity-operations#imd-json-token) or the [bare metal server](/docs/vpc?topic=vpc-bare-metal-server-metadata-about) to create a client certificate. 
   1. Make a `PUT /instance_identity/v1/token` (virtual server instance) or `PUT /identity/v1/tokens` (bare metal server) request to get a token from the VPC identity service to be used for subsequent calls. For more information, see the following topics:
      - [Acquiring an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations&interface=api#imd-json-token).
      - [Acquiring a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-json-token-bare-metal).
   1. Use the identity token to create an identity certificate. Make a `POST /instance_identity/v1/certificates` request for a virtual server instance or a `POST /identity/v1/certificates` for a bare metal server. Specify the identity token in the HTTP Authorization header, plus a Certificate Signing Request (as `csr` property) and a validity duration (as `expires_in` property). The call returns a new client certificate and intermediate certificate chain that allows the client to access file shares by using IPsec Encryption in Transit. For more information, see the following topics:
      - [Generating an instance identity certificate by using an instance identity access token](/docs/vpc?topic=vpc-imd-identity-operations&interface=api#imd-acquire-certificate).
      - [Generating a bare metal server identity certificate by using a bare metal server identity access token](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-acquire-certificate-bare-metal).
   1. Copy the API response output, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines, and save it to a file with a recognizable name, such as `ca-cert.pem`. Make sure that the file you create has the `.pem` extension.
   
4. Copy the instance identity certificate in the `/etc/ipsec.d/cacerts` directory.
   ```sh
   sudo cp /tmp/ca-cert.pem /etc/ipsec.d/cacerts
   ```
   {: pre}

### Configuring the host and mounting the share
{: #file-storage-eit-ipsec-manual-host-setup}

1. [Install](https://docs.strongswan.org/docs/latest/install/install.html){: external} and configure the strongSwan client. You must configure [IPsec Transport Mode](https://docs.strongswan.org/docs/latest/howtos/ipsecProtocol.html#_ipsec_transport_mode){: external} for the mount target address. 

1. Make sure that you install the required plug-ins (`libcharon-extra-plugins`) for authentication, and update the configuration files with the location of the instance identity certificate.

1. Establish a secure connection by starting the strongSwan client.

1. Mount your file share.
   * [Mounting file shares on Red Hat Linux](/docs/vpc?topic=vpc-file-storage-mount-RHEL).
   * [Mounting file shares in CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos).
   * [Mounting file shares on Ubuntu](/docs/vpc?topic=vpc-file-storage-mount-ubuntu).
