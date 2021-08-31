---

Copyright:
  years: 2021
lastupdated: "2021-08-31"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, gpu, graphics processing unit, set up gpu

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:external: target="_blank" .external}
{:beta: .beta}

GPU enabled profiles are available as a closed beta release for evaluation and testing purposes.
{: beta}

# Managing GPUs (Beta)
{: managing-gpus}

Certain instance profiles include GPU accelerators. You can use these instances to accelerate certain processing functions for workloads such as inferencing, machine learning, and more. To use the GPUs, make sure that you have the appropriate tool chain such as CUDA, ready.
{: shortdesc}


## Provisioning a virtual server instance with a GPU
{: #provision-gpu-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) with a GPU profile in the **Instance profile** field. These include profiles with the _gx2_ prefix. <!--For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles#gpu).--> Stock and custom images are supported.
2. Install the NVIDIA driver for Windows or Linux. For more information, see the [NVIDIA driver documentation](https://docs.nvidia.com/datacenter/tesla/index.html){: external} and [NVIDIA Driver Installation Quickstart Guide](https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html){: external}. A NVIDIA driver level of R440 or newer is recommended.

If you wish to automate the installation of the drivers, you can use the [User data](/docs/vpc?topic=vpc-user-data) section of the VSI. This allows you to input a script that executes the commands to install the NVIDIA drivers.
{: note}

## Integrating the GPU driver into a custom image from volume
{: #scaling-custom-image}

1. Provision a virtual server instance with a GPU and install the drivers.
2. Create an image from the virtual server instance stock image boot volume. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui).
3. Repeat the Image from volume process to deploy across multiple instances.
