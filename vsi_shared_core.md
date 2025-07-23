<shared-core>
---

copyright:
  years: 2025, 2025
lastupdated: "2025-07-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Shared core virtual servers
{: #shared-core-virtual-servers}

With shared cores, your virtual servers share the resources of a host CPU. Which means that multiple virtual servers that are associated with your account might draw on shared processing resources. This makes your deployments more scalable and cost-effective.
{: shortdesc}

Keep in mind that you can run into a noisy neighbors scenario because shared core virtual servers share the same NUMA node and L1 and L2 cache (hardware elements that affect memory access speed and processing efficiency). So performance might fluctuate depending on how busy the neighboring virtual servers are.

## Shared core benefits
{: #shared-core-benefits}

The following are benefits of deploying shared core virtual servers.

* Cost-effective - multiple virutal servers share a single CPU.
* Scalable - you can upgrade virtual servers to better manage demanding tasks.
* Quick deployment - you can quickly provision virtual server quickly.
* OS access - Admin access (depending on your IAM permissions) is available so your virtual servers can install software, configure settings, and manage the server.
* Most profiles offer a CPU baseline at less than 100%. For example, with a 2x4@50% profile, the virtual server is entitled to half of 2 vCPUs worth of CPU time, but you can burst if you're on a quiet node.

Share core virtual servers are best for the following uses.

* Hosting small websites
* Development and testing environments
* Running lightweight applications</shared-core>
