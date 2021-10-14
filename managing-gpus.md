---

Copyright:
  years: 2021
lastupdated: "2021-09-24"

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

This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access.
{: beta}

# Managing GPUs (Beta)
{: #managing-gpus}

The GPU-enabled family of profiles provides on demand, cost effective access to NVIDIA GPUs. GPUs help to accelerate the processing time required for compute intensitve workloads such as AI, machine learning, inferencing and more. To use the GPUs, make sure that you have the appropriate tool chain such as CUDA, ready.
{: shortdesc}


## Configuring a virtual server instance with a GPU
{: #provision-gpu-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) by choosing a GPU profile in the Virtual Server Instance selector window under Virtual Server for VPC with Public Virtual Server selected. GPU enabled profiles begin with the gx2 prefix. Stock and custom operating system images are supported.
2. The NVIDIA V100 GPU driver for Windows or Linux will need to be installed. For more information, see the [NVIDIA driver documentation](https://docs.nvidia.com/datacenter/tesla/index.html){: external}. A NVIDIA driver level of R440 or newer is recommended.

If you want to automate the installation of the drivers, you can use the [User data](/docs/vpc?topic=vpc-user-data) section of the VSI. This allows you to input a script that executes the commands to install the NVIDIA drivers.
{: note}

## Integrating the GPU driver into a custom image from volume
{: #scaling-custom-image}

1. Provision a virtual server instance with a GPU and install the drivers.
2. Create an image from the virtual server instance stock image boot volume. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui).
3. Repeat the Image from volume process to deploy across multiple instances.
