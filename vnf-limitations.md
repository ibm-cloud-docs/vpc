---

copyright:
  years: 2021
lastupdated: "2022-01-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VNF limitations
{: #vnf-limitations}

Known limitations for High Availability (HA) Virtual Network Function (VNF) deployments include:

* The Virtual Network Function (VNF) must share one subnet with the Network Load Balancer (NLB).
* Routing public internet "ingress" traffic to a VNF is not supported.
* Auto-scaling with the VNF is not supported. 
