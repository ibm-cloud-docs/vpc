---

copyright:
  years: 2022, 2025
lastupdated: "2025-03-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Stock images on VPC
{: #getting-started-images-on-vpc-stock}

When you provision {{site.data.keyword.vsi_is_full}} on x86 architecture, you can select from the supported virtual server operating system stock images, the virtual server operating system bundle stock image, or a custom image that you import from {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

The image that you select determines the operating system that is provisioned for your instance. If the image you select is a virtual server operating system bundle stock image, the software that is part of that bundle is also included in your instance.

In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![Navigation Menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute **> Images**.

You can choose from 3 different types of images for VPC.
* [Custom images](/docs/vpc?topic=vpc-planning-custom-images) (includes [Image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc))
* Stock images
* [Catalog images](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog)

## Images on VPC - Stock images
{: #images-on-vpc-stock-images}

When you provision a virtual server on your VPC, you need to select an image to determine the operating system for the server. The following are the virtual server operating system stock images available when you create a virtual server.

* [x86 virtual server images](/docs/vpc?topic=vpc-about-images)
* [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images)
* [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image)

All available stock images can be found in **Navigation Menu** icon![Navigation Menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute** **> Images** **> Stock images**.

When you use an {{site.data.keyword.IBM_notm}} stock image, you must apply the latest security patches and any updates that are provided by the operating system vendor. After you provision a virtual server, it is your responsibility to continue to regularly make package updates for the operating system.
{: important}

To create a virtual server using a stock image, see one of the following:
* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
* [Creating a virtual server instance with the API - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)
