---

copyright:
  years: 2018, 2019
lastupdated: "2019-10-14"

keywords: vpc, limitations, restrictions

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Limitations
{: #limitations}

Limitations might change as capabilities are added, so feel free to check back from time to time.
{:shortdesc}

## General restrictions
{: #general-restrictions}

* The following features are not supported, including all properties associated with these features:
  * Network ACLs (access control lists)
  * Shares
  * Load balancers
  * VPNs (virtual private networks)

* The following concepts are not supported:
  * IPV6
  * Secondary IP addresses
  * CRNs (Exception: Volumes include CRNs via RSOS)

* Virtual server instance name change: If you update the name of a virtual server, the name change may not appear consistently in different areas of the {{site.data.keyword.cloud_notm}} console. For example, the virtual server name change might not be reflected in the {{site.data.keyword.cloud_notm}} console, or on the billing invoice, yet it appears correctly in the user's list of running instances.


## Network restrictions
{: #network-restrictions}

* Multi-zone regions: 
  * A security group can be configured in a single zone only. 
  * A security group can’t reference another security group in a different zone in the same region.

* Bring Your Own IP:
  * Address prefixes must be within one of the "private" address ranges defined in RFC1918.
  * Address prefixes can't be configured in the IBM Cloud console.
   
* Network billing is currently disabled. 

* Multiple Virtual Network Interface Controllers: Only one Virtual Network Interface Controller is allowed for each virtual server instance. Currently, only the primary Virtual Network Interface Controller (VNIC) Ethernet 0 (eth0) works for a virtual server.


## Compute restrictions
{: #compute-restrictions}

* The following images are not supported:
  * Red Hat Enterprise Linux
  * Windows
* Every profile has a network performance value of 2 Gbps per vCPU, with a cap of 80 Gbps. 
* Each network interface has a network performance cap of 16 Gbps. <!-- You might need to attach multiple network interfaces to your virtual server instance to optimize network performance. -->

* API limitations:
  * List APIs don't support pagination. 
  * GET /instances doesn't support the query parameter `network_interfaces.subnet.id`.
* Start/Stop actions are not registered under virtual server instance activity in the UI.
* Activity Tracker logs (request logs and resource lifecycle event logs) are not available.
* Updating the profile of a created instance is not supported.

## Block storage, secondary storage restrictions
{: #storage-restrictions}

* Customer-managed encryption is not supported.
