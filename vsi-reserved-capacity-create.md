---

copyright:
  years: 2018, 2023
lastupdated: "2023-12-07"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning reserved capacity for VPC
{: #provisioning-reserved-capacity-vpc}

[Beta]{: tag-blue}

You can provision your reserved capacity through the UI, CLI, or API. You must provision your reserved capacity before you attach virtual servers.
{: shortdesc}

## Before you begin
{: #before-you-begin-provisioning-reserved-vpc}

You can provision your reserved capacity through the {{site.data.keyword.cloud}} catalog. You must provision your reserved capacity before you can attach virtual servers.

If you are not the account administrator, your user account must include the **Manage reserved capacities** permission. For more information about updating permissions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

## Provisioning reserved capacity with the UI
{: #provisioning-reserved-capacity-ui-vpc}
{: ui}

In the {{site.data.keyword.cloud_notm}}, complete the following steps to provision your reserved capacity.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](/login), click **menu icon ![the menu icon ](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Reservations**.
2. To create new reserved capacity, click **Create**.
3. From the **New reservation for VPC** page, enter or select the following information:

   | Field                   | Value               |
   | ----------------------- | ------------------- |
   | Geography               | Select the specific location for your workload. Locations are composed of regions; each region is a separate geographic area. Keep in mind that you can't select individual locations for each virtual server that you provision within this reserved capacity. Your selection is the location for all virtual server instances that you provision within this reserved capacity. |
   | Name                    | Enter a name for your reserved capacity. |
   | Resource group          | Select a resource group. |
   | Tags                    | Enter any applicable tags. |
   | Access management tags  | Enter any applicable access management tags. |
   | Reservation type        | Only available with Virtual Servers for VPC. |
   | Server quantity         | Enter the number of instances that you want to assign to this reserved capacity (limit of 200 vCPUs). |
   | Term length             | Choose either a 1 or 3-year term length. |
   | Server profile          | Select a profile for your reservation. Keep the following rules in mind when you reserve capacity.  /n You can't change profiles after your reservation is created.  /n You can't combine different profile sizes.  /n Your reserved virtual servers must all have the same size (all resources must be identical). |
   | Advanced options | Toggle **Auto renew** to **On** if you want to continue your reservation after your selected term length completes. |
   {: caption="Table 1. Reserved capacity provisioning selections" caption-side="top"}

4. Click **Create reservation**.

## Next steps
{: #next-steps-provisioning-reserved-vpc}

After your reserved capacity is provisioned and available to use, you can **Attach** an existing virtual server to your capacity. Or, you can **Create** virtual servers in your reserved capacity by using the {{site.data.keyword.cloud_notm}} console, the CLI, or the API. For more information about creating a virtual server, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).

## Attaching an existing virtual server to reserved capacity with the UI
{: #attach-virtual-server-ui-vpc}
{: ui}

You can attach existing virtual servers to your reserved capacity. Keep in mind that all existing virtual servers must be the same profile, size, and in the same location.

1. From your virtual server list, click **Attach**.

When a reservation expires, the servers that are attached to the reservation convert to on-demand pricing.
{: note}
