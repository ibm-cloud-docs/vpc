---

copyright:
  years: 2022, 2023
lastupdated: "2023-04-11"

keywords:  network overview of s390x bare metal servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Networking overview for s390x bare metal servers
{: #s390x-bare-metal-servers-network}

s390x Bare Metal Servers for VPC is available for customers with special approval to preview this service in the Washington DC (us-east), London (eu-gb), Tokyo (jp-tok), Toronto (ca-tor), and São Paulo (br-sao) regions.
{: preview}


The following information is an overview of the networking features of s390x bare metal servers and how it supports VPC networking. It is recommended that you go through this information before you build an s390x bare metal server. s390x bare metal servers on VPC provide full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.
{: shortdesc}

The following information is for users that have basic network knowledge of {{site.data.keyword.cloud}} VPC and LinuxONE. If you aren't familiar with VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc). If you are not familiar with HiperSockets, see [IBM HiperSockets Implementation Guide](https://www.redbooks.ibm.com/redbooks/pdfs/sg246816.pdf){: external}.

s390x bare metal servers provide full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.

Based on the profile that you select, the supported bandwidth can be 2 or 10 Gbps. The bandwidth is shared by the network interfaces. For more information, see [s390x bare metal server profiles](/docs/vpc?topic=vpc-s390x-bare-metal-servers-profile).

For more information about managing network interfaces, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).

## s390x bare metal server network interfaces
{: #bare-metal-servers-nics-intro-s390x}

You can create only one type of network interface on an s390x bare metal server:

| Network interface | Description |
|-----|-----|
| HiperSockets (znetworkInterface) | A hardware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory). |
{: caption="Table 1. Types of network interfaces for s390x bare metal servers" caption-side="bottom"}

### Characteristics of the HiperSocket interface
{: #bare-metal-servers-interface-characteristics-s390x}

The following information highlights characteristics of the HiperSocket interface.

* A firmware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory).

* Provides secure data flows between LPARs and high availability if it doesn't have a network attachment dependency or exposure to adapter failures.

* HiperSockets has no external components, so it provides a secure connection. For security purposes, servers can connect to different HiperSockets. All security features, like IPsec or IP filtering, are available for HiperSockets interfaces as they are with other Internet Protocol network interfaces.

* HiperSockets look like any other TCP/IP interface; therefore, it is transparent to applications and supported operating systems.

* You can attach security groups to HiperSockets interfaces to manage incoming and outgoing traffic to the network interface.

### Limitations of network interfaces on s390x bare metal servers
{: #nic-limits-s390x}

Keep the following limitation in mind when you create a network interface.

* You can attach maximum two network interfaces to each s390x bare metal server based on the profile you choose.
* PCI and VLAN aren't supported.
* To add or remove HiperSockets interfaces, the server must be **STOPPED**.
