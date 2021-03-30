---

copyright:
  years: 2021
lastupdated: "2021-03-30"

keywords: custom os, creating a custom os, custom operating system, creating a custom operating system, kernel, custom kernel

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}
{:download: .download}

# Configuration Requirements for Custom Linux Kernels
{: #configuration-requirements-for-custom-linux-kernels}

Custom Linux kernels can be used in IBM Cloud VPC. You can build a custom kernel locally on your VSI or on-premises.

When building your own custom Linux kernel for use in the IBM Cloud, please refer to these mandatory requirements.  It is also recommended that you [enable VSI console access](/docs/vpc?topic=vpc-vsi_is_connecting_console) when building your own custom kernel, it will help facilitate debugging any boot issues that may occur.
{: shortdesc}

## Hardware Requirements
Hardware supported by every virtual machine in IBM's VPC is detailed below in the device list. These devices are currently consistent for every virtual machine in the VPC and are subject to change as new features become available in the IBM Cloud. Custom kernels should support these devices to run in IBM Cloud VPC. Failure to include these may result in loss of features or capabilities in the IBM cloud.
{:shortdesc}

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
- **Ethernet Controller**:
  - TSO is supported
  - GRO is supported
  - GSO is supported
  - Checksum Offload
    - Rx-checksum is supported
    - Tx-checksum is supported

## Linux Kernel Build Options
The following kernel options are required when building a Linux operating system for IBM Cloud.

- CONFIG_ETHERNET = y
  - Ethernet driver support
- CONFIG_NETDEVICES = y
  - Network device support
- CONFIG_PCI = y
  - Enables support for the PCI local bus, including support for PCI-X and the foundations for PCI Express support
- CONFIG_NET = y
  - Networking support
- CONFIG_KVM_GUEST = y
  - Enables various optimizations for running under the KVM hypervisor. It includes a paravirtualized clock, so that instead of relying on a PIT (or probably other) emulation by the underlying device model
- CONFIG_SCSI_MOD = y
  - SCSI device support
- CONFIG_SCSI = y
  - To use a SCSI hard disk, SCSI tape drive, SCSI CD-ROM or any other SCSI device under Linux
- CONFIG_VIRTIO_PCI = y
  - This driver provides support for virtio based paravirtual device drivers over PCI
- CONFIG_SCSI_VIRTIO = y
  - The virtual HBA driver for virtio
- CONFIG_VIRTIO_NET = y
  - The virtual network driver for virtio
