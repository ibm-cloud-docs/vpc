---

copyright:
  years: 2021, 2024
lastupdated: "2024-08-26"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, gpu, graphics processing unit, set up gpu

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing GPUs
{: #managing-gpus}

The GPU-enabled family of profiles provides on demand, cost-effective access to NVIDIA GPUs. GPUs help to accelerate the processing time that is required for compute intensive workloads such as AI, machine learning, inferencing and more. To use the GPUs, make sure that you install the appropriate NVIDIA driver and associated toolkit for your workloads.
{: shortdesc}


## Configuring a virtual server instance with a GPU
{: #provision-gpu-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) by choosing a [GPU profile](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) in the Profile field. Stock and custom operating system images are supported.
2. Install the NVIDIA GPU driver for your Virtual Server Instance's image and GPU profile. A NVIDIA driver level of R440 or newer is recommended. To download the drivers, see NVIDIA's [Download drivers](https://www.nvidia.com/en-us/drivers/){: external} page.
3. Install associated toolkit for your workload. Visit NVIDIA's [CUDA toolkit downloads](https://developer.nvidia.com/cuda-downloads){: external} page.

For detailed instructions to complete Steps 2 and 3, other GPU tools, and examples, see [How to Use V100-Based GPUs on IBM Cloud VPC](https://www.ibm.com/blog/how-to-use-v100-based-gpus-on-ibm-cloud-vpc/){: external}.

For a Linux focused guide on installing the NVIDIA drivers, see the [NVIDIA Driver Installation Quickstart Guide](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/){: external}.

If you want to automate the installation of the drivers, you can use the [User data](/docs/vpc?topic=vpc-user-data) section of the VSI. By using the user data field, you can input a script that issues the commands to install the NVIDIA drivers.
{: tip}

## Integrating the GPU driver into a custom image from volume
{: #scaling-custom-image}

1. Provision a virtual server instance with a GPU and install the drivers.
2. Create an image from the virtual server instance stock image boot volume. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui).
3. Repeat the Image from volume process to deploy across multiple instances.

## Next steps
{: #managing-gpus-next-steps}

For more information, see the [NVIDIA driver documentation](https://docs.nvidia.com/datacenter/tesla/index.html){: external}.
