---

copyright:
  years: 2020, 2026
lastupdated: "2026-02-20"

keywords: migrate virtual server from classic infrastructure, migrate to vpc, migrate image template, image template, import image to vpc infrastructure, migrate virtual server, migrate instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Migrating a virtual server from the Classic infrastructure
{: #migrate-vsi-to-vpc}

You can migrate a virtual server instance from the Classic infrastructure to meet the {{site.data.keyword.vpc_short}} infrastructure requirements. Then, you can create an image template from the instance and deploy it in the VPC environment.
{: shortdesc}

[Deprecated]{: tag-deprecated} Migrating a virtual server instance from the Classic infrastructure isn't supported for LinuxONE (s390x processor architecture).
{: note}

## Before you begin
{: #before-migrating-vsi-from-classic}

Before you begin, make sure that the following prerequisites are fulfilled.

- You must have an instance of {{site.data.keyword.cos_full}} available.
- You must create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).

## Migrating a virtual server from the Classic infrastructure
{: #migrate-vsi-from-classic-to-vpc-on-classic-task}

Complete the following steps to migrate an image template that is associated with a virtual server in the Classic infrastructure to the {{site.data.keyword.vpc_short}} infrastructure. When the custom image is available in {{site.data.keyword.vpc_short}}, you can use it to create a virtual server.

The following list is an overview of the migration steps.

1. For the virtual server that you want to migrate to {{site.data.keyword.vpc_short}} infrastructure, create an image template. The image template must be created before you proceed with the following migration steps.
2. From the image template, provision a virtual server that you want to use as the backup instance.
3. Customize the backup virtual server to make sure that it meets the requirements to deploy in {{site.data.keyword.vpc_short}}.
4. Create an image template of your modified virtual server.
5. Export the image template to {{site.data.keyword.cos_full_notm}}.
6. Import the custom image to the {{site.data.keyword.vpc_short}} infrastructure.
7. Use the custom image to create a virtual server in {{site.data.keyword.vpc_short}}.

For more information about using shell scripts to migrate a virtual server, see [vpc-tutorials](https://github.com/IBM-Cloud/vpc-tutorials/tree/master/vpc-migrate-from-classic){: external}. For more information about migrating from Classic to {{site.data.keyword.vpc_short}}, see [Getting started with VPC+ Cloud Migration](/docs/wanclouds-vpc-plus?topic=wanclouds-vpc-plus-getting-started-tutorial).
{: tip}

### Step 1 - Identifying the virtual server to migrate and create an image template
{: #migrate-create-template}

You can create an image template from a virtual server in the Classic infrastructure that you want to migrate to the {{site.data.keyword.vpc_short}} infrastructure. The image template captures an image of the existing virtual server. Make sure that you understand the following information about image templates.

* Only image templates with a single primary boot volume (or disk) and associated files can be imported to {{site.data.keyword.vpc_short}} infrastructure.
* The image template includes the operating system on the primary boot disk and the items that you installed, such as PHP or Python. The image template can be between 10 GB and 250 GB of data. Images under 10 GB are rounded up to 10 GB.
* When you use the imported custom image in {{site.data.keyword.vpc_short}} to create a virtual server, you can select a new profile, assign an SSH key, specify user data, and configure network interfaces.

Secondary disks and their associated files for an image template aren't supported when an image template is imported as a custom image to {{site.data.keyword.vpc_short}}.
{: important}

Complete the following steps to create an image template for the virtual server that you want to migrate.

1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, create an image template by clicking **Navigation menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Infrastructure** > **Classic Infrastructure** > **Devices** > **Device List**.
2. Click the virtual server that you want to use.
3. From the **Actions** menu, select **Create Image Template**. Make sure to name it something you can easily recognize. For more information, see [Creating an image template](/docs/image-templates?topic=image-templates-creating-an-image-template).

### Step 2 - Locate the image template and provision a virtual server
{: #migrate-locate-template}

From the image template that you created, provision a virtual server. This instance is a backup. You can customize this backup virtual server to meet the requirements of {{site.data.keyword.vpc_short}}.

Complete the following steps to create a new virtual server from the image template.

1. Click **Devices > Manage > Images** to locate the image template that you created.
2. Provision a virtual server from the image template by clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg) for the image template and selecting **Order Public virtual server instance**.

### Step 3 - Customize the virtual server for {{site.data.keyword.vpc_short}}
{: #migrate-customize-image-vpc}

Complete the following customizations to prepare your image for the {{site.data.keyword.vpc_short}} infrastructure. Some of the customization requirements might already be met on your Classic infrastructure virtual server instance.

#### Customizing a Linux virtual server
{: #customize-linux-instance}

Follow the instructions in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) to customize on your Linux instance. Your instance needs to meet the following requirements.
   * The following arguments are present on the kernel command line: `nomodeset`, `nofb`, `vga=normal`, `console=ttyS0`.
   * [Virtio drivers](/docs/vpc?topic=vpc-create-linux-custom-image#virtio-drivers) are installed, plus any code that is needed by Virtio.
   * Your image is [cloud-init enabled](/docs/vpc?topic=vpc-create-linux-custom-image#cloud-init).
   * For any auxiliary storage volumes that are mounted, you must include the _fstab_ entry `nofail`.

#### Customizing a Windows instance
{: #customize-windows-instance}

Complete the following customizations on your Windows virtual server to prepare the image for {{site.data.keyword.vpc_short}}.

1. Use Remote Desktop to access your Classic Windows server.
1. Download and install the Windows VirtIO drivers in this server. The `virtio-win` driver files can be taken from an existing {{site.data.keyword.vpc_short}} virtual server by using the following steps.


   Microsoft recommends that you obtain the drivers from a licensed RHEL version 8 or 9 instance because drivers that are obtained from Red Hat are certified by Microsoft. The minimum recommended Red Hat virtio-win package version is `virtio-win-1.9.24`. However, using the most recent package is best.
   {: note}

   The Red Hat `virtio-win-1.9.24` ISO contains the following specific driver versions.

   ```text
   100.84.104.19500 oem10.inf \vioprot.inf_amd64_af0659efdaba9e4b\vioprot.inf
   100.90.104.21400 oem11.inf \viofs.inf_amd64_c6f785e21f3f6f80\viofs.inf
   100.85.104.20200 oem12.inf \viogpudo.inf_amd64_b19dcf9947e73e5a\viogpudo.inf
   100.85.104.19900 oem13.inf \vioinput.inf_amd64_4505a789e17b5f89\vioinput.inf
   100.81.104.17500 oem14.inf \viorng.inf_amd64_ef304eab276a3e61\viorng.inf
   100.85.104.19900 oem15.inf \vioser.inf_amd64_cb4783c018c10eba\vioser.inf
   100.90.104.21500 oem2.inf  \vioscsi.inf_amd64_02a46a7a223648d1\vioscsi.inf
   100.90.104.21500 oem3.inf  \viostor.inf_amd64_520417bbc533faba\viostor.inf
   100.85.104.20700 oem4.inf  \balloon.inf_amd64_afa8c93081df5458\balloon.inf
   100.90.104.21400 oem5.inf  \netkvm.inf_amd64_0efff05c07fcee39\netkvm.inf
   100.85.104.19900 oem6.inf  \pvpanic.inf_amd64_b7028360ef636f8b\pvpanic.inf
   10.0.0.21000     oem9.inf  \qxldod.inf_amd64_6199f9ecf2339133\qxldod.inf
   ```
   {: screen}

   1. Obtain the required virtio-win drivers by provisioning or accessing an existing RHEL virtual server in {{site.data.keyword.vpc_short}}.
   1. On your RHEL virtual server that you provisioned in {{site.data.keyword.vpc_short}}, install the `virtio-win` package by running the following command. In this example, the virtio-win package is installed on RHEL version 8.

      ```sh
      yum install virtio-win
      ```      {: pre}

      The output looks like the following example.

      ```text
      Installed: virtio-win-1.9.24-2.el8_5.noarch
      ```
      {: screen}

   1. Access the virtio-win ISO in the `/usr/share/virtio-win` directory.

      ```sh
      cd /usr/share/virtio-win/
      ```
      {: pre}

   1. Use the WinSCP utility to copy the `virtio-win` ISO file, such as `virtio-win-1.9.24.iso` from the RHEL VPC server to the Classic Windows server.
   1. Locate the downloaded ISO and double-click it to mount it.
   1. From the mounted ISO, run `virtio-win-guest-tools.exe` and complete the installation.
      1. Locate the downloaded ISO and double-click it to mount it.
      1. From the mounted ISO, run `virtio-win-guest-tools.exe` and complete the installation.
1. Install and configure cloudbase-init from [Cloudbase-Init installation package](https://www.cloudbase.it/downloads/CloudbaseInitSetup_1_1_5_x64.msi){: external}.
1. Modify the `cloudbase-init.conf` file (`C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init.conf`) to match the following values. Don't remove any other content from the file.

   ```text
   [DEFAULT]
   #  "cloudbase-init.conf" is used for every boot
   config_drive_types=vfat,iso
   config_drive_locations=hdd,partition
   activate_windows=true
   kms_host=kms.adn.networklayer.com:1688
   mtu_use_dhcp_config=false
   real_time_clock_utc=false
   bsdtar_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\bsdtar.exe
   mtools_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\
   debug=true
   log_dir=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\
   log_file=cloudbase-init.log
   default_log_levels=comtypes=INFO,suds=INFO,iso8601=WARN,requests=WARN
   local_scripts_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\LocalScripts\
   metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService,
   # enabled plugins - executed in order
   plugins=cloudbaseinit.plugins.common.mtu.MTUPlugin,
             cloudbaseinit.plugins.windows.ntpclient.NTPClientPlugin,
             cloudbaseinit.plugins.windows.licensing.WindowsLicensingPlugin,
             cloudbaseinit.plugins.windows.extendvolumes.ExtendVolumesPlugin,
             cloudbaseinit.plugins.common.userdata.UserDataPlugin,
             cloudbaseinit.plugins.common.localscripts.LocalScriptsPlugin
   ```
   {: screen}

   If you plan to bring your own license for your custom image, remove the following lines from the `cloudbase-init.conf` file.

   ```text
   activate_windows=true
   kms_host=kms.adn.networklayer.com:1688
   ```
   {: screen}

1. Modify the `cloudbase-init-unattend.conf` file (`C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init-unattend.conf`) to match the following values. Don't remove any other content from the file.

   ```text
   [DEFAULT]
   #  "cloudbase-init-unattend.conf" is used during the Sysprep phase
   username=Administrator
   inject_user_password=true
   first_logon_behaviour=no
   config_drive_types=vfat
   config_drive_locations=hdd
   allow_reboot=false
   stop_service_on_exit=false
   mtu_use_dhcp_config=false
   bsdtar_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\bsdtar.exe
   mtools_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\
   debug=true
   log_dir=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\
   log_file=cloudbase-init-unattend.log
   default_log_levels=comtypes=INFO,suds=INFO,iso8601=WARN,requests=WARN
   local_scripts_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\LocalScripts\
   metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService,
   # enabled plugins - executed in order
    plugins=cloudbaseinit.plugins.common.mtu.MTUPlugin,
             cloudbaseinit.plugins.common.sethostname.SetHostNamePlugin,
             cloudbaseinit.plugins.windows.createuser.CreateUserPlugin,
             cloudbaseinit.plugins.windows.extendvolumes.ExtendVolumesPlugin,
             cloudbaseinit.plugins.common.setuserpassword.SetUserPasswordPlugin,
             cloudbaseinit.plugins.common.localscripts.LocalScriptsPlugin
   ```
   {: screen}

1. Modify the `Unattend.xml` file (`C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml`) and set the `PersistAllDeviceInstalls` value to `false`.
1. Run `Sysprep` by using the following command from the command prompt.

   ```sh
   C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/unattend:C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml"
   ```
   {: pre}

   This command shuts down the system.
   {: important}

### Step 4 - Creating an image template of your customized virtual server
{: #migrate-new-image-template}

When your customizations are complete on your backup virtual server, create a new image template by using the following steps.

1. From the Dashboard in [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}, click **menu** ![Menu icon](../../icons/icon_hamburger.svg) > **Infrastructure** > **Classic Infrastructure** > **Devices** > **Device List** to create an image template.
1. Click the backup virtual server that you previously customized.
1. From the **Actions** menu, select **Create Image Template**. The image template must be created before you can proceed with the following steps.

### Step 5 - Exporting the image template to {{site.data.keyword.cos_full_notm}}
{: #migrate-export-template}

To export the image template that you created from the modified virtual server to {{site.data.keyword.cos_full_notm}}, complete the following steps.

1. Locate the new image template that you created on the **Image Templates** page by clicking **Devices > Manage > Images**.
1. From the **Image Templates** page, click **...** for the image template that you want to export and select **Export to IBM Cloud Object Storage**. For more information, see [Exporting an image to {{site.data.keyword.cos_full_notm}}](/docs/image-templates?topic=image-templates-exporting-an-image-to-ibm-cloud-object-storage).

### Step 6 - Importing the custom image to the {{site.data.keyword.vpc_short}} infrastructure
{: #migrate-import-image}

1. In the {{site.data.keyword.cloud_notm}} console, click **Navigation menu** icon ![Menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
1. On the **Custom images** tab, click **Import Custom Image**. For more information, see [Importing a custom image](/docs/vpc?topic=vpc-importing-custom-images-vpc).

### Step 7 - Using a custom image to create a virtual server in {{site.data.keyword.vpc_short}}
{: #migrate-create-virtual-server}

When the image that you imported is available on the **Custom images** tab of the **Images for VPC** page, you can use it to create a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure.

1. On the **Custom images** tab, find the name of the custom image that you imported, click Actions ![More Actions icon](../icons/action-menu-icon.svg), and select **New virtual server**.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, go to **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. On the **Virtual server instances** page, click Actions ![More Actions icon](../icons/action-menu-icon.svg). Stop and then start the virtual server before you access it.
1. Create inbound and outbound security groups to give access to the RDP traffic port 3389. For more information, see [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group&interface=ui).
1. To generate a password to allow access to Windows and RDP with a floating IP, see [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).
