---

copyright:
  years: 2023, 2026
lastupdated: "2026-08-27"

keywords: file share, file storage, encryption in transit, Mount Helper, IPsec, secure connection, mount share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Encryption in transit - Securing mount connections between file share and compute host
{: #file-storage-vpc-eit}

Enable encryption in transit for {{site.data.keyword.filestorage_vpc_short}} by using IPsec for zonal shares or TLS 1.2+ for regional shares with VNI-based mount targets.
{: shortdesc}

You can enable secure end-to-end encryption of your data when you use zonal and regional file shares with security-group-based access control mode and mount targets with virtual network interfaces. When such a mount target is attached and the share is mounted on a host, the VNI checks the security group policy to ensure that only authorized servers can access the share.

If you choose to use Encryption-in-transit, you need to balance your requirements between performance and enhanced security. Encrypting data in transit can have some performance impact due to the processing that is needed to encrypt and decrypt the data at the endpoints. The impact depends on the workload characteristics. Workloads that perform synchronous writes or bypass VSI caching, such as databases, might have a substantial performance impact when EIT is enabled. To determine EIT’s performance impact, benchmark your workload with and without EIT.

Even without EIT, the data moves through a secure data center network. For more information about network security, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc) and [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

{{site.data.keyword.filestorage_vpc_short}} is considered to be a Financial Services Validated service only when encryption-in-transit is enabled. For more information, see [what is a Financial Services Validated service](/docs/framework-financial-services?topic=framework-financial-services-faqs-framework#financial-services-validated){: external}.
{: important}

IBM Cloud provides two options for in-transit encryption for file shares.
- You can establish an encrypted mount connection between the compute host and a zonal file share by using the Internet Security Protocol (IPsec) security profile and X.509 certificate. For more information, see [Encryption in transit - IPsec encryption](/docs/vpc?topic=vpc-file-storage-vpc-eit-ipsec). For information about a known issue with IPsec-configured mounts, see [Potential mount disruption for IPsec-configured zonal file shares during maintenance](/docs/vpc?topic=vpc-storage-known-issues#ipsec-connection-delays).
- You can establish an encrypted mount connection between the compute host and a regional file share by creating a stunnel connection (TLS 1.2+) between the client virtual server instance and NFS server. For more information, see [Encryption in transit - TLS encryption](/docs/vpc?topic=vpc-file-storage-vpc-eit-tls).
