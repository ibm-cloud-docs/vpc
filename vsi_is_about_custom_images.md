---

copyright:
  years: 2022, 2023

lastupdated: "2023-11-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with custom images
{: #planning-custom-images}

Custom images are used to create new virtual servers with your own settings and configurations. You can create a Linux&reg; custom image, a Windows&reg; custom image, a z/OS Wazi aaS custom image, or a VMware custom image. You can import your custom image directly into {{site.data.keyword.vpc_full}} from {{site.data.keyword.cos_full}} or create one from an existing virtual server boot volume. You can also create an image template to migrate a virtual server from the Classic infrastructure. After a custom image is created and imported into {{site.data.keyword.vpc_short}}, you can import it into a private catalog, with some limitations. For more information about these limitations, see [VPC considerations when using custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui).
{: shortdesc}

On the console, you can find custom images by clicking *Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images > Custom images*. Image from a volume are part of the custom images tab. For more information on image from a volume, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc). 

## Using a custom image to create a server
{: #custom-image-create-vsi}

To create a virtual server using a custom image, see one of the following:

* [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
* [Creating a virtual server instance with the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
* [Creating a virtual server instance with the API - provision from a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
* [Creating an instance using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)

## Prerequisites and limitations
{: #custom-image-prerequisites}

Before you create a custom image, you must verify that your custom image meets the custom image requirements and that the operating system is supported.

### Custom image considerations
{: #custom-image-considerations}

{{site.data.content.custom-image-requirements-list}}

### Operating system considerations
{: #custom-image-operating-systems}

* Make sure that your image is supported as an [x86 stock image](/docs/vpc?topic=vpc-about-images) or an [s390x stock image](/docs/vpc?topic=vpc-vsabout-images) on {{site.data.keyword.vpc_full}}.
* If you choose to create a custom image with your own license, specify the appropriate operating system version that appends `-byol` to the name when you import the image. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).
* ISO images of licensed operating systems, such as Windows&reg; and Linux&reg;, and open source operating systems, such as CentOS and Ubuntu, aren't provided by {{site.data.keyword.cloud}}. If you need these ISO images, you can download them from the respective vendor website.

### {{site.data.keyword.cos_full_notm}} considerations
{: #custom-image-cloud-object-storage}

If you plan to import an image from a file, you must provision an instance of {{site.data.keyword.cos_full_notm}} if you don't already have one. You can then upload the file to a bucket there. You must also create an IAM authorization between the Image Service for VPC and {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).

## Custom image lifecycle
{: #custom-image-lifecycle}

<!-- Image life cycle content shared with custom images & image from volume -->
{{_include-segments/image_lifecycle.md}}

{{site.data.keyword.vpc_short}} managed images can be managed only this way. Custom images that are published to a private catalog must be in `available` status and their statuses are maintained in the private catalog. If you try to change or schedule a change to their status in {{site.data.keyword.vpc_short}}, the attempt fails. If a custom image is removed from a private catalog, that image retains its original private catalog status until the status is changed in {{site.data.keyword.vpc_short}}.

| Private catalog image status | Corresponding VPC image status |
|------------------------|----------------------------|
| `published/verified`   | `available`                |
| `deprecated`           | `deprecated`               |
| `archived`             | `obsolete`                 |
{: caption="Table 2. Catalog image lifecycle status and the corresponding VPC status" caption-side="bottom"}

## Bare metal server custom images
{: #bare-metal-server-custom-images-considerations}

Bare metal servers have some limitations that you need to be aware of.

* Encrypted images aren't supported

To create a custom image for bare metal servers, the custom image must support the following information:

* UEFI boot
   * UEFI boot requires a dedicated EFI partition that contains EFI firstware. Traditional BIOS boot is not supported.
* Pensando iconic network device drivers
* Intel chip set device drivers
   * These device drivers are usually part of the default kernel build options. Windows requires extra device drivers, but you can install these drivers later.

For more information, see [Custom Linux kernel build options for bare metal servers](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels#custom-linux-kernel-linuxone-options).

For more information about bare metal server images, see [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image).

## z/OS Wazi aaS custom images
{: #custom-image-zos}

You can use IBM Wazi Image Builder to create your own custom z/OS-based {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) image and import the custom image into {{site.data.keyword.vpc_full}}.

IBM Wazi Image Builder is a separately orderable product from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/){: external}. Extra requirements are needed to use Wazi Image Builder. The image cost is the premium that is applied to cover the cost of technologies that allows for z/OS dev and test images to run on IBM Z hardware in IBM’s cloud infrastructure as a service layer.

The z/OS Wazi aaS custom image must meet the following requirements:
* qcow2 format
* z/OS 2.4 or z/OS 2.5 operating system
* See [Prerequisites](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=builder-prerequisites){: external} for required PTF fixes on z/OS and other IBM software products to allow them to run as a guest of IBM Z Hypervisor as a Service (zHYPaaS).

For more information, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder){: external}.

## More information about custom images
{: #custom-image-additional-information}

* [Using granular RBAC permissions for custom images](/docs/vpc?topic=vpc-using-granular-RBAC-permissions-for-custom-images)
* [Configuration requirements for custom Linux kernel](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels)
* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui)
* [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).
* [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui)
* [Managing image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui)
