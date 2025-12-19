---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Bare metal server images
{: #bare-metal-image}

When you provision a bare metal server on your VPC, you need to select an image to determine the operating system for the server.
{: shortdesc}

## Supported images
{: #bare-metal-images-supported}

The following operating systems are available as images when you create a bare metal server.

| Image | Architecture |
|---|---|
| CentOS 7.x | x86-64 |
| CentOS Stream 9.x | x86-64 |
| [Custom image](#bare-metal-custom-images) | x86-64 |
| Debian 11 | x86-64 |
| Microsoft Windows 2016 Full standard, 2019 Full standard, 2022 Full standard | x86-64 |
| [Red Hat Enterprise Linux](#bare-metal-images-rhel-considerations) 9.x, 8.x| x86-64 |
| Red Hat Enterprise Linux for SAP 9.x, 8.8, 8.6, 8.4 | x86-64 |
| SUSE Linux Enterprise Server 15.x, 12.x | x86-64 |
| SUSE Linux Enterprise Server for SAP 15.x, 12.x  \n  \n For more information about SAP and bare metal servers, see [SAP fast path for IBM Cloud Intel bare metal servers](/docs/sap?topic=sap-fast-path-site-map-intel-bm). | x86-64 |
| [Ubuntu](#bare-metal-images-ubuntu-considerations) 22, 20.04 | x86-64 |
| [VMware ESXi](#bare-metal-images-vmware-esxi-considerations) | x86-64 |
{: caption="Bare metal server images" caption-side="bottom"}

### Special considerations for Red Hat Enterprise Linux 8.4
{: #bare-metal-images-rhel-considerations}

By default, the release lock feature for RHEL 8.4 is disabled. To prevent the RHEL from going beyond version 8.4 when you run an update, run the following commands from the command line:

   ```text
   # subscription-manager release --set=8.4
   ```
 {: codeblock}

   ```text
   # yum clean all
   ```
   {: codeblock}

### Special considerations for VMware ESXi images
{: #bare-metal-images-vmware-esxi-considerations}

* If you want to use TPM with a ESXi image, make sure that secure boot mode is enabled.

* ESXi on Bare Metal Servers for VPC is charged monthly and is calculated per CPU based on the selected profile. If you choose to rent VMware ESXi with your server, you are subject to a prorated monthly cost for the license instead of an hourly rate. The proration amount is variable based on your billing anniversary date.

For more information about how to license ESXi, see [License and Subscription Management](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vcenter-and-host-management-7-0/license-management-host-management.html){: external}.

### Special considerations for Ubuntu images
{: #bare-metal-images-ubuntu-considerations}

Ubuntu images don't include the VMD device driver in the standard kernel package that is needed to view attached NVMe drives on the system. To obtain this driver, install the `linux-modules-extra-ibm` package and then run `modprobe vmd`.

```java
# export DEBIAN_FRONTEND=noninteractive
# apt update
# apt install -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" linux-modules-extra-ibm linux-modules-extra-$(uname -r)
# modprobe vmd
```
{: codeblock}

## Custom images
{: #bare-metal-custom-images}

You can import an image from {{site.data.keyword.cos_full_notm}} that you can use to create a bare metal server.

### Custom image considerations
{: #bare-metal-custom-images-requirements}

Custom images for a bare metal server must meet the following requirements:

* Support UEFI boot mode
* A Pensando&reg; driver for networking
* Support x86 architecture

### Custom images have the following limitations
{: #bare-metal-custom-images-limitations}

Custom images for a bare metal server don't support encrypted images.

For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).

## Special considerations for bare metal network performance upgrade
{: #bare-metal-pensando-considerations}

For improved packet rates on bare metal servers, update stock and custom Linux images to the most recent available Pensando DSC drivers. For more information, see the [AMD Pensando Support](https://www.amd.com/en/support/accelerators/pensando.html){: external}.

### Linux distributions
{: #bare-metal-pensando-linux}

To install the most recent driver on a Linux distribution:

1. Download the Pensando Linux driver package at [AMD Pensando Support](https://www.amd.com/en/support/accelerators/pensando.html){: external}.

2. Extract the image.

   ```sh
   tar -xvf PNSO-23.06.2-001-linux-08022023-1222.tgz
   ```

3. Change the directory to the image that you extracted.

   ```sh
   cd PNSO-1.64.0-E-58-linux-08022023-1222
   ```

   If you are running RHEL or SUSE, run the `install.sh` script to install the pre-built drivers.

4. Run the installation.

   `./install.sh`

   If you are running something other than RHEL or SUSE, you need to build the drivers. Make sure that you have the 'git', 'make' and 'gcc' packages installed. Inspect any error messages that are returned from the 'make' command, some distributions require more packages. The error messages help to determine whether more packages are required.

   ```sh
   cd drivers
   tar xvf drivers-linux-eth.tar.xz
   cd drivers-linux-eth
   ./build.sh
   ```

5. When the drivers are built, you can install the kernel modules. The modules are used when the server is started again.

   ```sh
   make installation -C drivers/
   ```

6. Activate the new module.

   Activate the module by using the console to avoid issues that are caused by a lost network connection.
   {: note}

   ```sh
   rmmod ionic && modprobe ionic
   ```

   The module is installed into `/usr/lib/modules/${uname -r}/updates/eth/ionic/ionic.ko`. If you upgrade your kernel, you need to repeat these steps.

## Next steps
{: #bare-metal-images-next-steps}

* [Planning for bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating a bare metal server](/docs/vpc?topic=vpc-creating-bare-metal-servers)
