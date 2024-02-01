---

copyright:
  years: 2019, 2024
lastupdated: "2024-02-01"

keywords: migrate virtual server from classic infrastructure, migrate to vpc, migrate image template, image template, import image to vpc infrastructure, migrate virtual server, migrate instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Migrating a virtual server from the classic infrastructure
{: #migrate-vsi-to-vpc}

You can migrate a virtual server instance from the classic infrastructure by customizing a backup version of the instance to meet the requirements of the {{site.data.keyword.vpc_short}} infrastructure. Next, create an image template from the instance and deploy it in the VPC environment.
{: shortdesc}

Migrating a virtual server instance from the classic infrastructure is not supported for LinuxONE (s390x processor architecture).
{: note}

To complete this task, you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).
{: important}

## Migrating an instance from the classic infrastructure
{: #migrate-vsi-from-classic-to-vpc-on-classic-task}

Complete the following steps to migrate an image template that is associated with a virtual server instance in the classic infrastructure to the {{site.data.keyword.vpc_short}} infrastructure. When the custom image is available in {{site.data.keyword.vpc_short}}, you can use it to create a virtual server.

The following list is an overview of the migration steps:

1. For the virtual server that you want to migrate to {{site.data.keyword.vpc_short}} infrastructure, create an image template.
2. From the image template, provision a virtual server instance, a backup instance.
3. Customize the backup virtual server instance to make sure that you meet the requirements for deploying in {{site.data.keyword.vpc_short}}.
4. Create an image template of your modified virtual server instance.
5. Export the image template to {{site.data.keyword.cos_full_notm}}.
6. Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure.
7. Use the custom image to create a virtual server instance in {{site.data.keyword.vpc_short}}.

For more information about using shell scripts to migrate an instance, see [Automate the migration of a workload based on Virtual Servers from Classic Infrastructure to VPC](https://www.ibm.com/cloud/blog/automate-the-migration-of-a-workload-based-on-virtual-servers){: external} and [vpc-tutorials](https://github.com/IBM-Cloud/vpc-tutorials/tree/master/vpc-migrate-from-classic){: external}.
{: tip}

### Step 1 - Identify the virtual server instance to migrate and create an image template
{: #migrate-create-template}

You can create an image template from a virtual server in the classic infrastructure that you want to migrate to the {{site.data.keyword.vpc_short}} infrastructure. The image template captures an image of the existing virtual server. Make sure that you understand the following information about image templates.

* Only image templates with a single primary boot volume (or disk) and associated file can be imported to {{site.data.keyword.vpc_short}} infrastructure.
* The image template includes the operating system on the primary boot disk and the items that you installed, such as PHP or Python. The image template can be between 10 GB and 250 GB of data. Images under 10 GB are rounded up to 10 GB.
* When you use the imported custom image in {{site.data.keyword.vpc_short}} to create a virtual server, you can select a new profile, assign an SSH key, specify user data, and configure network interfaces.

Secondary disks and their associated files for an image template are not supported when an image template is imported as a custom image to {{site.data.keyword.vpc_short}}.
{: important}

Complete the following steps to create an image template for the virtual server instance that you want to migrate.

1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **menu** ![menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Devices** > **Device List**.
2. Click the virtual server that you want to use.
3. From the **Actions** menu, select **Create Image Template**. Make sure to name it something you can easily recognize. For more information, see [Creating an image template](/docs/image-templates?topic=image-templates-creating-an-image-template).

### Step 2 - Locate the image template and provision a virtual server instance
{: #migrate-locate-template}

From the image template that you created, provision a virtual server instance. This instance is a backup. You can customize this backup virtual server to meet the requirements of {{site.data.keyword.vpc_short}}. Complete the following steps to create a new instance from the image template.

1. Locate the image template that you created on the **Image Templates** page by selecting **Devices > Manage > Images**.
2. Provision a backup virtual server instance from the image template by clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg) for the image template and selecting **Order Public virtual server instance**.

### Step 3 - Customize the virtual server instance for {{site.data.keyword.vpc_short}}
{: #migrate-customize-image-vpc}

Complete the required virtual server instance customizations to prepare your image to run in the {{site.data.keyword.vpc_short}} infrastructure. Some of the customization requirements might be already met on your classic infrastructure virtual server instance.

#### Customizing a Linux instance
{: #customize-linux-instance}

Follow the instructions in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) to customize on your Linux instance. Your instance needs to meet the following requirements.
* The following [arguments](/docs/vpc?topic=vpc-create-linux-custom-image#kernel-args) are present on the kernel command line: `nomodeset`, `nofb`, `vga=normal`, `console=ttyS0`.
* [Virtio drivers](/docs/vpc?topic=vpc-create-linux-custom-image#virtio-drivers) are installed, plus any code that is needed by Virtio.
* Your image is [cloud-init enabled](/docs/vpc?topic=vpc-create-linux-custom-image#cloud-init).
* For any auxiliary storage volumes that are mounted, you must include the fstab entry `nofail`.

#### Customizing a Windows instance
{: #customize-windows-instance}

Complete the following customizations on your Windows instance to prepare the image for {{site.data.keyword.vpc_short}}.
1. Use Remote Desktop to access your Windows server.
1. Obtain the required virtio-win drivers by [provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) or accessing an existing Red Hat Enterprise Linux virtual server in {{site.data.keyword.vpc_short}}. Then, install the virtio-win package on the server. Finally, copy the virtio-win ISO file, for example, *virtio-win-1.9.24.iso*, to use for your Windows custom image.

   1. On your Red Hat Enterprise Linux virtual server in {{site.data.keyword.vpc_short}}, install the virtio-win package by running the following command:

      ```sh
      yum install virtio-win
      ```
      {: codeblock}

      In this example, the virtio-win package is being installed on Red Hat Enterprise Linux version 8. Your returned output is similar to the following example:

      ```sh
      Installed:
        virtio-win-1.9.24-2.el8_5.noarch
      ```
      {: screen}

   1. Access the virtio-win ISO in the `/usr/share/virtio-win` directory.

      ```sh
      cd /usr/share/virtio-win/
      ```
      {: codeblock}

   1. Use SCP to copy the virtio-win ISO file, for example `virtio-win-1.9.24.iso`, to use for your Windows custom image.
1. Install and configure cloudbase-init by completing steps 4 through 8 in [Creating a Windows custom image > Customize the server](/docs/vpc?topic=vpc-create-windows-custom-image#customize-virtual-machine). Make sure to modify the cloudbase-init configuration files and Unattend file according to the instructions and then run Sysprep.

### Step 4 - Create an image template of your customized virtual server instance
{: #migrate-new-image template}

When your customizations are complete on your backup virtual server instance, create a new image template.
1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **menu** ![menu icon](../../icons/icon_hamburger.svg) > **Classic Infrastructure** > **Devices** > **Device List**.
2. Click the backup virtual server that you previously customized for {{site.data.keyword.vpc_short}}.
3. From the **Actions** menu, select **Create Image Template**.


### Step 5 - Export the image template to {{site.data.keyword.cos_full_notm}}
{: #migrate-export-template}

To export the new image template that you created from the modified virtual server instance to {{site.data.keyword.cos_full_notm}}, complete the following steps.
1. Locate the new image template that you created on the **Image Templates** page by selecting **Devices > Manage > Images**.
2. From the **Image Templates** page, click **...** for the image template that you want to export and select **Export to IBM COS**. For more information, see [Exporting an image to {{site.data.keyword.cos_full_notm}}](/docs/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage).

### Step 6 - Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure
{: #migrate-import-image}

1. In {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click **Import Custom Image**. For more information, see [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image).

### Step 7 - Use the custom image to create a virtual server instance in {{site.data.keyword.vpc_short}}
{: #migrate-create-virtual-server}

1. When the image that you imported is available on the **Custom images** tab of the **Images for VPC** page, you can use it to create a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure.
2. On the **Custom images** tab, identify the name of the custom image that you imported, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **New virtual server**.
