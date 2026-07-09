---

copyright:
  years: 2020, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot bare metal server, custom image boot failure, bare metal custom image, UEFI boot, secure boot, EFI partition, image fails to boot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my custom image fail to boot on a bare metal server?
{: #troubleshoot-bare-metal-custom-image-boot}
{: troubleshoot}
{: support}

Your custom image fails to boot when you start a bare metal server.
{: shortdesc}

Your bare metal server does not start, or the operating system does not load, after you use a custom image to provision or restart the server.
{: tsSymptoms}

Custom image boot failures are commonly caused by one of the following issues:

- The operating system is unsigned and secure boot is enabled.
- The image does not have a UEFI-compatible boot configuration.
{: tsCauses}

Open the KVM console and review the operating system boot sequence to diagnose the issue. Then, try one of the following resolutions:
{: tsResolve}

Unsigned operating system
:   If the operating system is unsigned, disable secure boot and try starting the server again.

UEFI boot is not supported
:   Verify that the image has an EFI disk partition that contains a valid EFI boot loader.

If you need more help after trying these steps, create a support case. For more information, see [Creating support cases](/docs/support?topic=support-open-case).
