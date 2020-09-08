---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-08"

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
{:shortdesc}

Your image must adhere to the following custom image requirements:
* Contains a single file or volume 
* Is in qcow2 format
* Is cloud-init enabled
* The operating system is supported as a stock image operating system
* Size doesn't exceed 100 GB

After your custom image is created, you can [encrypt](/docs/vpc?topic=vpc-create-encrypted-custom-image) it by using the encryption support that is available in beta.
{: note}

The following procedure describes how to create a Windows custom image that can be successfully deployed in the {{site.data.keyword.vpc_short}} infrastructure environment. The procedure encompasses these high-level tasks:
* Use VirtualBox to create a Windows image in VHD format.
* Customize the image with virtIO drivers and Cloudbase-init.
* Convert the Windows VHD image to qcow2 format, the image format that is supported in the {{site.data.keyword.vpc_short}} environment.

Complete the following steps to create a Windows custom image.
1. Begin with a Windows ISO image file. For example, `Windows-2012-install.iso`.
2. Create a VHD image where you can install Windows. 
    
    ```
    qemu-img create -f vpc Windows-2012.vhd 100G
    ``` 
    {: codeblock}
    
    To import the Windows image into the VPC environment, you must use the qcow2 format, but Virtual Box doesn't support qcow2. After the VHD image is customized in Virtual Box, you'll convert the image to qcow2 format.
    {:note}
    
3. Download the [virtio-win drivers](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso){: external}. For more information, see the Fedora user documentation, [Creating Windows virtual machines using virtIO drivers](https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/){: external}.
4. Use VirtualBox to create a virtual machine with the VHD image that you created in step 2. For more information, see [Oracle VM VirtualBox User Manual](https://www.virtualbox.org/manual/){: external}. Complete the following steps to customize the virtual machine.
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
    
5. Convert the Windows VHD image to qcow2 by running the following command:
     ```
     qemu-img convert -f vpc -O qcow2 -o cluster_size=512k Windows-2012.vhd Windows-2012.qcow2
     ``` 
     {: codeblock}
    
6. Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your IBM Cloud Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-windows-image}

When your Windows custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import](/docs/vpc?topic=vpc-managing-images) it to VPC to provision images.
Make sure that you have [Granted access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).
