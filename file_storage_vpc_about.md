---

copyright:
  years: 2021
lastupdated: "2021-10-01"

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
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:note: .note}

# About File Storage for VPC
{: #file-storage-vpc-about}

{{site.data.keyword.cloud}} File Storage for {{site.data.keyword.vpc_full}} (VPC) is a zonal file storage offering that provides NFS-based file storage services for VPC customers. A file share is an instance of file storage and created in an availability zone within a region.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, and London regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Overview
{: #file-storage-overview}

File Storage for VPC provides file shares within the bounds of a VPC. You create a file share in a single availability zone. You then a create mount target to mount the file share.

You pay for only the capacity you need. File share capacity ranges from 10 GB up to 32,000 GB for all available profiles.

File share data is encrypted by default using IBM-managed encryption for data-at-rest.

### File storage IOPS profiles
{: #fs-profiles-intro}

You have two options for creating a file share, either using an IOPS tier profile or using a custom IOPS profile.

IOPS tiers provide a guaranteed level of performance for your workloads. You can select from three tiers:

* 3 IOPS/GB
* 5 IOPS/GB
* 10 IOPS/GB

Custom IOPS profile let you select the performance you want, by selecting custom allocated I/O operations per second (IOPS).

For more information about these options, see [File Storage for VPC profiles](/docs/vpc?topic=vpc-file-storage-profiles#fs-tiers).

### Zonal file shares
{: #fs-zonal-file-shares}

File Storage for VPC lets you create a file share at the zonal level. This means that file shares are accessible only within the zone in which you created it, for example, `us-south-1`. File shares are identified by name and associated with a resource group in your {{site.data.keyword.cloud_notm}} customer account.

You create a file share using the UI, CLI, or API. You access file shares from virtual server instances or Kubernetes clusters by way of an NFS mount. To create an NFS mount path, you need to create file share mount targets.

You can also [increase the file share size](/docs/vpc?topic=vpc-file-storage-expand-capacity) from its original capacity in GB increments up to 32,000 GB capacity, depending on your file share profile.

### Mount targets for file shares
{: #fs-share-mount-targets}

A mount target for a file share is a network endpoint or path. When you create a mount target, an NFS mount path is created for the file share. You use the mount path to attach the file share to virtual server instances or Kubernetes clusters in the same VPC and zone.

To mount a file share to an instance using the API, you create a mount target by providing a VPC or subnet information. If you want to connect a file share to instances running in multiple VPCs in the same zone, you can create multiple mount targets for different VPCs. After creating the mount target (or targets), you can SSH into the virtual server instance and attach the file share.

### NFS version
{: #fs-nfs-version}

File Storage for VPC requires NFS versions v4.1 or higher.

## Limitations in this release
{: #fs-limitations}

The following limitations apply to this release of File Storage for VPC:

* File share size cannot be increased after it is created.
* File share profile cannot be changed after the file share is created.
* Granular Host Authorization for VSI level access control in not supported.
* Minimum capacity is 10 GB per file share.
* Maximum capacity is 32,000 GB per file share.
* There is no data retention policy for deleted file shares. You cannot undelete a file share after you delete it.

## File share encryption
{: #FS-encryption}

By default, file share data are encrypted at rest with IBM-managed encryption.

## Related information
{: #related-info-file-storage-vpc}

For information about the {{site.data.keyword.cloud_notm}} VPC, see [Virtual Private Cloud](https://www.ibm.com/cloud/learn/vpc){: external}.

For VPC documentation, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).

## Next steps
{: #file-storage-vpc-next-steps}

* [Plan for creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-planning)
* [Create a file share and mount targets](/docs/vpc?topic=vpc-file-storage-create)
