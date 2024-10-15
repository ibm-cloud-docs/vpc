---

copyright:
  years: 2024
lastupdated: "2024-10-15"

keywords: cluster profiles, cluster network, cluster-network, cluster network profile, cluster network profiles, gpu, nvidia, h100, rdma, roce, accelerated, rocev2, accelerated network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# NVIDIA H100 cluster network profile
{: #cluster-network-h100-profile}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the gx3d-160x1792x8h100 virtual server instance profiles in the us-east region. 
{: beta}

The H100 cluster network profile provides isolated networks for [Hopper HGX H100 instances](/docs/vpc?topic=vpc-accelerated-profile-family#hopper-hgx-profiles) running workloads that require high-bandwidth, low-latency interconnectivity, such as AI training and large-scale simulations.
{: shortdesc}

## Overview
{: #cluster-network-h100-profile-overview}

The H100 cluster network profile supports Remote Direct Memory Access (RDMA) over the Convergent Ethernet v2 (RoCEv2) network protocol for increased throughput, reduced latency, and improved system performance.

The supported instance profile is the [Hopper HGX H100 instance profile](/docs/vpc?topic=vpc-accelerated-profile-family#hopper-hgx-profiles).

## Availability
{: #cluster-network-h100-profile-availability}

Currently, use of the H100 cluster network profile is available to select customers in certain regions/zones: 

| Region                    | Zone          | Cluster network |
| ------------------------  | ------------- | --------------- |
| Dallas (`us-south`)       | Not Available | No              |
| Washington DC (`us-east`) | us-east-3     | Yes             |
| Toronto (`ca-tor`)        | Not Available | No              |
| Sao Paulo (`br-sao`)      | Not Available | No              |
| Frankfurt (`eu-de`)       | eu-de-2       | Yes             |
| London (`eu-gb`)          | Not Available | No              |
| Madrid (`eu-es`)          | Not Available | No              |
| Sydney (`au-syd`)         | Not Available | No              |
| Tokyo (`jp-tok`)          | Not Available | No              |
| Osaka (`jp-osa`)          | Not Available | No              |
{: caption="Supported regions and zones" caption-side="bottom"}

## Capabilities and restrictions:
{: #cluster-network-h100-profile-capabilities}

The cluster network H100 profile has the following capabilities and restrictions:

- Type: Dedicated
- Bandwidth: 3.2 Tbps (8x 400 Gbps)
- Custom routing tables: No
- Dynamic route servers: No
- Endpoint gateways: No
- Floating IPs: No
- Flow logs: No
- LBaaS: No
- NVLink: Yes (900 GB/s)
- Private Path: No
- Public gateway: No
- Reserved IPs: Yes
- Secondary IPs: No
- VPN: No
