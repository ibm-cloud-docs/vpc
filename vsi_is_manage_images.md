---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-16"

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
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Importing and managing custom images
{: #managing-images}

You can create your own custom image, and import it to {{site.data.keyword.vpc_full}} infrastructure from {{site.data.keyword.cos_full_notm}}. Then, you can use your custom image to create new virtual server instances that run on the KVM hypervisor.
{:shortdesc}

## Importing a custom image
{: #import-custom-image}

When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server instances from that image.  

To complete this task you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).
{:important}

Make sure that your image meets custom image requirements:
* Contains a single file or volume 
* Is in qcow2 format
* Is cloud-init enabled
* The operating system is supported as a stock image operating system
* Size doesn't exceed 100 GB

Encrypted images aren't supported for custom images.
{: note}

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. To upload an image to {{site.data.keyword.cos_full_notm}}, on the **Objects** page of your bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) or [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
2. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
3. Click **Import Custom Image**. 
4. Complete the required fields and click **Create Custom Image**.

## Managing custom images
{: #managing-custom images}

After you import custom images, you can deploy and manage them from the Custom images page. 

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, you can click **...** and select from the available options. 
