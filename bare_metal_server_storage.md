---

copyright:
  years: 2021
lastupdated: "2021-05-19"

keywords: bare metal servers, storage, 

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

# Storage of Bare Metal Servers for VPC (beta) - draft
{: #bare-metal-servers-storage}

All profiles of Bare Metal Servers for VPC provide one 0.96 TB SATA M.2 mirrored SSD as the bare metal server's boot disk. Profile `bx2d-metal-192x768` provide an extra set of NVMe (Non-Volatile Memory Express) U.2 solid-state drives (SSD) as secondary local storage. NVMe SSDs provide fast and affordable storage to support use cases such as VMware vSAN, or customer-managed RAID. 

<!--The total size of the NVMe SSD set varies depending on the profile you select. The NVMe drives are empty by default.-->

The bare metal servers support Virtual RAID on CPU (VROC) solution on the NVMe drives. You can enable this as needed.

Bare Metal Servers for VPC provides unmanaged storage solution. You are responsible for managing (such as backing up, encrypting) the storage.
{: important}

**Note:** The following network-attached storages are not supported for now:

  * Block Storage for VPC
  * File Storage for VPC
  
