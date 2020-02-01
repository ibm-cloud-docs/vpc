---

copyright:
  years: 2019, 2020
lastupdated: "2019-12-20"

keywords: image, stock image, custom image, virtual private cloud, virtual server, power

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}


# Images
{: #about-images}

When you provision {{site.data.keyword.vsi_is_full}}, you can select from the supported stock images or a custom image that you import from {{site.data.keyword.cos_full_notm}}. The image that you select determines the operating system that is provisioned for your instance. 
{:shortdesc}

## Stock images
{: #stock-images}

The following operating systems are available as stock images when you create a virtual server.

<!-- * CentOS 7.x
* Debian 8.x, 9.x
* Red Hat Enterprise Linux 7.x-0
* Ubuntu 16.04, 18.04
* Windows 2012, 2012 R2, 2016 -->

| Image | Architectures | Notes |
|---------|---------|---------|
| CentOS 7.x | ppc64le, x86-64 | |
| Debian 9.x | ppc64le, x86-64 | |
| Ubuntu 16.04.x | x86-64 | <!--"xenial xerus"--> |
| Ubuntu 18.04.x | ppc64le, x86-64 | <!--"bionic beaver"--> |
| Ubuntu 18.04 with GPU | ppc64le  | Supports accelerated compute profiles |
{: caption="Table 1. Stock boot images provided" caption-side="top"}

 When you order an instance, the images are cloud-init enabled to optimize creation times. With a cloud-init enabled image, you can provide user data. In the **User Data** field on the order form, you can enter optional cloud-init user data for the server. For more information about user data and automation, see [User data](/docs/vpc?topic=vpc-user-data).

### Image support for GPUs
{: #gpu-images}

The only stock image that supports GPU profiles is the Ubuntu 18.04 image for the Power Architecture. The GPU profiles are available in the Power beta program. The Ubuntu 18.04 image has the NVIDIA kernel driver that is needed to support the Tesla V100 GPUs in the GPU profiles.

The NVIDIA driver version that is validated is 418.87. The validated version (and the version that is built into the stock image) will change in the future. Be sure to keep up to date on the validated version number, and update the driver level, as needed. 
{:important}

If you are installing the CUDA toolkit from NVIDIA, you should install the CUDA level that matches your installed NVIDIA driver version. The CUDA level and driver version should match, so the driver level does not inadvertently update to an unsupported level for your cognitive workloads.

## Custom images
{: #custom-images}

You can import an image from {{site.data.keyword.cos_full_notm}} to use for creating a new virtual server instance. 

### Requirements 
{: #custom-image-reqs}

Custom images must meet the following requirements: 
- Contain a single file or volume 
- Is in qcow2 format 
- Is cloud-init enabled
- The operating system is supported as a stock image operating system
- Size doesn't exceed 100 GB

Encrypted images aren't supported for custom images.
{: note}

For more information about importing custom images, see [Importing and managing images](/docs/vpc?topic=vpc-managing-images).

<!--### Storage costs
{: #custom-image-storage}

Storage costs are incurred for storing custom images. This charge is separate from charges for storing images in {{site.data.keyword.cos_full_notm}}.-->

 