---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: rhel, red hat enterprise linux, stock images, faq, lifecycle, support, updates, eus, els

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Red Hat Enterprise Linux (RHEL) stock images
{: #rhel-stock-images-faq}

Frequently asked questions for Red Hat&reg; Enterprise Linux&reg; (RHEL) stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which RHEL releases are supported on IBM Cloud?
{: #rhel-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides RHEL stock images for commonly used major versions that remain within Red Hat's active support window.

See [Lifecycle for guest operating systems - Red Hat Enterprise Linux (RHEL)](/docs/vpc?topic=vpc-guest-os-lifecycle#rhel) for the Red Hat versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit Red Hat support phases.

## What are the RHEL support phases?
{: #rhel-support-phases}
{: faq}

Most RHEL major releases have an approximately 10-year standard lifecycle, progressing from Full Support Phase to Maintenance Support Phase, with Extended Life Cycle Support (ELS) available optionally beyond the standard support period.

* RHEL stock images distributed by {{site.data.keyword.cloud_notm}} follow [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata#RHEL9_Planning_Guide){: external}.
* {{site.data.keyword.cloud_notm}} RHEL stock images include Red Hat support during the Full Support Phase and Maintenance Support Phase as defined by Red Hat's life cycle, while Extended Life Cycle Support (ELS) is not included and requires a separate Red Hat subscription.

## When does IBM Cloud release RHEL images?
{: #rhel-release-timing}
{: faq}

New RHEL major versions become available as stock images after Red Hat GA and internal {{site.data.keyword.cloud_notm}} validation.

{{site.data.keyword.cloud_notm}} provides Red Hat Enterprise Linux (RHEL) images only for releases that are eligible for Extended Update Support (EUS) and for final minor (dot) releases (for example, RHEL 9.10). Instances deployed using these images are entitled to the corresponding RHEL support, in accordance with Red Hat's life cycle and support policies.

Minor version stock images follow Red Hat's minor release schedule, typically shortly after vendor publication.

## How often are the images updated?
{: #rhel-update-frequency}
{: faq}

Patch and update frequency
:   RHEL images are refreshed on a monthly basis to incorporate all security fixes for the included software released by Red Hat up to the build date.

Image update behavior
:   A new image with an incremented subscript is made available. For example, for a RHEL 9.6 OS, the current month updated image name would be `ibm-redhat-9-6-minimal-amd64-5`, while the previous month image name would be `ibm-redhat-9-6-minimal-amd64-4`, which would be set to deprecated state when the updated image is made available.

## What support is included?
{: #rhel-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Red Hat stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers any issue with the cloud infrastructure and the stock images, such as driver mismatch or licensing.

* In addition, support is available for the standard Red Hat server included in the stock image, as well as for software available in the Red Hat repository, such as iSCSI configuration. These items are considered part of the base operating system functionality provided by Red Hat and included in the stock image.
* For Red Hat software that is purchased natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact for any issue with a Red Hat software-related issue. If Red Hat involvement is required to solve a potential issue, {{site.data.keyword.cloud_notm}} support engages Red Hat support. L2 support is provided and if resolution cannot be provided, {{site.data.keyword.cloud_notm}} Support team escalates directly to the Red Hat team.
* Clients who build custom images that are derived from {{site.data.keyword.cloud_notm}} stock images are responsible for resolving any issues that are related to the custom image, such as driver mismatch, licensing, or additional software installations.
* If clients desire a direct support relationship with Red Hat, then purchasing your Red Hat Enterprise Linux licenses directly from Red Hat through their Cloud Access program is the best option. Visit the [Red Hat website](https://www.redhat.com/en/technologies/cloud-computing/cloud-access){: external} for more information about Red Hat Cloud Access Program.
* For additional support information, see the [FAQ about Red Hat and IBM Cloud infrastructure](/docs/infrastructure-hub?topic=infrastructure-hub-faqs-for-red-hat-ibm-cloud).

Paid support
:   No paid support options exist (extended support, etc).

## How do clients update the software on their RHEL instances?
{: #rhel-software-updates}
{: faq}

RHEL instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal Red Hat Satellite mirrors via `YUM` or `DNF` package managers.

These servers mirror the content from [Red Hat CDN](https://cdn.redhat.com/){: external}, which is controlled by Red Hat.

Access to this content requires valid Red Hat credentials tied to an active subscription.

The Satellite server is configured to connect directly to the Red Hat Content Delivery Network (CDN), and client systems then pull content from Satellite.

{{site.data.keyword.cloud_notm}} Satellite servers are synchronized with the Red Hat Content Delivery Network (CDN) twice daily.

## What are the lifecycle policies of RHEL OS, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #rhel-lifecycle-policies}
{: faq}

In the lifecycle of an operating system, EOS is the last date that {{site.data.keyword.cloud_notm}} delivers standard support for a version or release of a product. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#rhel) align with the Red Hat EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from the public catalog.
* If the virtual server is provisioned before the EOS date, customer can continue to use EOS software at their own risk. No bug or security fixes are available.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #rhel-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific RHEL configurations are applied:

* `gpgcheck` is enabled.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
* Permissive mode is enabled for SELinux.
