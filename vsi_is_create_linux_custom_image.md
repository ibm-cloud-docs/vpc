---

copyright:
  years: 2019, 2022

lastupdated: "2022-09-16"

keywords: creating a linux custom image, cloud-init, qcow2, vhd

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Linux custom image
{: #create-linux-custom-image}

You can create your own custom Linux-based image to deploy a virtual server instance in the {{site.data.keyword.vpc_full}}
infrastructure.
{: shortdesc}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).
Did you know that your can also create a custom image of a boot volume attached to an instance at import time? For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{: tip}

{{site.data.content.custom-image-requirements-list}}

To create secure execution-based custom images by using the Cloud Object Storage option, see [Preparing the workload](https://www.ibm.com/docs/en/linux-on-systems?topic=tasks-prepare-workload). For information about creating secure execution-based images, [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=overview-introducing-secure-execution-linux).

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

    If you are using a Debian operating system, use the following command:

    ```
    lsinitramfs /boot/initrd.img-$(uname -r) | grep virtio
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

    For Red Hat Enterprise Linux images, the following packages are included as part of the base image by default and are required for cloud-init to run successfully: `subscription-manager`, `ethtool`, and `rpm`. In addition, make sure that these services are enabled: `cloud-init-local.service`, `cloud-init.service`, `cloud-config.service`, and  `cloud-final.service`.
    {: note}

5.  Make sure to configure your image to use SSH for logging in to your virtual server instance.

6. For more information, see [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/){: external}.

## Step 6 - Boot disk size
{: #boot-disk-100GB}

Make sure that the image has a boot disk size between 10 GB and 250 GB. Images below 10 GB are rounded up to 10 GB.

If you are customizing a virtual server instance to migrate from classic infrastructure, return to [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue completing migration steps.
{: tip}

## Step 7 - Upload image to {{site.data.keyword.cos_full_notm}}
{: #upload-image-icos}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your IBM Cloud Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-linux-image}

When your Linux custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import the custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc) and [Onboard a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). Make sure that you have [Granted access to IBM Cloud Object Storage to import images](/docs/vpc?topic=vpc-object-storage-prereq).

If you plan to use a private catalog to manage your custom images, you must first import that image into {{site.data.keyword.vpc_short}} and then onboard the virtual server image into a private catalog.
{: note}

After the custom image is imported, you can then use the custom image to deploy a virtual server in the {{site.data.keyword.vpc_full}} infrastructure.
