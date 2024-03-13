---

copyright:
  years: 2024
lastupdated: "2024-01-16"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Reserved capacity status reasons
{: #reserved-capacity-status-reasons}

[Select Availability]{: tag-green}

If you receive a capacity health status message for a reservation, you can use the following information to help determine the message.
{: shortdesc}

## Health states
{: #reservation-health-states}

See the following information to understand the health state of your reservation.

| Status  | Reason |
| ------------ | ----------- |
| `allocating` | The capacity reservation is preparing for use. |
| `allocated` | The total capacity of the reservation is ready for use. |
| `degraded` | Capacity in the reservation is allocated, but some of the capacity is not available. |
| `unallocated` | Capacity in the reservation isn't allocated and isn't available to use. |
{: caption="Table 1. Reservation capacity status" caption-side="top"}

If you need more help, contact [support](/docs/vpc?topic=vpc-help-and-support-vpc).

For more information about managing reserved capacity, see [Managing reserved capacity for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc).
