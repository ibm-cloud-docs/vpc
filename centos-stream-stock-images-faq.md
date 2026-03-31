---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: centos stream, stock images, faq, lifecycle, support, updates

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for CentOS Stream stock images
{: #centos-stream-stock-images-faq}

Frequently asked questions for CentOS Stream stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which CentOS Stream releases are supported on IBM Cloud?
{: #centos-stream-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides CentOS Stream stock images for commonly used major versions that remain within CentOS Stream's active support window.

See [Lifecycle for guest operating systems - CentOS Stream](/docs/vpc?topic=vpc-guest-os-lifecycle#centos) for the CentOS Stream versions that {{site.data.keyword.cloud_notm}} VPC supports.

Older versions are deprecated when they exit CentOS Stream support phases.

## What are the CentOS Stream support phases?
{: #centos-stream-support-phases}
{: faq}

Most CentOS Stream major releases have an approximately 5-year standard lifecycle, progressing through Active Support and Security Support phases, after which the release reaches End of Support (EOS).

* CentOS Stream stock images distributed by {{site.data.keyword.cloud_notm}} follow [CentOS Stream's lifecycle](https://docs.centos.org/centos-stream-docs/){: external}.
* {{site.data.keyword.cloud_notm}} CentOS Stream stock images include community-supported updates during the Active Support and Security Support phases as defined by the CentOS Stream lifecycle. Extended Life-cycle Support (ELS) is not available for CentOS Stream, and support ends at the published EOS date.

## When does IBM Cloud release CentOS Stream images?
{: #centos-stream-release-timing}
{: faq}

New CentOS Stream major versions become available as stock images after upstream GA and internal {{site.data.keyword.cloud_notm}} validation.

## How often are the images updated?
{: #centos-stream-update-frequency}
{: faq}

Patch and update frequency
:   CentOS Stream images are refreshed on a quarterly basis.

Image update behavior
:   A new image with an incremented subscript is made available that includes the latest OS updates.

## What support is included?
{: #centos-stream-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and CentOS Stream stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard CentOS Stream server included in the stock image, as well as for software available in the CentOS Stream repositories. These items are considered part of the base operating system functionality provided by CentOS Community.
* For CentOS Stream software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If CentOS Community involvement is required, {{site.data.keyword.cloud_notm}} support engages CentOS Community support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.

## How do clients update the software on their CentOS Stream instances?
{: #centos-stream-software-updates}
{: faq}

CentOS Stream instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal CentOS Stream mirrors via yum or dnf package managers.

Clients are expected to patch on a regular basis for their CentOS Stream instances and not wait for an updated image release. For more information, see [Understanding your responsibilities when you use Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #centos-stream-lifecycle-policies}
{: faq}

EOS is the last date {{site.data.keyword.cloud_notm}} delivers standard support for a release. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#centos) align with CentOS Community EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from the public catalog.
* Virtual servers provisioned before the EOS date can continue using EOS software at the customer's risk; no bug or security fixes are provided.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #centos-stream-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific CentOS Stream configurations are applied:

* `gpgcheck` is enabled for the `yum` package manager.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
* Permissive mode is enabled for SELinux.
