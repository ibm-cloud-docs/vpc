---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords: cluster profiles, cluster network, cluster-network, cluster network profile, cluster network profiles, gpu, nvidia, hopper-1, rdma, roce, accelerated, rocev2, accelerated network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# NVIDIA Hopper 1 cluster network profile
{: #cluster-network-hopper-1-profile}

The Hopper 1 cluster network profile (`hopper-1`) provides isolated networks for [NVIDIA Hopper HGX instances](/docs/vpc?topic=vpc-accelerated-profile-family#hopper-hgx-profiles) running workloads that require high-bandwidth, low-latency interconnectivity, such as AI training and large-scale simulations.
{: shortdesc}

The Hopper 1 cluster network profile, which supports both NVIDIA H100 and H200 instance profiles, replaces the deprecated H100 cluster network profile.
{: note}

## Overview
{: #cluster-network-hopper-1-profile-overview}

The Hopper 1 cluster network profile supports Remote Direct Memory Access (RDMA) over the Convergent Ethernet v2 (RoCEv2) network protocol for increased throughput, reduced latency, and improved system performance.

For a list of supported instance profiles with this cluster network, see [Hopper HGX instance profiles](/docs/vpc?topic=vpc-accelerated-profile-family#hopper-hgx-profiles).

## Availability
{: #cluster-network-hopper-1-profile-availability}

Currently, the use of the Hopper 1 cluster network profile is available in the following supported regions and zones:

| Region                    | Zone                     | Universal zone name           |
| ------------------------  | -------------------------|-------------------------------|
| Frankfurt (`eu-de`)       | `eu-de-2`                | `eu-de-fra04-a`               |
| Washington DC (`us-east`) | `us-east-3`              | `us-east-wdc07-a`             |
{: caption="Supported regions and zones for Hopper 1" caption-side="bottom"}

To understand how various regions correspond to zones, see [zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).
{: note}

## Capabilities and restrictions
{: #cluster-network-hopper-1-profile-capabilities}

The cluster network Hopper 1 profile has the following capabilities and restrictions:

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

## Tested NCCL configuration
{: #performance-recs}

The NVIDIA Collective Communications Library (NCCL) can determine the optimal paths between system components, including GPUs and NICs, by referencing VSI-provided PCI topology information. This [topology file](/media/docs/downloads/vpc/topo.xml){: external} is tested for an H100 VSI with eight cluster subnets, and can be used if you want to provide a topology file that uses the `NCCL_TOPO_FILE` environment variable.

The following information provides tested NCCL tunings for an H100 VM profile with an 8-subnet cluster network. All testing was done on NCCL version 2.22.3. For more information, see the [NVIDIA NCCL documentation](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/overview.html){: external}.

```sh
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
