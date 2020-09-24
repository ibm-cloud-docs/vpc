---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-24"

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

You can migrate a virtual server instance from the classic infrastructure by customizing a backup version of the instance to meet the requirements of the {{site.data.keyword.vpc_short}} infrastructure. Next, create an image template from the instance, and export it to {{site.data.keyword.cos_full}}. After you convert the image to qcow2 format, you can import it to {{site.data.keyword.vpc_short}} and deploy it in the VPC environment. 
{:shortdesc}

To complete this task you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).
{:important}

## Migrating an instance from the classic infrastructure
{: #migrate-vsi-from-classic-to-vpc-on-classic-task}

Complete the following steps to migrate an image template that is associated with a virtual server instance in the classic infrastructure to the {{site.data.keyword.vpc_short}} infrastructure. When the custom image is available in {{site.data.keyword.vpc_short}}, you can use it to create a virtual server. 

Here is an overview of the steps:

1. For the virtual server that you want to migrate to {{site.data.keyword.vpc_short}} infrastructure, create an image template.
2. From the image template, provision a new virtual server instance, a backup instance.
3. Customize the backup virtual server instance to ensure that you meet the requirements for deploying in {{site.data.keyword.vpc_short}}.
4. Create an image template of your modified virtual server instance. 
5. Export the image template to {{site.data.keyword.cos_full_notm}}.
6. Download the image file from {{site.data.keyword.cos_full_notm}}, then convert VHD file to qcow2 format. After the image is converted to qcow2 format, upload it to {{site.data.keyword.cos_full_notm}}.
7. Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure.
8. Use the custom image to create a virtual server instance in {{site.data.keyword.vpc_short}}.

For an example of using shell scripts to migrate a classic instance to an {{site.data.keyword.vpc_short}} instance, see [Automate the Migration of a Workload Based on Virtual Servers from Classic Infrastructure to VPC](https://www.ibm.com/cloud/blog/automate-the-migration-of-a-workload-based-on-virtual-servers){: external} and [vpc-tutorials](https://github.com/IBM-Cloud/vpc-tutorials/tree/master/vpc-migrate-from-classic){: external}. 
{: tip}

### Step 1 - Identify the virtual server instance to migrate and create an image template
{: #migrate-create-template}

You can create an image template from a virtual server in the classic infrastructure that you want to migrate to the {{site.data.keyword.vpc_short}} infrastructure. The image template captures an image of the existing virtual server so that you can create a new one based on the captured image. Make sure you understand the following information about image templates. 

* Only image templates with a single primary boot volume (or disk) and associated file can be imported to {{site.data.keyword.vpc_short}} infrastructure. 
* The image template includes the operating system on the primary boot disk along with items you might have installed, such as PHP or Python, up to 100 GB of data. 
* When you use the imported custom image in {{site.data.keyword.vpc_short}} to create a new virtual server, you can select a new profile, assign an SSH key, specify user data, and configure network interfaces. 

Secondary disks and their associated files for an image template are not supported when importing an image template as a custom image to {{site.data.keyword.vpc_short}}.  
{:important}

Complete the following steps to create an image template for the virtual server instance that you want to migrate. 

1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **Menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Devices** > **Device List**. 
2. Click the virtual server that you want to use to create an image template. 
3. From the **Actions** menu, select **Create Image Template**. Make sure to name it something you will easily recognize. For more information, see [Creating an image template](/docs/image-templates?topic=image-templates-creating-an-image-template).
    
### Step 2 - Locate the image template and provision a new virtual server instance
{: #migrate-locate-template}

From the image template you just created, provision a new virtual server instance, a backup instance. You can customize this backup virtual server to meet the requirements of {{site.data.keyword.vpc_short}}. Complete the following steps to create a new instance from the image template.  

1. Locate the image template you created on the **Image Templates** page by selecting **Devices > Manage > Images**. 
2. Provision a backup virtual server instance from the image template by clicking the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the image template and selecting **Order Public VSI**.  

### Step 3 - Customize the virtual server instance for {{site.data.keyword.vpc_short}} 
{: #migrate-customize-image-vpc} 

Complete the required virtual server instance customizations to prepare your image to run in the {{site.data.keyword.vpc_short}} infrastructure. Some of the customization requirements might already be complete on your classic infrastructure virtual server instance.

#### Customizing a Linux instance 
{: #customize-linux-instance} 

Use the instructions in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) to complete the following customizations on your Linux instance:
* The following [arguments](/docs/vpc?topic=vpc-create-linux-custom-image#kernel-args) are present on the kernel command line: *nomodeset*, *nofb*, *vga=normal*, *console=ttyS0*. 
* Your operating system image has [virtio drivers](/docs/vpc?topic=vpc-create-linux-custom-image#virtio-drivers) installed, along with any code that is needed by virtio.
* Your image is [cloud-init enabled](/docs/vpc?topic=vpc-create-linux-custom-image#cloud-init).
* For any secondary storage volumes that are mounted, you must include the fstab entry “nofail.”

#### Customizing a Windows instance 
{: #customize-linux-instance} 

Complete the following customizations on your Windows instance to prepare the image for {{site.data.keyword.vpc_short}}. 
1. Use Remote Desktop to access your Windows server. 
2. Download and install the Windows VirtIO drivers. 
    1. Download the stable [virtio-win drivers ISO](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso){: external} from Fedora. 
    2. Locate the downloaded ISO and double-click it to mount it.
    3. From the mounted ISO run `virtio-win-guest-tools.exe` and complete the installation.
3. Install and configure Cloudbase-Init by completing steps 4.d through 4.h in [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image). Make sure to modify the Cloudbase-Init configuration files and Unattend file according to the instructions and then run Sysprep.    

### Step 4 - Create a new image template of your customized virtual server instance 
{: #migrate-new-image template} 

When your customizations are complete on your backup virtual server instance, create a new image template. 
1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **Menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Devices** > **Device List**. 
2. Click the backup virtual server where you have completed customization requirements for {{site.data.keyword.vpc_short}}. 
3. From the **Actions** menu, select **Create Image Template**.


### Step 5 - Export the image template to {{site.data.keyword.cos_full_notm}}
{: #migrate-export-template} 

To export the new image template that you created from the modified virtual server instance to {{site.data.keyword.cos_full_notm}}, complete the following steps.
1. Locate the new image template you created on the **Image Templates** page by selecting **Devices > Manage > Images**. 
2. From the **Image Templates** page, click **...** for the image template that you want to export and select **Export to IBM COS**. For more information, see [Exporting an image to {{site.data.keyword.cos_full_notm}}](/docs/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage).  

### Step 6 - Convert VHD file to qcow2 format
{: #migrate-convert-image} 

A custom image must be in qcow2 format in the {{site.data.keyword.vpc_short}}. You must convert the classic infrastructure VHD image template to be in qcow2 format. Complete the following steps to download the file from {{site.data.keyword.cos_full_notm}}, convert it from VHD to qcow2 format, and upload it again to {{site.data.keyword.cos_full_notm}}.

1. Download the image file from {{site.data.keyword.cos_full_notm}} to a secure local machine. On the **Objects** page of your {{site.data.keyword.cos_full_notm}} bucket, locate your image, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg), and select **Download**. You can use the Aspera high-speed transfer plug-in to download images larger than 200 MB.
2. Convert the VHD image file to qcow2 format by using the `qemu-img convert` command. (Make sure that your [QEMU](https://www.qemu.org/){: external} version is at version 2.12 or later.) For example, you can run the following command to convert a VHD image to qcow2 format, `qemu-img convert -f vpc -O qcow2 -o cluster_size=512k <filename>.vhd <filename>.qcow2`
3. Verify that the qcow2 image is valid by running the following commands: `qemu-img info <filename>.qcow2` and `qemu-img check <filename>.qcow2`. If the commands return no errors, continue to the next step.  
4. Upload the qcow2 image to {{site.data.keyword.cos_full_notm}}. Make sure that your customized image file has a descriptive name so that you can easily identify it later. On the **Objects** page of your {{site.data.keyword.cos_full_notm}} bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.

### Step 7 - Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure
{: #migrate-import-image} 

1. In {{site.data.keyword.cloud_notm}} console, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**. 
2. Click **Import Custom Image**. For more information, see [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image).

### Step 8 - Use the custom image to create a virtual server instance in {{site.data.keyword.vpc_short}}
{: #migrate-create-virtual-server} 

1. When the image that you imported is available on the **Custom Images for VPC** page, you can use it to create a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure. 
2. On the **Custom Images for VPC** page, identify the name of the custom image that you imported, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **New virtual server**.
