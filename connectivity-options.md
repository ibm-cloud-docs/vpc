---

copyright:
  years: 2020
lastupdated: "2020-10-26"

keywords: vpc, vpc network, flow, connections, connectivity, scenarios, hosts

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Understanding connectivity options for the VPC environment
{: #connectivity-options}

The following table provides information on VPC network flows and connections for various scenarios you might encounter.

| **Use Case** | **From** | **To** | **Connection through** | **Connection method** |
|-|-|-|-|-|
| Local networks | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Subnet 1, Zone 1 | Private backbone | Automatic, through VPC routing table |
| Local networks | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Subnet 2, Zone 2 | Public internet | VPN gateway with VPN Connection |
| Multi-region networks | Virtual Server (VPC)<br /><br />**Location:** Region A, VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)<br /><br />**Locations:** Region B, VPC 2, Subnet 1, Zone 3 <br /><br />Region C, VPC 3, Subnet 1, Zone 1 | Private backbone | Transit gateway |
| Local networks | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | Bare Metal<br /><br />**Location:** Classic Infrastructure data center 1 | Public internet | IPSec VPN |
| Local networks | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | Bare Metal<br /><br />**Location:** Classic Infrastructure data center 1 | Private backbone | VRF-enabled account |
| Multi-region networks | Virtual Server (VPC)<br /><br />**Location:** Region A, VPC 1, Zone 1, Subnet 1 | Bare Metal<br /><br />**Locations:** Region B, Classic Infrastructure data center 1<br /><br />Region C, Classic Infrastructure data center 1 | Private backbone | Transit gateway |
| Cloud-to-intranet (Dev/Test) | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | On-Premise network | Public internet | VPN gateway with VPN Connection |
| Cloud-to-intranet (Production) | Virtual Server (VPC)<br /><br />**Location:** VPC 1, Zone 1, Subnet 1 | On-Premise network | Private backbone | Direct Link 2.0 |
