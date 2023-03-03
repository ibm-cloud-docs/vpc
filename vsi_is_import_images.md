---

copyright:
  years: 2019, 2022
lastupdated: "2022-11-04"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Importing and validating custom images into VPC
{: #importing-custom-images-vpc}

You can create your own custom image on premises and then import it to {{site.data.keyword.vpc_full}} infrastructure from {{site.data.keyword.cos_full}}. Then, you can use your custom image to create new virtual server instances that runs on the KVM hypervisor. If you plan to use your custom image in a private catalog, you must first import and validate the custom image into {{site.data.keyword.vpc_short}}.
{: shortdesc}

You can also create a custom image of a boot volume that is attached to a server at import time. For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc). 
{: note}

## Prerequisites
{: #pre-req-import-custom-image-cloud-object-storage}

To complete this task, you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).

{{site.data.content.custom-image-requirements-list}}

Keep the following considerations to keep in mind when you import a custom image:

* When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server from that image.  
* You cannot create an image from an encrypted boot volume (image from a volume) that is not 100 GB. The operation is blocked.
* For custom images with Red Hat Enterprise Linux&reg; or Windows&reg; operating systems, you must select the appropriate version of the operating system. Depending on how you configured the image, select either bring your own license `byol`, or if you plan to license the OS through {{site.data.keyword.cloud_notm}}, select the version without `byol` appended. 

{{site.data.content.custom-image-information-link}}

## Importing a custom image
{: #import-custom-image-cloud-object-storage}

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. To upload an image to {{site.data.keyword.cos_full_notm}}, on the **Objects** page of your bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image), [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image), [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
2. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
3. Click **Create**.
4. Complete the required fields that are described in Table 1 and click **Create Custom Image**.

| Field | Value |
|-------|-------|
| Name  | A name is required for your custom image. |
| Resource group | Select a resource group for the server. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Region | Select the location, or specific geographic area, where you want your custom image to be available for provisioning.|
| Source | Select **Cloud Object Storage** as the source. A list displays from which you select a Cloud Object Storage service instance where the image that you want to import is stored. \n Need to create a custom image from a volume instead? Select the source for the custom image by choosing either **Virtual server instance boot volume** or **Block storage boot volume**. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv). |
| Location | Select the specific geographic region where your image is stored. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored.|
| Name | Select the image file in the {{site.data.keyword.cos_full_notm}} service instance that you want to import. If you are importing an encrypted image, the image must be encrypted with LUKS encryption by using QEMU and your own passphrase. For more information, see [Encrypting the image](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image). |
| Operating System | Select the operating system that is included in your image. \n \n For custom images with Red Hat Enterprise Linux or Windows operating systems, you can bring your own license (BYOL) or to license through {{site.data.keyword.cloud_notm}}. \n \n For Red Hat Enterprise Linux or Windows [BYOL custom images](/docs/vpc?topic=vpc-byol-vpc-about), you must select the OS with `-byol` appended to the name. For example, if you have a Windows 2019 BYOL custom image, select *Windows-2019-amd64-byol* for the operating system. Failure to select the `-byol` version of the operating system when you import a BYOL custom image might result in a virtual server that is unable to start. \n \n If you configured your Red Hat Enterprise Linux or Windows custom image to license through {{site.data.keyword.cloud_notm}}, you must select the non-byol operating system. For example, if you have a Windows 2019 custom image that you plan to license through {{site.data.keyword.cloud_notm}}, select *Windows-2019-amd64* as the operating system when you import the custom image. |
| Encryption | The default selection is **No encryption**. If you have not encrypted your image by using QEMU, use the default value, **No encryption**. If you are importing an image that you have encrypted by using QEMU and your own passphrase, select the key management service where your customer root key (CRK) that protects your passphrase is stored. Select either **Key Protect** or **Hyper Protect Crypto Services**. VHD format images are not supported for encryption. |
| Encryption service instance | For an encrypted image, select the specific instance of the key management service where your CRK that wraps your encryption passphrase is stored. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Key name | Select the customer root key (CRK) that you used to wrap your encryption passphrase. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Wrapped data encryption key | For an encrypted image, specify the ciphertext that is associated with the wrapped data encryption key (WDEK). The WDEK is produced by wrapping the passphrase that you used to encrypt your image with your customer root key. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).|
{: caption="Table 1. Import custom image user interface fields" caption-side="bottom"}

## Validating an imported custom image
{: #validate-custom-images-cloud-object-storage}

After you import a custom image, you can view the checksum that was generated for the image when it is imported to {{site.data.keyword.vpc_short}}.

If you generate a checksum locally for your image before you import it, you can compare the two checksums to make sure that they are identical. Matching checksums indicate that the image is unaltered.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, click the name of the custom image that you want to validate. 
3. In the image details side panel, locate the **Checksum (SHA256)** field. You see content similar to, *6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7*. 
4. Compare the Checksum (SHA256) value to the output that is generated when you calculate a checksum for the image locally. 

   * Example Linux command: `sha256sum ubuntu_image.qcow2`
   * Example Mac command: `shasum -a 256 ubuntu_image.qcow2`
   * You receive output similar to: `6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7`

5. [Create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) using this image.

## Next steps
{: #next-validate-custom-image}

After you validate the custom images, you can deploy and manage your custom images. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui). If you plan to manage your custom images with a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
