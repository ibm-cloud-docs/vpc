---

copyright:
  years: 2022
lastupdated: "2022-09-19"

keywords: bare metal server network, bare metal network, nics, hipersocket, network overview

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

# Networking overview for LinuxONE Bare Metal Servers
{: #linuxone-bare-metal-servers-network}


The following information is an overview of the networking features of LinuxONE Bare Metal servers and how it supports VPC networking. It is recommended that you go through this information before you build a LinuxONE bare metal server.
{: shortdesc}


The following information is for customers with basic network knowledge of {{site.data.keyword.cloud}} VPC and LinuxONE. If you aren't familiar with VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc). If you are not familiar with HiperSockets, see [IBM HiperSockets Implementation Guide](https://www.redbooks.ibm.com/redbooks/pdfs/sg246816.pdf){: external}.

LinuxONE Bare Metal Server for VPC provides full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.

Each LinuxONE bare metal server supports 10 Gbps bandwidth. The bandwidth is shared by the network interfaces that are on the LinuxONE bare metal server.

For more information about managing network interfaces, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).

## LinuxONE Bare metal server network interfaces
{: #bare-metal-servers-nics-intro}

You can create one type of network interface on a LinuxONE bare metal server:

| Network interface | Description |
|-----|-----|
| HiperSockets (znetworkInterface) | A hardware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory). |
{: caption="Table 1. Types of network interfaces for LinuxONE bare metal servers" caption-side="bottom"}


### Characteristics of the HiperSocket interface
{: #bare-metal-servers-interface-characteristics}

The following list highlights characteristics of the HiperSocket interface.

* It is a firmware feature that provides high-speed LPAR-to-LPAR communications within the same server (through memory).

* It also provides secure data flows between LPARs and high availability, if there is no network attachment dependency or exposure to adapter failures.

* HiperSockets has no external components. It provides a secure connection. For security purposes, servers can be connected to different HiperSockets. All security features, like IPsec or IP filtering, are available for HiperSockets interfaces as they are with other Internet Protocol network interfaces.

* HiperSockets looks like any other TCP/IP interface; therefore, it is transparent to applications and supported operating systems.

* To add or remove HiperSockets interfaces, the LinuxONE bare metal server must be **STOPPED**.

* You can attach maximum two HiperSockets interfaces to each LinuxONE bare metal server instance.

* You can attach security groups to HiperSockets interfaces to handle incoming and outgoing traffic to the network interface.

## Mapping network concepts between Bare Metal Servers for VPC and LinuxONE
{: #mapping-network-concepts}

LinuxONE Bare Metal servers fully supports LinuxONE networking functions. To set up networks in the LinuxONE environment, you need to first understand the mapping of the networking concepts between your bare metal server and LinuxONE.

The following table describes the mapping of network concepts between LinuxONE Bare Metal servers and LinuxONE.

| Bare Metal Servers for VPC | LinuxONE |
|---------|---------|
| HiperSockets interface | The HiperSockets device is represented by the IQD channel ID (CHPID) and its associated subchannel devices. All LPARs that are configured in HCD/IOCP to use the same IQD CHPID have internal connectivity and the capability to communicate by using HiperSockets. |
{: caption="Table 2. Mapping of network concepts between Bare Metal Servers for VPC and LinuxONE" caption-side="bottom"}


### Configuration tips for HiperSockets network interfaces
{: #nic-config-tips}

HiperSockets are configured and ready for use.


### Limitations of HiperSockets network interfaces
{: #nic-limits}

* You can attach maximum two network interfaces to each LinuxONE Bare Metal server based on the profile you choose.
* VLAN is not supported.


