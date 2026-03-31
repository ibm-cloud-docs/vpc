---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: windows server, stock images, faq, lifecycle, support, updates, patch tuesday, microsoft

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Microsoft Windows Server stock images
{: #windows-server-stock-images-faq}

Frequently asked questions for Microsoft Windows Server stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which Windows Server releases are supported on IBM Cloud?
{: #windows-server-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides Windows Server stock images for commonly used major versions that remain within Microsoft Windows Server's active support window.

See [Lifecycle for guest operating systems - Microsoft Windows Server](/docs/vpc?topic=vpc-guest-os-lifecycle#windows-server) for the Windows Server versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit Microsoft Windows Server support phases.

## What are the Microsoft support phases?
{: #windows-server-support-phases}
{: faq}

Windows Server major releases have an approximately 10-year standard lifecycle, progressing from Mainstream support to Extended support, with Extended Security Updates (ESU) available optionally beyond the standard support period.

* Windows Server stock images distributed by {{site.data.keyword.cloud_notm}} follow [Microsoft's Windows Server lifecycle](https://learn.microsoft.com/en-us/windows/release-health/windows-server-release-info){: external}.
* During the Mainstream support phase, Windows Server includes full security updates, bug fixes, feature updates, minor releases, and new hardware enablement.
* During the Extended support phase, Windows Server includes security updates and critical bug fixes only, with no new features or hardware enablement.
* Extended Security Updates (ESU) are not included by default and require a separate, paid subscription to receive critical and select urgent security fixes beyond the standard lifecycle.

## How often are the images updated?
{: #windows-server-update-frequency}
{: faq}

Patch and update frequency
:   Windows images are refreshed on a monthly basis. Patch Tuesday updates are applied to newly published Windows images.

Image update behavior
:   A new Windows image is made available with an incremented subscript to reflect the updated release.

## When does IBM Cloud release Windows Server images?
{: #windows-server-release-timing}
{: faq}

New Windows Server versions become available as stock images after Microsoft Windows Server GA and internal {{site.data.keyword.cloud_notm}} validation.

## What support is included?
{: #windows-server-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Microsoft Windows Server stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers any issue with the cloud infrastructure and the stock images, such as driver mismatch or licensing.

* For Microsoft Windows Server software that is purchased natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact for any issue with a Microsoft Windows Server software-related issue. If Microsoft Windows Server involvement is required to solve a potential issue, {{site.data.keyword.cloud_notm}} Support engages Microsoft Windows Server support.
* For custom images, it is the client's responsibility to resolve any issues that are related to the custom image, such as driver mismatch, licensing, or additional software installations.
* If clients desire a direct support relationship with Microsoft, then purchasing support directly from Microsoft is the best option. Visit the [Microsoft website](https://support.microsoft.com/){: external} for more information.

## How do clients update the software on their Windows Server instances?
{: #windows-server-software-updates}
{: faq}

Customers receive updates through Patch Tuesday, with the latest Windows updates made available in the published Windows images.

{{site.data.keyword.cloud_notm}} uses an internal Windows KMS server (`kms.adn.networklayer.com`) to support activation and update-related services.

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #windows-server-lifecycle-policies}
{: faq}

In the lifecycle of an operating system, EOS is the last date that {{site.data.keyword.cloud_notm}} delivers standard support for a version or release of a product. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#windows-server) align with the Microsoft Windows Server EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from public catalogs.
* If the virtual server is provisioned before the EOS date, customer can continue to use EOS software at their own risk while no bug and security fixes are available.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #windows-server-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific Windows configurations are applied:

* TLS 1.0 and TLS 1.1 are disabled. TLS 1.2 is enabled
* Server Message Block (SMB) Protocol Version 2 is enabled.
* SSL 3.0 protocol is disabled.
* Compression block for SMB 3.0 is disabled.
* VirtIO drivers are installed.
* Link Local Multicast Name Resolution (LLMNR) is disabled.
