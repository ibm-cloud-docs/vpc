---

copyright:
  years: 2021
lastupdated: "2021-10-11"

keywords: bare metal server storage, storage, bare metal storage

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Storage overview for Bare Metal Servers for VPC (Beta)
{: #bare-metal-servers-storage}

Bare Metal Servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access.
{: beta}

All profiles of Bare Metal Servers for VPC provide one 0.96 TB SATA M.2 mirrored SSD as the boot disk. Profile `bx2d-metal-192x768` provides an extra set of NVMe (Non-Volatile Memory Express) U.2 solid-state drives (SSD) as secondary local storage. NVMe SSDs provides fast and affordable storage to support options such as VMware vSAN, or customer-managed RAID. 
{: shortdesc}

Storage for Bare Metal Servers for VPC is unmanaged. You are responsible for encryption and backing up your data.
{: important}
<!--The total size of the NVMe SSD set varies depending on the profile you select. The NVMe drives are empty by default.-->

The bare metal servers support Virtual RAID on CPU (VROC) solution on the NVMe drives. You can enable VROC as needed.

The following network-attached storages are not supported:
* Block Storage for VPC
* File Storage for VPC

  
