---

copyright:
  years: 2021,2022
lastupdated: "2022-09-12"

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

# Storage overview for LinuxONE Bare Metal servers
{: #linuxone-bare-metal-servers-storage}

Storage for both boot and data volumes for LinuxONE Bare Metal servers are provided via Fibre Channel Protocol (FCP) connected to IBM FlashSystems 9200.
{: shortdesc}

For more information about IBM FlashSystem 9200 systems, see [SAN and FCP](https://www.ibm.com/docs/en/linux-on-systems?topic=introduction-san-fcp).

LinuxONE Bare Metal Servers do not encrypt the data at rest and data at transit for IBM FlashShstem 9200. You need to use the standard LUKS (Linux Unified Key Setup-on-disk-format) for Linux hard disk encryption.
{: important}

The following network-attached storages are not supported:

* Block Storage for VPC
* File Storage for VPC
