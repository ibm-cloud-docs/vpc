---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# End of support operating system considerations
{: #eos-os-considerations-intro}

If you use an operating system (OS) that is at or past its end-of-support (EOS) date, security and stability risks are possible. The vendor no longer provides updates or security fixes for deprecated OS versions.
{: short-desc}

Follow the guidance from your OS vendor on when to upgrade. {{site.data.keyword.IBM}} doesn't require you to migrate your active instances to a supported OS, but you assume the risks that are associated with using an outdated OS. Unsupported operating systems don't receive security updates or fixes and can't be used to deploy new instances. Plan to modernize the instances' OS before the EOS date. For more information, see [Lifecycle for guest operating systems](/docs/vpc?topic=vpc-guest-os-lifecycle).

## Available options when you modernize your OS
{: #options-when-upgrading-os}

When you plan for an OS EOS, keep the following information in mind:

* You can use the assistance of an IBM partner.
* You can upgrade your instances on your own.
* Continue with your EOS OS (at your own risk).

### Wanclouds partnership
{: #wanclouds-partnership-vpc}

{{site.data.keyword.IBM_notm}} has a partnership with Wanclouds. For more information, contact [Wanclouds](https://wanclouds.net/ibm/request){: external}.

### Upgrade your OS on your own
{: #upgrading-your-os}

If you decide to upgrade your OS yourself, you can use one of two options:

- Side-by-side upgrade (migration).
- In-place upgrade.

   Make sure that you follow your OS vendor's guidance when you upgrade your OS.
   {: important}

#### Side-by-side upgrades
{: #side-by-side-upgrade}

When you upgrade with the side-by-side method, you create a new instance with the new OS version. You must migrate data and applications from the old instance after the new instance is created.

Extra charges are incurred if both instances are active in a side-by-side upgrade.
{: important}

**Best practices for side-by-side upgrades:**

- Read your OS vendor's documentation to understand the differences between the old and new OS versions.
- Consider the instance downtime that is required for the upgrade.
- Create a backup of your original instance. For more information, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices&interface=ui).
- Test that the new instance with the updated OS works properly before you migrate data or delete your original instance.

#### In-place upgrade
{: #in-place-upgrade}

When you use the in-place upgrade method, you can use your previous configurations and data when you upgrade your OS. Follow your OS's vendor documentation if you choose the in-place upgrade method. Refer to the section in this document for your OS for specific considerations.

If you encounter problems during the in-place upgrade process, IBM can't provide support. For more information, see your OS vendor's documentation.
{: note}

**Best practices for in-place upgrades:**

- Review your OS vendor's documentation to identify process steps and associated risks.
- Consider the instance downtime that is required for the upgrade. Workloads can be unavailable when you upgrade.
- Before you upgrade your OS, download and install the most recent software updates for your OS.
- Create a backup of your original instance. For more information, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices&interface=ui).
- Upgrade in a test environment before you upgrade in a production environment.
- Test that the new instance with the updated OS works properly before you delete your original instance.
- After the in-place upgrade is complete, create a full backup of the upgraded instance, not an incremental backup.

### Continue with your EOS OS at your own risk
{: #continue-using-your-eos-os-vpc}

If you choose to continue with an EOS OS, consider the following information:

- The EOS OS can't receive security updates.
- The OS isn't tested for hardware, drivers, or firmware.
- Support isn't available for performance or operational issues on the server with an EOS OS.
   - Vendor support is not available for an EOS OS.
   - EOS operating systems are not supported by {{site.data.keyword.Bluemix_notm}} Technical Support.

## End of Support (EOS) announcements
{: #eos-announcements-vpc}

End of Support (EOS) is the last date that {{site.data.keyword.cloud}} delivers standard support for a version or release of a product. The end of support date is aligned to the vendor and community support dates. The EOS date is also the effective date that the product ceases to exist (is deprecated) and can no longer be ordered or purchased.

### SUSE Linux Enterprise Server (SLES) 12
{: #sles-12-eos-vpc}

SUSE Linux Enterprise Server (SLES) 12 EOS date is 31 October 2024. After deprecation, you can't download the software. You are encouraged to upgrade to the most recent version for continued support.

For more information about upgrading, see [SUSE documentation](https://documentation.suse.com/sles/15-SP6/single-html/SLES-upgrade/#sec-upgrade-paths-supported){: external} or contact [IBM support](/docs/account?topic=account-using-avatar).

### Debian 10
{: #debian-ten-eos-vpc}

Debian 10 EOS date is 30 June 2024. Support for this software discontinues on 30 June 2024. After deprecation, you can't download the software. For existing customers, upgrade to the most recent version.
For more information, see the [Debian documentation](https://www.debian.org/releases/bookworm/amd64/release-notes/ch-upgrading.en.html){: external}.

### Red Hat Enterprise Linux 7
{: #upgrading-rhel-7-os-vpc-eos}

Red Hat Enterprise Linux 7 EOS date is 30 June 2024. Support for this software discontinues on 30 June 2024. After deprecation, you can't download the software.

If you decide to upgrade with the in-place method on Red Hat OS, open a case with {{site.data.keyword.IBM_notm}} support to update the OS version that is recorded on {{site.data.keyword.Bluemix}}. You must inform {{site.data.keyword.IBM_notm}} support when your upgrade is completed for uninterrupted support for your upgraded instances. You can open a support case in the [customer portal](/login){: external}.

For existing customers, upgrade to the most recent version. For more information, see the [Red Hat Enterprise Linux documentation](https://access.redhat.com/support/policy/updates/errata).

### Windows 2012 and Windows 2012 R2 EOS
{: #upgrading-windows-2012-os}

Windows Server 2012 and Windows Server 2012 R2 EOS date is 10 October 2023. For more information, see [Overview of Windows Server upgrades](https://learn.microsoft.com/en-us/windows-server/get-started/upgrade-overview){: external}.

If you decide to upgrade with the in-place method on Windows OS, open a case with {{site.data.keyword.IBM_notm}} support to accomplish the following tasks. You can open a support case in the [customer portal](/login){: external}.
- Obtain a Windows ISO image. A Windows ISO is required to complete the upgrade process.
- Update the OS version that is recorded on {{site.data.keyword.Bluemix}}. You must inform {{site.data.keyword.IBM_notm}} support when your upgrade is completed for uninterrupted support for your upgraded instances.

No additional licensing costs are incurred to move to a newer software version when you use {{site.data.keyword.IBM_notm}}’s License-Included options. {{site.data.keyword.Bluemix}} is governed by the Service Provider License Agreement (SPLA) with Microsoft. For more information, see [License Mobility Deployment Process](/docs/microsoft?topic=microsoft-microsoft-license-mobility-process).



### CentOS 7 and CentOS Stream 8
{: #centos-7-centos-stream-8-eos}

CentOS 7 EOS date is 30 June 2024. Support for this software discontinues on 30 June 2024. After deprecation, you can't download the software.

CentOS Stream 8 EOS date is 31 May 2024. Support for this software discontinues on 31 May 2024. After deprecation, you can't download the software.

To migrate workloads from CentOS, you can switch to a compatible OS distribution, or choose to a different operating system. Compatible distributions include CentOS Stream 9, Rocky Linux 8/9, and RHEL 8/9. You can also migrate to a different OS such as Debian or Ubuntu LTS. You can use conversion tools to change your operating system to Rocky Linux 8/9 or RHEL 8/9. Alternatively, you can perform OS reload, side-by-side upgrade, or migrate to a new server with a CentOS Stream 9.

**You can use the following conversion tools to change your OS:**

- The migration tool available from the OS distribution you are migrating to, {{site.data.keyword.cloud_notm}} does not provide or support for these OS conversion tools.
- The [migrate2rocky](https://github.com/rocky-linux/rocky-tools/tree/main/migrate2rocky){: external} tool available from Rocky Linux. For more information, see the [Rocky Linux documentation](https://docs.rockylinux.org/){: external}.
- The [Convert2RHEL](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux/migration-process/convert2rhel){: external} tool available from RHEL. For more information, see the [RHEL documentation](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux/migration-process/convert2rhel){: external}.

### Ubuntu 20.04
{: #ubuntu-20-eos}

Ubuntu 20.04 EOS date is 31 May 2025. Support for this software discontinues on 31 May 2025. After deprecation, you can't download the software. For existing customers, upgrade to the most recent version. For more information, see the [Ubuntu documentation](https://ubuntu.com/20-04){external}.

### Debian 11
{: #debian-11-eos}

Debian 11 EOS date is 31 August 2026. Support for this software discontinues on 31 August 2026. Debian 11 reaches its End of Marketing (EOM) on 31 July 2026. After 31 July 2026, you can't download the software. For existing customers, upgrade to the most recent version. For more information, see the [Debian documentation](https://www.debian.org/releases/bookworm/amd64/release-notes/ch-upgrading.en.html){: external}.

### Windows Server 2016
{: #windows-server-2016-eos-vpc}

Windows Server 2016 EOS date is 12 January 2027. For more information, see [Overview of Windows Server upgrades](https://learn.microsoft.com/en-us/windows-server/get-started/upgrade-overview){: external}.

No extra licensing costs are incurred to move to a newer software version when you use an {{site.data.keyword.cloud_notm}} License-Included option. {{site.data.keyword.cloud_notm}} is governed by the Service Provider License Agreement (SPLA) with Microsoft. For more information, see [License Mobility Deployment Process](/docs/microsoft?topic=microsoft-microsoft-license-mobility-process).

## Troubleshooting
{: #troubleshooting-upgrading-your-os-vpc}

If you encounter problems after your OS is upgrade is complete, open a support case. IBM Support investigates each issue on a
case-by-case basis. You can open a support case in the customer portal. For more information about support, see [Using the Support Center](/docs/account?topic=account-using-avatar).
