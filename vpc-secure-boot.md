---

copyright:
  years: 2023, 2024
lastupdated: "2024-08-26"

keywords: secure boot, tpm, Trusted Platform Module

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Secure boot with Trusted Platform Module (TPM) for bare metal servers
{: #secure-boot-tpm}

Secure boot makes sure that your server starts with trusted software by verifying the signatures for all code in the boot process. So, your images need to support secure boot with a signed boot loader. Trusted Platform Module (TPM) provides hardware-based security functions. With supporting software, TPM helps maintain platform integrity and generates cryptographic keys. Generally, TPM is used with secure boot.
{: shortdesc}

Servers that you provisioned before [31 January 2023](/docs/vpc?topic=vpc-release-notes#vpc-jan3123) aren't supported.
{: important}

## Supported images for secure boot
{: #secure-boot-tpm-images}

The following images are available when you provision a server with secure boot. Keep in mind that these features are available on only the x86-64 architecture.

| Image | Architecture |
|-----|-----|
| Red Hat Enterprise Linux 8 | x86-64 |
| Red Hat Enterprise Linux 8.4 for SAP | x86-64 |
| Red Hat Enterprise Linux 8.4 for SAP HANA | x86-64 |
| SUSE Linux Enterprise server (SLES) 15 SP3 | x86-64 |
| SUSE Linux Enterprise server (SLES) 15 SP4 | x86-64 |
| Ubuntu 18.04, 20.04, 22.04 | x86-64 |
| VMWare ESXi 7 | x86-64 |
| Windows 2016, 2019, 2022 | x86-64 |
{: caption="Table 1. Supported operating systems for secure boot" caption-side='top"}

## Limitations
{: #secure-boot-tpm-limitations}

Consider the following limitations before you enable secure boot and TPM.

* Servers that you provisioned before this feature was released aren't supported.
* Secure boot requires a signed OS. If you enable secure boot without a signed OS, the server can't boot.
* Secure boot and TPM are available on only x86 servers.
* ESXi images require secure boot to use TPM.

## Enable secure boot with TPM
{: #secure-boot-tpm-enable}

When you create or update a server, you can specify whether you want to enable secure boot. You can find the secure boot and TPM selections in the **Advanced options** on the provisioning page.

To use secure boot or TPM on an existing supported server, you can enable them from the server details page.

To enable secure boot or TPM, the server needs to be powered off.
{: important}

### Keys that are stored in BIOS firmware
{: #secure-boot-keys-BIOS}

The following keys are stored in the BIOS firmware that you can use to sign a custom image. For more information about custom images, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).

* Microsoft Corporation UEFI CA 2011
* Microsoft Windows Production PCA 2011
* SUPERMICRO Product CA 2018
