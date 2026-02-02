---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-02"

subcollection: vpc

---
{{site.data.keyword.attribute-definition-list}}


# x86 virtual server images
{: #about-images}

When you provision {{site.data.keyword.vsi_is_full}} on x86 architecture, you can select from the supported virtual server operating system stock images, the virtual server operating system bundle stock image, or a custom image that you import from {{site.data.keyword.cos_full_notm}}. The image that you select determines the operating system that is provisioned for your instance. If the image you select is a virtual server operating system bundle stock image, the software that is part of that bundle is also included in your instance.
{: shortdesc}

## Stock images
{: #stock-images}

The following operating systems are available as stock images when you create a virtual server.

### Supported x86_64 stock image operating systems
{: #x86-supported-os}

| Image | Architectures |
|---------|---------|
| CentOS 7.x | x86-64 |
| CentOS Stream 9.x, 10.x | x86-64 |
| Debian 11.x, 12.x, 13.x | x86-64 |
| Fedora Core OS | x86-64 |
| Red Hat Enterprise Linux 8.x, 9.x | x86-64 |
| Red Hat Enterprise Linux for SAP 8.x, 9.x | x86-64 |
| RHEL AI 1.x for Nvidia | x86-64 |
| RHEL AI 1.x for AMD| x86-64 |
| RHEL AI 1.x for Intel | x86-64 |
| Rocky Linux 8.x, 9.x, 10.x | x86-64 |
| SUSE Linux Enterprise Server 12.x, 15.x, 16.x | x86-64 |
| SUSE Linux Enterprise Server for SAP 12.x, 15.x | x86-64 |
| Ubuntu 20.04.x, 22.04.x, 24.04.x | x86-64 |
| Windows 2016, 2019, 2022, 2025 | x86-64 |
{: caption="Supported x86_64 stock image operating systems" caption-side="top"}

For more information about support for Red Hat, see [FAQs about Red Hat and IBM Cloud® infrastructure](/docs/infrastructure-hub?topic=infrastructure-hub-faqs-for-red-hat-ibm-cloud).

When you select an RHEL AI 1.x image, make sure that you are using the correct RHEL AI image for the instance profile you chose. For more information about the GPU profiles you can use with RHEL AI images, see [Accelerated instance profiles - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family). For information about RHEL AI, including information such as the supported use cases for RHEL AI, see [Red Hat Enterprise Linux AI](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_ai/1.5){: external}.
{: note}

### Supported x86_64 virtual server operating system bundle stock image
{: #x86-supported-os-bundle-image}

| Image | Architectures |
|---------|---------|
| Windows Server 2019 Standard Edition with SQL Server 2019 Web Edition | x86-64
{: caption="Supported x86_64 virtual server operating system bundle stock image" caption-side="bottom"}

If you plan to use Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about).
{: note}

 For more information about images for IBM Z (s390x processor architecture), see [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images).

When you order an instance, the images are cloud-init enabled to optimize creation times. With a cloud-init enabled image, you can provide user data. In the **User Data** field on the order form, you can enter optional cloud-init user data for the server. For more information about user data and automation, see [User data](/docs/vpc?topic=vpc-user-data).

You can access details about each operating system, such as the url for the operating system, by using the API call, [List all operating systems](https://cloud.ibm.com/apidocs/vpc#list-operating-systems){: external}.
{: tip}

### Stock image naming conventions
{: #image-naming-conventions}

All IBM-provided stock, public images are named by using the following convention:

```sh
ibm-<family>-<version>-<type>-<architecture>-<build>
```

For example,

```sh
ibm-centos-7-6-minimal-amd64-2
```

The following list explains the variables that make up the components of the image name:
* The leading prefix of `ibm-` is used for IBM-provided images. Custom images cannot be named with this prefix.
* The `family` component provides the operating system family, such as *redhat*, *debian*, or *windows-server*.
* The `version` component provides the operating system version, such as *18-04* for Ubuntu 18.04, or *2012-r2* for Windows 2012 R2.
* The `type` component provides the minimization level of the operating system image, such as *minimal* or *full*.
* The `architecture` component provides the vCPU architecture that is supported by the operating system image, such as *amd64*.
* The `build` component is a small, nonnegative integer that is incremented each time a new build of the operating system is created. For image names that are otherwise identical, the image with the highest build value is the most recent image for that operating system.

You can obtain the current list of images, including stock images, by running the following command in the command-line interface: [ibmcloud is images](/docs/vpc?topic=vpc-vpc-reference#compute-images).

The image naming convention is subject to change. The list of image names is not intended to be programmatically parsed or interpreted. You can use the [GET /images](/apidocs/vpc/latest#get-image) API to obtain metadata in a structured format.
{: important}

## Custom images
{: #custom-images}

You can import an image from {{site.data.keyword.cos_full_notm}} to use for creating a new virtual server instance.

### Requirements
{: #custom-image-reqs}

Custom images must meet the following requirements:

- Contain a single file or volume
- Is in qcow2 or vhd format
- Is cloud-init enabled
- The operating system is supported as a stock image
- Size doesn't exceed 250 GB
- Size isn't less than 10 GB, images less than 10 GB are rounded up to 10 GB

For more information about custom images, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images).



## Next steps
{: #next-steps-images}

After you choose a profile, it's time to plan for and create an instance.
* [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
