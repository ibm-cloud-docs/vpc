---

copyright:
  years: 2019
lastupdated: "2019-10-17"

keywords: custom image, vpc, virtual private cloud, virtual server instances

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

## Before you begin
{: #prereq-custom-images}

To import a custom image to {{site.data.keyword.vpc_full}}, you must have an instance of {{site.data.keyword.cos_full_notm}} available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. 

1. If you don't already have an instance of {{site.data.keyword.cos_full_notm}}, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started).
2. If you need to upload an image to {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to 
[upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) the image. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.
3. From IBM {{site.data.keyword.iamshort}} (IAM), [create an authorization](/docs/iam?topic=iam-serviceauth#serviceauth) between **VPC Infrastructure** (source service) > **Image Service for VPC** (resource type) and **Cloud Object Storage** (target service).
4. Make sure that your image meets custom image requirements:
  - Contains a single file or volume 
  - Is in qcow2 format
  - Is cloud-init enabled
  - The operating system is supported as a stock image operating system
  - Size doesn't exceed 100 GB

Encrypted images aren't supported for custom images.
{: note}

## Creating a custom image 
{: #create-deployable-custom-image}

You can create your own custom Linux-based image to deploy a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure. 

### Creating a Linux custom image
{: #create-linux-custom-image}

Complete the following steps to ensure that your own Linux custom image can be successfully deployed in the {{site.data.keyword.vpc_short}} infrastructure environment.

1. Begin with a single image file in qcow2 format. You can convert an image to qcow2 format by using the `qemu-img convert` command. (Make sure that your [QEMU](https://www.qemu.org/){: external} version is at version 2.12 or later.) For example, you can run the following command to convert a VHD image to qcow2 format, `qemu-img convert -f vpc -O qcow2 -o cluster_size=512k <filename>.vhd <filename>.qcow2`

2. Make sure that the following arguments are present on the kernel command line:
    * nomodeset
    * nofb
    * vga=normal
    * console=ttyS0
    
3. Ensure that your operating system image has virtio drivers installed.
    * `virtio-blk` driver
    * Any code needed by virtio must also be installed.
    * Virtio network drivers are required to enable networking.
    
4. Make sure that the guest image, in its description of the network interfaces, has at least one network interface set to auto-configure.    
        
5. Make sure that your image is cloud-init enabled. 

    * For more information about configuring images, see
[cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/){: external}.
    * For more information about datasources, see [Datasources](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html){: external}. {{site.data.keyword.cloud_notm}} cloud-init images are created for the
environment by using the [NoCloud](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html){: external} data source to supply the metadata.
    * Make sure to configure your image to use SSH for logging in to your virtual server instance.
    * Linux images require Cloud-init version 0.7.7 or greater.
    
6. Upload your image to {{site.data.keyword.cos_full_notm}}. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload).    

## Importing a custom image
{: #import-custom-image}

When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server instances from that image.  

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. For more information, see [Creating a custom image](/docs/vpc?topic=vpc-managing-images#create-deployable-custom-image) and [Uploading data](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
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
