---

copyright:
  years: 2021, 2025
lastupdated: "2025-06-09"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, gpu, graphics processing unit, set up gpu

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing GPUs and accelerators
{: #managing-gpus}

The GPU-enabled family of profiles provides on demand, cost-effective access to GPUs and accelerators. GPUs and accelerators help to accelerate the processing time that is required for compute intensive workloads such as AI, machine learning, inferencing and more. To use the GPUs and accelerators, make sure that you install the appropriate driver and associated toolkit for your workloads.
{: shortdesc}


## Configuring a virtual server instance with an NVIDIA GPU
{: #provision-gpu-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) by choosing an NVIDIA [GPU profile](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) in the Profile field. Stock and custom operating system images are supported.
2. Install the NVIDIA GPU driver for your virtual server instance's image and GPU profile.  For more information, see NVIDIA's [Download drivers](https://www.nvidia.com/en-us/drivers/){: external} page.

3. Install associated toolkit for your workload. Visit NVIDIA's [CUDA toolkit downloads](https://developer.nvidia.com/cuda-downloads){: external} page.

For detailed instructions to complete Steps 2 and 3, other GPU tools, and examples, see [How to Use V100-Based GPUs on IBM Cloud VPC](https://www.ibm.com/products/tutorials/how-to-use-v100-based-gpus-on-ibm-cloud-vpc){: external}.

For a Linux focused guide on installing the NVIDIA drivers, see the [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/index.html){: external}.

If you want to automate the installation of the drivers, you can use the [User data](/docs/vpc?topic=vpc-user-data) section of the virtual server. By using the user data field, you can input a script that issues the commands to install the NVIDIA drivers.
{: tip}

## Configuring a virtual server instance with an Intel Gaudi 3 AI Accelerator
{: #provision-gaudi-3-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) by choosing the Intel® Gaudi® 3 AI Accelerator [instance profile](/docs/vpc?topic=vpc-accelerated-profile-family&interface=ui#gaudi-3-vsi-profiles) in the Profile field. Stock and custom operating system images are supported.
2. Install the Intel Gaudi 3 AI Accelerator software and drivers for your virtual server. To download the drivers, see [Intel Gaudi Driver and Software Installation](https://docs.habana.ai/en/latest/Installation_Guide/Driver_Installation.html){: external} page.

## Configuring a virtual server instance with an AMD Instinct MI300X Accelerator
{: #provision-amd-mi300x-on-vsi}

1. Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers) by choosing the AMD Instinct™ MI300X Accelerator [instance profile](/docs/vpc?topic=vpc-accelerated-profile-family) in the Profile field. Stock and custom operating system images are supported.
2. Install the necessary drivers for your virtual server. To download the drivers, see [Installing ROCm and machine learning frameworks](https://rocm.docs.amd.com/en/latest/how-to/rocm-for-ai/install.html){: external} page.
3. If the guest OS for your virtual server is Ubuntu, you must remove `nomodeset` from the command line and restart the virtual server.
    1. These commands must be run as root. Sudo to root.
       ```sh
       sudo -i
       ```
       {: pre}

    2. Remove `nomodeset` from the settings file. The following example uses vi.
       ```sh
       vi /etc/default/grub.d/50-cloudimg-settings.cfg
       ```
       {: pre}

    3. Verify that `nomodeset` is removed from the settings file.
       ```sh
       cat /etc/default/grub.d/50-cloudimg-settings.cfg
       ```
       {: pre}

    4. Update grub.
       ```sh
       update-grub
       ```
       {: pre}

    5. Restart the virtual server.


## Integrating drivers into a custom image from volume
{: #scaling-custom-image}

1. Provision a virtual server instance with a GPU and install the drivers.
2. Create an image from the virtual server instance stock image boot volume. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui).
3. Repeat the Image from volume process to deploy across multiple instances.

## Next steps
{: #managing-gpus-next-steps}

For more information, see the [NVIDIA driver documentation](https://docs.nvidia.com/datacenter/tesla/index.html){: external}.
