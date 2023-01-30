---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VNF limitations
{: #vnf-limitations}

High Availability (HA) Virtual Network Function (VNF) deployments have the following known limitations.

* The Virtual Network Function (VNF) must share one subnet with the Network Load Balancer (NLB).
* Routing public internet "ingress" traffic to a VNF is not supported.
* Auto-scaling with the VNF is not supported. 
