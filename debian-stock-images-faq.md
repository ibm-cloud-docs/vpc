---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: debian, stock images, faq, lifecycle, support, updates

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Debian stock images
{: #debian-stock-images-faq}

Frequently asked questions for Debian stock images on {{site.data.keyword.cloud}} might include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which Debian releases are supported on IBM Cloud?
{: #debian-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides Debian stock images for commonly used major versions that remain within Debian's active support window.

See [Lifecycle for guest operating systems - Debian](/docs/vpc?topic=vpc-guest-os-lifecycle#debian) for the Debian versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit Debian support phases.

## What are the Debian support phases?
{: #debian-support-phases}
{: faq}

Most Debian major releases have an approximately 5-year standard lifecycle, progressing from Debian's Standard Security Support to Long Term Support (LTS).

* Debian stock images distributed by {{site.data.keyword.cloud_notm}} follow the [Debian Project's official release and support lifecycle](https://www.debian.org/releases/){: external} as defined by the Debian community.
* {{site.data.keyword.cloud_notm}} Debian stock images include Debian support during the Debian Security Support phase and Debian Long Term support as defined by Debian's lifecycle, while Extended Long Term Support (ELTS) continues beyond EOS and is provided by the Debian community.

## When does IBM Cloud release Debian images?
{: #debian-release-timing}
{: faq}

New Debian major versions become available as stock images after Debian *stable* GA and internal {{site.data.keyword.cloud_notm}} validation.

## How often are the images updated?
{: #debian-update-frequency}
{: faq}

Patch and update frequency
:   Debian images are refreshed on a quarterly basis.

Image update behavior
:   A new image with an incremented subscript is made available that includes the latest OS updates.

## What support is included?
{: #debian-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Debian stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard Debian server included in the stock image, as well as for software available in the Debian repositories. These items are considered part of the base operating system functionality provided by Debian community.
* For Debian software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If Debian community involvement is required, {{site.data.keyword.cloud_notm}} Support engages Debian community support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.

## How do clients update the software on their Debian instances?
{: #debian-software-updates}
{: faq}

Debian instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal Debian mirrors via the Advanced Package Tool (APT) package manager.

Clients are expected to patch on a regular basis for their Debian instances and not wait for an updated image release. For more information, see [Understanding your responsibilities when you use Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #debian-lifecycle-policies}
{: faq}

EOS is the last date {{site.data.keyword.cloud_notm}} delivers standard support for a release. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#debian) align with Debian community EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from the public catalog.
* Virtual servers provisioned before the EOS date can continue using EOS software at the customer's risk. No bug or security fixes are provided.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #debian-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific Debian configurations are applied:

* GPG check is enabled.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
