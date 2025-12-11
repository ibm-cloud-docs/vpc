---

copyright:
  years: 2023
lastupdated: "2025-12-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# End of support hardware considerations
{: #eos-hardware-considerations-intro}

If you use hardware that is at or past its end-of-support (EOS) date, security and stability risks are possible. The vendor no longer provides updates or security fixes for deprecated hardware.
{: short-desc}

Follow the guidance from your hardware vendor on when to upgrade. {{site.data.keyword.IBM}} doesn't require you to migrate your active instances to supported hardware, but you assume the risks that are associated with using outdated hardware. Unsupported hardware doesn't receive security updates or fixes and can't be used to deploy new instances. Plan to modernize the instances' hardware before the EOS date.

## Available options when you modernize your hardware
{: #options-when-upgrading-hardware}

When you plan for hardware EOS, keep the following information in mind:

* You can use the assistance of an IBM partner.
* You can upgrade your instances on your own.
* Continue with your EOS hardware (at your own risk).

### Wanclouds partnership
{: #wanclouds-partner-hardware}

{{site.data.keyword.IBM_notm}} has a partnership with Wanclouds. Contact [Wanclouds](https://wanclouds.net/){: external} for more information.

### Upgrade your hardware on your own
{: #upgrading-your-hardware}

   Make sure that you follow your hardware vendor's guidance when you upgrade your hardware.
   {: important}

**Best practices for upgrades:**

- Read your hardware vendor's documentation to understand the differences between the old and new hardware.
- Consider the instance downtime that is required for the upgrade.
- Create a backup of your original instance. For more information, see [Best practices for backups](/docs/vpc?topic=vpc-backups-vpc-best-practices&interface=ui).
- Test that the new instance with the updated hardware works properly before you migrate data or delete your original instance.

### Continue with your EOS hardware at your own risk
{: #continue-using-your-eos-hardware}

If you choose to continue with an EOS hardware, consider the following information:

- The EOS hardware can't receive security updates.
- After the EOS date, you can no longer deploy new instances on that hardware.
- The hardware isn't tested for hardware, drivers, or firmware.
- Support isn't available for performance or operational issues on the server with an EOS hardware.
   - Vendor support is unavailable for an EOS hardware.
   - EOS hardware is not supported by {{site.data.keyword.Bluemix_notm}} Technical Support.

## Troubleshooting
{: #troubleshooting-upgrading-your-hardware}

If you encounter problems after your hardware is upgrade is complete, open a support ticket. IBM Support investigates each issue on a
case-by-case basis. You can open a support ticket in the customer portal.
