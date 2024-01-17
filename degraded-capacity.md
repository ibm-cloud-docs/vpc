---
copyright:
  years: 2023, 2024
lastupdated: "2024-01-17"

keywords: health status reasons, instance health status, virtual server health status,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Degraded capacity
{: #reserved-capacity-degraded-capacity}

[Select Availability]{: tag-green}

A reservation that experiences a loss of capacity reports a _capacity status_ of `degraded`.
{: shortdesc}

## Degraded capacity details
{: #degraded-capacity-details}

An active reservation that loses capacity after attaining full capacity reports a _capacity status_ of `degraded`.

You are not billed for any lost capacity. The reservation details show the amount of capacity that is allocated to the reservation. You are not billed for any amount of allocated capacity that is less than the total of your requested capacity. When capacity becomes available, the system replenishes any lost capacity. At which time, the reservation capacity status is "allocated". Billing resumes for that portion of lost capacity.

For more information about managing a reservation, see [Managing a reservation for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc) and [Deleting a reservation](/docs/vpc?topic=vpc-reserved-capacity-cancel-reservation).
