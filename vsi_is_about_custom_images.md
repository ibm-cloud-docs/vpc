---

copyright:
  years: 2022
lastupdated: "2022-07-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with custom images
{: #planning-custom-images}

Custom images are used to create new virtual servers with your own settings and configurations. You can create a Linux&reg; custom image, a Windows&reg; custom image, a z/OS custom image, or a VMWare vSphere custom image. You can import your custom image directly into {{site.data.keyword.vpc_full}} from {{site.data.keyword.cos_full}} or create one from an existing virtual server boot volume. You can also create an image template to migrate a virtual server from the Classic infrastructure. After a custom image is created and imported into {{site.data.keyword.vpc_short}}, you can import it into a private catalog, with some limitations. For more information regarding these limitations, see [Private catalog considerations](/docs/vpc?topic=vpc-about-custom-images#custom-image-cloud-private-catalog).
{: shortdesc}

## Prerequisites and limitations
{: #custom-image-prerequisites}

Before you create a custom image, you must verify that your custom image meets the custom image requirements and that the operating system is supported.

### Custom image considerations
{: #custom-image-considerations}

{{site.data.content.custom-image-requirements-list}}

### Operating system considerations
{: #custom-image-operating-systems}

* Make sure that your image is supported as an [x86 stock image](/docs/vpc?topic=vpc-about-images) or an [s390x stock image](/docs/vpc?topic=vpc-vsabout-images) on {{site.data.keyword.vpc_full}}.
* If you choose to create a custom image with your own license, select the [BYOL](/docs/vpc?topic=vpc-byol-vpc-about) option.
* ISO images of licensed operating systems, such as Windows&reg; and Linux&reg;, and open source operating systems, such as CentOS and Ubuntu, aren't provided by {{site.data.keyword.cloud}}. If you need these ISO images, you can download them from the respective vendor website.

### {{site.data.keyword.cos_full_notm}} considerations
{: #custom-image-cloud-object-storage}

If you plan to import an image from a file, you must provision an instance of {{site.data.keyword.cos_full_notm}} if you don't already have one. You can then upload the file to a bucket there. You must also create an IAM authorization between the Image Service for VPC and {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).

### Private catalog considerations
{: #custom-image-cloud-private-catalog}

This is a Beta feature that is available for evaluation and testing purposes for customers with special approval to preview this feature.
{: beta}

If you want to use a custom image to share or publish that image to other accounts, you need to create a private catalog. A private catalog provides a way for you to manage access to products for multiple accounts. You can use any existing x86 virtual server custom image with a private catalog, with the exception of an encrypted image.

Before you can import a custom image into a private catalog, the following items must be completed.

* Create a private catalog
* Create and import the custom image into {{site.data.keyword.vpc_short}}
* The custom image must be in `available` status

{{site.data.content.delete-custom-image-private-catalog}}

Custom images in a private catalog have the following restrictions:

* Must be an x86 image
* Can't be encrypted
* Can't be used with a bare metal profile
* Can't be used with instance groups

The information about which custom image that is in a private catalog that was used for provisioning an instance doesn't persist across all {{site.data.keyword.vpc_short}} resources. Information about the custom image in a private catalog isn't available on Snapshot for VPC or Image from Volume. The operating system information, which is required when you provision a virtual server, is available.

For more information, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

## Create a custom image
{: #custom-image-creation}

Use one of the following procedures to create the custom image.

* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image)
* [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc)
* [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=ui)
* For IBM z/OS software, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder)

Did you know that you can use the IBM Cloud [Packer Plug-in](https://github.com/IBM/packer-plugin-ibmcloud){: external} to create and manage custom images on IBM Cloud? For more information, see this [blog post](https://www.ibm.com/cloud/blog/build-hardened-and-pre-configured-vpc-custom-images-with-packer){: external} for an example of how to use the IBM Cloud plugin for Packer.
{: tip}

## Import your custom image into {{site.data.keyword.vpc_short}}
{: #custom-image-using-COS}

After your custom image is created and stored on {{site.data.keyword.cos_full_notm}}, you must import that custom image directly to {{site.data.keyword.vpc_short}}. This requirement includes any custom images that you plan to import into a private catalog. The custom images must be imported into {{site.data.keyword.vpc_short}} first and then imported into the private catalog.

* Upload your custom image to {{site.data.keyword.cos_full_notm}}.
* On the Objects page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB.
* Import your custom image to {{site.data.keyword.vpc_short}}. When a supported custom image is available in {{site.data.keyword.cos_full_notm}}, the image is ready to import. For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-and-validating-custom-images-into-vpc&interface=ui).

### Import your custom image into a private catalog
{: #custom-image-using-private-catalog}

After your custom image is imported into {{site.data.keyword.vpc_short}}, you can then import that custom image into a private catalog.

* [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui)

## Using a custom image to create a virtual server instance
{: #custom-image-create-vsi}

When you [create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers), you select the custom image in the `Image` tab of the `Operating system`. To select from the list of {{site.data.keyword.vpc_short}} custom images, select `Custom image`. To select from the list of private catalog images, select `Catalog image` instead.


## Additional information about custom images
{: #custom-image-additional-information}

* [Using granular RBAC permissions for custom images](/docs/vpc?topic=vpc-using-granular-RBAC-permissions-for-custom-images)
* [Configuration requirements for custom Linux kernel](/docs/vpc?topic=vpc-configuration-requirements-for-custom-linux-kernels)
* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc&interface=ui)
* [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui)
* [Managing image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui)
