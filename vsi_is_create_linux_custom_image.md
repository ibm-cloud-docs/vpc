---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-10"

keywords: creating a linux custom image, cloud-init, qcow2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Linux custom image
{: #create-linux-custom-image}

You can create your own custom Linux-based image to import into {{site.data.keyword.vpc_full}}. You can then use the custom image to deploy a virtual server or bare metal server in the {{site.data.keyword.vpc_full}}  infrastructure.
{: shortdesc}

You can begin with an image template from the {{site.data.keyword.cloud_notm}} Classic infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc). Did you know that you can also create a custom image of a boot volume that is attached to an instance at import time? For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{: tip}

{{site.data.content.custom-image-requirements-list}}

To create secure execution-based custom images by using the {{site.data.keyword.cos_full_notm}} option, see [Preparing the workload](https://www.ibm.com/docs/en/linux-on-systems?topic=execution-workload-owner-tasks). For more information about creating secure execution-based images, [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=management-secure-execution).


{{_include-segments/linux-ed25519-ssh-key-type-note.md}}

Complete the following steps to make sure that you can successfully deploy your own Linux custom image in the {{site.data.keyword.vpc_short}} infrastructure environment. Keep in mind that you can't create an image from an encrypted boot volume that is not 100 GB. The operation is blocked.

## Step 1 - Start with a single image file in qcow2 or VHD format
{: #single-image-qcow2}

If you're creating your own custom Linux-based image, begin with a single image file in qcow2 or VHD format. It might be helpful to begin with a cloud-enabled vendor image.

Kernel logs are important in debugging boot related issues. To ensure kernel logs are printed to the serial console, use the `console=ttyS0` kernel command line argument. In addition, the `nomodeset` and `nofb` kernel parameters are used to resolve display-related issues during the boot process for older kernels that were released before mid 2021. The newer kernels use the video mode setting into the kernel.
{: note}

## Step 2 - Check for virtio drivers
{: #virtio-drivers}

1. Make sure that virtio drivers are installed on your operating system image, along with any code that is needed by virtio. Virtio network drivers are required to enable networking. Check whether virtio drivers are installed in the kernel, by running the following command:

    ```sh
    grep -i virtio /boot/config-$(uname -r)
    ```
    {: pre}

    Look for `VIRTIO_BLK` and `VIRTIO_NET` in the output. If those lines are not present, then the virtio driver is not built into the kernel.

2. If the `VIRTIO_BLK` and `VIRTIO_NET` lines are present, verify that the drivers are present in the temporary root file system by running the following command:

    ```sh
    lsinitrd /boot/initramfs-$(uname -r).img | grep virtio
    ```
    {: pre}

    If you are using a Debian operating system, use the following command:

    ```sh
    lsinitramfs /boot/initrd.img-$(uname -r) | grep virtio
    ```
    {: pre}

    Verify that the `virtio blk` driver and its dependencies `virtio.ko`, `virtio_pci`, and `virtio_ring` are present. If the virtio dependencies are not present, you must recover the root file system.

## Step 3 - Network interface is set to auto-configure
{: #network-auto-config}

If network interfaces are defined in the guest image, make sure that at least one network interface is set to
auto-configure. All interfaces can't be designated for manual configuration. Typically the interface is set to
use DHCP. For more information about configuring DHCP, see the documentation for your Linux distribution.

## Step 4 - Make sure that image is cloud-init enabled
{: #cloud-init}

Make sure that your image is cloud-init enabled. Cloud-init version 0.7.9 or greater is required.

1. To determine whether cloud-init is installed, run the following command: `cloud-init --version`
    * In some cases, cloud-init might be installed but not in your environment PATH.
    * To find the path for cloud-init on ExecStart, run the following command:`systemctl cat cloud-init`

2. To install cloud-init, use one of the following commands.
    * For Ubuntu or Debian, run the following command: `apt-get install cloud-init`
    * For CentOS or Red Hat, run the following command: `yum install cloud-init`

3. If the `datasources_list` property exists in */etc/cloud/cloud.cfg*, verify that it only contains `NoCloud` or remove the `datasources_list` property entirely. `ConfigDrive` is not supported. For more information about data sources, see [data sources](https://cloudinit.readthedocs.io/en/latest/reference/datasources.html){: external}. {{site.data.keyword.cloud_notm}} cloud-init images are created for the environment by using the [NoCloud](https://cloudinit.readthedocs.io/en/latest/reference/datasources/nocloud.html){: external} data source to supply the metadata.

   A block device is provided. See the following example.

   ```sh
   blkid
   /dev/vdb: UUID="2023-03-15-16-50-02-00" LABEL="cidata" TYPE="iso9660"
   ```
   {: pre}

   This block device is found by cloud-init and contains the following files.
   * meta-data
      instance-id: INSTANCE_ID
      local-hostname: NAME
   * user-data
      Optional user data that was provided when the instance was created. See [User data](/docs/vpc?topic=vpc-user-data) for more information.
   * vendor-data
      A MIME formatted file containing the cloud config SSH authorization derived from the SSH keys that were provided when the instance is created and additional initialization information.

4. In the */etc/cloud/cloud.cfg* file, verify that the `cloud_final_modules` section includes the `scripts-vendor` module and that it is enabled. By default, Red Hat Enterprise Linux and CentOS do not include the `scripts-vendor` module that is required to provision instances in the {{site.data.keyword.vpc_short}} infrastructure. You must enable the `scripts-vendor` module to provision an {{site.data.keyword.vpc_short}} virtual server instance with the Linux custom image.

    For Red Hat Enterprise Linux (RHEL) images, the following packages are included as part of the base image by default and are required for cloud-init to run successfully: `subscription-manager`, `ethtool`, and `rpm`. In addition, make sure that these services are enabled: `cloud-init-local.service`, `cloud-init.service`, `cloud-config.service`, and `cloud-final.service`.
    {: note}

5. Make sure that you configure your image to use SSH for logging in to your virtual server.

The default user for an RHEL-based custom images is `cloud-user`. For more information about enabling cloud-init, see [Setting up cloud-init](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_atomic_host/7/html/installation_and_configuration_guide/setting_up_cloud_init){: external}.

## Step 5 - Boot disk size
{: #boot-disk}

Make sure that the image has a boot disk size of 10 - 250 GB. Images that are less than 10 GB are rounded up to 10 GB.

If you are customizing a virtual server to migrate from the Classic infrastructure, return to [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc#migrate-customize-image-vpc) and continue completing migration steps.
{: tip}

## Step 6 - Upload image to {{site.data.keyword.cos_full_notm}}
{: #upload-image-icos}

Upload your image to {{site.data.keyword.cos_full_notm}}. On the **Objects** page of your {{site.data.keyword.cloud}} Object Storage bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images that are larger than 200 MB. For more information about uploading to {{site.data.keyword.cos_full_notm}}, see [Upload data](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Next steps
{: #next-steps-creating-linux-image}

When your Linux custom image is created and available in {{site.data.keyword.cos_full_notm}}, you can [import the custom image into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc) and [Onboard a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui). Make sure that you [Granted access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).

If you plan to use a private catalog to manage your custom images, you must first import that image into {{site.data.keyword.vpc_short}}, and then onboard the virtual server image into a private catalog.
{: note}

After the custom image is imported, you can then use the custom image to deploy a virtual server in the {{site.data.keyword.vpc_full}} infrastructure.
