---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-02"

keywords: image, stock image, custom image, virtual private cloud, virtual server, power, generation 2, gen 2

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
| Red Hat Enterprise Linux 7.x | x86-64 | Not supported on POWER architecture. |
| Ubuntu 16.04.x | x86-64 | <!--"xenial xerus"--> |
| Ubuntu 18.04.x | ppc64le, x86-64 | Supports accelerated compute profiles on POWER processors |
| Windows 2012, 2012 R2, 2016 | x86-64 | Not supported on POWER architecture. |
{: caption="Table 1. Stock boot images provided" caption-side="top"}

 When you order an instance, the images are cloud-init enabled to optimize creation times. With a cloud-init enabled image, you can provide user data. In the **User Data** field on the order form, you can enter optional cloud-init user data for the server. For more information about user data and automation, see [User data](/docs/vpc?topic=vpc-user-data).

### Image support for GPUs
{: #gpu-images}

The only stock image that currently supports GPU profiles is the Ubuntu 18.04 image for the Power Architecture. The GPU profiles are available for the POWER architecture. The NVIDIA kernel driver for the Tesla V100 GPUs must be installed in your instance before use. For more information, see [Setting up GPU drivers for POWER-based instances](/docs/vpc?topic=vpc-setup-gpus).

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

For more information about custom images, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images).

<!--### Storage costs
{: #custom-image-storage}

Storage costs are incurred for storing custom images. This charge is separate from charges for storing images in {{site.data.keyword.cos_full_notm}}.-->

 
