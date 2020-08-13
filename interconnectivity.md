---

copyright:
  years: 2020
lastupdated: "2020-08-12"

keywords: vpc peering, vpc interconnectivity, vpc direct link, vpc transit gateway, interconnect, hybrid cloud, multicloud

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

# Interconnect your VPC using {{site.data.keyword.cloud_notm}} offerings
{: #interconnectivity}

Given that VPCs are regional constructs, the following questions quickly arise:
* How can I interconnect my VPCs with my on-premises network?
* How can I interconnect my VPCs?

## Interconnecting with on-premises networks
{: #interconnectivity-onprem}

IBM has the following offerings that can interconnect a VPC with an on-premises network:

   * **{{site.data.keyword.dl_full_notm}} Dedicated** provides low-latency, high-throughput connections between {{site.data.keyword.cloud_notm}} VPC networks directly to a service provider-managed WAN, or a client-managed cloud backbone. Optimize egress traffic from your VPC network and reduce your egress costs. If you can’t connect at an {{site.data.keyword.cloud_notm}} data center, or don’t need more than 5 Gbps of bandwidth on a Virtual Network Connection, you can use {{site.data.keyword.dl_full_notm}} Connect to connect to {{site.data.keyword.cloud_notm}} through a supported service provider.

      With {{site.data.keyword.dl_full_notm}} Global Routing capabilities, you get connectivity to all IBM Cloud regions worldwide from a single {{site.data.keyword.dl_full_notm}} connection. You can also take advantage of {{site.data.keyword.dl_full_notm}} service provider partners to establish more secure hybrid connections for your workloads across the globe. Easily provision multiple connections as more capacity is needed.    

   ![Sample Direct Link on-premises interconnect use case](images/direct-link-dedicated.png "Example Direct Link on-premises interconnect use case")

   * **{{site.data.keyword.dl_full_notm}} Connect** provides connectivity between your on-premises and {{site.data.keyword.cloud_notm}} VPC networks through a supported service provider. A service provider connection is useful if your data center is in a physical location that can't reach a dedicated colocation facility, or if your data needs don't warrant a 5 Gbps+ connection. Connect service providers  are often used to facilitate multicloud connectivity (public clouds from multiple vendors) via their network. Connect service providers offer Layer-2 connectivity, Layer-3 connectivity, or both. Work with your service provider to understand their offerings and requirements.

To access the ordering pages for these offerings, select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) on the upper left of the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){:external}, then select **Interconnectivity**.
{: note}

    * **VPN for VPC** can securely connect your virtual private cloud to another private network. You can use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC. See [Connecting to your on-premises network using a VPN gateway](/docs/vpc?topic=vpc-vpn-onprem-example) for details.

## Interconnecting VPCs
{: #interconnecting-vpcs}

**{{site.data.keyword.tg_full_notm}}** provisions and defines connections between resources on the {{site.data.keyword.cloud_notm}} network, providing private interconnectivity between {{site.data.keyword.cloud_notm}} data centers worldwide. {{site.data.keyword.tg_full_notm}} provides a central hub for connectivity, making it easier to provision and manage your networks. With {{site.data.keyword.tg_full_notm}}, you can create a single transit gateway or multiple transit gateways to connect {{site.data.keyword.cloud_notm}} VPCs. You can also connect your {{site.data.keyword.cloud_notm}} classic infrastructure to a transit gateway to provide seamless communication with classic infrastructure resources. Any new resource that you connect to a transit gateway is automatically made available to every other resource connected to it. All data remains within the private {{site.data.keyword.cloud_notm}} backbone and is optimized for performance.

![Sample Transit Gateway use case](images/TGW_Multi-Multi.png "Sample Transit Gateway use case")

## Benefits of using these {{site.data.keyword.cloud_notm}} options
{: #interconnectivity-benefits}

Benefits of these interconnectivity offerings include:

* Traffic between your on-premises network and your VPC network doesn't traverse the public internet. Traffic traverses a dedicated connection, or through a service provider with a dedicated connection.
* By bypassing the public internet, your traffic takes fewer hops, so there are fewer points of failure where your traffic might get dropped or disrupted.
* Move data to and from your on-premises data centers into the {{site.data.keyword.cloud_notm}} with uninterrupted, consistent network performance while protecting sensitive, business-critical data.
* Save on data transfer to and from servers in every {{site.data.keyword.cloud_notm}} data center across our private network, avoiding bandwidth fees.

## Related links
{: #interconnectivity-related-links}

To learn about these offerings:

* [Get started with {{site.data.keyword.dl_full_notm}} 2.0](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl)
* [Get started with {{site.data.keyword.tg_full_notm}}](/docs/transit-gateway)
* [Get started with VPN for VPC](/docs/vpc?topic=vpc-using-vpn)
