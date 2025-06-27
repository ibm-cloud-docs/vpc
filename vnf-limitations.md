---

copyright:
  years: 2021, 2025
lastupdated: "2025-06-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for virtual network functions
{: #vnf-limitations}

High Availability (HA) Virtual Network Function (VNF) deployments have the following known limitations.

* The Virtual Network Function (VNF) must share one subnet with the Network Load Balancer (NLB).
* Routing public internet "ingress" traffic to a VNF is not supported.
* Auto-scaling with the VNF is not supported.
