---

copyright:

  years: 2022, 2023

lastupdated: "2023-06-16"


keywords: operating system end of support (eos)

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Lifecycle for guest operating systems
{: #guest-os-lifecycle}

In the lifecycle of an operating system, end of support (EOS) is the last date that {{site.data.keyword.cloud}} delivers standard support for a version or release of a product. The end of support date is aligned to the vendor and community support dates. It is also the effective date that the product ceases to exist (is deprecated) and can no longer be ordered or purchased.
{: shortdesc}

A stock image starts out in available status. When a newer version of the image becomes available, the status of the older image changes to deprecated. A deprecated image can still be used to provision a virtual server instance, but such use is discouraged. If an image moves to obsolete status, such as when the image operating system reaches EOS, it can't be used to provision new virtual server instances.

## Viewing the image lifecycle of a stock image by using the UI
{: #image-lifecycle-ui}
{: ui}

You can list all of the {{site.data.keyword.vpc_short}} stock images in your region that are in available status by going to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**. Click the **Stock Images** tab to see all the available stock images. Images that are in deprecated status are not shown.

## Viewing the image lifecycle of a stock image by using the CLI
{: #image-lifecycle-cli}
{: cli}

You can list all of the {{site.data.keyword.vpc_short}} stock images in your region by using the command-line interface (CLI).

To list all stock images by using the CLI, use the **ibmcloud is images** command. Stock images are public, so the `--visibility` option is `public`.

```sh
ibmcloud is images --visibility public
```
{: pre}

## Viewing the image lifecycle of a stock image by using the API
{: #image-lifecycle-api}
{: api}

You can list all of the {{site.data.keyword.vpc_short}} stock images in your region by using the application programming interface (API).

Stock images are public, so you can set the `visibility` property to `public` to retrieve only stock images.

```sh
curl -X GET \
”$vpc_api_endpoint/v1/images?version=2022-11-21&generation=2&visibility=public" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## CentOS
{: #centos}

The following table describes the end of support date and license model for CentOS operating systems. This guest OS is a free operating system. For more information, see [CentOS Linux](https://www.centos.org/about/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| CentOS 8 minimal | 31 December 2021 | Free |
| CentOS 7.9 minimal | 30 June 2024  | Free |
{: caption="Table 1. Lifecycle for CentOS operating systems" caption-side="bottom"}

## Debian
{: #debian}

The following table describes the end of support date and license model for Debian operating systems. This guest OS is a free operating system. For more information, see [Debian community](https://www.debian.org/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| Debian 11 minimal | 01 June 2026 | Free |
| Debian 10 minimal | 01 June 2024 | Free |
| Debian 9 minimal | 30 June 2022 | Free |
{: caption="Table 2. Lifecycle for Debian operating systems" caption-side="bottom"}

## Fedora CoreOS
{: #fedora-coreos}

The version of Fedora&reg; CoreOS is updated regularly, with the previous release deprecating when a new version is released. This guest OS is a free operating system. For more information, see [Fedora](https://getfedora.org/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| Fedora CoreOS latest | N/A | Free |
{: caption="Table 3. Lifecycle for Fedora CoreOS operating systems" caption-side="bottom"}

## Red Hat Enterprise Linux (RHEL)
{: #rhel}

The following table describes the end of support date and license model for Red Hat&reg; Enterprise Linux&reg; operating systems. This guest OS is a paid operating system. For more information, see [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| RHEL 9.0 minimal | 31 May 2024    | Pay-as-you-Go / BYOL  |
| RHEL 9.0 (SAP HANA and SAP applications) | 31 May 2026    | Pay-as-you-Go |
| RHEL 8.6 minimal | 31 May 2024 | Pay-as-you-Go / BYOL |
| RHEL 8.6 (SAP HANA and SAP applications) | 31 May 2026 | Pay-as-you-Go |
| RHEL 8.4 minimal | 31 May 2023 | Pay-as-you-Go / BYOL |
| RHEL 8.4 (SAP HANA and SAP applications) | 31 May 2025 | Pay-as-you-Go |
| RHEL 8.3 minimal | 30 April 2021 | Pay-as-you-Go / BYOL |
| RHEL 8.2 minimal | 30 April 2022 |  Pay-as-you-Go / BYOL |
| RHEL 8.2 (SAP HANA and SAP applications) | 28 February 2023 | Pay-as-you-Go |
| RHEL 8.1 minimal | 30 November 2021 | Pay-as-you-Go / BYOL |
| RHEL 8.1 (SAP HANA and SAP applications) | 28 February 2023 | Pay-as-you-Go |
| RHEL 7.9 minimal | 30 June 2024 | Pay-as-you-Go / BYOL |
| RHEL 7.9 (SAP HANA and SAP applications) | 30 June 2024 | Pay-as-you-Go |
| RHEL 7.6 (SAP HANA and SAP applications) | 28 February 2023  | Pay-as-you-Go |
{: caption="Table 4. Lifecycle for Red Hat Enterprise Linux (RHEL) operating systems" caption-side="bottom"}

**BYOL**: For Red Hat Enterprise Linux® (RHEL) operating systems, you can bring your own license (BYOL) to {{site.data.keyword.vpc_short}} when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor. BYOL is not supported on Red Hat Enterprise Linux for SAP.

## Rocky Linux
{: #rocky-linux}

The version of Rocky Linux is updated regularly, with the previous release deprecating when a new version is released. This guest OS is a free operating system. For more information, see [Rocky Linux](https://rockylinux.org/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| Rocky Linux 8.latest | N/A | Free |
{: caption="Table 5. Lifecycle for Rocky Linux operating systems" caption-side="bottom"}

## SUSE Linux Enterprise Server (SLES)
{: #suse}

The following table describes the end of support date and license model for SUSE Linux Enterprise Server (SLES) operating systems. This guest OS is a paid operating system. For more information, see [SUSE Linux Enterprise Server](https://www.suse.com/products/server/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| SLES 15 SP4 | Release SP5 +6 Months | Pay-as-you-Go |
| SLES 15 SP3 | 31 December 2022 | Pay-as-you-Go |
| SLES 15 SP3 (SAP HANA and SAP applications) | 31 December 2025 | Pay-as-you-Go |
| SLES 15 SP2 (SAP HANA and SAP applications) | 31 December 2024 | Pay-as-you-Go  |
| SLES 15 SP1 (SAP HANA and SAP applications) | 31 January 2024 | Pay-as-you-Go  |
| SLES 12 SP5 | 31 October 2024 |  Pay-as-you-Go  |
| SLES 12 SP5 (SAP HANA and SAP applications) | 31 October 2024 |  Pay-as-you-Go  |
| SLES 12 SP4 (SAP HANA and SAP applications) | 30 June 2023 | Pay-as-you-Go  |
{: caption="Table 6. Lifecycle for SUSE Linux Enterprise Server (SLES) operating systems" caption-side="bottom"}

## Ubuntu LTS
{: #ubuntu-lts}

The following table describes the end of support date and license model for Ubuntu operating systems. This guest OS is a free operating system. For more information, see [Ubuntu](https://ubuntu.com/){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| Ubuntu 22.04 minimal | 30 April 2027 | Free |
| Ubuntu 20.04 minimal | 30 April 2025 | Free |
| Ubuntu 18.04 minimal | 31 May 2023  | Free |
| Ubuntu 16.04 minimal | 01 April 2021  | Free |
{: caption="Table 7. Lifecycle for Ubuntu operating systems" caption-side="bottom"}

## Windows Server
{: #windows-server}

The following table describes the end of support date and license model for Windows Server operating systems. This guest OS is a paid operating system. For more information, see [Microsoft Windows Server](https://www.microsoft.com/en-us/windows-server){: external}.

| Operating sytem | End of support | License model |
|-----------------|----------------|---------------|
| Windows Server 2022 full standard | 14 October 2031 | Pay-as-you-Go / BYOL|
| Windows Server 2019 core | 09 January 2029 | Pay-as-you-Go / BYOL|
| Windows Server 2019 full standard | 09 January 2029 | Pay-as-you-Go / BYOL |
| Windows Server 2016 core | 11 January 2027 | Pay-as-you-Go / BYOL |
| Windows Server 2016 full standard  | 11 January 2027  |  Pay-as-you-Go / BYOL |
| Windows Server 2012 full standard | 10 October 2023 | Pay-as-you-Go / BYOL |
| Windows Server 2012 r2 full standard | 10 October 2023 | Pay-as-you-Go / BYOL |
| Windows Server 2019 Standard Edition with SQL Server 2019 Web Edition | 08 January 2030 | Pay-as-you-Go |
{: caption="Table 8. Lifecycle for Windows Server operating systems" caption-side="bottom"}

**BYOL**: For Windows operating systems, you can bring your own license (BYOL) to {{site.data.keyword.vpc_short}} when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor.

**SQL Server**: For more information about Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about) documentation for information on creating the SQL server.
