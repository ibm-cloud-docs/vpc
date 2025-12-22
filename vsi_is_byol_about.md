---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-22"

keywords: license, virtual private cloud, BYOL, virtual server instance, instance, custom image, encryption, RHEL, SUSE
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Bring your own license
{: #byol-vpc-about}

For Red Hat Enterprise Linux&reg; and Windows operating systems, you can bring your own license (BYOL) to the {{site.data.keyword.cloud}} VPC when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor.
{: shortdesc}

## BYOL concepts
{: #byol-vpc-concepts}

With the BYOL feature, you can use your own license for a custom image that you create at your site and then upload to {{site.data.keyword.cos_full_notm}}. When you import your custom image from {{site.data.keyword.cos_full_notm}} to the VPC, {{site.data.keyword.cloud_notm}} provides Red Hat Enterprise Linux and Windows _BYOL_ operating system options that you must select to indicate that you are using a BYOL operating system. You use your BYOL custom images to create virtual server instances, just as you would from a stock image.

The BYOL feature on LinuxONE (s390x processor architecture) only supports SUSE Linux Enterprise Server (SLES) operating systems currently.

Images with a Red Hat Enterprise Linux BYOL or a Windows BYOL operating system can be used to provision an instance on a public or dedicated host.

You don't have extra licensing charges for any virtual server instances that you create by using your BYOL custom image. For auditing and reporting purposes, {{site.data.keyword.cloud_notm}} retains information for virtual server instances that you create from BYOL custom images.

## BYOL for Red Hat Enterprise Linux operating systems
{: #byol-vpc-linux}


You can use your own license for a [custom Linux image](/docs/vpc?topic=vpc-create-linux-custom-image) that you create on premises. This BYOL custom image is a single qcow2 or vhd file that you upload to {{site.data.keyword.cos_full_notm}} and then import to the VPC. When you import your BYOL custom image, you must select a _BYOL_ operating system from the list of OS versions. Supported Linux versions are 64-bit RHEL 7, RHEL 8, and RHEL 9.

To see all of the operating system versions from the API, make a `GET /operating_systems` call. In the response, you see Red Hat Enterprise Linux BYOL OS versions among the list of operating systems. This example response shows information that is returned for RHEL 7:

```json
{
      "href": "https://cloud.ibm.com/v1/operating_systems/red-7-amd64-byol",
      "name": "red-7-amd64-byol",
      "architecture": "amd64",
      "display_name": "Red Hat Enterprise Linux 7.x - BYOL (amd64)",
      "family": "Red Hat Enterprise Linux",
      "vendor": "Red Hat",
      "version": "7.x - Minimal Install"
    },
    .
    .
    .
```
{: codeblock}

For more information about creating and importing Linux custom images, see:

* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Importing a custom image](/docs/vpc?topic=vpc-custom-image-using-COS)

## BYOL for SUSE Linux Enterprise Server (SLES)
{: #byol-vpc-suse}

You can use your own license for a [custom Linux image](/docs/vpc?topic=vpc-create-linux-custom-image) that you create on premises. This BYOL custom image is a single qcow2 or vhd file that you upload to {{site.data.keyword.cos_full_notm}} and then import to the VPC. When you import your BYOL custom image, you must select a _BYOL_ operating system from the list of OS versions. Supported Linux version is SLES 15.

To see all of the operating system versions from the API, make a `GET /operating_systems` call. In the response, you see SUSE Linux Enterprise Server BYOL OS versions among the list of operating systems. This example response shows information that is returned for SLES 15:

```json
{
    ...
    "48": {
	"id": "jp-tok~sles-15-s390x-byol",
	"name": "sles-15-s390x-byol",
	"family": "SUSE Linux Enterprise Server",
	"href": "https://jp-tok.private.iaas.cloud.ibm.com/v1/operating_systems/sles-15-s390x-byol",
	"architecture": "s390x",
	"status": null,
	"description": null,
	"vendor": "SUSE",
	"version": "15",
	"bootType": null,
	"allowImageCreation": null,
	"__typename": "OperatingSystem"
    }
    ...
}
```
{: codeblock}

## BYOL for Windows operating systems
{: #byol-vpc-windows}

You can create a Windows BYOL custom image by using your own license, uploading the image to {{site.data.keyword.cos_full_notm}}, and importing the image to the VPC. Privileged users within your account can use the Windows BYOL custom image to provision an instance on a single-tenant dedicated host or on a shared, multi-tenant host.

Depending on the {{site.data.keyword.cloud_notm}} solution, Microsoft software is purchased pay-as-you-go (hourly, monthly) on shared or dedicated resources. For BYOL, as of October 19th 2022, you can provision BYOL Microsoft licenses on dedicated and shared hosts, which are priced on a monthly and yearly basis.
{: important}

When you import your BYOL custom image, you must select a _BYOL_ operating system from the list of operating system versions. The following are supported Windows versions:

* Windows 2012 and Windows 2012 R2 64-bit
* Windows 2016 and Windows 2016 64-bit
* Windows 2019 and Windows 2019 64-bit
* Windows 2022 64-bit

To see all of the operating system versions from the API, make a`GET /operating_systems` call. In the response, you see Windows BYOL OS versions among the list of operating systems. This example response shows information that is returned for Windows 2012:

```json
{
      "href": "https://cloud.ibm.com/v1/operating_systems/windows-2012-amd64-byol",
      "name": "windows-2012-amd64-byol",
      "architecture": "amd64",
      "display_name": "Windows 2012 - BYOL (amd64)",
      "family": "Windows",
      "vendor": "Microsoft",
      "version": "2012"
    },
    .
    .
    .
```
{: codeblock}

For more information from Microsoft about licensing, see the [Microsoft Licensing FAQ](https://www.microsoft.com/en-us/licensing/news/new-software-assurance-benefit-to-support-hosting-from-third-party-providers){: external}; the blog post, [New licensing benefits make bringing workloads and licenses to partners’ clouds easier](https://partner.microsoft.com/en-US/blog/article/new-licensing-benefits-make-bringing-workloads-and-licenses-to-partners-clouds-easier){: external}; and a training video, [Outsourcing software licensing changes](https://licensingschool.eventbuilder.com/hostingcustomer){: external}.

For more information about creating a Windows custom image, see [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image).

For more information about creating dedicated hosts and groups, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

For more information about importing your BYOL custom image to the VPC, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc&interface=ui).

For the UI procedure for creating a new instance and specifying a BYOL custom image, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).
