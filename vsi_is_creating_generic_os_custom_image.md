---

copyright:
  years: 2022, 2024
lastupdated: "2024-04-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a generic operating system custom image
{: #create-generic-os-custom-image}

[Beta]{: tag-blue}

Generic operating system custom images is a beta feature that is available to select customers for evaluation and testing purposes. To request to be included in the evaluation of this beta feature, contact IBM Support. 
{: beta}

You can specify a generic operating system during image import to create an image that contains any operating system that you want. When you provision a server from a generic operating system custom image, the operating system-specific initialization actions aren't performed automatically. You must specify each specific action by using user data.

<!-- Reused information list of custom image requirements shared with vsi_is_about_custom_images.md- see conref.md file-->
{{site.data.content.custom-image-requirements-list}}

For more information about generic operating system custom images, see [Generic operating system custom images](/docs/vpc?topic=vpc-planning-custom-images#generic-os-custom-images) and [User data format considerations](/docs/vpc?topic=vpc-planning-custom-images#generic-os-custom-images).

## Step 1 - Start with a single image file in qcow2 or VHD format
{: #generic-os-single-image-qcow2}

If you're creating your own generic operating system custom image, begin with a single image file in qcow2 or VHD format. It might be helpful to begin with a cloud-enabled vendor image.

Kernel logs are important in debugging boot-related issues. To make sure that kernel logs are printed to the serial console, use the `console=ttyS0` kernel command line argument. In addition, the `nomodeset` and `nofb` kernel parameters are used to resolve display-related issues during the boot process for older kernels that were released before mid 2021. The newer kernels use the video mode setting into the kernel.
{: note}

## Step 2 - Set up the specific operating system
{: #generic-os-os-setup}

Set up the image with the specific operating system that you choose. Make sure that you set up the network drivers and the cloud-init or cloudbase-init options for the operating system.

- [Creating a custom Linux image](/docs/vpc?topic=vpc-create-linux-custom-image&interface=ui)
- [Creating a custom Windows image](/docs/vpc?topic=vpc-create-windows-custom-image)

## Step 3 - Verify the boot disk size
{: #generic-os-boot-disk-size}

Make sure that the image has a boot disk size of 10 - 250 GB. Images that are less than 10 GB are rounded up to 10 GB.

If you are customizing a virtual server to migrate from the Classic infrastructure, return to [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue the migration steps.
{: tip}

## Step 5 - Upload image to {{site.data.keyword.cos_full_notm}}
{: #generic-os-upload-image-icos}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your {{site.data.keyword.cloud}} Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-generic-os-image}

When your generic operating system custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import the custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc) and [Onboard a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). Make sure that you [Granted access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).

If you plan to use a private catalog to manage your custom images, you must first import that image into {{site.data.keyword.vpc_short}}, and then onboard the virtual server image into a private catalog.
{: note}

After the custom image imports, you can then use the custom image to deploy a virtual server in the {{site.data.keyword.vpc_full}} infrastructure.
