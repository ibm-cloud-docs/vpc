---

copyright:
  years: 2020, 2024

lastupdated: "2024-02-21"

keywords: troubleshooting bare metal servers, hardware issues, firmware

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting Bare Metal Servers for VPC
{: #troubleshoot_bare_metal}

The following topics cover common difficulties that you might encounter, and offers some helpful tips.

## How do I fix a hardware issue?
{: #bare-metal-troubleshoot-hardware-issues}
{: troubleshoot}
{: support}

If your bare metal server experiences a hardware issue, you can request support by creating a support case. For more information, see [Creating support cases](/docs/get-support?topic=get-support-open-case).

After the operation team receives the case, the server goes into a maintenance state.

You must power off the bare metal server before it can go into the **Maintenance** state.
{: note}

You can't start a bare metal server that's in a **Maintenance** state. The KVM console is also unavailable. You can't connect to the system over the network.

During maintenance, IBM also has no access to your network and workloads. The data on the disks isn't examined. But we still recommend you use software encryption for added security.

You can delete the bare metal server that is in the **Maintenance** state.
{: note}

When the issues are fixed, the server is handed back to you and the state returns to **Stopped**.

## How do I manage firmware updates?
{: #bare-metal-troubleshoot-firmware-update}
{: troubleshoot}
{: support}

Firmware for bare metal servers is managed by IBM. Manual firmware changes aren’t supported on the disk controller or disk drives without direction from IBM.

## Why did my custom image fail to boot?
{: #bare-metal-troubleshoot-custom-image-fail-boot}
{: troubleshoot}
{: support}

If your custom image fails to boot, open the console and debug the operating system boot sequence. The following are common issues that can cause a custom image boot failure.
* An unsigned operating system. Disable secure boot and try starting the system again.
* UEFI boot not support. Verify that the image has an EFI disk partition with an EFI boot loader.

If you need more help, you can request support by creating a support case. For more information, see [Creating support cases](/docs/get-support?topic=get-support-open-case).
