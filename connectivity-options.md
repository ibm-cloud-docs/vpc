---

copyright:
  years: 2020
lastupdated: "2020-10-26"

keywords: flow, connections, connectivity, scenarios, hosts

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding connectivity options for the VPC environment
{: #connectivity-options}

The following table provides information on VPC network flows and connections for various scenarios you might encounter.

| **Use Case** | **From** | **To** | **Connection through** | **Connection method** |
|-|-|-|-|-|
| Local networks | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | Private backbone | Automatic, through VPC routing table |
| Local networks | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 2, Subnet 2 | Public internet | VPN gateway with VPN Connection |
| Multi-region networks | Virtual Server (VPC)  \n  \n**Location:** Region A, VPC 1, Zone 1, Subnet 1 | Virtual Server (VPC)  \n  \n**Locations:** Region B, VPC 2, Zone 3, Subnet 1  \n  \nRegion C, VPC 3, Zone 1, Subnet 1 | Private backbone | Transit gateway |
| Local networks | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | Bare Metal  \n  \n**Location:** Classic Infrastructure data center 1 | Public internet | IPSec VPN |
| Local networks | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | Bare Metal  \n  \n**Location:** Classic Infrastructure data center 1 | Private backbone | VRF-enabled account |
| Multi-region networks | Virtual Server (VPC)  \n  \n**Location:** Region A, VPC 1, Zone 1, Subnet 1 | Bare Metal  \n  \n**Locations:** Region B, Classic Infrastructure data center 1  \n  \nRegion C, Classic Infrastructure data center 1 | Private backbone | Transit gateway |
| Cloud-to-intranet (Dev/Test) | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | On-Premise network | Public internet | VPN gateway with VPN Connection |
| Cloud-to-intranet (Production) | Virtual Server (VPC)  \n  \n**Location:** VPC 1, Zone 1, Subnet 1 | On-Premise network | Private backbone | Direct Link |
{: caption="Table 1. Details about VPC network flows and connections" caption-side="bottom"}
