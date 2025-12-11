---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-10"

keywords: creating a Windows custom image, cloudbase-init, qcow2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a custom Windows&reg; image
{: #create-windows-custom-image}

You can create your own custom Windows&reg;-based image to import the custom image into {{site.data.keyword.vpc_full}}. Then, you can use the custom image to deploy a virtual server or bare metal server in your {{site.data.keyword.vpc_full}} infrastructure.
{: shortdesc}

## Before you begin
{: #create-win-custom-image-before-you-begin}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} Classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc). You can also create a custom image of a boot volume that is attached to a server at import time. For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc). Keep in mind that Windows&reg; custom images aren't supported for LinuxONE (s390x processor architecture).

{{site.data.content.custom-image-requirements-list}}

You can't create an image from an encrypted boot volume that is not 100 GB.
{: note}

Use the following steps to create a Windows&reg; custom image to deploy in the {{site.data.keyword.vpc_short}} infrastructure environment. The procedure encompasses the following high-level tasks.

* Use VirtualBox to create a Windows&reg; image in VHD format.
* Customize the image with virtIO drivers and Cloudbase-init.

Keep the following requirements in mind before you create a Windows&reg; custom image.

Virtio-win drivers must be installed. Microsoft&reg; recommends that you obtain the drivers from a licensed RHEL version 8 or 9 instance. Red Hat uses Microsoft-certifed drivers. The minimum recommended Red Hat virtio-win package version is `virtio-win-1.9.24`. However, using the most recent package is best.
{: requirement}

The Red Hat virtio-win-1.9.24 ISO contains the following specific driver versions.

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

## 1. Creating the Windows&reg; custom image
{: #create-win-custom-image-first-steps}

Use the following steps to create a custom Windows&reg; image.

1. You can download the evaluation version of [Windows 2016](https://www.microsoft.com/en-gb/evalcenter/evaluate-windows-server-2016){: external}, [Windows 2019](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019){: external}, or [Windows 2022](https://www.microsoft.com/en-gb/evalcenter/evaluate-windows-server-2022){: external} operating system ISOs.
1. With the `qemu-img` utility, create a VHD image where you want to install Windows. Download the [qemu-img utility](https://cloudbase.it/downloads/qemu-img-win-x64-2_3_0.zip){: external} and extract it to your Windows laptop client. In the folder where you extracted the `qemu-img` utility, create the VHD image.

   ```sh
   qemu-img.exe create -f vpc Windows-2019.vhd 100G
   ```
   {: codeblock}

    {{site.data.keyword.cloud}} supports custom image import with VHD or qcow2. However, Virtual Box doesn't support the qcow2 format.
    {: note}

1. Obtain the required virtio-win drivers by [provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) or accessing an existing RHEL server in {{site.data.keyword.vpc_short}}.

   1. On your RHEL server in {{site.data.keyword.vpc_short}}, install the virtio-win package by running the following command.

      ```sh
      yum install virtio-win
      ```
      {: codeblock}

      In this example, the virtio-win package installs on RHEL version 8. Your returned output is similar to the following example.

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

   1. Use a secure copy to copy the virtio-win ISO file, for example `virtio-win-1.9.24.iso`, to use for your Windows&reg; custom image.

      * Use winSCP to copy the ISO file on a &reg; client.
      * Use SCP to copy the ISO file on a Linux or macOS client.

   1. Mount and open the ISO file.
   1. Copy all relevant virtio drivers from `virtio-win.iso` file for the respective operating system, put the drivers into a folder called `Drivers`, then copy the `Drivers` folder into the operating system ISO folder.
   1. Download the [Windows Assessment and Deployment toolkit (ADK)](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install){: external}. To get the `oscdimg.exe` utility, install only the Deployment Tools.
   1. Create a bootable Windows ISO that incorporates all virtio drivers in the `Drivers` folder by using the `oscdimg.exe` command.

      ```sh
      C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\x86\Oscdimg> oscdimg -m -bC:\..\..\Downloads\<extracted_ISO_folder>\boot\etfsboot.com -u2 C:\..\..\Downloads\extracted_ISO_folder  C:\..\..\Downloads\win2019new.iso
      ```
      {: pre}

1. Use VirtualBox to create a virtual server with the VHD image that you created in step 2 by using the bootable Windows&reg; ISO you created in Step 3. For more information, see [Oracle VM VirtualBox User Manual](https://www.virtualbox.org/manual/){: external}.

   If you want to use a method other than VirtualBox to create the custom image, such as VMware, you must remove all drivers that are specific to that hypervisor from the custom image.
   {: important}

1. In Storage settings, add the Windows&reg; installation bootable ISO that you created in Step 3.
1. Start the server and begin the Windows&reg; installation.
1. Select the **Edition** (Standard / Data Center) of the operating system and **Desktop Experience** (with Desktop Experience or without Desktop Experience).
1. Select **Drive 0** and continue with the installation.
1. When the installation is complete, shut down the virtual server and remove the installation ISO. You can ignore warnings about removing the installation ISO.
1. Use the default Windows&reg; updater to download and install Windows updates. Repeat the process of downloading and installing updates until no updates are available.

### Making the virtio-win drivers available in the recovery image
{: #virtio-win-drivers-windows-recovery-image}

After you create your Windows&reg; image, make sure that the virtio-win drivers are available in the recovery image. This step makes sure that you can use the recovery image to troubleshoot any issues or to use the image in rescue mode.

After you download the required virtio drivers `vioscsi.inf` and `viostor.inf`, use the following steps to make the drivers available in the recovery image.

1. Find the recovery image by using the `reagentc /info` command.

   - If the recovery image is in a separate recovery partition, use the `diskpart` utility to assign a drive letter and to unhide the drive. These steps are not required if no separate recovery partitions exist. The following example is for partition `3`, with drive `R`, and uses setting `id=07`.

      ```sh
      diskpart
      select disk 0
      list part
      select part 3
      assign letter R
      set id=07
      exit
      ```
      {: codeblock}

1. To modify the recovery image, use `reagentc /disable` to disable the recovery mode.
1. Create a directory to mount the recovery image.

    ```sh
    md C:\mount\winre
    ```
    {: pre}

1. Copy the recovery image to the mount directory.

   ```sh
   xcopy /h R:\Recovery\WindowsRE\Winre.wim C:\mount\
   ```
   {: pre}

1. Make the recovery image visible by using `attrib -s -h -r C:\mount\Winre.wim`.
1. Mount the contents of the recovery image by using `dism /Mount-Image /ImageFile:c:\mount\Winre.wim /index:1 /MountDir:c:\mount\winre`.
1. Add the `vioscsi` and `viostor` drivers to the recovery image. The following example assumes that the virtio drivers are on drive E. Select the corresponding Windows&reg; version for the `vioscsi` and `viostor` drivers.

   ```sh
   Dism /image:c:\mount\winre /Add-Driver /Driver:"E:\vioscsi\2k19\amd64\vioscsi.inf"
   Dism /image:c:\mount\winre /Add-Driver /Driver:"E:\viostor\2k19\amd64\viostor.inf"
   ```
   {: codeblock}

1. After you add all the drivers, you are ready to create the recovery image.

   ```sh
   Dism /Image:c:\mount\winre /Cleanup-Image /StartComponentCleanup
   Dism /Unmount-Image /MountDir:c:\mount\winre /Commit
   ```
   {: codeblock}

1. Optimize the recovery image.

   ```sh
   Dism /Export-Image /SourceImageFile:c:\mount\WinRE.wim /SourceIndex:1 /DestinationImageFile:c:\mount\winre-optimized.wim
   ```
   {: pre}

1. Copy the recovery image to its original location as noted in step 1 with its original attributes.

   ```she
   xcopy C:\mount\winre-optimized.wim  R:\Recovery\WindowsRE\WinEe.wim /h /r
   ```
   {: pre}

   - If the recovery image is in a separate recovery partition, use the `diskpart` utility to remove the assigned drive letter and hide the drive. These steps are not required if no separate recovery partitions exist. The following example is for partition `3` with drive `R`.

      ```sh
      diskpart
      select disk 0
      list part
      select part 3
      remove letter R
      set id=27
      exit
      ```
      {: codeblock}

1. Enable the recovery image by using `reagentc /enable`.
1. Verify that recover mode is enabled by using `reagentc /info`.

Your image is set up with a recovery image that includes the necessary virtio drivers.

## 2. Customizing a virtual server
{: #customize-virtual-machine}

Complete the following steps to customize the virtual server that you created by using VirtualBox.

1. Install and configure cloudbase-init from [Cloudbase-Init installation package](https://www.cloudbase.it/downloads/CloudbaseInitSetup_1_1_4_x64.msi){: external}

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

1. After you run sysprep, your virtual server shuts down. Then, you can continue with [Step 4 - Creating an image template of your customized virtual server](/docs/vpc?topic=vpc-migrate-vsi-to-vpc&interface=api#migrate-new-image-template)

## 3. Uploading a custom image
{: #complete-custom-image-win}

Use the following information to upload a custom image to {{site.data.keyword.cos_full_notm}}.

On the **Objects** page of your {{site.data.keyword.cloud}} Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-windows-image}

When your Windows&reg; custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import the custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc) and [onboard a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). Make sure that you [grant access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).

If you plan to use a private catalog to manage your custom images, you must first import that image into {{site.data.keyword.vpc_short}}, then onboard the image into a private catalog.
{: note}

After the custom image is imported, you can use it to deploy a server in your {{site.data.keyword.vpc_full}} infrastructure.

After you create a new virtual server from the imported image, stop and then start the virtual server before you access it:

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. On the **Virtual server instances** page, click Actions icon ![More Actions icon](../icons/action-menu-icon.svg).
