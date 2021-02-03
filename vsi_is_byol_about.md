---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-27"

keywords: license, virtual private cloud, BYOL, virtual server instance, instance, custom image, encryption
subcollection: vpc

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:beta: .beta}

# Bring your own license (Beta)
{: #byol-vpc-about}

For RedHat Enterprise Linux (RHEL) and Windows operating systems, you can bring your own license (BYOL) to the IBM Cloud VPC when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and and the OS vendor.
{:shortdesc}

BYOL is a beta feature that is available for evaluation and testing purposes. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

## BYOL concepts
{: #byol-vpc-concepts}
With this service, you can use your own license for a custom image you create at your site and then upload to Cloud Object Storage (COS). When you import your custom image from COS to the VPC, IBM provides Red Hat Linux and Windows BYOL operating system options you can choose. You use your BYOL custom images to create virtual server instances, just as you would from a stock image. 

There are no additional licensing charges for any virtual server instances you create using your BYOL custom image. For audit and reporting, IBM retains information for virtual server instances you create from BYOL custom images.

## BYOL for Red Hat Linux operating systems
{: #byol-vpc-linux}

You can use your own license for a [custom RHEL image](/docs/vpc?topic=vpc-create-linux-custom-image) you create on premesis. This BYOL custom image is a single QCOW2 file that you upload to Cloud Object Storage (COS) and then import to the VPC. When you import your BYOL custom image, you select a _BYOL_ operating system from the list of OS versions. Supported Linux versions are 64-bit RHEL 7 and RHEL 8. 

To see all OS versions from the API, make a`GET /operating_systems` call. In the response, you would see RHEL BYOL OS versions among the list of operatng systems. This example response shows information returned for RHEL 7:

```
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
{:codeblock}

For more information about creating and importing Linux custom images, see:

* [Create a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Import a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image)

## BYOL for Windows operating systems
{: #byol-vpc-linux}

You can create a Windows custom image using your own license, upload it to COS, and then import it to the VPC. The Windows BYOL custom image runs on a dedicated host, a single-tenant compute node for privileged users within your account. When you import your BYOL custom image, you select a _BYOL_ operating system from the list of OS versions. Supported Windows versions are Windows 2012 and Windows 2012 R2, 64-bit.

To see all OS versions from the API, make a`GET /operating_systems` call. In the response, you will see Windows BYOL OS versions among the list of operatng systems. This example response shows information returned for Windows 2012:

```
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
{:codeblock}

After you create a dedicated host, you can create virtual server instances using a BYOL custom image and your license. 

For information about creating a Windows custom image, see [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image).

For information on creating a dedicated host, see [Dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances#dedicated-hosts).

For information about importing your BYOL custom image to the VPC, see [Import a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image).
