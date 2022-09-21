---

copyright:
  years: 2022
lastupdated: "2022-09-27"

keywords:  network overview of s390x bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Networking overview for LinuxONE Bare Metal Servers
{: #s390x-bare-metal-servers-network}

The following information is an overview of the networking features of LinuxONE Bare Metal Servers and how it supports VPC networking. It is recommended that you go through this information before you build a s390x arechitecture based bare metal server. s390x bare metal servers on VPC provides full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.
{: shortdesc}

The following information is for customers with basic network knowledge of {{site.data.keyword.cloud}} VPC and LinuxONE. If you aren't familiar with VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc). If you are not familiar with HiperSockets, see [IBM HiperSockets Implementation Guide ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.redbooks.ibm.com/redbooks/pdfs/sg246816.pdf).

s390x bare metal servers on VPC provides full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.

Based on the profile you select for a s390x bare metal server, the supported bandwidth can be 2 or 10 Gbps. The bandwidth is shared by the network interfaces that are created on the s390x bare metal server. For more information, see [s390x bare metal server profiles](/dcos/vpc?topic=vpc-s390x-bare-metal-servers-profile).

For more information about managing network interfaces, see the s390x specific information on [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).

## s390x bare metal server network interfaces
{: #bare-metal-servers-nics-intro}

You can create only one type of network interface on a s390x bare metal server:

| Network interface | Description |
|-----|-----|
| HiperSockets (znetworkInterface) | A hardware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory). |
{: caption="Table 1. Types of network interfaces for LinuxONE bare metal servers" caption-side="bottom"}

### Characteristics of the HiperSocket interface
{: #bare-metal-servers-interface-characteristics}

The following list highlights characteristics of the HiperSocket interface.

* A firmware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory).

* Provides secure data flows between LPARs and high availability, if it doesn't have a network attachment dependency or exposure to adapter failures.

* HiperSockets has no external components, so it provides a secure connection. For security purposes, servers can be connected to different HiperSockets. All security features, like IPsec or IP filtering, are available for HiperSockets interfaces as they are with other Internet Protocol network interfaces.

* HiperSockets looks like any other TCP/IP interface; therefore, it is transparent to applications and supported operating systems.

* You can attach security groups to HiperSockets interfaces to manage incoming and outgoing traffic to the network interface.

### Limitations of network interfaces on s390x bare metal servers
{: #nic-limits}

* You can attach maximum two network interfaces to each s390x bare metal server based on the profile you choose.
* PCI and VLAN are not supported.
* To add or remove HiperSockets interfaces, the s390x bare metal server must be **STOPPED**. 

