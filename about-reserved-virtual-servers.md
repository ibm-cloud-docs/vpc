---

copyright:
  years: 2023, 2024
lastupdated: "2024-01-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About reservations for VPC
{: #about-reservations-servers-vpc}

[Beta]{: tag-blue}

IBM Cloud Reservations are a great option when you want significant cost savings and guaranteed resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

IBM Cloud Reservations are available in only Sydney region.
{: note}

Reservations offers many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Guaranteed capacity | When you provision a reservation, your capacity is guaranteed for the life of your contract term. |
| Reliable provisioning | You can provision and attach a virtual server to a reservation with available reserved capacity. |
| Cost savings | Choosing either a 1 or 3-year contract term allows reduced costs when compared to hourly or monthly billing cycles. |
{: caption="Table 1. Reserved virtual server benefits" caption-side="top"}

## Supported profile families
{: #reserved-virtual-servers-vpc-supported-profiles}

The following profile families for virtual servers are available when you provision a reservation.

| Family | Description |
| -------- | ----------- |
| Balanced | Best for midsized databases and common cloud applications with moderate traffic. |
| Compute | Best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| Memory  | Best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
| Very High Memory | Optimized for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA.  |
| Ultra High Memory | Optimized to run large in-memory databases and OLTP workloads, such as SAP S/4 HANA. |
| GPU | Provide on-demand access to NVIDIA V100 and A100 GPUs to accelerate AI, high-performance computing, data science, and graphics workloads. |
| Optimized storage | Offer temporary SSD instance storage disks at a ratio of 1 vCPU to 300 GB instance storage with a smaller price point per GB. These profiles are designed for storage-dense workloads and offer `virtio` interface type for attached disks. |
| s390x (LinuxONE) | Provides dedicated CPU core, memory, and I/O channel to better manage your high-performance workloads. |
{: caption="Table 2. Supported reserved virtual server profile families" caption-side="top"}

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles) and [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles).

## Limitations
{: #limitations-reserved-virtual-servers-vpc}

Consider the following limitations before you provision a reservation and provision a virtual server within that reservation.

* You can't resize a virtual server to a smaller profile that's in a reservation. But you can upgrade to another profile of equal or greater cost that is in the same profile family. For more information, [contact support](/docs/get-support?topic=get-support-using-avatar).
* A reservation can be canceled only within the first 120 hours of placing the reservation. However, you can detach virtual servers that are in that reservation. For more information, see [Canceling a reservation](/docs/vpc?topic=vpc-reserved-capacity-cancel-reservation&interface=ui).

* Maximum of 200 vCPUs per region per account.
* Limited to 5 POC accounts.
* SGX isn't supported.

## Notifications
{: #notifications-reserved-virtual-servers-vpc}

When your reservation is complete, a confirmation message is sent.

You receive a [notification](https://cloud.ibm.com/user/notifications){: external} before the end of the term of your reservation. From here, you can decide whether to renew your reservation or cancel.

If **Auto-renew** is selected when your reservation, your contract automatically renews when your current contract ends.
{: tip}

## Next steps
{: #next-steps-reserved-virtual-servers-vpc}

After you review and decided on your options, you're ready to provision your reservation. For more information, [Provisioning a reservation for VPC](/docs/vpc?topic=vpc-provisioning-reservation-vpc).
