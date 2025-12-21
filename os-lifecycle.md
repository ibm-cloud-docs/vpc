---

copyright:

  years: 2022, 2025
lastupdated: "2025-12-21"

keywords: operating system end of support (eos)

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Lifecycle for guest operating systems
{: #guest-os-lifecycle}

In the lifecycle of an operating system, end of support (EOS) is the last date that {{site.data.keyword.cloud}} delivers standard support for a version or release of a product. The end of support date is aligned to the vendor and community support dates. A notification is sent out 90 days before support for an operating system is withdrawn.
{: shortdesc}

If you are using a generic operating system custom image, refer to your vendor documentation for EOS dates. For more information, see [Generic operating system custom images](/docs/vpc?topic=vpc-planning-custom-images#generic-os-custom-images).

| Image status | Description |
| -------------- | -------------- |
| `available` | The most current version of the stock operating system image is `available`. When a new version of an operating system is made `available`, the older version image of that guest operating system changes to `deprecated`. No stock operating system that reaches EOS has an `available` image. |
| `deprecated` | Older versions of stock operating systems are `deprecated`. Also, any stock operating system that reached EOS is also `deprecated`. You can still provision an instance with these images. The UI doesn't display any `deprecated` images when you create an instance. These images are still visible in the CLI and API. |
{: caption="Image lifecycle status" caption-side="bottom"}

If you choose to continue to use a `deprecated` stock operating system image after EOS, review the [End of support operating system considerations](/docs/vpc?topic=vpc-eos-os-considerations-intro).

## CentOS
{: #centos}

The following table describes the end of support date and license model for CentOS operating systems. This guest OS is a free operating system. For more information, see [CentOS Linux](https://www.centos.org/about/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| CentOS 8 minimal | 31 December 2021 | Free |
| CentOS 7.9 minimal | 30 June 2024  | Free |
| CentOS Stream 8 | 31 May 2024 | Free |
| CentOS Stream 9 | 31 May 2027 | Free |
| CentOS Stream 10 | 01 Jan 2030 | Free |
{: caption="Lifecycle for CentOS operating systems" caption-side="bottom"}

## Debian
{: #debian}

The following table describes the end of support date and license model for Debian operating systems. This guest OS is a free operating system. For more information, see [Debian community](https://www.debian.org/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| Debian 13 minimal | 30 June 2030 | Free |
| Debian 12 minimal | 10 June 2028 | Free |
| Debian 11 minimal | 31 August 2026 | Free |
| Debian 10 minimal | 30 June 2024 | Free |
| Debian 9 minimal | 30 June 2022 | Free |
{: caption="Lifecycle for Debian operating systems" caption-side="bottom"}

## Fedora CoreOS
{: #fedora-coreos}

The version of Fedora&reg; CoreOS is updated regularly, with the previous release deprecating when a new version is released. This guest OS is a free operating system. For more information, see [Fedora](https://fedoraproject.org/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| Fedora CoreOS latest | N/A | Free |
{: caption="Lifecycle for Fedora CoreOS operating systems" caption-side="bottom"}

## Red Hat Enterprise Linux (RHEL)
{: #rhel}

The following table describes the end of support date and license model for Red Hat&reg; Enterprise Linux&reg; operating systems. This guest OS is a paid operating system. For more information, see [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux){: external}. For more information about support for Red Hat, see [FAQs about Red Hat and IBM Cloud® infrastructure](/docs/infrastructure-hub?topic=infrastructure-hub-faqs-for-red-hat-ibm-cloud).

When you select an RHEL AI 1.x image, make sure that you are using the correct RHEL AI image for the instance profile you chose. For more information about the GPU profiles you can use with RHEL AI images, see [Accelerated instance profiles - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family). For information about RHEL AI, including information such as the supported use cases for RHEL AI, see [Red Hat Enterprise Linux AI](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_ai/1.5){: external}.
{: note}


| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| RHEL 9.6 minimal | 31 May 2027    | Pay-as-you-Go  |
| RHEL 9.6 (SAP HANA and SAP applications) | 31 May 2029    | Pay-as-you-Go |
| RHEL 9.4 minimal | 30 April 2026    | Pay-as-you-Go  |
| RHEL 9.4 (SAP HANA and SAP applications) | 31 May 2028    | Pay-as-you-Go |
| RHEL 9.2 minimal | 31 May 2025    | Pay-as-you-Go  |
| RHEL 9.2 (SAP HANA and SAP applications) | 31 May 2027    | Pay-as-you-Go |
| RHEL 9.0 minimal | 31 May 2024    | Pay-as-you-Go  |
| RHEL 9.0 (SAP HANA and SAP applications) | 31 May 2026    | Pay-as-you-Go |
| RHEL 8.10 minimal | 31 May 2029 | Pay-as-you-Go |
| RHEL 8.10 (SAP HANA and SAP applications) | 31 May 2029 | Pay-as-you-Go |
| RHEL 8.8 minimal | 31 May 2025 | Pay-as-you-Go |
| RHEL 8.8 (SAP HANA and SAP applications) | 31 May 2027 | Pay-as-you-Go |
| RHEL 8.6 minimal | 31 May 2024 | Pay-as-you-Go |
| RHEL 8.6 (SAP HANA and SAP applications) | 31 May 2026 | Pay-as-you-Go |
| RHEL 8.4 minimal | 31 May 2023 | Pay-as-you-Go |
| RHEL 8.4 (SAP HANA and SAP applications) | 31 May 2025 | Pay-as-you-Go |
| RHEL 8.3 minimal | 30 April 2021 | Pay-as-you-Go |
| RHEL 8.2 minimal | 30 April 2022 |  Pay-as-you-Go |
| RHEL 8.2 (SAP HANA and SAP applications) | 28 February 2023 | Pay-as-you-Go |
| RHEL 8.1 minimal | 30 November 2021 | Pay-as-you-Go |
| RHEL 8.1 (SAP HANA and SAP applications) | 28 February 2023 | Pay-as-you-Go |
| RHEL 7.9 minimal | 30 June 2024 | Pay-as-you-Go |
| RHEL 7.9 (SAP HANA and SAP applications) | 30 June 2024 | Pay-as-you-Go |
| RHEL 7.6 (SAP HANA and SAP applications) | 28 February 2023  | Pay-as-you-Go |
| RHEL AI 1.X (Nvidia) | [RHEL AI Life Cycle](https://access.redhat.com/support/policy/updates/rhelai) | Pay-as-you-Go |
| RHEL AI 1.X (AMD) | [RHEL AI Life Cycle](https://access.redhat.com/support/policy/updates/rhelai) | Pay-as-you-Go |
| RHEL AI 1.X (Intel) | [RHEL AI Life Cycle](https://access.redhat.com/support/policy/updates/rhelai) | Pay-as-you-Go |
{: caption="Lifecycle for Red Hat Enterprise Linux (RHEL) operating systems" caption-side="bottom"}

**BYOL**: For Red Hat Enterprise Linux® (RHEL) operating systems, you can bring your own license (BYOL) to {{site.data.keyword.vpc_short}} when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor. BYOL is not supported on Red Hat Enterprise Linux for SAP.

## Rocky Linux
{: #rocky-linux}

The version of Rocky Linux is updated regularly, with the previous release deprecating when a new version is released. This guest OS is a free operating system. For more information, see [Rocky Linux](https://rockylinux.org/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| Rocky Linux 8.x | 31 May 2029 | Free |
| Rocky Linux 9.x | 31 May 2032 | Free |
| Rocky Linux 10.x | 31 May 2035 | Free |
{: caption="Lifecycle for Rocky Linux operating systems" caption-side="bottom"}

## SUSE Linux Enterprise Server (SLES)
{: #suse}

The following table describes the end of support date and license model for SUSE Linux Enterprise Server (SLES) operating systems. This guest OS is a paid operating system. For more information, see [SUSE Linux Enterprise Server](https://www.suse.com/products/server/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| SLES 15 SP7 | 31 July 2031 | Pay-as-you-Go |
| SLES 15 SP6 | 31 December 2025 | Pay-as-you-Go |
| SLES 15 SP5 | 31 December 2024 | Pay-as-you-Go |
| SLES 15 SP4 | 31 December 2023 | Pay-as-you-Go |
| SLES 15 SP3 | 31 December 2022 | Pay-as-you-Go |
| SLES 12 SP5 | 31 October 2024 |  Pay-as-you-Go |
| SLES 15 SP7 (SAP HANA and SAP applications) | 31 December 2029 | Pay-as-you-Go |
| SLES 15 SP6 (SAP HANA and SAP applications) | 31 December 2028 | Pay-as-you-Go |
| SLES 15 SP5 (SAP HANA and SAP applications) | 31 December 2027 | Pay-as-you-Go |
| SLES 15 SP4 (SAP HANA and SAP applications) | 31 December 2026 | Pay-as-you-Go |
| SLES 15 SP3 (SAP HANA and SAP applications) | 31 December 2025 | Pay-as-you-Go |
| SLES 15 SP2 (SAP HANA and SAP applications) | 31 December 2024 | Pay-as-you-Go  |
| SLES 15 SP1 (SAP HANA and SAP applications) | 31 January 2024 | Pay-as-you-Go  |
| SLES 12 SP5 (SAP HANA and SAP applications) | 31 October 2024 |  Pay-as-you-Go  |
| SLES 12 SP4 (SAP HANA and SAP applications) | 30 June 2023 | Pay-as-you-Go  |
{: caption="Lifecycle for SUSE Linux Enterprise Server (SLES) operating systems" caption-side="bottom"}

## Ubuntu LTS
{: #ubuntu-lts}

The following table describes the end of support date and license model for Ubuntu operating systems. This guest OS is a free operating system. For more information, see [Ubuntu](https://ubuntu.com/){: external}.

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| Ubuntu 24.04 minimal | 25 April 2029 | Free |
| Ubuntu 22.04 minimal | 30 April 2027 | Free |
| Ubuntu 20.04 minimal [^tabletext]| 30 April 2025 | Free |
| Ubuntu 18.04 minimal [^tabletext]| 31 May 2023  | Free |
| Ubuntu 16.04 minimal | 01 April 2021  | Free |
{: caption="Lifecycle for Ubuntu operating systems" caption-side="bottom"}

[^tabletext]: Out of standard support. [Upgrade to Ubuntu Pro.](https://ubuntu.com/18-04/ibm){: external}

## Windows Server
{: #windows-server}

The following table describes the end of support date and license model for Windows Server operating systems. This guest OS is a paid operating system. For more information, see [Microsoft Windows Server](https://www.microsoft.com/en-us/windows-server){: external}. For more information about support for Microsoft, see [FAQs about Microsoft and IBM Cloud® infrastructure](/docs/infrastructure-hub?topic=infrastructure-hub-faqs-for-microsoft-software-ibm-cloud).

| Operating system | End of support | License model |
|-----------------|----------------|---------------|
| Windows Server 2025 full standard | 10 October 2034 | Pay-as-you-Go / BYOL|
| Windows Server 2022 full standard | 14 October 2031 | Pay-as-you-Go / BYOL|
| Windows Server 2019 core | 09 January 2029 | Pay-as-you-Go / BYOL|
| Windows Server 2019 full standard | 09 January 2029 | Pay-as-you-Go / BYOL |
| Windows Server 2016 core | 11 January 2027 | Pay-as-you-Go / BYOL |
| Windows Server 2016 full standard  | 11 January 2027  |  Pay-as-you-Go / BYOL |
| Windows Server 2012 full standard | 10 October 2023 | Pay-as-you-Go / BYOL |
| Windows Server 2012 r2 full standard | 10 October 2023 | Pay-as-you-Go / BYOL |
| Windows Server 2019 Standard Edition with SQL Server 2019 Web Edition | 08 January 2030 | Pay-as-you-Go |
{: caption="Lifecycle for Windows Server operating systems" caption-side="bottom"}

**BYOL**: For Windows operating systems, you can bring your own license (BYOL) to {{site.data.keyword.vpc_short}} when you import a custom image. These images are registered and licensed by you. You maintain control over your license and incur no additional costs by using your license. Acquisition and activation of the license is between you and the OS vendor.

**SQL Server**: For more information about Windows operating systems with SQL Server, see the [About Microsoft SQL on VPC](/docs/microsoft?topic=microsoft-mssql-about) documentation for information on creating the SQL server.
