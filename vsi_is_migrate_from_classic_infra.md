---

copyright:
  years: 2019, 2020
lastupdated: "2019-12-18"

keywords: migrate virtual server from classic infrastructure, migrate to vpc, migrate image template, image template, import image to vpc infrastructure, migrate virtual server, migrate instance

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

# Migrating a virtual server from the classic infrastructure
{: #migrate-vsi-to-vpc}

You can migrate a virtual server instance from the classic infrastructure by creating an image template and exporting it to 
{{site.data.keyword.cos_full}}. Then, you can customize the image to meet the requirements of the {{site.data.keyword.vpc_short}} infrastructure, and 
import it to {{site.data.keyword.vpc_short}} so that it can be deployed. 
{:shortdesc}

## Before you begin
{: #prereq-migrate}

To import a custom image to {{site.data.keyword.vpc_short}}, you must have an instance of {{site.data.keyword.cos_full_notm}} available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. Finally, you must create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}.

### Creating an {{site.data.keyword.cos_full_notm}} service instance
{: #migrate-prereq-icos-instance}

If you need to create an instance of {{site.data.keyword.cos_full_notm}}, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started).


### Granting access between services
{: #migrate-prereq-create-service-authorization}

From IBM {{site.data.keyword.iamshort}}, you must create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. 

1. From the [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external} menu bar, click **Manage** &gt; **Access (IAM)**, and select **Authorizations**.
2. Click **Create**.
3. Select a source and target service for the authorization. Specify **VPC Infrastructure** as the source service. Specify **Image Service for VPC** as the resource type. Specify **Cloud Object Storage** as the target service.
4. Select a role to assign access to the source service that accesses the target service.
5. Click **Authorize**.

For more information, see [Using authorizations to grant access between services](/docs/iam?topic=iam-serviceauth#serviceauth).

## Migrating an instance from the classic infrastructure
{: #migrate-vsi-from-classic-to-vpc-on-classic-task}

Complete the following steps to migrate an image template that is associated with a virtual server instance in the classic infrastructure to the {{site.data.keyword.vpc_short}} infrastructure. When the custom image is available in {{site.data.keyword.vpc_short}}, you can use it to create a virtual server.

For an example of using shell scripts to migrate a classic instance to an {{site.data.keyword.vpc_short}} instance, see [Automate the Migration of a Workload Based on Virtual Servers from Classic Infrastructure to VPC](https://www.ibm.com/cloud/blog/automate-the-migration-of-a-workload-based-on-virtual-servers){: external} and [vpc-tutorials](https://github.com/IBM-Cloud/vpc-tutorials/tree/master/vpc-migrate-from-classic){: external}. 
{: tip}

### Step 1 - Create an image template
{: #migrate-create-template}

You can create an image template from a virtual server in the classic infrastructure that you want to migrate to the {{site.data.keyword.vpc_short}} infrastructure. The virtual server that the image template is created from must use a [cloud-init enabled image](/docs/infrastructure/image-templates?topic=image-templates-provisioning-with-a-cloud-init-enabled-image) that was provisioned without add-ons. (Add-ons include additional software, post-provisioning scripts, and advanced monitoring.) Make sure that you complete any customization of the virtual server instance before you create the image template.

The image template captures an image of the existing virtual server so that you can create a new one based on the captured image. Only image templates with a single primary boot volume (or disk) and associated file can be imported to {{site.data.keyword.vpc_short}} infrastructure. The image template includes the operating system on the primary boot disk along with items you might have installed, such as PHP or Python, up to 100 GB of data. When you use the imported custom image in {{site.data.keyword.vpc_short}} to create a new virtual server, you can select a new profile, assign an SSH key, specify user data, and configure network interfaces. 

Secondary disks and their associated files for an image template are not supported when importing an image template as a custom image to {{site.data.keyword.vpc_short}}.  
{:important}

1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **Menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Devices** > **Device List**. 
2. Click the virtual server that you want to use to create an image template. 
3. From the **Actions** menu, select **Create Image Template**. 

For more information, see [Creating an image template](/docs/infrastructure/image-templates?topic=image-templates-creating-an-image-template).
    
### Step 2 - Locate the image template
{: #migrate-locate-template}

Access the **Image Templates** page by selecting **Devices > Manage > Images**. 

Locate the image template that you want to migrate. For more information about accessing image templates in the classic infrastructure, see [Managing images](/docs/infrastructure/image-templates?topic=image-templates-managing-images-from-the-image-templates-page).
 
### Step 3 - Export the image template to {{site.data.keyword.cos_full_notm}}
{: #migrate-export-template} 
 
From the **Image Templates** page, click **...** for the image template that you want to export and select **Export to IBM COS**. 

For more information, see [Exporting an image to {{site.data.keyword.cos_full_notm}}](/docs/infrastructure/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage).  

### Step 4 - Customize the image for {{site.data.keyword.vpc_short}} infrastructure
{: #migrate-customize-image-vpc} 

1. Download the image file from {{site.data.keyword.cos_full_notm}} to a secure local machine to complete {{site.data.keyword.vpc_short}} 
customization requirements. On the **Objects** page of your {{site.data.keyword.cos_full_notm}} bucket, locate your image, click **...**, and select **Download**. You can use the Aspera high-speed transfer plug-in to download images larger than 200 MB.
2. With your classic infrastructure image template downloaded, complete the required image customizations for {{site.data.keyword.vpc_short}} infrastructure. For more information, see 
[Creating a custom image](/docs/vpc?topic=vpc-managing-images#create-deployable-custom-image). Follow the instructions for a [Linux custom image](/docs/vpc?topic=vpc-managing-images#create-linux-custom-image). Some of the customization requirements 
might already be complete on your classic infrastructure image template. 
3. When the image customization requirements are complete, you can upload the image to {{site.data.keyword.cos_full_notm}}. Make sure that your customized image file has a descriptive name so that you can easily identify it later. On the **Objects** page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.

### Step 5 - Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure
{: #migrate-import-image} 

1. In {{site.data.keyword.cloud_notm}} console, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**. 
2. Click **Import Custom Image**. 

For more information, see [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image).

### Step 6 - Use the custom image to create a virtual server instance in {{site.data.keyword.vpc_short}}
{: #migrate-create-virtual-server} 

1. When the image that you imported is available on the **Custom Images for VPC** page, you can use it to create a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure. 
2. On the **Custom Images for VPC** page, identify the name of the custom image that you imported, click **...** and select **New virtual server**.
