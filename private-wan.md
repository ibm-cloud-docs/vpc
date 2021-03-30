---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-25"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:beta: .beta}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}


# Private network connectivity within {{site.data.keyword.cloud_notm}}
{: #private-network-connectivity}

For technical, cost, regulatory and/or compliance reasons, you might require all connectivity to, within, and between your VPC infrastructures to be isolated from all public backbone and the internet.

Example use cases of private connectivity include:

1. Instance to instance within an Availability Zone (AZ)
1. Instance to instance between AZs within a single region
1. Instance to instances, region to region
1. Connection from a remote network (on-premises) to {{site.data.keyword.cloud}}


{{site.data.keyword.cloud_notm}} supports these topologies with:

* A private backbone for all connectivity between resources deployed in your VPC virtual networks with an AZ and between AZs (this backbone is separate from the public backbone for connectivity using public addressing). IP addressing on the private backbone is non-internet routable, or not announced toward the public backbone or the internet. The private backbone is owned, managed, and operated exclusively by IBM Cloud.
* IBM Cloud Transit Gateway, which meets use cases 1 through 3 by using this private backbone exclusively for connectivity between your virtual networks within a single region, across multiple regions, and to your IBM Cloud classic workloads.
* IBM Direct Link (2.0), which meets use case 4 by providing the ability to attach your remote networks (on-premises) to your virtual
networks within IBM Cloud VPC or IBM Cloud Classic via this same private backbone.

By default, a VPC is private, and remains private until it is configured to enable public connectivity. For instance, a VPC might be connected to a public gateway or associated with floating IPs.
{: note}

## Architecture
{: #private-network-arch}

### Single-region, multi-zone VPC virtual network
{: #single-region-multi-zone}

![Architecture of a single-region, multi-zone VPC virtual network](images/private-network-connectivity.png "Architecture of a single region, multi-zone VPC virtual network")

### Multi-region, multi-zone VPC virtual networks
{: #multi-region-multi-zone}

![Architecture of multi-region, multi-zone VPC virtual networks](images/private-network-connectivity2.png "Architecture of multi-region, multi-zone VPC virtual networks")

### Connection from a remote network (on-premises) to {{site.data.keyword.cloud_notm}}
{: #direct-link-use-case}

![Direct Link on-premises interconnect use case](images/direct-link-dedicated.png "Direct Link on-premises interconnect use case")

See [IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about) and [IBM Cloud Direct Link (2.0)](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) use case documentation for detailed private connectivity use cases and topologies. For information about native, private connectivity to {{site.data.keyword.cloud_notm}} services, see [Endpoints available](/docs/vpc?topic=vpc-service-endpoints-for-vpc).
