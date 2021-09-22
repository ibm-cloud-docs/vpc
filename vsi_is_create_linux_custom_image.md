---

copyright:
  years: 2019, 2021
lastupdated: "2021-08-20"

keywords: creating a linux custom image for vpc, cloud-init, qcow2, vhd

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

# Creating a Linux custom image
{: #create-linux-custom-image}

You can create your own custom Linux-based image to deploy a virtual server instance in the {{site.data.keyword.vpc_full}}
infrastructure.
{: shortdesc}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).
Did you know that your can also create a custom image of a boot volume attached to an instance at import time? For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{: tip}

Your image must adhere to the following custom image requirements:
* Contains a single file or volume, qcow2 or VHD format
* Is cloud-init enabled
* The operating system is supported as a stock image operating system
* Size doesn't exceed 250 GB
* Size isn't below 10 GB, Images below 10 GB are rounded up to 10 GB

Complete the following steps to ensure that your own Linux custom image can be successfully deployed in the
{{site.data.keyword.vpc_short}} infrastructure environment.

You cannot create an image from an encrypted boot volume (Image from a volume feature) that is not 100GB.  The operation will be blocked.
{: note}

## Step 1 - Start with a single image file in qcow2 or VHD format
{: #single-image-qcow2}

If you're creating your own custom Linux-based image, begin with a single image file in qcow2 or VHD format.

It might be helpful to begin with a cloud-enabled vendor image.
{: tip}

## Step 2 - Check kernel arguments
{: #kernel-args}

Skip these steps for LinuxONE (s390x processor architecture).  
{: note}

Make sure that the following arguments are present on the kernel command line:
* `nomodeset`
* `nofb`
* `vga=normal`
* `console=ttyS0`

Follow the instructions for your Linux distribution to update the kernel command line arguments. See the following outline of general steps to add kernel arguments.

1. Back up */etc/default/grub*.
2. Edit */etc/default/grub* and add the required arguments to the `GRUB_CMDLINE_LINUX` line.
3. Back up the *grub.cfg* file.
4. Generate a new *grub.cfg* file.
5. Reboot your system.

## Step 3 - Check for virtio drivers
{: #virtio-drivers}

1. Ensure that your operating system image has virtio drivers installed, along with any code that is needed by virtio. Virtio network drivers are required to enable networking. Check if virtio drivers are installed in the kernel, by running the following command:

    ```
    grep -i virtio /boot/config-$(uname -r)
    ```
    {: pre}

    Look for `VIRTIO_BLK` and `VIRTIO_NET` in the output. If those lines are not present, then the virtio driver is not built into the kernel.

2. If the `VIRTIO_BLK` and `VIRTIO_NET` lines are present, verify that the drivers are present in the temporary root filesystem by running the following command:

    ```
    lsinitrd /boot/initramfs-$(uname -r).img | grep virtio
    ```
    {: pre}

    Verify the the virtio blk driver and its dependencies *virtio.ko*, *virtio_pci*, and *virtio_ring* are present.  If the    virtio dependencies are not present, you must recover the root filesystem.

## Step 4 - Network interface is set to auto-configure
{: #network-auto-config}

For network interfaces that are defined in the guest image, make sure that at least one network interface is set to
auto-configure. All interfaces cannot be designated for manual configuration. Typically the interface is set to
use DHCP.  For more information about configuring DHCP, see the documentation for your Linux distribution.

## Step 5 - Ensure image is cloud-init enabled
{: #cloud-init}

Make sure that your image is cloud-init enabled. Cloud-init version 0.7.9 or greater is required.

1. To determine if cloud-init is installed, run the following command: `cloud-init --version`
    * In some cases, cloud-init might be installed but not in your environment PATH.
    * To find the path for cloud-init on ExecStart, run the following command:`systemctl cat cloud-init`

2. To install cloud-init, use one of the following commands.
    * For Ubuntu 16, 18 & Debian 9, run the following command: `apt-get install cloud-init`
    * For CentOS 7 & RedHat 7, run the following command: `yum install cloud-init`

3. If the `datasources_list` property exists in */etc/cloud/cloud.cfg*, verify that it contains `ConfigDrive` and `NoCloud` or remove the `datasources_list` property entirely. For more information about data sources, see [Data sources](http://cloudinit.readthedocs.io/en/latest/topics/datasources.html){: external}. {{site.data.keyword.cloud_notm}} cloud-init images are created for the environment by using the [NoCloud](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html){: external} data source to supply the metadata.

4. In the */etc/cloud/cloud.cfg* file, verify that the `cloud_final_modules` section includes the `scripts-vendor` module and that it is enabled. By default, Red Hat Enterprise Linux and CentOS do not include the `scripts-vendor` module that is required to provision instances in the {{site.data.keyword.vpc_short}} infrastructure.

5.  Make sure to configure your image to use SSH for logging in to your virtual server instance.

6. For more information, see [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/){: external}.

## Step 6 - Boot disk size is 100 GB
{: #boot-disk-100GB}

Make sure that the image has a boot disk size of 100 GB.

If you are customizing a virtual server instance to migrate from classic infrastructure, return to [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue completing migration steps.
{: tip}

## Step 7 - Upload image to {{site.data.keyword.cos_full_notm}}
{: #upload-image-icos}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your IBM Cloud Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-linux-image}

When your Linux custom image is created, you can [import](/docs/vpc?topic=vpc-managing-images) it to VPC to provision images.
Make sure that you have [Granted access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).  
