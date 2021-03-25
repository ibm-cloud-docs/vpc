---

copyright:
  years: 2019, 2021
lastupdated: "2021-03-19"

keywords: custom image

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Importing and managing custom images 
{: #managing-images}

You can create your own custom image on premises and then import it to {{site.data.keyword.vpc_full}} infrastructure from {{site.data.keyword.cos_full_notm}}. Then, you can use your custom image to create new virtual server instances that runs on the KVM hypervisor.
{:shortdesc}

You can also create a custom image of a boot volume attached to an instance at import time. For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{:note}

## Importing a custom image
{: #import-custom-image}

When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server instances from that image.  

To complete this task you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).
{:important}

Make sure that your image meets custom image requirements:
* Contains a single file or volume 
* Is in qcow2 or VHD format
* Is cloud-init enabled
* The operating system is supported as a [stock image](/docs/vpc?topic=vpc-about-images#stock-images) operating system

  For custom images with Red Hat Enterprise Linux (RHEL) or Windows operating systems, you must select the bring your own license (BYOL) operating system version when you import the custom image. For example, if you have a Windows 19 custom image, select *Windows-2019-amd64-byol* for the operating system when you import the custom image. For more information, see [Bring your own license (Beta)](/docs/vpc?topic=vpc-byol-vpc-about). 
  {: important}

* Size doesn't exceed 100 GB

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. To upload an image to {{site.data.keyword.cos_full_notm}}, on the **Objects** page of your bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) or [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
2. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
3. Click **Create**. 
4. Complete the required fields described in Table 1 and click **Create Custom Image**.

| Field | Value |
|-------|-------|
| Name  | A name is required for your custom image. |
| Resource group | Select a resource group for the instance. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Region | Select the location, or specific geographic area, where you want your custom image to be available for provisioning.|
| Source | Select **Cloud Object Storage** as the source. A list displays from which you select a COS service instance where the image that you want to import is stored. |
| Location | Select the specific geographic region where your image is stored. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored.|
| Name | Select the image file in the {{site.data.keyword.cos_full_notm}} service instance that you want to import. If you are importing an encrypted image, the image must be encrypted with LUKS encryption by using QEMU and your own passphrase. For more information, see [Encrypting the image](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image). |
| Operating System | Select the operating system that is included in your image. For Red Hat Enterprise Linux or Windows [BYOL custom images](/docs/vpc?topic=vpc-byol-vpc-about), you must select the OS with with `-byol` appended to the name. For example, if you have a Windows 19 custom image, select *Windows-2019-amd64-byol* for the operating system. Failure to select the `-byol` version of the operating system when importing a BYOL custom image might result in a virtual server instance that is unable to start. |
| Encryption | The default selection is **No encryption**. If you have not encrypted your image by using QEMU, use the default value, **No encryption**. If you are importing an image that you have encrypted by using QEMU and your own passphrase, select the key management service where your customer root key (CRK) that protects your passphrase is stored. Select either **Key Protect** or **Hyper Protect Crypto Services**. VHD format images are not suported for encryption. |
| Encryption service instance | For an encrypted image, select the specific instance of the key management service where your CRK that wraps your encryption passphrase is stored. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Key name | Select the customer root key (CRK) that you used to wrap your encryption passphrase. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Wrapped data encryption key | For an encrypted image, specify the ciphertext that is associated with the wrapped data encryption key (WDEK). The WDEK is produced by wrapping the passphrase that you used to encrypt your image with your customer root key. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).|
{: caption="Table 1. Import custom image user interface fields" caption-side="top"}

## Validating a custom image after importing (Beta)
{: #validate-custom images}

After you import a custom image, you can view the checksum that's generated for the image when it is imported to {{site.data.keyword.vpc_short}}. If you generate a checksum locally for your image before importing it, you can compare the two checksums to ensure that they are identical. Matching checksums indicate that the image is unaltered. 

The checksum feature is currently in beta. 
{:beta}

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, click the name of the custom image that you want to validate. 
3. In the image details side panel that displays, locate the **Checksum (SHA256)** field. You'll see content similar to, *6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7*. 
4. Compare the Checksum (SHA256) value to the output that is generated when you calculate a checksum for the image locally. For more information, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images). 

## Managing custom images
{: #managing-custom images}

After you import custom images, you can deploy and manage them from the Custom images page. On the Custom images page you can rename an image, create a new virtual server from the image, copy the UUID of the image, view the checksum for the image, or delete a custom image.

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, you can click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options. Encrypted custom images are identified by a lock icon after the image name.
