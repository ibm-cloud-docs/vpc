---

copyright:
  years: 2019
lastupdated: "2019-09-04"

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

When you provision {{site.data.keyword.vsi_is_full}}, you can select a custom image that you import from {{site.data.keyword.cos_full_notm}}. The image that you select determines the operating system that is provisioned for your instance. 
{:shortdesc}

## Before you begin
{: #prereq-custom-images}

To import a custom image to {{site.data.keyword.vpc_full}}, you must have an instance of {{site.data.keyword.cos_full_notm}} available. You must also create a bucket in {{site.data.keyword.cos_full_notm}} to store your images. 

1. If you don't already have an instance of {{site.data.keyword.cos_full_notm}}, see [Getting started with {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started).
2. If you need to upload an image to {{site.data.keyword.cos_full_notm}}, navigate to your bucket and click **Add Objects** to 
[upload](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) the image. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.
3. From IBM {{site.data.keyword.iamshort}} (IAM), [create an authorization](/docs/iam?topic=iam-serviceauth#serviceauth) between **VPC Infrastructure** (source service) > **Image Service for VPC** (resource type) and **Cloud Object Storage** (target service).
4. Make sure your image meets custom image requirements:
  - Contains a single file or volume 
  - Is in qcow2 format
  - Is cloud-init enabled
  - The operating system is supported as a stock image operating system
  - Size doesn't exceed 100 GB

Encrypted images aren't supported for custom images.
{: note}

## Creating a custom image 
{: #create-deployable-custom-image}

You can create your own custom image, either a Linux-based custom image or a Windows custom image, to deploy a virtual server instance in the {{site.data.keyword.vpc_short}} infrastructure. 

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
    * Make sure to configure your image to use SSH for logging in to your  virtual server instance.
    * Linux images require Cloud-init version 0.7.7 or greater.
    
6. Upload your image to {{site.data.keyword.cos_full_notm}}. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload).    
    
### Creating a Windows custom image
{: #create-windows-custom-image}

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
    2. Start the virtual machine and begin the Windows installation. When you see the page **Where do you want to install Windows?** you must you must load all of the `virtio-win\` drivers. You might need to uncheck "Hide drivers that aren't compatible with this computer's hardware" to access all of the required drivers. Then you can select **Drive 0** and continue with the installation. When the installation is complete, shut down the virtual machine and remove the optical drives that you added previously: Windows installation ISO and virtio-win driver ISO. You can ignore any warnings about removing an optical drive. 
    3. Use the default Windows updater to download and install Windows updates. Repeat the process of downloading and installing updates until there are no more updates available.
    4. Install [cloudbase-init](https://cloudbase.it/cloudbase-init/){: external}. For more information, see [cloudbase-init’s documentation](https://cloudbase-init.readthedocs.io/en/latest/index.html){: external}.
    5. Modify the cloudbase-init configuration file, `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init.conf` to match the values shown in the following example. 
   
         ```
         [DEFAULT]
         username=Administrator
         groups=Administrators
         inject_user_password=true
         first_logon_behaviour=no
         config_drive_types=vfat
         config_drive_locations=hdd
         bsdtar_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\bsdtar.exe
         mtools_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\
         verbose=true
         debug=true
         logdir=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\
         logfile=cloudbase-init.log
         default_log_levels=comtypes=INFO,suds=INFO,iso8601=WARN,requests=WARN
         logging_serial_port_settings=
         mtu_use_dhcp_config=false
         ntp_use_dhcp_config=false
         local_scripts_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\LocalScripts\
         metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService
         activate_windows=true
         kms_host=161.26.20.4:1688
         check_latest_version=false
         ```
         {: screen}
        
    6. Modify the `cloudbase-init-unattend.conf` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\cloudbase-init-unattend.conf` to match the values shown in the following example.  
       
         ```
         [DEFAULT]
         username=Administrator
         groups=Administrators
         inject_user_password=true
         first_logon_behaviour=no
         config_drive_types=vfat
         config_drive_locations=hdd
         bsdtar_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\bsdtar.exe
         mtools_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\
         verbose=true
         debug=true
         logdir=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\
         logfile=cloudbase-init-unattend.log
         default_log_levels=comtypes=INFO,suds=INFO,iso8601=WARN,requests=WARN
         logging_serial_port_settings=
         mtu_use_dhcp_config=false
         ntp_use_dhcp_config=false
         local_scripts_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\LocalScripts\
         metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService
         plugins=cloudbaseinit.plugins.common.mtu.MTUPlugin,
         cloudbaseinit.plugins.common.sethostname.SetHostNamePlugin,
         cloudbaseinit.plugins.common.localscripts.LocalScriptsPlugin
         allow_reboot=false
         stop_service_on_exit=false
         check_latest_version=false
         activate_windows=true
         ```
         {: screen}
         
    7. Modify the `Unattend.xml` file in `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml` to set Set **PersistAllDeviceInstalls** to *false*.

    8. Run Sysprep by using the following command:
    
         ```
         C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/unattend:C:\Program Files\Cloudbase        Solutions\Cloudbase-Init\conf\Unattend.xml"
         ``` 
         {: codeblock}
    
5. Convert the Windows VHD image to qcow2 by running the following command:

     ```
     qemu-img convert -f vpc -O qcow2 -o cluster_size=512k Windows-2012.vhd Windows-2012.qcow2
     ``` 
     {: codeblock}
    
6. Upload your image to {{site.data.keyword.cos_full_notm}}. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload).

## Importing a custom image
{: #import-custom-image}

When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server instances from that image.  

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. For more information, see [Creating a custom image](/docs/vpc?topic=vpc-managing-images#create-deployable-custom-image) and [Uploading data](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
2. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
3. Click **Import Custom Image**. 
4. Complete the required fields and click **Create Custom Image**.

## Managing custom images
{: #managing-custom images}

After you import custom images, you can deploy and manage them from the Custom images page. 

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. From your list of custom images, you can click **...** and select from the available options. 
