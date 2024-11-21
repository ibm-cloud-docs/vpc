---

copyright:
  years: 2024
lastupdated: "2024-11-21"

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

## Tested NCCL configuration
{: #performance-recs}

The following information provides tested NCCL tunings for an H100 VM profile with an 8-subnet cluster network. All testing was done on NCCL version 2.22.3. For more information, refer to the [NVIDIA Documentation](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/overview.html).

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

NCCL can determine the optimal paths between system components, including GPU's and NIC's, by referencing VSI-provided PCI topology information. This [topology file](https://cloud.ibm.com/media/docs/downloads/vpc/topo.xml){: external} has been tested for an H100 VSI with eight cluster subnets, and can be used if you want to provide a topology file using the `NCCL_TOPO_FILE` environment variable.
{: note}

```json
<system version="1">
  <cpu host_hash="0xa5f31daa95182da8" numaid="0" affinity="00000000,00000000,0000ffff,ffffffff,ffffffff" arch="x86_64" vendor="GenuineIntel" familyid="6" modelid="143">
    <pci busid="0000:a1:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:a3:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_7" dev="7" speed="200000" port="1" latency="0.000000" guid="0x34f4e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:a4:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="0" sm="90" rank="0" gdr="1">
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:ab:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:ad:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_6" dev="6" speed="200000" port="1" latency="0.000000" guid="0x4f8e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:ae:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="1" sm="90" rank="1" gdr="1">
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:b5:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:b7:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_5" dev="5" speed="200000" port="1" latency="0.000000" guid="0x24f7e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:b8:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="2" sm="90" rank="2" gdr="1">
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:bf:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:c1:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_4" dev="4" speed="200000" port="1" latency="0.000000" guid="0xe406e40003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:c2:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="3" sm="90" rank="3" gdr="1">
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
  </cpu>
  <cpu host_hash="0xa5f31daa95182da8" numaid="1" affinity="ffffffff,ffffffff,ffff0000,00000000,00000000" arch="x86_64" vendor="GenuineIntel" familyid="6" modelid="143">
    <pci busid="0000:c9:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:cb:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_3" dev="3" speed="200000" port="1" latency="0.000000" guid="0x34f7e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:cc:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="4" sm="90" rank="4" gdr="1">
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:d3:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:d5:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_2" dev="2" speed="200000" port="1" latency="0.000000" guid="0xf4f7e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:d6:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="5" sm="90" rank="5" gdr="1">
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:dd:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:df:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_1" dev="1" speed="200000" port="1" latency="0.000000" guid="0xd4f5e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:e0:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="6" sm="90" rank="6" gdr="1">
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
    <pci busid="0000:e7:00.0" class="0x060400" vendor="0x104c" device="0x8232" subsystem_vendor="0x0000" subsystem_device="0x0000" link_speed="2.5 GT/s PCIe" link_width="1">
      <pci busid="0000:e9:00.0" class="0x020000" vendor="0x15b3" device="0x101e" subsystem_vendor="0x15b3" subsystem_device="0x0127" link_speed="32.0 GT/s PCIe" link_width="0">
        <nic>
          <net name="mlx5_0" dev="0" speed="200000" port="1" latency="0.000000" guid="0xd4f6e30003e1a258" maxconn="131072" gdr="1"/>
        </nic>
      </pci>
      <pci busid="0000:ea:00.0" class="0x030200" vendor="0x10de" device="0x2330" subsystem_vendor="0x10de" subsystem_device="0x16c1" link_speed="32.0 GT/s PCIe" link_width="0">
        <gpu dev="7" sm="90" rank="7" gdr="1">
          <nvlink target="0000:f4:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f6:00.0" count="4" tclass="0x068000"/>
          <nvlink target="0000:f5:00.0" count="5" tclass="0x068000"/>
          <nvlink target="0000:f3:00.0" count="4" tclass="0x068000"/>
        </gpu>
      </pci>
    </pci>
  </cpu>
</system>
```
{: codeblock}
