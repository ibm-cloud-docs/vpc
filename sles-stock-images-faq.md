---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: SLES, SUSE Linux Enterprise Server, stock images, faq, lifecycle, support, updates

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for SUSE Linux Enterprise Server stock images
{: #sles-stock-images-faq}

Frequently asked questions for SUSE Linux Enterprise Server (SLES) stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which SLES releases are supported on IBM Cloud?
{: #sles-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides SLES stock images for commonly-used major versions that remain within the  active support window for SUSE.

See [Lifecycle for guest operating systems - SLES](/docs/vpc?topic=vpc-guest-os-lifecycle#suse) for the SLES versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit SUSE support phases.

## What are the SLES support phases?
{: #sles-support-phases}
{: faq}

SUSE Linux Enterprise Server (SLES) follows a thirteen-year product lifecycle for each major release. This lifecycle includes ten years of General Support, followed by up to three additional years of paid Long Term Service Pack Support (LTSS).

Major SLES versions are typically released every three to four years, while minor releases, referred to as Service Packs, are issued approximately once every twelve months.

* SLES stock images distributed by {{site.data.keyword.cloud_notm}} follow the [SUSE Product Support Lifecycle](https://www.suse.com/lifecycle/){: external}.
* {{site.data.keyword.cloud_notm}} SLES stock images include SUSE support during the Standard Support as defined by the SUSE lifecycle, while LTSS Extended Security is not included and requires a separate subscription from SUSE.

## When does IBM Cloud release SLES images?
{: #sles-release-timing}
{: faq}

New SLES versions become available as {{site.data.keyword.cloud_notm}} stock images after SUSE GA and internal {{site.data.keyword.cloud_notm}} validation.

## How often are the images updated?
{: #sles-update-frequency}
{: faq}

Patch and update frequency
:   SLES images are refreshed on a quarterly basis.

Image update behavior
:   A new image with an incremented subscript is made available that includes the latest OS updates.

## What support is included?
{: #sles-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and SLES stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard SLES server included in the stock image, as well as for software available in the SLES repositories. These items are considered part of the base operating system functionality provided by SUSE.
* For SLES software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If SUSE involvement is required, {{site.data.keyword.cloud_notm}} Support engages SUSE support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.
* For a direct support relationship with SUSE, customers may purchase and register a SUSE subscription directly with SUSE. This gives a direct contractual relationship with SUSE for support.

## How do clients update the software on their SLES instances?
{: #sles-software-updates}
{: faq}

SLES instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal RMT servers via `Zypper` package manager.

Clients are expected to patch on a regular basis for their SLES instances and not wait for an updated image release. For more information, see [Understanding your responsibilities when you use Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #sles-lifecycle-policies}
{: faq}

EOS is the last date {{site.data.keyword.cloud_notm}} delivers standard support for a release. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#suse) align with SLES EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from the public catalog.
* Virtual servers provisioned before the EOS date can continue using EOS software at the customer's risk. No bug or security fixes are provided.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #sles-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific SLES configurations are applied:

* `gpgcheck` is enabled.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
