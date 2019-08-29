---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: vpc, limitations, early access

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

Limitations might be changing during the early access program as we add capabilities, so feel free to check back from time to time.
{:shortdesc}

## General restrictions
{: #general-restrictions}

The following features are not supported, including all properties associated with these features:
* Network ACLs (a fixed default NACL policy will be enforced instead)
* LBaaS
* VPNaaS
* Classic peering (also known as Classic Access)
* Shares

The following concepts are not supported:
* Custom images
* Tags
* IPv6
* Secondary IP addresses
* CRNs (Exception: Volumes include CRNs via RSOS.)


## Network restrictions
{: #network-restrictions}

* Multi-zone regions: 
  * Virtual server instances can communicate over the private network only with instances in the same zone and VPC. Instances can communicate with instances in other zones in the same VPC over the public internet only.
  * A security group can be configured in a single zone only. 
  * A security group can’t reference another security group in a different zone in the same region.

* Bring Your Own IP:
   * Address prefixes must be within one of the "private" address ranges defined in RFC1918.
   * Address prefixes can't be configured in the IBM Cloud console.

* Limited network QoS. Network QoS is policed on TX only. RX is not policed.


## Compute restrictions
{: #compute-restrictions}

* Red Hat Enterprise Linux images are not supported.
* Every profile has a network performance value of 2 Gbps per vCPU, with a cap of 80 Gbps.

* Each network interface has a network performance cap of 16 Gbps.

* API limitations:
  * List APIs don't support pagination. 
  * GET /instances doesn't support the query parameter `network_interfaces.subnet.id`.
* Start/Stop actions are not registered under virtual server instance activity in the UI.
* Activity Tracker logs (request logs and resource lifecycle event logs) are not available.
* Updating the profile of a created instance is not supported.

## Block storage, secondary storage restrictions
{: #storage-restrictions}

* Customer-managed encryption is not supported.
