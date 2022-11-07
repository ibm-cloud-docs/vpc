---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-11"

keywords: s390x bare metal server storage, s390x bare metal storage, linuxone bare metal storage

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Storage overview for s390x bare metal servers
{: #s390x-bare-metal-servers-storage}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east) and São Paulo (br-sao) regions. 
{: preview}

Storage for both boot and data volumes for s390x bare metal servers are provided through Fibre Channel Protocol (FCP) that is connected to IBM FlashSystems 9200.
{: shortdesc}

For more information about IBM FlashSystem 9200 systems, see [SAN and FCP](https://www.ibm.com/docs/en/linux-on-systems?topic=introduction-san-fcp).

s390x bare metal servers don't encrypt the data at rest and data in transit for IBM FlashShstem 9200. You need to use the standard LUKS (Linux Unified Key Setup-on-disk-format) for Linux hard disk encryption.
{: important}

The following network-attached storages aren't supported:

* Block Storage for VPC
* File Storage for VPC
