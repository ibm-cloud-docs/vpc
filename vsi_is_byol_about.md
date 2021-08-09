---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-06"

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

# Bring your own license
{: #byol-vpc-about}

For Red Hat Enterprise Linux&reg; (RHEL) and Windows&reg; operating systems, you can bring your own license (BYOL) to the {{site.data.keyword.cloud}} VPC when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor.
{:shortdesc}

## BYOL concepts
{: #byol-vpc-concepts}
With the BYOL feature, you can use your own license for a custom image that you create and upload it to Cloud Object Storage. When you import your custom image from Cloud Object Storage to the VPC, {{site.data.keyword.cloud}} provides Red Hat Enterprise Linux and Windows *BYOL* operating system options that you must select to indicate that you are using a BYOL operating system. You use your BYOL custom images to create virtual server instances, just as you would from a stock image.

The BYOL feature on LinuxONE (s390x processor architecture) supports Red Hat Enterprise Linux (RHEL), and SUSE Linux Enterprise Server (SLES) operating systems.

Images with a Red Hat Enterprise Linux BYOL operating system can be used to provision any public or dedicated host instance. Images with a Windows BYOL operating system can be provisioned only to instances on a dedicated host or in a dedicated host group.

You don't have extra licensing charges for any virtual server instances that you create by using your BYOL custom image. For auditing and reporting purposes, {{site.data.keyword.cloud}} retains information for virtual server instances that you create from BYOL custom images.

## BYOL for Red Hat Linux operating systems
{: #byol-vpc-linux}

You can use your own license for a [custom RHEL image](/docs/vpc?topic=vpc-create-linux-custom-image) that you create on premises. This BYOL custom image is a single QCOW2 file that you upload to Cloud Object Storage and then import to the VPC. When you import your BYOL custom image, you must select a _BYOL_ operating system from the list of OS versions. Supported Linux versions are 64-bit RHEL 7 and RHEL 8.

To see all of the operating system versions from the API, make a`GET /operating_systems` call. In the response, you see Red Hat Enterprise Linux BYOL OS versions among the list of operating systems. This example response shows information that is returned for RHEL 7:

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
{: codeblock}

For more information about creating and importing Linux custom images, see:

* [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image)
* [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image)

## BYOL for Windows operating systems
{: #byol-vpc-windows}

You can create a Windows BYOL custom image by using your own license, upload it to Cloud Object Storage, and import it to the VPC. The Windows BYOL custom image must be provisioned on a dedicated host single-tenantfor privileged users within your account.

Windows BYOL custom images can be used to provision virtual server instances on dedicated hosts only. Windows BYOL custom images cannot be used to provision public instances.  
{:important}

When you import your BYOL custom image, you must select a _BYOL_ operating system from the list of operating system versions. The following are supported Windows versions:

* Windows 2012 and Windows 2012 R2 64-bit
* Windows 2016 and Windows 2016 64-bit
* Windows 2019 and Windows 2019 64-bit

To see all of the operating system versions from the API, make a`GET /operating_systems` call. In the response, you see Windows BYOL OS versions among the list of operating systems. This example response shows information that is returned for Windows 2012:

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
{: codeblock}

After you create a dedicated host, you can create virtual server instances by using a BYOL custom image and your license. For the UI procedure, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

For more information about creating a Windows custom image, see [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image).

## BYOL for SLES
{: #byol-zvsi-sles}

If you are running LinuxONE (s390x processor architecture), you can use an {{site.data.keyword.cloud}} provided SLES image and the `SUSEConnect` command line tool to add the license to your installation and perform all the activation steps. For the procedure, see [How to register SLES by using the SUSEConnect command line tool](https://www.suse.com/support/kb/doc/?id=000018564).


For more information about creating dedicated hosts and groups, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances).

For more information about importing yoåur BYOL custom image to the VPC, see [Importing a custom image](/docs/vpc?topic=vpc-managing-images#import-custom-image).

For the UI procedure for creating a new instance and specifying a BYOL custom image, see [Creating virtual server instances by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
