---

copyright:
  years: 2024
lastupdated: "2024-11-12"

keywords: cluster profiles, cluster network, cluster-network, cluster network profile, cluster network profiles, gpu, nvidia, h100, rdma, roce, accelerated, rocev2, accelerated network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# NVIDIA H100 cluster network profile
{: #cluster-network-h100-profile}

Cluster Networks for VPC is available for select customers only. Contact IBM Support if you are interested in using this functionality.
{: preview}

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

## Tested NICL configuration
{: #performance-recs}

The following information provides tested NCCL tunings for an H100 VM profile with an 8-subnet cluster network. All testing was done on NCCL version 2.22.3. For more information, refer to the [NVIDIA Documentation](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/overview.html).

```ssh
export NCCL_IB_PCI_RELAXED_ORDERING=2
export NCCL_IB_QPS_PER_CONNECTION=16
export NCCL_IB_ADAPTIVE_ROUTING=1
export NCCL_IB_TIMEOUT=22
export NCCL_IB_RETRY_CNT=10
export NCCL_CHECKS_DISABLE=1
export NCCL_CHECK_POINTERS=0
export NCCL_CROSS_NIC=2
export NCCL_ASYNC_ERROR_HANDLING=1
export TORCH_NCCL_ASYNC_ERROR_HANDLING=1
export NCCL_SOCKET_NTHREADS=4
export NCCL_NSOCKS_PERTHREAD=4
export NCCL_IB_HCA=mlx5_0,mlx5_1,mlx5_2,mlx5_3,mlx5_4,mlx5_5,mlx5_6,mlx5_7 # valid for an 8-subnet cluster network
export NCCL_TOPO_FILE=<path-to-xml-topology-file> #Sample file provided below, valid for gx3d-160x1792x8h100 profile VSI, with an 8-subnet cluster network
```
{: codeblock}

NCCL can determine the optimal paths between system components, including GPU's and NIC's, by referencing VSI-provided PCI topology information. If you want to provide a topology file using the `NCCL_TOPO_FILE` environment variable, a tested topology file for an H100 VSI with eight cluster subnets is provided at HYPERLINK.
{: note}
