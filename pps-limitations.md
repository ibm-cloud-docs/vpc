---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-08"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Private Path service limitations
{: #ppsg-limitations}

* Private Path service supports providers and consumers in the same region. Currently, Private Path service does not support providers and consumers in different regions.
* Providers may not update service endpoints once the Private Path service is created. If they want to change endpoints, they must create a new Private Path service.
* Private Path service supports TCP in datapath. Currently, Private Path service does not support UDP in datapath.
* The VPE can only be connected to from within the VPC it resides in.

## Related links
{: #ppsg-limitations-related-links}

* [Network load balancer limitations](/docs/vpc?topic=vpc-nlb-limitations)
