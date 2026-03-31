---

copyright:
  years: 2026
lastupdated: "2026-03-31"

keywords: ubuntu, stock images, faq, lifecycle, support, updates, lts

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Ubuntu stock images
{: #ubuntu-stock-images-faq}

Frequently asked questions for Ubuntu stock images on {{site.data.keyword.cloud}} include questions about which releases are supported and how often stock images are updated.
{: shortdesc}

## Which Ubuntu releases are supported on IBM Cloud?
{: #ubuntu-supported-releases}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} provides Ubuntu stock images for commonly used major versions that remain within the active support window from Canonical.

See [Lifecycle for guest operating systems - Ubuntu LTS](/docs/vpc?topic=vpc-guest-os-lifecycle#ubuntu-lts) for the Ubuntu versions that {{site.data.keyword.cloud_notm}} VPC supports.

## What are the Ubuntu support phases?
{: #ubuntu-support-phases}
{: faq}

Ubuntu follows a structured release and support model consisting of Long-Term Support (LTS) and interim (non-LTS) releases.

* LTS releases are supported for a total of 10 years, including 5 years of General Support followed by 5 years of Expanded Security Maintenance (ESM).
* The General Support phase is further divided into Hardware and Maintenance Updates for the first 2 years, followed by Maintenance Updates for the subsequent 3 years.
* Throughout the supported lifecycle of an Ubuntu release, Canonical provides security maintenance. Basic Security Maintenance applies to binary packages included in the main and restricted components of the Ubuntu archive and is typically provided for 5 years from the LTS release date.
* Ubuntu stock images distributed by {{site.data.keyword.cloud_notm}} follow the [Ubuntu kernel lifecycle](https://ubuntu.com/kernel/lifecycle){: external}.
* {{site.data.keyword.cloud_notm}} Ubuntu stock images include Canonical support during the Standard Support as defined by Canonical's lifecycle, while Extended Security Maintenance Support (ESM) is not included and requires a separate subscription from Canonical.

## When does IBM Cloud release Ubuntu images?
{: #ubuntu-release-timing}
{: faq}

New Ubuntu LTS versions become available as stock images after Canonical GA and internal {{site.data.keyword.cloud_notm}} validation.

## How often are the images updated?
{: #ubuntu-update-frequency}
{: faq}

Patch and update frequency
:   Ubuntu images are patched and updated on a monthly basis.

Image update behavior
:   A new Ubuntu image is made available with an incremented subscript to reflect the updated release.

## What support is included?
{: #ubuntu-support-included}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Ubuntu stock images. {{site.data.keyword.cloud_notm}} [Support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) covers issues related to the cloud infrastructure and stock images, such as driver mismatches or licensing.

* Support is available for the standard Ubuntu server included in the stock image, as well as for software available in the Ubuntu repositories. These items are considered part of the base operating system functionality provided by Canonical.
* For Ubuntu software purchased or subscribed natively on {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Support is the first point of contact. If Canonical involvement is required, {{site.data.keyword.cloud_notm}} Support engages Canonical support.
* Clients who build custom images derived from {{site.data.keyword.cloud_notm}} images are responsible for resolving any issues related to the custom image, such as driver mismatches, licensing, or additional software installations.
* For a direct support relationship with Canonical, customers can enroll in an Ubuntu Pro subscription.

## How do clients update the software on their Ubuntu instances?
{: #ubuntu-software-updates}
{: faq}

Ubuntu instances created from {{site.data.keyword.cloud_notm}} stock images are configured to receive updates from internal Ubuntu mirrors via the `Apt` package manager.

## What are the lifecycle policies of Ubuntu OS, including End-of-Life (EOL) and End-of-Support (EOS)?
{: #ubuntu-lifecycle-policies}
{: faq}

In the lifecycle of an operating system, EOS is the last date that {{site.data.keyword.cloud_notm}} delivers standard support for a version or release of a product. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle#ubuntu-lts) align with the Ubuntu EOS dates.

* Deprecated stock images no longer receive updates and are eventually removed from the public catalog.
* If the virtual server is provisioned before the EOS date, the customer can continue to use EOS software at their own risk. No bug and security fixes are available.
* Customers must migrate instances to supported versions before lifecycle deadlines.

## How are configuration and hardening managed?
{: #ubuntu-configuration-hardening}
{: faq}

The following {{site.data.keyword.cloud_notm}}-specific Ubuntu configurations are applied:

* `gpgcheck` is enabled.
* Password authentication is disabled.
* The `scripts-vendor` module is enabled in `cloud.cfg`.
