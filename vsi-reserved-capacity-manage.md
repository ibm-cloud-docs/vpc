---

copyright:
  years: 2018, 2023
lastupdated: "2023-11-18"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing reserved capacity for VPC
{: #managing-reserved-capacity-vpc}

[Beta]{: tag-blue}

After you provision your reserved capacity, you can manage the reserved capacity with the following actions.
{: shortdesc}

| Action | Description |
| --- | --- |
| List all reservations | List all reservations in the region. |
| Create a reservation | Create a reservation in the region. |
| Delete a reservation | Delete an `inactive` reservation. You can't delete an `active` reservation. This action can't be reversed. |
| Retrieve a reservation | Retrieve a reservation as specified by its identifier in the URL. |
| Update a reservation | Update the reservation with new information. |
| Activate a reservation | Activate an `inactive` reservation. |
{: caption="Table 1. Actions for managing reserved capacity for virtual servers" caption-side="bottom"}

## Before you begin
{: #before-you-begin-managing-reserved-vpc}

You must provision your reserved capacity before you attach virtual servers. For more information, see [Provisioning reserved capacity for virtual servers](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

If you are not the account administrator, your user account must include the **Manage reserved capacities** permission. For more information about updating permissions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

## Managing reserved capacity with the UI
{: #managing-reserved-capacity-ui-vpc}
{: ui}

You can use the UI to manage your reserved capacity and the servers that are in the reservation.

### Deleting reserved capacity with the UI
{: #deleting-reserved-capacity-ui-vpc}
{: ui}

You can delete reserved capacity.

   Keep in mind that you can't delete reserved capacity beyond the first 120 hours of creation unless the reservation is expired.
   {: note}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Reservations**.
1. From your Reservations list, click **Actions** > **Delete reservation**.
1. To confirm, click **Delete**.

### Detaching a server from reserved capacity with the UI
{: #removing-adding-server-reserved-capacity-ui-vpc}
{: ui}

You can detach a virtual server from your reserved capacity.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Reservations**.
1. From your virtual server list or in the Reservation details page, click the server that you want to detach and then click **Actions** > **Detach**.
1. To confirm, click **Detach**.
