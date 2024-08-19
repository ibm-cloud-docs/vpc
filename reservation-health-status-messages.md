---
copyright:
  years: 2023, 2024
lastupdated: "2024-08-19"

keywords: health status reasons, instance health status, virtual server health status,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Server health status reasons
{: #virtual-server-health-status-reasons}

If you receive a health status message, you can use the following information to help determine the message.
{: shortdesc}

## Health states
{: #reservation-health-states}

See the following information to understand the health state of your reservation.

| Health state | Description |
| ------------ | ----------- |
| `ok` | No abnormal behavior was detected with the reservation. |
| `degraded` | Reservation is experiencing compromised performance, capacity, or connectivity. |
| `faulted` | Reservation is unreachable, inoperative, or otherwise entirely incapacitated. |
| `inapplicable` | The reservation health state does not apply because of the current lifecycle state. A resource with a lifecycle state of `failed` or `deleting` has a health state of inapplicable. A pending resource can also have this state (`degraded`, `faulted`, `inapplicable`, `ok`). |
{: caption="Table 1. Reservation health states" caption-side="top"}

## Message: `reservation_capacity_unavailable`
{: #capacity-unavailable}

**Message:** _The reservation affinity pool has no available capacity._

Your reservation was not made because the affinity pool doesn't have the available capacity. To resolve this issue, try the following possible resolutions:

* Retry the reservation provisioning process.
* Your account has the maximum of 200 vCPUs per region per account. Try another region or account.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).

For more information about managing reserved capacity, see [Managing reserved capacity for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc).

## Message: `reservation_deleted`
{: #reservation-deleted}

**Message:** _The reservation affinity pool has a deleted reservation._

A reservation that is in your reservation affinity pool was deleted. To resolve this issue, try the following possible resolutions:

* If this deletion is intended, the deletion was successful.
* Provision the reservation again.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).

For more information about managing reserved capacity, see [Managing reserved capacity for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc).

## Message: `reservation_expired`
{: #reservation-expired}

**Message:** : _The reservation affinity pool has an expired reservation._

Your reservation affinity pool has an expired reservation and can't be used. To resolve this issue, try the following possible resolutions:

* Renew the reservation
* If no longer needed, delete the expired reservation.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).

For more information about managing reserved capacity, see [Managing reserved capacity for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc).

## Message: `reservation_failed`
{: #reservation-failed}

**Message**: _The reservation affinity pool has a failed reservation._

You have a failed reservation in your reservation affinity pool. To resolve this issue, try the following possible resolutions:

* Deleted the failed reservation and provision the reservation again.

If you still need help, contact [support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc).

For more information about managing reserved capacity, see [Managing reserved capacity for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc).
