---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: rocky linux, stock images, faq, lifecycle, support, updates

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Rocky Linux stock images
{: #rocky-linux-stock-images-faq}

Frequently asked questions for Rocky Linux stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which Rocky Linux releases are supported on IBM Cloud?
{: #rocky-linux-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides Rocky Linux stock images for commonly used major versions that remain within the active support window for Rocky Linux.

See [Lifecycle for guest operating systems - Rocky Linux](/docs/vpc?topic=vpc-guest-os-lifecycle#rocky-linux) for the Rocky Linux versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit Rocky Linux support phases.

## What are the Rocky Linux support phases?
{: #rocky-linux-support-phases}
{: faq}

Most Rocky Linux major releases have an approximately 10-year standard lifecycle, providing Active Support and security maintenance support through the community-supported Rocky Linux lifecycle.

* Rocky Linux stock images distributed by {{site.data.keyword.cloud_notm}} follow the [Rocky Linux Release and Version Guide](https://wiki.rockylinux.org/rocky/version/){: external}.
* {{site.data.keyword.cloud_notm}} Rocky Linux stock images include Rocky Linux support during the active support and the security support phases as defined by the Rocky Linux life cycle.

## When does IBM Cloud release Rocky Linux images?
{: #rocky-linux-release-timing}
{: faq}

New Rocky Linux major versions become available as {{site.data.keyword.cloud_notm}} stock images after the Rocky Linux community release and completion of internal {{site.data.keyword.cloud_notm}} validation.

Minor version stock images follow the Rocky Linux release cadence, typically becoming available on {{site.data.keyword.cloud_notm}} shortly after the corresponding community update is published.

{{site.data.keyword.cloud_notm}} publishes Rocky Linux stock images for all supported minor releases during the active lifecycle of the major version.

## How often are the images updated?
{: #rocky-linux-update-frequency}
{: faq}

Patch and update frequency
:   Rocky Linux images are refreshed on a quarterly basis.

Image update behavior
:   A new image with an incremented subscript is made available that includes the latest OS updates.

## What support is included?
{: #rocky-linux-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Rocky Linux stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard Rocky Linux server included in the stock image, as well as for software available in the Rocky Linux repositories. These items are considered part of the base operating system functionality provided by the Rocky Linux community.
* For Rocky Linux software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If Rocky Linux community involvement is required, {{site.data.keyword.cloud_notm}} Support engages Rocky Linux community support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.

## How do clients update the software on their Rocky Linux instances?
{: #rocky-linux-software-updates}
{: faq}

Rocky Linux instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal Rocky Linux mirrors via `YUM` or `DNF` package managers.

Clients are expected to patch on a regular basis for their Rocky Linux instances and not wait for an updated image release. For more information, see [Understanding your responsibilities when you use Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #rocky-linux-lifecycle-policies}
{: faq}

EOS is the last date {{site.data.keyword.cloud_notm}} delivers standard support for a release. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#rocky-linux) align with Rocky Linux community EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from public catalog.
* Virtual servers provisioned before the EOS date can continue using EOS software at the customer's risk. No bug or security fixes are provided.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #rocky-linux-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific Rocky Linux configurations are applied:

* `gpgcheck` is enabled.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
* Permissive mode is enabled for SELinux.
