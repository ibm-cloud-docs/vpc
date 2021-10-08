---

copyright:
  years: 2019, 2021
lastupdated: "2021-10-07"

keywords: creating a Windows custom image, cloudbase-init, qcow2

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

# Creating a Windows custom image
{: #create-windows-custom-image}

You can create your own custom Windows-based image to deploy a virtual server instance in the {{site.data.keyword.vpc_short}}
infrastructure.
{: shortdesc}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).
Did you know that your can also create a custom image of a boot volume attached to an instance at import time? For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{: tip}

Windows custom image are not supported for LinuxONE (s390x processor architecture).  
{: note}


Your image must adhere to the following custom image requirements:
* Contains a single file or volume.
* Is cloud-init enabled.
* The operating system is supported as a stock image operating system.
* The image is configured to use BIOS boot mode. UEFI boot mode is not supported. 
* The min/max volume size is 10 GB to 250 GB. Images below 10 GB are rounded up to 10 GB.

You cannot create an image from an encrypted boot volume (Image from a volume feature) that is not 100GB.  The operation will be blocked.
{: note}

The following procedure describes how to create a Windows custom image that can be successfully deployed in the {{site.data.keyword.vpc_short}} infrastructure environment. The procedure encompasses these high-level tasks:
* Use VirtualBox to create a Windows image in VHD format.
* Customize the image with virtIO drivers and Cloudbase-init.

## Initial steps for creating a windows custom image
{: #create-win-custom-image-first-steps
}
Complete the following steps to start creating a Windows custom image.

1. Begin with a Windows ISO image file. For example, `Windows-2012-install.iso`.
2. Create a VHD image where you can install Windows.

    ```
    qemu-img create -f vpc Windows-2012.vhd 100G
    ```
    {: codeblock}

     IBM Cloud supports custom image import with VHD or qcow2.  However, Virtual Box does not support the qcow2 format.
     {: note}

3. Obtain the required virtio-win drivers by [provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) or accessing an existing Red Hat Enterprise Linux virtual server instance in {{site.data.keyword.vpc_short}}. Then install the virtio-win package on the instance. Finally, copy the virtio-win ISO file, for example, *virtio-win-1.9.15.iso*, to use for your Windows custom image creation.

   1. On your Red Hat Enterprise Linux virtual server instance in {{site.data.keyword.vpc_short}}, install the virtio-win package by running the following command:

     ```
     yum install virtio-win
     ```
     {: codeblock}

     In this example, the virtio-win package is being installed on Red Hat Enterprise Linux version 8. You should see output similar to the following:

     ```
     Installed:
       virtio-win-1.9.15-0.el8.noarch
     ```
     {: screen}

   2. Access the virtio-win ISO in the `/usr/share/virtio-win` directory.  

     ```
     cd /usr/share/virtio-win/
     ```
     {: codeblock}

   3. Use SCP to copy the virtio-win ISO file, for example `virtio-win-1.9.15.iso` to use for your Windows custom image creation.  

4. Use VirtualBox to create a virtual machine with the VHD image that you created in step 2. For more information, see [Oracle VM VirtualBox User Manual](https://www.virtualbox.org/manual/){: external}.

If you choose to use a method other than VirtualBox to create the custom image, such as VMware, you must remove all drivers that are specific to that hypervisor from the custom image.
{: important}

## Customize the virtual machine
{: #customize-virtual-machine}

Complete the following steps to customize the virtual machine.

1. In Storage settings, add the Windows installation ISO and the virtio-win driver ISO as optical drives. For example, `Windows-2012-install.iso` and `virtio-win-0.1.141.iso`.

2. Start the virtual machine and begin the Windows installation. When you see the page **Where do you want to install Windows?** you must load all of the `virtio-win\` drivers. You might need to clear "Hide drivers that aren't compatible with this computer's hardware" to access all of the required drivers. Then, you can select **Drive 0** and continue with the installation. When the installation is complete, shut down the virtual machine and remove the optical drives that you added previously: Windows installation ISO and virtio-win driver ISO. You can ignore any warnings about removing an optical drive.

3. Use the default Windows updater to download and install Windows updates. Repeat the process of downloading and installing updates until there are no more updates available.

4. Install [cloudbase-init](https://cloudbase.it/cloudbase-init/){: external}. For more information, see [cloudbase-init’s documentation](https://cloudbase-init.readthedocs.io/en/latest/index.html){: external}.

5. Modify the cloudbase-init configuration file, `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init.conf` to match the values shown in the following example.

     ```
     [DEFAULT]
     #  "cloudbase-init.conf" is used for every boot
     config_drive_types=vfat
     config_drive_locations=hdd
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

If you plan to [bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) to the cloud for your custom image, omit the following lines from the cloudbase-init configuration file:

     ```
     activate_windows=true
     kms_host=kms.adn.networklayer.com:1688
     ```
     {.codeblock}

6. Modify the `cloudbase-init-unattend.conf` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init-unattend.conf` to match the values shown in the following example.  

     ```
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

7. Modify the `Unattend.xml` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml` to set  **PersistAllDeviceInstalls** to *false*.

8. Run Sysprep by using the following command:

     ```
     C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/unattend:C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml"
     ```
     {: codeblock}

     If you are customizing a virtual server instance to migrate from classic infrastructure, return to [Migrating a virtual    server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue completing migration steps.
     {: tip}

## Complete creating the custom image
{: #complete-custom-image-win}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your IBM Cloud Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-windows-image}

When your Windows custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import](/docs/vpc?topic=vpc-managing-images) it to VPC to provision images.
Make sure that you have [Granted access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).
