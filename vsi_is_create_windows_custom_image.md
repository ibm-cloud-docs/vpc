---

copyright:
  years: 2019, 2023

lastupdated: "2023-11-28"


keywords: creating a Windows custom image, cloudbase-init, qcow2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Windows custom image
{: #create-windows-custom-image}

You can create your own custom Windows-based image to import the custom image into {{site.data.keyword.vpc_full}}. You can then use the custom image to deploy a virtual server or bare metal server in the {{site.data.keyword.vpc_full}} infrastructure.
{: shortdesc}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc). Did you know that your can also create a custom image of a boot volume that is attached to a server at import time? For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc). Keep in mind that Windows custom images aren't supported for LinuxONE (s390x processor architecture).  
{: tip}

{{site.data.content.custom-image-requirements-list}}

You can't create an image from an encrypted boot volume (image from a volume) that is not 100 GB. The operation is blocked.
{: note}

Use the following steps to create a Windows custom image to deploy in the {{site.data.keyword.vpc_short}} infrastructure environment. The procedure encompasses these high-level tasks:
* Use VirtualBox to create a Windows image in VHD format.
* Customize the image with virtIO drivers and Cloudbase-init.
* Upload the image to {{site.data.keyword.cos_full_notm}}.

## Initial steps for creating a Windows custom image
{: #create-win-custom-image-first-steps}

Complete the following steps to start creating a Windows custom image.

1. Begin with a Windows ISO image file. For example, `Windows-2012-install.iso`.
2. Create a VHD image where you can install Windows.

   ```sh
   qemu-img create -f vpc Windows-2012.vhd 100G
   ```
   {: codeblock}

    {{site.data.keyword.cloud}} supports custom image import with VHD or qcow2. However, Virtual Box does not support the qcow2 format.
    {: note}

3. Obtain the required virtio-win drivers by [provisioning](/docs/vpc?topic=vpc-creating-virtual-servers) or accessing an existing Red Hat Enterprise Linux virtual server in {{site.data.keyword.vpc_short}}. Then, install the virtio-win package on the server. Finally, copy the virtio-win ISO file, for example, *virtio-win-1.9.24.iso*, to use for your Windows custom image.

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

   2. Access the virtio-win ISO in the `/usr/share/virtio-win` directory.  

      ```sh
      cd /usr/share/virtio-win/
      ```
      {: codeblock}

   3. Use SCP to copy the virtio-win ISO file, for example `virtio-win-1.9.24.iso`, to use for your Windows custom image.  

4. Use VirtualBox to create a virtual machine with the VHD image that you created in step 2. For more information, see [Oracle VM VirtualBox User Manual](https://www.virtualbox.org/manual/){: external}.

If you choose to use a method other than VirtualBox to create the custom image, such as VMware, you must remove all drivers that are specific to that hypervisor from the custom image.
{: important}

## Customizing the virtual machine
{: #customize-virtual-machine}

Complete the following steps to customize the virtual machine.

1. In Storage settings, add the Windows installation ISO and the virtio-win driver ISO as optical drives. For example, `Windows-2012-install.iso` and `virtio-win-1.9.24.iso`.

2. Start the virtual machine and begin the Windows installation. When you see the page **Where do you want to install Windows?** you must load all of the `virtio-win\` drivers. You might need to clear "Hide drivers that aren't compatible with this computer's hardware" to access all of the required drivers. Then, you can select **Drive 0** and continue with the installation. When the installation is complete, shut down the virtual machine and remove the optical drives that you added: Windows installation ISO and virtio-win driver ISO. You can ignore any warnings about removing an optical drive.

3. Use the default Windows updater to download and install Windows updates. Repeat the process of downloading and installing updates until no updates are available.

4. Install [cloudbase-init](https://cloudbase.it/cloudbase-init/){: external}. For more information, see [cloudbase-init’s documentation](https://cloudbase-init.readthedocs.io/en/latest/index.html){: external}.

5. Modify the cloudbase-init configuration file, `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init.conf` to match the values that are shown in the following example.

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

    If you plan to [bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) to the cloud for your custom image, omit the following lines from the cloudbase-init configuration file:

   ```sh
   activate_windows=true
   kms_host=kms.adn.networklayer.com:1688
   ```
   {: codeblock}

6. Modify the `cloudbase-init-unattend.conf` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init-unattend.conf` to match the values shown in the following example.

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

7. Modify the `Unattend.xml` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml` to set  **PersistAllDeviceInstalls** to *false*.

8. Run Sysprep by using the following command:

   ```sh
   C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/unattend:C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml"
   ```
   {: codeblock}   

    If you are customizing a virtual server to migrate from the Classic infrastructure, return to [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue completing migration steps.
    {: tip}

## Uploading the custom image
{: #complete-custom-image-win}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your {{site.data.keyword.cloud}} Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-windows-image}

When your Windows&reg;custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import the custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc) and [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). Make sure that you have [Granted access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).

If you plan to use a private catalog to manage your custom images, you must first import that image into {{site.data.keyword.vpc_short}} and then onboard the virtual server image into a private catalog.
{: note}

After the custom image is imported, you can then use the custom image to deploy a virtual server in the {{site.data.keyword.vpc_full}} infrastructure.
