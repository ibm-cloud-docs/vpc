---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: custom os, creating a custom os, custom operating system, creating a custom operating system, kernel, custom kernel

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuration requirements for custom Linux kernels
{: #configuration-requirements-for-custom-linux-kernels}

Custom Linux kernels can be used in {{site.data.keyword.vpc_full}} VPC. You can build a custom kernel locally on your virtual server instance or on-premises. Then, [create a custom image by using your boot volume](/docs/vpc?topic=vpc-image-from-volume-vpc), or use [Custom Image Import](/docs/vpc?topic=vpc-importing-custom-images-vpc) to bring your on-premises image into the account's image catalog.

When you build your own custom Linux kernel for use in the {{site.data.keyword.vpc_full}}, refer to these requirements. It is also recommended that you [enable virtual server instance console access](/docs/vpc?topic=vpc-vsi_is_connecting_console) when you build your own custom kernel. Doing so helps facilitate debugging any potential boot issues.
{: shortdesc}

## Hardware requirements
{: #hardware-requirements-custom-linux-kernels}

Hardware that is supported by every virtual machine in IBM's VPC is detailed in the following device list. These devices are currently consistent for every virtual machine in the VPC and are subject to change as new features become available in the {{site.data.keyword.vpc_full}}. Custom kernels need to support these devices to run in {{site.data.keyword.vpc_full}} VPC. Failure to include these kernels can result in loss of features or capabilities in the {{site.data.keyword.vpc_full}}.
{: shortdesc}

- **Host bridge**:
    - Intel Corporation 440FX - 82441FX PMC [Natoma] - (rev 02)
- **ISA bridge**:
   - Intel Corporation 82371SB PIIX3 ISA [Natoma/Triton II]
- **IDE interface**:
   - Intel Corporation 82371SB PIIX3 IDE [Natoma/Triton II]
        - Subsystem: XenSource, Inc. Device 0001
        - Kernel driver in use: ata_piix
        - Kernel modules: ata_piix, pata_acpi, ata_generic
- **USB controller**:
   - USB controller: Intel Corporation 82371SB PIIX3 USB [Natoma/Triton II] - (rev 01)
        - **Subsystem**: XenSource, Inc. Device 0001
        - **Kernel driver in use**: uhci_hcd
- **Bridge**:
   - Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 01)
        - **Subsystem**: Red Hat, Inc. Qemu virtual machine
        - **Kernel modules**: i2c_piix4
- **VGA compatible controller**:
   - Cirrus Logic GD 5446 (prog-if 00 [VGA controller])
        - **Subsystem**: XenSource, Inc. Device 0001
        - **Expansion ROM**: [disabled]
        - **Kernel driver in use**: cirrus
        - **Kernel modules**: cirrus
- **SCSI storage controller**:
   - XenSource, Inc. Xen Platform Device (rev 01)
        - **Subsystem**: XenSource, Inc. Xen Platform Device
        - **Kernel driver in use**: xen-platform-pci
- **System peripheral**:
   - XenSource, Inc. Citrix XenServer PCI Device for Windows Update (rev 01)
        - **Subsystem**: XenSource, Inc. Citrix XenServer PCI Device for Windows Update
- **Ethernet controller**:
    - TSO is supported
    - GRO is supported
    - GSO is supported
    - Checksum offload
       - Rx-checksum is supported
       - Tx-checksum is supported


## Custom Linux kernel build options
{: #custom-linux-kernel-build-options}

The following kernel options are required when you build a Linux operating system for {{site.data.keyword.vpc_full}}.

- CONFIG_ETHERNET = y
   - Ethernet driver support
- CONFIG_NETDEVICES = y
   - Network device support
- CONFIG_PCI = y
   - Enables support for the PCI local bus, including support for PCI-X and the foundations for PCI Express support
- CONFIG_NET = y
   - Networking support
- CONFIG_KVM_GUEST = y
   - Enables various optimizations to run under the KVM hypervisor - including a paravirtualized clock. So that instead of relying on a PIT (or other) emulation by the underlying device model
- CONFIG_SCSI_MOD = y
   - SCSI device support
- CONFIG_SCSI = y
   - To use a SCSI hard disk, SCSI tape drive, SCSI CD-ROM, or any other SCSI device under Linux
- CONFIG_VIRTIO_PCI = y
   - This driver adds support for virtio-based para-virtual device drivers over PCI
- CONFIG_SCSI_VIRTIO = y
   - The virtual HBA driver for virtio
- CONFIG_VIRTIO_NET = y
   - The virtual network driver for virtio


## Hardware requirements for LinuxONE (s390x processor architecture)
{: #hardware-requirements-linuxone}

The following hardware is provided for LinuxONE (s390x processor architecture).

- VIRTIO_BLK
- VIRTIO_NET
- IBM z15
- Virtual ASCII console
- Virtual channel subsystem


## Custom Linux kernel build options for LinuxONE (s390x processor architecture)
{: #custom-linux-kernel-linuxone-options}

The following kernel options are required:

- CONFIG_VIRTIO_BLK=Y
   - Block device support
- CONFIG_VIRTIO_NET=Y
   - Network device support
- CONFIG_S390_GUEST=Y
   - KVM guest handling, including virtio-ccw
- CONFIG_SCLP_VT220_TTY=Y
   - tty on virtual ascii console
- CONFIG_SCLP_VT220_CONSOLE=Y
   - Boot console on virtual ascii console

## Custom Linux kernel build options for bare metal servers
{: #custom-linux-kernel-bare-metal-servers}

The custom image that you create for bare metal servers must support the following:
* UEFI boot
   *  Legacy BIOS boot is not supported. As such, you need a dedicated EFI partition that contains the EFI firmware.
* Intel chipset device drivers.
* Bare Metal servers require the pensando ionic device driver for networking. This driver is normally an in-box driver for 5.x linux kernels. If the ionic driver is not part of your kernel, you can include it as a kernel module and use DKMS to manage kernel upgrades.

To create secure execution-based custom images by using the {{site.data.keyword.cos_full_notm}} option, see [Preparing the workload](https://www.ibm.com/docs/en/linux-on-systems?topic=execution-workload-owner-tasks). For more information about creating secure execution-based images, [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=management-secure-execution).
{: note}
