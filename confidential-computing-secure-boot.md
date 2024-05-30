---
copyright:
  years: 2023, 2024
lastupdated: "2024-05-29"

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

   The Unified Extensible Firmware Interface (UEFI) firmware securely manages the certificates that contain the keys that are used by the software manufacturers to sign the system firmware, the system boot loader, and any binaries that they load.

* GPT partitioned disk

   The secure boot feature requires a GUID Partitioning Table (GPT) which replaces the old partitioning schema Master Boot Record (MBR). GPT partitioning schema is required for booting the image in UEFI mode, else the image would be booted in BIOS (Basic Input/Output System) mode.

All major OS distributions provide signed boot loaders, signed kernels and signed kernel modules which are required for secure boot to succeed when the above two requirements are fulfilled.
{: note}

When you use a custom kernel or custom kernel modules, you must sign it with your own certificate and make that certificate known to the firmware or Machine Owner Key (MOK). You can use the mokutil utility to help manage Linux keys, but changes to the MOK keys must be confirmed directly from the console at boot time. Therefore, only a user that is present at the console can confirm user generated keys used for signing custom kernel and custom kernel modules.

## Limitations
{: #confidential-computing-secure-boot-limitations-vpc}

Secure boot does not provide protection in the following circumstances.

* Operating system-based attacks
* Physical hardware attacks
* Malicious system administrators

A secure boot-enabled virtual server instance fails to boot if any software component that is in the boot path is not signed or if signed certificates are not loaded. Even if the deployment is successful, if it isn't signed, the instance can't boot. Or, you can't log in or receive a ping from the instance. Keep in mind that you aren't notified about boot failure. You need to use the console logs to verify boot success and to help identify the cause of any boot failures. 
