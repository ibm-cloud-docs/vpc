---

copyright:
  years: 2023, 2026
lastupdated: "2026-02-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Reservations for VPC
{: #about-reserved-virtual-servers-vpc}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and dedicated resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

Reservations offer many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Cost savings | Save up to 60% by choosing a 1 or 3-year term compared to on-demand virtual server billing cycles. |
|  |  Save up to 55% by choosing a 1 or 3-year term compared to on-demand bare metal server billing cycles. |
| Dedicated capacity | Your servers are there when you need them. {{site.data.keyword.cloud}} Reservations provide dedicated capacity within the zone of your choice for the life of your committed term. |
| Predictable management | Accurately plan for the future and define your budgeting roadmap with reserved capacity for 1-year or 3-year terms. Your reserved pricing is a fixed discount rate that is applied to the corresponding virtual server profile on-demand rates that are listed in the {{site.data.keyword.cloud}} portal. |
| Flexibility in deployment | Convert any existing on-demand virtual server to {{site.data.keyword.cloud}} Reservation billing. Attach or detach any compatible virtual server to your reservation. |
{: caption="Benefits of IBM Cloud Reservations" caption-side="top"}

A reservation cannot be used for a container service discount such as IBM Kubernetes Service or ROKS.
{: note}

## Supported virtual server profiles
{: #reserved-virtual-servers-vpc-supported-profiles}

The following x86 profiles for virtual servers are available when you provision a reservation in all [available locations](/docs/overview?topic=overview-locations) except Montreal and Chennai - Airtel.

* bx2 profiles
* bx2d profiles
* cx2 profiles
* cx2d profiles
* mx2 profiles
* mx2d profiles
* ux2d profiles
* vx2d profiles

The following virtual server profiles are available in specific MZRs and SC-MZRs. Chennai - Airtel, Montreal,  and Osaka are SC-MZRs.

* bx2a profiles are available in Toronto only.
* bx3d, cx3d, and mx3d profiles are available in Chennai - Airtel, Dallas, Frankfurt, London, Madrid, Montreal, Osaka, Sydney, Toronto, and Washington DC.
* gx2 profiles are available in all regions except Madrid.
* gx3d profiles are available in Chennai - Airtel, Frankfurt, London, Madrid, Sao Paulo, Sydney, Tokyo, Toronto, Montreal, and Washington DC.
* ox2 profiles are available in Dallas, Frankfurt, London, Osaka, Tokyo, and Washington DC.

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Supported bare metal server profiles
{: #reserved-bare-metal-servers-vpc-supported-profiles}

All [Gen 2 and Gen 3 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui#bare-metal-servers-profile-list) are available when you provision a reservation in an available MZR.

## Attachment policies
{: #attachment-policies}

A reservation attachment policy determines whether a resource is automatically or manually attached to a reservation. If you select automatic, the server automatically attaches to a compatible reservation. If you select manual, you need to manually attach the server to a compatible reservation.

### Automatic attachments
{: #reservation-automatic-reservation-vpc}

If a server is configured to enable automatic attachments, the system attempts to automatically attach the resource to a reservation in the same account that allows automatic attachments. The reservation must be configured to support automatic attachments and must contain matching properties (account, profile, zone) with available capacity.

The following examples are scenarios of how the system attempts to attach automatic resources to automatic reservations.

   * When a new automatic reservation is created, the system looks for any automatic resources that are candidates and attaches them to the reservation up to the reservation's capacity.
   * When a new automatic resource is created, the system looks for any available automatic reservations that are candidates that have available capacity and attaches the resource.
   * If an automatic reservation expires, the system attempts to attach the resources that were attached to the reservation to another automatic reservation. If a reservation or capacity isn't available, the resource remains unattached.
   * If an automatic resource is deleted, the system attempts to fill the now available capacity with another automatic resource in your account.

To automatically attach resources to a reservation, the following settings must be true:

   * For a reservation to be available for automatic attachment, it must have an `affinity_policy` of `automatic`.
   * To enable a resource for automatic attachment, it must have a `reservation_affinity.policy` of `automatic`.
   * The `reservation_affinity.pool` must be empty.

The billing discount for the attached compute resource is applied immediately and automatically. If you have more than one automatic reservation that is a resource candidate that is set to automatically attach, you have no guaranteed ordering for which reservation are chosen by the system. This ordering has no pricing impact because all resources that are attached to a reservation are not billed.
{: note}

#### Limitations
{: #reservation-automatic-reservations-limitations-vpc}

* You can't target a specific reservation and configure a resource automatically. They are mutually exclusive.

* When an auto-attached compute resource is stopped or deleted, the resource automatically releases its hold on reserved capacity, unlike targeted attachments that maintain their hold on reserved capacity when stopped.

* Bare metal servers do not get stopped and de-provisioned. Instead, they can be powered down. Because bare metal servers are not de-provisioned when powered down, bare metal servers that are automatically attached to a reservation are not detached. They also don't release their hold on the reserved capacity when powered down.

* When a compute resource detaches and releases its hold on a reservation, the system automatically attempts to attach another matching resource to the reservation.

* Reservations with an `affinity_policy` of `automatic` can't be targeted by resources with a `reservation_affinity.policy` of `manual`.

* Reservations with an `affinity_policy` of `restricted` can't be considered for attachment by resources with a `reservation_affinity.policy` of `automatic`.

### Manual attachments
{: #reservation-manual-attachments}

If a server is configured for manual attachments, you need to manually attach the server to a reservation.

To manually attach a resource to a reservation, the following settings must be true:

   * The reservation `affinity_policy` must be `restricted`.
   * The resource that you want to attach to a reservation must have a `reservation_affinity.policy` of `manual`.
   * The `reservation_affinity.pool` must have one target reservation ID and the targeted reservation must have an `affinity_policy` of `restricted`.

#### Limitations
{: #manual-attachment-limitations}

The target reservation can't be enabled as an automatic reservation.

### Quotas and reservations
{: #reservation-quotas-and-reservations}

Reservations count against the appropriate quota for the resource type. So bare metal reservations count against the bare metal server quota and virtual server reservations count against instance quotas. However, compute resources that are attached to a reservation don't count against quotas.

When you create a new compute resource that is configured for automatic attachment, space is dynamically determined in an existing, matching reservation and counts the quota. So if no capacity is available in a matching automatic reservation, compute resources count against your quota. If capacity is available in a matching automatic reservation, then the new resource isn't counted against your quota.

The same dynamic quota calculation applies to new automatic reservations. Typically, the full quantity of the automatic reservation counts against your account quota. However, if you have automatic compute resources that match the reservation, they are considered. Then, the difference of the reservation and the number of resources are deducted from your account quota.

## Special considerations for reservations
{: #special-considerations-for-reservations}

Keep the following considerations in mind when you create a reservation.

* It takes up to 10 minutes for a reservation to reach a failed state. During this time, the capacity is not dedicated. However, when the reservation creation is successful, the capacity for that reservation is dedicated. Then, you can attach servers to the reservation.

* Your bare metal server is not provisioned when the reservation is created, but the reserved capacity is dedicated and is available after the reservation is created.

* Billing for a single server doesn't change until that server is attached to a reservation.

### Bandwidth speed billing for a reservation
{: #reservations-bandwidth-billing-bare-metal}

Bandwidth speed billing occurs when a server is attached to the reservation. This means that billing for premium operating systems and dynamic network bandwidth occurs at the start event for a server that is attached to a reservation. Dynamic network bandwidth is discounted at the same 1 or 3-year rates as the reservation term, but bills only while a server is attached to the reservation.

## Notifications
{: #notifications-reserved-virtual-servers-vpc}

When your reservation is complete, a confirmation message is sent.

You receive a [notification](https://cloud.ibm.com/user/notifications){: external} before the end of the term of your reservation. From here, you can decide whether to renew your reservation or cancel.

If **Auto-renew** is selected when you provision a reservation, your reservation automatically renews when it expires.
{: tip}

## Next steps
{: #next-steps-reserved-virtual-servers-vpc}

After you review and decided on your options, you're ready to provision a reservation. For more information, [Provisioning a reservation for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).
