---

copyright:
  years: 2023, 2026
lastupdated: "2026-08-17"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share, stunnel

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Mount Helper for secure file share connections
{: #fs-mount-helper-utility}

Mount Helper automates encryption in transit for {{site.data.keyword.filestorage_vpc_short}}. The utility configures IPsec tunnels for zonal shares and TLS connections for regional shares.
{: shortdesc}

## What is Mount Helper?
{: #fs-mount-helper-overview}

Mount Helper is an open source automation tool that configures and establishes secure communication between your compute resources (virtual server instances or bare metal servers) and {{site.data.keyword.filestorage_vpc_short}}. The utility handles the complexity of setting up encryption in transit, managing certificates, and establishing secure tunnels, so you can focus on using your file shares rather than configuring security infrastructure.

The encryption method that Mount Helper uses is determined by your file share type:
- **IPsec encapsulation** for zonal file shares
- **Stunnel secure connection** for regional file shares

## In-transit encryption types
{: #fs-eit-types}

### IPsec encapsulated connection for zonal shares
{: #fs-eit-ipsec}

The utility uses strongSwan and [`swanctl`](https://docs.strongswan.org/docs/5.9/swanctl/swanctl.html){: external} to configure IPsec on the compute host that's running a Linux OS.

{{site.data.keyword.filestorage_vpc_short}} IPsec connection requires mutual authentication. The Mount Helper retrieves the instance or bare metal server identity token from the Metadata service. Then, it requests the creation of the instance or bare metal server identity certificate by using the identity token.

The Mount Helper makes new certificate requests every 45 minutes, as the lifetime of the certificate is 1 hour. The new certificate is generated before the old certificate expires to ensure seamless connection. The certificates are generated with the shorter life span for security reasons.

You can use Mount Helper to set up encrypted and non-encrypted connections. For encrypted connections, Mount Helper uses the metadata service protocol option that is set to either `http` or `https`. For more information, see the API reference for `metadata_service` option of [instance provisioning](/docs/apis/vpc/latest#create-instance){: external} and [bare metal server provisioning](/docs/apis/vpc/latest#create-bare-metal-server){: external}.

### Stunnel secure connection for regional shares
{: #fs-eit-stunnel}

The Mount Helper utility installs stunnel on the compute host that's running a Linux OS. Stunnel is an application that creates encrypted TLS tunnels between clients and servers for secure communication. In client mode, Stunnel initiates a connection from your virtual server instance or bare metal server to the file share, and tunnels data over a secure connection. Stunnel requires a PEM file, which typically contains a private key and a certificate. When stunnel operates in client mode, it relies on the system-wide SSL/TLS configuration and certificates. It can use the default PEM file that is provided by the Linux distribution rather than generating a custom certificate. The PEM file is often located in `/etc/ssl/private` or `/etc/pki/tls/private` folder.

## Requirements
{: #fs-mount-helper-requirements}

Before you use Mount Helper, confirm that your environment meets the following requirements:

* For setting up a secure connection with a **zonal** file share, the [Metadata service](/docs/vpc?topic=vpc-imd-about) must be enabled on the compute host.
   * If the compute host is a virtual server instance, see [enabling metadata in the console.](/docs/vpc?topic=vpc-imd-configure-service&interface=ui#imd-enable-service-ui){: ui} [enabling metadata from the CLI.](/docs/vpc?topic=vpc-imd-configure-service&interface=cli#imd-metadata-service-enable-cli){: cli} [enabling metadata from the API.](/docs/vpc?topic=vpc-imd-configure-service&interface=api#imd-metadata-service-enable-api){: api}
   * If the compute host is a bare metal server, see [enabling metadata in the console.](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=ui#metadata-enable-service-ui-bare-metal){: ui} [enabling metadata from the CLI.](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=cli#metadata-service-enable-cli-bare-metal){: cli} [enabling metadata from the API.](/docs/vpc?topic=vpc-configure-metadata-service-bare-metal&interface=api#metadata-service-enable-api-bare-metal){: api}
* The zonal or regional file share must have [security group access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode), so the VPC's security access groups can be used to define which compute host can mount the share.
* Data encryption in transit must be enabled for the mount target.
* The compute host and the mount target must be members of the same [security group](/docs/vpc?topic=vpc-using-security-groups).
* The mount target must be created with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), so it has an IP address within the VPC that represents the virtual NFS server.

Do not add the Mount Helper installer script to your `cloud-init` configuration. The utility requires a running and stable VSI to operate, as the script makes Metadata API requests for host identity verification.
{: note}

## Restrictions and limitations
{: #fs-mount-helper-restrictions}

Be aware of the following restrictions:

* For IPsec encapsulation, the same certificates cannot be used across multiple regions.
* The Mount Helper is supported on Linux hosts only. See the following table for the supported distributions:

   | Supported OS    | Supported OS | Supported OS                 |
   |-----------------|--------------|------------------------------|
   | RHEL_8 [8.4, 8.6, 8.8, 8.10] | RHEL_9 [9.0, 9.2, 9.4] | RHCOS 4.1 and newer |
   | UBUNTU_2204     | UBUNTU_2404  | SAP_SLES_15_SP3_HANA         |
   | CENTOS_STREAM_9 | CENTOS_STREAM_10 | SAP_SLES_15_SP4_HANA       |
   | DEBIAN_11       | DEBIAN_12    | SAP_SLES_15_SP3_APPLICATIONS |
   | ROCKYLINUX_8 [8.9, 8.10] | ROCKYLINUX_9 [9.4, 9.5]  | SAP_SLES_15_SP4_APPLICATIONS |
   {: caption="Supported host OS distributions" caption-side="bottom"}

## Next steps
{: #fs-mount-helper-about-next-steps}

* [Installing the Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-install)
* [Mounting file shares with the Mount Helper utility](/docs/vpc?topic=vpc-fs-mount-helper-mount)
