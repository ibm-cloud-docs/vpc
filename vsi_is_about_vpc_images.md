---

copyright:
  years: 2022, 2023

lastupdated: "2023-05-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Images on VPC
{: #images-on-vpc}

When you provision {{site.data.keyword.vsi_is_full}} on x86 architecture, you can select from the supported virtual server operating system stock images, the virtual server operating system bundle stock image, or a custom image that you import from {{site.data.keyword.cos_full_notm}}. The image that you select determines the operating system that is provisioned for your instance. If the image you select is a virtual server operating system bundle stock image, the software that is part of that bundle is also included in your instance.
{: shortdesc}

In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.

There are 3 different types of images for VPC.
* Custom images (includes Image from volume)
* Stock images
* Catalog images

## Images on VPC - Custom images
{: #images-on-vpc-custom-images}

Custom images are used to create new virtual servers with your own settings and configurations. You can create a Linux&reg; custom image, a Windows&reg; custom image, a z/OS Wazi aaS custom image, or a VMware custom image. You can import your custom image directly into {{site.data.keyword.vpc_full}} from {{site.data.keyword.cos_full}} or create one from an existing virtual server boot volume. You can also create an image template to migrate a virtual server from the Classic infrastructure. 

All custom images, including image from volume, can be found in **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images > Custom images**

To create a virtual server using a custom image, see one of the following:
* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
* [Creating a virtual server instance with the CLI - provision from an existing volume](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#create-instance-volume-cli)
* [Creating a virtual server instance with the API - provision from a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
* [Creating a virtual server instance with the API - provision from an existing volume](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-ipbv-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)

## Images on VPC - Stock images
{: #images-on-vpc-stock-images}

When you provision a virtual server on your VPC, you need to select an image to determine the operating system for the server. The following are the virtual server operating system stock images available when you create a virtual server.

* [x86 virtual server images](/docs/vpc?topic=vpc-about-images)
* [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images)
* [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image)
* [s390x bare metal server images](/docs/vpc?topic=vpc-s390x-bare-metal-images)

All available stock images can be found in **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images > Stock images**.

To create a virtual server using a stock image, see one of the following:
* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
* [Creating a virtual server instance with the API - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)

## Images on VPC - Catalog images
{: #images-on-vpc-catalog-images}

If you plan to share or publish a custom image to other accounts within your enterprise, you need to create a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts while those accounts are within the same enterprise. You can share any existing x86 virtual server custom image with a private catalog, except for an encrypted image. For more information about these limitations, see [Custom images in a private catalog](/docs/vpc topic=vpc-planning-custom-images#custom-image-cloud-private-catalog).

Any private catalog images available to your VPC can be found in **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images > Catalog images**

To create a virtual server using a private catalog image, see one of the following:
* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-private-catalog-image-cli)
* [Creating a virtual server instance with the API - provision with a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-private-catalog-image-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)
