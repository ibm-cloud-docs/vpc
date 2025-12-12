---

copyright:
  years: 2025, 2025
lastupdated: "2025-12-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Burstable virtual servers
{: #burstable-virtual-servers}

Burstable Flex virtual servers are designed to provide flexible CPU performance so workloads can operate at a smaller baseline level and opportunistically burst to higher performance when needed. Flex virtual servers are ideal for applications that require a baseline of CPU performance and experience intermittent spikes in CPU demand but don't require sustained high performance.

Burstable Flex virtual servers are used to balance performance and reduces costs by offering configurable CPU baselines of 10%, 25%, and 50%. These baselines give your virtual servers the ability to burst up to twice the provisioned baseline, at no additional charge. For example, a burstable instance with a 25% baseline instance can burst up to 50% of the configured vCPUs when idle CPU time is available on the host.

Flex virtual servers also support 1 vCPU Nano Flex profiles with a fixed 100% baseline for small workloads that require a more consistent level of performance.

With Flex profiles, you can choose the best profile that fits your workload characteristics, performance needs, and price point. Burstable Flex profiles are ideal for general-purpose and lightweight workloads such as the following examples.

* Web applications
* CI/CD pipelines
* Microservices
* Small to medium databases
* Proof-of-concept environments.

For burstable Flex-supported profiles, see [General purpose - Flex profiles](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers).

## Burstable virtual server benefits
{: #burstable-benefits}

The following are benefits of deploying Burstable Flex virtual servers.

* Cost-effective - reduced cost for reduced CPU needs.
* Scalable - you can upgrade virtual servers to better manage tasks that are demanding.
* Burstable - can handle demands for intermittent CPU spikes, up to 2x over provisioned CPU

## Managing burstable virtual servers
{: #burstable-settings}

You can manage a virtual server instance's vCPU burst capability by configuring the following settings.

* **vCPU percentage** - `vcpu.percentage` - The baseline percentage of clock cycles that are allocated to the vCPU. The acceptable values are defined on the instance profile. You can configure this allocation when you create an instance, or you can update an instance while the instance `status` is `stopped` or `stopping`.
* vCPU burst limit - `vcpu.burst.limit` - Defines the maximum extra vCPU capacity, as a percent, that the instance can consume during a burst period. A burst limit of 200% means that the instance can use up to double the vCPU percentage. Example - a percentage baseline of 25% can consume up to 50% of the vCPU capacity.
