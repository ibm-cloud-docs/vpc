---

copyright:
  years: 2022, 2024
lastupdated: "2024-05-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with custom images
{: #planning-custom-images}

A custom image contains an operating system image with specific configurations that are customized by you. You can manage the lifecycle, share the custom image, and use it to create new virtual servers or bare metal servers with your own settings and configurations. You can create a Linux&reg; custom image, a Windows&reg; custom image, a z/OS Wazi aaS custom image, or a generic operating system custom image. 
{: shortdesc}

You have a few options for creating a custom image. 

* You can import your custom image directly into {{site.data.keyword.vpc_full}} from {{site.data.keyword.cos_full}}. For more information, see [Importing your custom image into IBM Cloud VPC](/docs/vpc?topic=vpc-custom-image-using-COS&interface=ui).
* You can create a custom image from an existing VPC virtual server boot volume. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui). 
* If you have virtual servers running on the Classic Infrastructure, you can create an image template to migrate to a VPC virtual server. For more information, see [Migrating a virtual server from the Classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

When your custom image is created, you can plan and manage the lifecycle of the image by using three statuses: available, deprecated, or obsolete. For more information on these status and how to manage them, see [Custom image lifecycle](/docs/vpc?topic=vpc-planning-custom-images&interface=ui#custom-image-lifecycle).

You can also share your custom images with other accounts by using a private catalog. For more information on using custom images in a private catalog, see [Getting started with Catalog Images on VPC](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog&interface=ui). 

On the console, you can find custom images by clicking **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure ![VPC icon](../../icons/vpc.svg) > Compute > Images > Custom images**. Images that are from a volume are part of the custom images tab. 

## Creating a custom image
{: #create-custom-image}

To create a custom image, see one of the following links.

- [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv)
- [Creating a custom Linux image](/docs/vpc?topic=vpc-create-linux-custom-image)
- [Creating a custom Windows image](/docs/vpc?topic=vpc-create-windows-custom-image)
- [Creating a z/OS Wazi aaS custom image](/docs/vpc?topic=vpc-create-zos-custom-image)
- [Creating a generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image&interface=ui)

## Sharing a custom image
{: #sharing-custom-image}

After a custom image is created in {{site.data.keyword.vpc_short}}, you can import it into a private catalog, and share it with other accounts with some limitations. For more information about the limitations, see [VPC considerations when you use custom images in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui).

## Using a custom image to create a server
{: #custom-image-create-vsi}

To create a virtual server by using a custom image, see one of the following links.

- [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
- [Creating a virtual server instance with the CLI - provision with a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#instance-create-from-image-cli)
- [Creating a virtual server instance with the API - provision from a stock or custom image](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-stock-custom-image-api)
- [Creating an instance by using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-using-terraform)

## Prerequisites and limitations
{: #custom-image-prerequisites}

Before you create a custom image, you must verify that your custom image meets the custom image requirements and that the operating system is supported.

### Custom image considerations
{: #custom-image-considerations}

<!-- Reused information list of custom image requirements shared with vsi_is_creating_generic_os_custom_image.md- see conref.md file-->
{{site.data.content.custom-image-requirements-list}}

### Operating system considerations
{: #custom-image-operating-systems}

- Make sure that the selected operating system specifies the correct user data format type. For more information, see [User data format considerations](#custom-image-user-data-format).
- If you choose to create a custom image with your own license, specify the appropriate operating system version that appends `-byol` to the name when you import the image. For more information, see [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about).
- ISO images of licensed operating systems, such as Windows&reg; and Linux&reg;, and open source operating systems, such as CentOS and Ubuntu, aren't provided by {{site.data.keyword.cloud}}. If you need these ISO images, you can download them from the respective vendor website.

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

## Generic operating system custom images
{: #generic-os-custom-images}

[Beta]{: tag-blue}

Generic operating system custom images support is a beta feature that is available for evaluation and testing purposes.
{: beta}

You can use a specific operating system that is not listed in {{site.data.keyword.Bluemix_notm}} by specifying a generic operating system when you import a custom image. You have multiple generic operating system options. You can select a generic operating system that is based on the CPU architecture and initialization strategy appropriate for your custom image operating system.

Generic operating system custom images are supported for x86 (amd64) architecture. These images are listed in the custom images list. Bare metal server generic operating system custom images must follow requirements for all bare metal server custom images. For more information, see [Bare metal server custom images](/docs/vpc?topic=vpc-planning-custom-images#bare-metal-server-custom-images-considerations).

The generic operating systems use a generic value for some of their properties, such as `generic` for the `vendor` property and `Generic` for the `family` property. When you create a generic operating system custom image, select the generic operating system based on the initialization type of the actual operating system. For more information, see [User data format considerations](/docs/vpc?topic=vpc-planning-custom-images#custom-image-user-data-format).

When you provision a server by using a generic operating system custom image, most operating system-specific provisioning steps aren't performed, such as console setup and automatic registration. You must provide the appropriate user data if you want your generic operating system custom image to perform these steps. You are also responsible for handling licensing and related costs because {{site.data.keyword.IBM_notm}} is not aware of the actual operating system installed.

## User data format considerations
{: #custom-image-user-data-format}

[Beta]{: tag-blue}

When you create a server that specifies your custom image, the initialization strategy determines how the user data is used. The `user_data_format` property of an image specifies this initialization strategy. This property is set from the image operating system `user_data_format` property, which contains one of the following values.

- `cloud_init`
- `esxi_kickstart` (This value works only for bare metal servers.)

- For cloud-init, user data and SSH keys are provided to the operating system on a cloud-init disk. A default user account is not created. You must use your SSH key to log in unless you set up another login mechanism through cloud-init. For virtual server instances, you might need to set up networking during initialization. For example,

   See [Creating a custom Linux image](/docs/vpc?topic=vpc-create-linux-custom-image) or [Creating a custom Windows image](/docs/vpc?topic=vpc-create-windows-custom-image) documentation for details about using cloud-init data.

- For ESXi kickstart, user data is provided to the operating system in a kickstart script. A default user account is created. Retrieve the server's initialization data to obtain the generated password. See VMware documentation for details of kickstart scripts.

Use security best practices to limit access to any files that you expose on the internet.
{: important}

## Bare metal server custom images
{: #bare-metal-server-custom-images-considerations}

Bare metal servers have some limitations that you need to be aware of.

- Encrypted images aren't supported.

To create a custom image for bare metal servers, the custom image must support the following information:

- UEFI boot
   - UEFI boot requires a dedicated EFI partition that contains EFI firmware. Traditional BIOS boot isn't supported.
- Pensando iconic network device drivers
- Intel chip set device drivers
   - These device drivers are usually part of the default kernel build options. Windows requires extra device drivers, but you can install these drivers later.

For more information, see [Custom Linux kernel build options for bare metal servers](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels#custom-linux-kernel-linuxone-options).

For more information about bare metal server images, see [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image).

## Secure boot-supported custom images
{: #bare-metal-server-custom-images-secure-boot-considerations}

[Select availability]{: tag-green}

Secure boot helps make sure that the system runs only authentic software by verifying the digital signature of all boot components. Secure boot halts the boot process if the signature verification fails. Secure boot prevents the loading of unsigned or malicious code during boot.

Custom images that support secure boot have some requirements that you need to be aware of.

- UEFI boot
   - UEFI boot requires a dedicated EFI partition that contains EFI firmware. Traditional BIOS boot is not supported.
- GPT partitioned disk

You can verify that the image successfully booted in secure boot mode by using the following mokutil command.

```text
mokutil --sb-state
```
{: codeblock}

For more information about secure boot, see [Confidential computing with secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc).

## z/OS Wazi aaS custom images
{: #custom-image-zos}

You can use IBM Wazi Image Builder to create your own custom z/OS-based {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) image and import the custom image into {{site.data.keyword.vpc_full}}.

IBM Wazi Image Builder is a separately orderable product from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/){: external}. Extra requirements are needed to use Wazi Image Builder. The image cost is the premium that is applied to cover the cost of technologies that allows for z/OS dev and test images to run on IBM Z hardware on IBM’s cloud infrastructure as a service layer.

The z/OS Wazi aaS custom image must meet the following requirements:
- qcow2 format
- z/OS 2.4 or z/OS 2.5 operating system

For more information, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder){: external}.

## More information about custom images
{: #custom-image-additional-information}

* [Using granular RBAC permissions for custom images](/docs/vpc?topic=vpc-using-granular-RBAC-permissions-for-custom-images)
* [Configuration requirements for custom Linux kernel](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels)
* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui)
* [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc).
* [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui)
* [Managing image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui)
