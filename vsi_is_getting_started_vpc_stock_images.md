---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Stock images on VPC
{: #getting-started-images-on-vpc-stock}

A stock image is the out-of-the-box operating system that is customized for {{site.data.keyword.vsi_is_full}} environments. It is used to deploy virtual servers or bare metal servers by using different architecture types.
{: shortdesc}

The image that you select determines the operating system that is provisioned for your instance. If the image you select is an operating system bundle stock image, the software that is part of that bundle is also included in your instance.

In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **Navigation menu** icon![Navigation menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute **> Images**.

## Images on VPC - Stock images
{: #images-on-vpc-stock-images}

When you provision a virtual server on your VPC, you need to select an image to determine the operating system for the server. The following groups of operating system stock images are available:

* [x86 virtual server images](/docs/vpc?topic=vpc-about-images)
*  [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images)
* [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image)

All available stock images can be found in the console. Go to the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute** **> Images** **> Stock images**.

When you use an {{site.data.keyword.IBM_notm}} stock image, you must apply the most recent security patches and any updates that are provided by the operating system vendor. After you provision a virtual server, it is your responsibility to continue to regularly make package updates for the operating system. If you are using the RHEL AI 1.x stock image, you can't apply updates to an existing virtual server instance. You must provision a new virtual server instance with the most recent image.
{: important}

To create a virtual server with a stock image, see one of the following topics:
* [Creating a virtual server instance in the console](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance from the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
* [Creating a virtual server instance with the API - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
* [Creating an instance with Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)
