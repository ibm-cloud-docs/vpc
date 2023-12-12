---

copyright:
  years: 2021, 2023
lastupdated: "2023-12-12"

keywords: file share, mount target, virtual network interface, customer-managed encryption, encryption at rest, encryption in transit, file storage, share,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About {{site.data.keyword.filestorage_vpc_short}}
{: #file-storage-vpc-about}

{{site.data.keyword.filestorage_vpc_full}} is a zonal file storage offering that provides NFS-based file storage services. You can create file shares in an availability zone within a region. You can share them with multiple virtual server instances within the same zone or other zones in your region, across multiple VPCs. You can also limit access to a file share to a specific virtual server instance within a VPC and encrypt the data in transit.
{: shortdesc}

## Overview
{: #file-storage-overview}

{{site.data.keyword.filestorage_vpc_short}} provides file shares within the VPC Infrastructure. You can create file shares at the zonal level, for example `us-south-1`. File shares are identified by name and associated with a resource group in your {{site.data.keyword.cloud_notm}} customer account.

You create a file share in a zone and create the mount targets for the share per VPC. You can control how the file share is accessed by specifying the [access mode](#fs-mount-access-mode): VPC-wide access or targeted access for specific instances within a zone. 

You can set up [replication](/docs/vpc?topic=vpc-file-storage-replication) between the source file share and a replica file share. So if an outage at the primary site was to occur, you can fail over to the replica file share.

Data on a file share is encrypted at rest with IBM-managed encryption by default. For added security, you can use your own root keys to protect your file shares with customer-managed keys. When you specify the security group access mode and attach a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to the file share mount target, you can enable encryption of data in transit. For more information, see [File share encryption](#FS-encryption).

You can apply user tags and access management tags to your file shares. Add tags when you create a share or update an existing share with the UI, CLI, API, or Terraform. For more information, see [Tags for file shares](#fs-about-fs-tags).

You can enable context-based restrictions (CBR) for all file share operations. These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure, such as creating a file share. For more information, see [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

{{site.data.keyword.filestorage_vpc_short}} is integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For more information, see [Managing security and compliance](/docs/vpc?topic=vpc-file-storage-managing#fs-vpc-manage-security).

You can [increase the file share size](/docs/vpc?topic=vpc-file-storage-expand-capacity) from its original capacity in GB increments up to 32,000 GB capacity, depending on your file share profile. You can also [increase or decrease file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops) to meet your performance needs. Adjust IOPS by specifying a different IOPS tier profile or different IOPS value withing a custom IOPS band. Operations to increase the capacity or adjust the IOPS cause no outage or lack of access to the storage. Billing is adjusted automatically. You pay for only the capacity and performance that you need. 

### File Storage IOPS profiles
{: #fs-profiles-intro}

When you create a file share in your availability zone, you use the **dp2** profile to specify the total IOPS for the file share based on the share size.

If you have existing file shares that are based on either the IOPS tier profiles or custom IOPS profile; you can update those shares to use the **dp2** profile. You can also change back to an earlier profile. However, you cannot use the earlier profiles when you provision a file share.

All profiles are backed by solid-state drives (SSDs).

For more information, see [{{site.data.keyword.filestorage_vpc_short}} profiles](/docs/vpc?topic=vpc-file-storage-profiles).

### NFS version
{: #fs-nfs-version}

{{site.data.keyword.filestorage_vpc_short}} requires NFS versions v4.1 or higher. When multiple users cooperate and run a series of read and write operations on the file share, data consistency is achieved by locking mechanisms that are native to the NFS protocol. NFS version 4.1 includes support for advisory byte-range file locking. Byte-range locking is used to serialize activity to a range of bytes within a file. As an advisory locking mechanism, it doesn’t prevent access to any application but provides a mechanism for applications to communicate cooperatively through obtaining locks and querying if a lock is held. For more information, see [RFC8881](https://datatracker.ietf.org/doc/html/rfc8881#name-locking-facilities){: external}.

## Mount targets for file shares
{: #fs-share-mount-targets}

To mount a file share on a virtual server instance or to use it in a Kubernetes cluster, you need the NFS mount path. To create an NFS mount path, you need to create a mount target.

A mount target for a file share is a network endpoint. When you create a mount target, an NFS mount path is created for the file share. You use the mount path to attach the file share to virtual server instances or Kubernetes clusters in the same region. Depending on the [access mode](#fs-mount-access-mode) you choose, you can restrict access to a share to a specific instance in the VPC or allow all the virtual server instances to mount the share.

If you want to connect a file share to instances that are running in different VPCs in a zone, you can create multiple mount targets, one mount target for each VPC. After the mount target is created, you can SSH into the virtual server instance and attach the file share.

### Mount target access modes
{: #fs-mount-access-mode}

When you create or update a mount target, you can specify the manner in which you want the mount target to be accessed on the file share. You have two options:

* Use the **security groups access** mode to authorize access to the file share for a specific virtual server instance or instances within a subnet. This option is available to newer file shares based on the `dp2` profile. Communication between an authorized virtual server instance and the file share can optionally be IPsec encapsulated. For more information, see [Encryption in Transit](#fs-eit). Cross-zone mounting is also supported.

* Use the **VPC access** mode to allow access to the file share to a bare metal server or any virtual server instances in the same zone of a VPC. This option is available for all [file share profiles](/docs/vpc?topic=vpc-file-storage-profiles). Cross-zone mounting and encryption of data in transit are not supported for shares with VPC access mode.

### Granular authorization
{: #fs-mount-granular-auth}

When you set the access control mode of a file share to use [security groups](/docs/vpc?topic=vpc-using-security-groups), and create a mount target with a [virtual network interface](/docs/vpc?topic=vpc-vni-about), the VPC Infrastructure creates a file share gateway that provides a 1:1:1 granular authorization for the share. 

The security groups that are associated with the mount target act as a virtual firewall that controls the traffic between the mount target and the Compute host.

You can select a specific security group or use the VPC's default security group. By modifying the rules of the security groups in your VPC, you can restrict access to the file share from one or more specific virtual server instances or bare metal server.

To enable traffic between a virtual server instance and a mount target, you must configure the following rules in the security groups:

- The security groups that you attach to a mount target must allow inbound access for the TCP protocol on the NFS port from all virtual server instances on which you want to mount the file share.
- Each virtual server instance that you want the file share to be mounted on must have a security group that allows outbound access to the mount target on the NFS port.

You can configure your security group in a more dynamic way by allowing all traffic between members of the security group. Then, attach this security group to the network interface of the virtual server instance and the virtual network interface of the mount target. For more information, see [Allow traffic between members of a security group](/docs/vpc?topic=vpc-using-security-groups#sg-use-case-3).

It's also recommended that UDP ports 500 and 4500 are allowed. UDP port 500 is intended for Internet Key Exchange (IKE) to manage encryption keys, and UDP port 4500 is for IPsec NAT-Traversal (NAT-T). A VPN gateway for VPC accepts VPN packets with [UDP Encapsulation of IPsec ESP Packets](https://tools.ietf.org/html/rfc3948){: external} only.

When you create the mount target, you can specify a subnet and reserved IP address for the virtual network interface, or have the service pick an IP address for you in the specified subnet. The mount target must have a VPC private IP address, and the IP address must be in a subnet that is in the same zone as the share. The IP address that is assigned to the mount target cannot be changed later.

When you create the mount target with a virtual network interface, its IP address is determined in either of the following ways:

* By subnet - You specify the subnet and allow the system to choose an IP address from the reserved IP addresses within that subnet. A network interface is created with the selected IP address, and then that network interface is attached to the file share mount target.

* By subnet and IP address - You specify the IP address in the subnet. Then, the network interface is created and attached to the mount target.

When the mount target is attached and the share is mounted, the virtual network interface performs security group policy check to ensure that only authorized virtual server instances can communicate with the share.

For greater security, [enable encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit) for your file share mount targets.

### Cross-zone mount targets
{: #fs-cross-zone-mount}

When you create a mount target for a share with security access group mode, you can attach a virtual network interface with a specific reserved IP in the zone of your file share. By using such a mount target, you can mount a file share from zone A to a virtual server instance in zone B. When the virtual server instance and the file share are in different zones, the performance can be impacted.

Cross-zone mounting is not supported for file shares with VPC-wide access mode. 

## Encryption at rest
{: #FS-encryption}

By default, file shares are encrypted at rest with IBM-managed encryption. 

You can bring your own customer root key (CRK) to the cloud for customer-managed encryption or you can have a key management service (KMS) generate a key for you. You can select the root key when you [create an encrypted file share](/docs/vpc?topic=vpc-file-storage-vpc-encryption). For more information, see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).

After you specified an encryption type for a file share, you can't change it.
{: restriction}

## Encryption in transit
{: #fs-eit}

You can [establish an encrypted mount connection](/docs/vpc?topic=vpc-file-storage-vpc-eit) between the authorized virtual server instance and the storage system by using IPsec. For file shares based on the `dp2` profile, mount targets that are created with a virtual network interface can support the encryption in transit. The mount targets can be for a source or replica share.

If you choose to use Encryption-in-transit, you need to balance your requirements between performance and enhanced security. Encrypting data in transit can have some performance impact due to the processing that is needed to encrypt and decrypt the data at the endpoints. The impact depends on the workload characteristics. Workloads that perform synchronous writes or bypass VSI caching, such as databases, might have a substantial performance impact when EIT is enabled. To determine EIT’s performance impact, benchmark your workload with and without EIT.

Even without EIT, the data is moving through a secure data center network. For more information about network security, see [Security in your VPC](/docs/vpc?topic=vpc-security-in-your-vpc) and [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

Encryption in transit is not supported for {{site.data.keyword.bm_is_short}}.
{: restriction}

## File share replication and failover
{: #fs-repl-failover-overview}

You can create read-only replicas of your file shares in another zone within your VPC, or another zone in a different region if you have multiple VPCs in the same geography. The replica is updated regularly based on the replication schedule that you specify. Using replication is a good way to recover from incidents at the primary site when data becomes inaccessible or applications fail. [Failover](/docs/vpc?topic=vpc-file-storage-failover) to the replica share makes it the new, writeable primary share. For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

For cross-regional replication, you must configure [service to service authorizations](/docs/vpc?topic=vpc-file-s2s-auth) before you create your replica file share.
{: requirement}

## Supplemental IDs and Groups for file shares
{: #FS-supplemental-ids}

When a process runs on Unix and Linux, the operating system identifies a user with a user ID (UID) and a group with a group ID (GID). These IDs determine which system resources a user or group can access. For example, if the file storage user ID is 12345 and its group ID is 6789, then the mount on the host node and in the container must have those same IDs. The container’s main process must match one or both of those IDs to access the file share.

With the API, you can set these attributes for controlling access to your file shares when you create a file share. The API provides an `initial owner` property where you can set the `UID` and `GID` values. Wherever you mount the file share, the root folder where you mount it uses that UID or GID owner. For more information, see [Add supplemental IDs when you create a file share](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-add-supplemental-id-api).

## Tags for file shares
{: #fs-about-fs-tags}

{{site.data.keyword.filestorage_vpc_short}} is enabled for Global Searching and Tagging (GhoST). You can create and apply [user tags](#fs-about-user-tags) and [access management tags](#fs-about-mgt-tags) to file shares to better control and organize your file storage resources across the VPC. User tags can be added from the file service UI, CLI, or API. To apply access management tags to file shares, you must use the GhoST API.

### User tags for file shares
{: #fs-about-user-tags}

You can create new user tags or add existing tags when you provision a new file share or update an existing file share. You can create, view, and manage tags from the UI, CLI, or API, and remove then at any time.

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format. Behind the scenes, the file service sends and receives tags directly to the GhoST service. GhoST stores its key attributes and the array of tags. GhoST also stores user resource information, so you can view, tag, and search for resources that you own.

For more information, see [Add user tags to file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags) and [Working with tags](/docs/account?topic=account-tag&interface=ui).

### Access management tags for file shares
{: #fs-about-mgt-tags}

Access management tags help organize access control by creating flexible resource groupings, enabling your file storage resources to grow without requiring updates to IAM policies.

You can create access management tags and then apply them to new or existing file shares and replica file shares. Use the IAM UI or the Global Search and Tagging API to create the access management tag. Then, from the VPC UI or API, add the tags to a file share. After the tags are added, you can manage access to them using the IAM policies. For more information, see [Add access management tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-access-mgt-tags).

## File Storage data eradication
{: #file-storage-data-eradication}

When you delete a file share, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a file share in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted file storage is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. When you delete a file share, those blocks must be overwritten before that file storage is made available again, either to you or to another customer.

Further, when IBM decommissions a physical drive, the drive is destroyed before their disposal. Decommissioned physical drives are unusable and any data on them is inaccessible.

## Managing security and compliance
{: #fs-vpc-manage-security}

{{site.data.keyword.filestorage_vpc_short}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization. You can set up goals that check whether file shares are encrypted by using customer-managed keys. By using the {{site.data.keyword.compliance_short}} to validate the file service configurations in your account against a profile, you can identify potential issues as they arise.

For more information, see [Monitoring security and compliance posture with VPC](/docs/vpc?topic=vpc-manage-security-compliance#monitor-vpc). For more information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Limitations in this release
{: #fs-limitations}

The following limitations apply to this release of {{site.data.keyword.filestorage_vpc_short}}.

* Previous profiles are not supported when you provision a new file share, which is based on the `dp2` profile. However, earlier version file shares can continue to use existing profiles.
* Restricting file share access to specific virtual server instances and data encryption in transit is available only for shares that are based on the `dp2` profile.
* Windows operating systems are not supported.
* The minimum capacity is 10 GB per file share.
* The maximum capacity is 32,000 GB per file share.
* No data retention policy exists for deleted file shares. You cannot undelete a file share after you delete it.
* Up to 256 hosts per zone per VPC can be concurrently connected to a single file share.
* You can create up to 300 file shares within your VPC.
* A file share cannot be deleted by using a `DELETE /shares/<id>` API request, if an existing mount target is associated with that file share or if replica operations are in progress.
* Only {{site.data.keyword.bm_is_short}} that are provisioned after 31 August 2023 support {{site.data.keyword.filestorage_vpc_short}}.
* Encryption in transit is not supported between {{site.data.keyword.filestorage_vpc_short}} for VPC and {{site.data.keyword.bm_is_short}}.
* A file share cannot be split from its replica by using a `DELETE /shares/<id>/source` API request, if the `lifecycle_state` of the file share is `updating` or if replica operations are in progress.
* Cross-regional replication is supported within the same geography when both source and replica shares belong to the same account. Cross-geography replication is not supported.

## Related information
{: #related-info-file-storage-vpc}

For more information about the {{site.data.keyword.cloud_notm}} VPC, see [Virtual Private Cloud](https://www.ibm.com/cloud/learn/vpc){: external}.

For more information about VPC, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).

## Next steps
{: #file-storage-vpc-next-steps}

* [Plan for creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-planning).
* [Create a file share and mount targets](/docs/vpc?topic=vpc-file-storage-create).
