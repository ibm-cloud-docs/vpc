---

copyright:
  years: 2021
lastupdated: "2021-04-05"

keywords: virtual private cloud, file storage, file share, mount point

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:beta: .beta}

# About File Storage for VPC (beta)
{: #file-storage-vpc-about}

{{site.data.keyword.cloud}} File Storage for {{site.data.keyword.vpc_full}} (VPC) is a zonal file storage offering that provides NFS-based file storage services for VPC customers. File shares are created in an availability zone within a region. File shares can be shared with multiple virtual service instances within the same zone across multiple VPCs.
{:shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{:beta}

## Overview
{: #file-storage-overview}

File Storage for VPC provides a shared file service that operates within the bounds of a VPC. You create a shared file storage in an availability zone. You then mount the shared file system by creating mount targets.

You pay for only the capacity you need. The capacity ranges from 10 GB up to 16 TB for all available profiles.

### File storage IOPS profiles
{: #fs-profiles-intro}

You have two options for creating file shares, by using an IOPS tier profile or by using a custom IOPS profile.

IOPS tiers provide a guaranteed level of performance for your workloads. You can select from three tiers:

* 3 IOPS/GB
* 5 IOPS/GB
* 10 IOPS/GB

Custom IOPS profile let you select the performance you want, by selecting custom allocated I/O operations per second (IOPS).

For more information about these options, see [File storage profiles](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers).

### Zonal file shares
{: #fs-zonal-file-shares}

File Storage for VPC lets you create a file share at the zonal level. This means that file shares are accessible only within the zone in which you created it, for example, `us-south-1`. File shares are identified by name and associated with a resource group in your {{site.data.keyword.cloud_notm}} customer account.

You create a file share using the UI, CLI, or API. You access file shares from virtual service instances or Kubernetes clusters by way of an NFS mount. To create an NFS mount path, you need to create share mount targets.

### Mount targets for file shares
{: #fs-share-mount-targets}

A mount target for a file share is a network endpoint or path. When you create a mount target, an NFS mount path is created for the file share. You use the mount path to attach the shared file system to virtual service instances or Kubernetes clusters in the same VPC and zone.

To mount a share to an instance using the API, you create a mount target by providing a VPC or subnet information. If you want to connect a share to instances running in multiple VPCs in the same zone, you can create multiple mount targets for different VPCs. After creating the mount target (or targets), you can SSH into the virtual server instance and attach the share.

### NFS version
{: #fs-nfs-version}

File Storage for VPC requires NFS versions v4.1 or higher. You can mount file shares on all windows and Linux operating systems that support NFSv4.

## Limitations in the Beta release
{: #fs-limitations-beta}

The following limitations apply to this release of File Storage for VPC:

* File share size cannot be increased after the share is created.
* File share profile cannot be changed after the share is created.
* Granular Host Authorization for virtual server instance level access control in not supported.
* Minimum capacity is 10 GB per share.
* Maximum capacity is 16 TB per share.
* Customer-managed encryption is not available. File share data is encrypted using IBM-managed encryption for data-at-rest.
* Customer-managed data-in-transit is not available. Data-in-transit is encrypted using IBM-managed encryption for data-in-transit.
* There is no data retention policy for deleted file shares. You cannot undelete a file share after you delete it.

## Related information
{: #related-info-file-storage-vpc}

For information about the {{site.data.keyword.cloud_notm}} VPC, see [Virtual Private Cloud](https://www.ibm.com/cloud/learn/vpc){: external}.

For VPC documentation, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).

## Next steps
{: #file-storage-vpc-next-steps}

* [Plan for creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-planning)
* [Create a file share and mount targets](/docs/vpc?topic=vpc-file-storage-create)
