---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-21"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Action required: Prepare your automation for revised network load balancer capabilities
{: #nlb-revised-capabilities}

## What are we changing?
{: #nlb-what-changed}

In an upcoming release, the Load Balancer for VPC service will make generally available the Private Path network load balancer using a new network-private-path load balancer profile. This load balancer has a couple of differences from existing network load balancers, which may affect automation that provisions or manages network load balancer pools:

* The "least connections" pool load balancing algorithm will not be supported.
* Pool source IP session persistence will not be supported.

## Why are we making this change?
{: #nlb-why-change}

The distributed architecture of the Private Path network load balancer is at odds with the implementation of session persistence and the "least connections" algorithm. Private Path network load balancer workloads will not require these features.

## Who will be affected by this change?
{: #nlb-those-affected-by-change}

Any code or automation that expects to use session persistence or the "least connections" algorithm when provisioning or managing network load balancer pools will be affected.

## What actions can you take to avoid a disruption?
{: #nlb-actions-to-avoid-disruption}

As documented in the [VPC API](/apidocs/vpc/latest#get-load-balancer-profile), load balancer profile properties have been introduced to indicate support for these features:

* The new `source_ip_session_persistence_supported` property will have its `value` sub-property set to `true` if a given load balancer profile supports session persistence.

* The new `availability` property will have its `value` sub-property set to `subnet` if a given load balancer supports the `least_connections` algorithm.

Additionally, analogous properties have been introduced on each provisioned load balancer ([https://cloud.ibm.com/apidocs/vpc/latest#get-load-balancer](/apidocs/vpc/latest#get-load-balancer)) to indicate whether the load balancer supports these features.

While existing load balancers and load balancer profiles have also been updated with these properties, there is no change in functionality for existing network load balancers. They continue to support these features.
{: note}

If you or other users in your account are currently using load balancers and plan to use Private Path network load balancer, perform the following tasks before this feature becomes generally available:

* Check for code or automation that provisions or updates load balancer pools and specifies the `session_persistence` or `algorithm` properties.
* Check if that code or automation is limited to interacting with existing load balancers or profiles or both.
* If it is not (for example, because it retrieves a list of load balancers and operates on them), harden by checking the new `source_ip_session_persistence_supported` and `availability` properties. Specifically, when creating or updating a load balancer pool:
   * Check if the load balancer property `source_ip_session_persistence_supported` is `true` before attempting to set `session_persistence`.
   * Check if the load balancer property `availability` is set to `subnet` before attempting to set the pool property `algorithm` to `least_connections`.

   In addition, if a pool is being created as part of load balancer creation, harden by checking the `source_ip_session_persistence_supported` and `availability` properties on the selected load balancer profile.
