---
copyright:
  years: 2023, 2025
lastupdated: "2025-07-21"

keywords: secure boot, secure boot for virtual servers

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Secure boot for Virtual Servers for VPC
{: #confidential-computing-with-secure-boot-vpc}

[Select availability]{: tag-green}

Secure boot is a security standard that makes sure that your server starts with trusted software by verifying the digital signatures for all code in the boot process. When a server starts in secure boot mode, the firmware checks the signature of the boot software, including UEFI firmware drivers, EFI applications, and the operating system. If the signatures are valid, the server boots, and the firmware grants control to the operating system. Which means that secure boot helps prevent malicious software from loading when your server starts.
{: shortdesc}

## Secure boot requirements
{: #confidential-computing-secure-boot-requirements-vpc}

Keep the following information in mind when you use secure boot.

* UEFI boot-supported image

   The Unified Extensible Firmware Interface (UEFI) firmware securely manages the certificates that contain the keys that are used by the software manufacturers to sign the system firmware, the system boot loader, and any binary files that they load.

* GPT-partitioned disk

   The secure boot feature requires a GUID Partitioning Table (GPT) that replaces the old partitioning schema Master Boot Record (MBR). A GPT partitioning schema is required to boot the image in UEFI mode. Otherwise, the image boots in BIOS (Basic Input Output System) mode.

All major OS distributions provide signed boot loaders, signed kernels, and signed kernel modules that are required for secure boot to succeed when the two requirements are fulfilled.

When you use a custom kernel or a custom kernel module, you must sign it with your own certificate and make that certificate known to the firmware or Machine Owner Key (MOK). You can use the mokutil utility to help manage Linux keys, but changes to the MOK keys must be confirmed directly from the console at boot time. Therefore, only a user that is present at the console can confirm user-generated keys that are used for signing custom kernel and custom kernel modules.

After the virtual server is stopped and restarted, the MOK disappears. You must reinstall the MOK after every stop and start of a virtual server.
{: important}

## Disabling secure boot requirements
{: #disabling-secure-boot-vpc}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can choose to disable secure boot when you create the virtual server instance. To disable secure boot for an existing virtual server instance, you must first stop your virtual server instance. You can then disable the secure boot option. After disabling secure boot, you can then restart your virtual server instance. For more information, see [Managing virtual server instances: Disable or enable secure boot](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#disable-secure-boot-ui).

## Limitations
{: #confidential-computing-secure-boot-limitations-vpc}

Keep the following limitations in mind when you use secure boot.

* Secure boot is available on only third-generation Sapphire Rapids-based virtual servers except the following profiles.
   - mx3d-128x1280
   - mx3d-176x1760
   - bx3d-176x880

If you resize a virtual server that has secure boot-enabled to a profile that is secure boot-disabled (and vice-versa), the topology of PCIe devices changes. Depending on the operating system, this topology change can rename devices and realign the PCI address. The I/O performance can also change.

Secure boot does not provide protection in the following circumstances.

* Operating system-based attacks
* Physical hardware attacks
* Malicious system administrators

A secure boot-enabled virtual server fails to boot if any software component that is in the boot path is not signed or if signed certificates are not loaded. Even if the deployment is successful, if it isn't signed, the instance can't boot. Which means that you can't log in or receive a ping from the virtual server. Keep in mind that you aren't notified about boot failure. You need to use the console logs to verify boot success and to help identify the cause of any boot failures.
