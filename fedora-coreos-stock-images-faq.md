---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: fedora coreos, stock images, faq, lifecycle, support, updates, stream-based

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Fedora CoreOS stock images
{: #fedora-coreos-stock-images-faq}

Frequently asked questions for Fedora CoreOS stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which Fedora CoreOS releases are supported on IBM Cloud?
{: #fedora-coreos-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides Fedora CoreOS stock images on {{site.data.keyword.cloud_notm}} VPC infrastructure based on a rolling, stream-based release model. Fedora CoreOS does not have fixed lifecycle dates. Support is tied to remaining on a supported update stream and applying continuous updates.

See [Lifecycle for guest operating systems - Fedora CoreOS](/docs/vpc?topic=vpc-guest-os-lifecycle#fedora-coreos) for the Fedora CoreOS that {{site.data.keyword.cloud_notm}} VPC supports.

## What are the Fedora CoreOS support phases?
{: #fedora-coreos-support-phases}
{: faq}

Fedora CoreOS follows a stream-based, continuously delivered lifecycle rather than fixed major or minor versions, and therefore does not have traditional support phases such as Full Support, Maintenance Support, or Extended Life-cycle Support (ELS).

* Fedora CoreOS stock images distributed by {{site.data.keyword.cloud_notm}} follow the [Fedora CoreOS stream-based lifecycle and auto-update model](https://docs.fedoraproject.org/en-US/fedora-coreos/){: external} as documented by the Fedora Project.
* {{site.data.keyword.cloud_notm}} Fedora CoreOS stock images are community supported through Fedora CoreOS update streams and do not include defined support phases or Extended Life-cycle Support (ELS).

## When does IBM Cloud release Fedora CoreOS images?
{: #fedora-coreos-release-timing}
{: faq}

Fedora CoreOS stock images become available on {{site.data.keyword.cloud_notm}} following upstream Fedora CoreOS stream releases.

As Fedora CoreOS follows a rolling, stream-based lifecycle, concepts such as minor versions or Extended Update Support (EUS) do not apply, and {{site.data.keyword.cloud_notm}} publishes images aligned with the supported Fedora CoreOS streams.

## How often are the images updated?
{: #fedora-coreos-update-frequency}
{: faq}

Patch and update frequency
:   Fedora CoreOS images are refreshed on a quarterly basis.

Image update behavior
:   A new image with an incremented subscript is made available that includes the latest OS updates.

## What support is included?
{: #fedora-coreos-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Fedora CoreOS stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard Fedora CoreOS server included in the stock image, as well as for software available in the Fedora CoreOS repositories. These items are considered part of the base operating system functionality provided by Fedora Community.
* For Fedora CoreOS software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If Fedora community involvement is required, {{site.data.keyword.cloud_notm}} Support engages Fedora community support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.

## How do clients update the software on their Fedora CoreOS instances?
{: #fedora-coreos-software-updates}
{: faq}

Fedora CoreOS instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive automatic updates by Zincati daemon configured on these images.

## What are the lifecycle policies, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #fedora-coreos-lifecycle-policies}
{: faq}

Fedora CoreOS does not follow a fixed End-of-Support (EOS) or End-of-Life (EOL) date model. {{site.data.keyword.cloud_notm}} [VPC](/docs/vpc?topic=vpc-guest-os-lifecycle#fedora-coreos) aligns with the Fedora CoreOS community's continuous delivery and stream policies.

* Fedora CoreOS stock images are continuously updated. Older images are periodically deprecated and removed from the {{site.data.keyword.cloud_notm}} public catalog as newer validated images become available.
* Virtual servers provisioned from older images can continue running at the customer's risk. However, they may no longer receive updates or security fixes if they are not kept on a supported stream.
* Customers should keep their instances on the latest supported Fedora CoreOS stream to ensure automatic updates, security fixes, and continued compliance with lifecycle policies.

## How are configuration and hardening managed?
{: #fedora-coreos-configuration-hardening}
{: faq}

Fedora CoreOS images for {{site.data.keyword.cloud_notm}} are utilized from Fedora cloud builds with no additional configuration or hardening. Example of [Fedora CoreOS Stable stream download URL](https://www.fedoraproject.org/coreos/download?stream=stable){: external}.
