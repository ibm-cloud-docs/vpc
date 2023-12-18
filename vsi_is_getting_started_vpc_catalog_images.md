---

copyright:
  years: 2022, 2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Catalog Images on VPC
{: #getting-started-images-on-vpc-catalog}

When you provision {{site.data.keyword.vsi_is_full}} on x86 architecture, you can select from the supported virtual server operating system stock images, the virtual server operating system bundle stock image, or a custom image that you import from {{site.data.keyword.cos_full_notm}}. The image that you select determines the operating system that is provisioned for your instance. If the image you select is a virtual server operating system bundle stock image, the software that is part of that bundle is also included in your instance.
{: shortdesc}

In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.

There are 3 different types of images for VPC.
* [Custom images](/docs/vpc?topic=vpc-planning-custom-images) (includes [Image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc))
* [Stock images](/docs/vpc?topic=vpc-getting-started-images-on-vpc-stock)
* Catalog images

## Images on VPC - Catalog images
{: #images-on-vpc-catalog-images}

If you plan to share or publish a custom image to other accounts within your enterprise, you need to create a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts while those accounts are within the same enterprise. You can share any existing x86 virtual server custom image with a private catalog, except for an encrypted image. For more information about these limitations, see [VPC considerations when using custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=api).

Any private catalog images available to your VPC can be found in **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images > Catalog images**

To create a virtual server using a private catalog image, see one of the following:
* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-private-catalog-image-cli)
* [Creating a virtual server instance with the API - provision with a private catalog image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-private-catalog-image-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)
