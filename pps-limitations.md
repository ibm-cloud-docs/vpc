---

copyright:
  years: 2023, 2025
lastupdated: "2025-06-27"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for Private Path services
{: #ppsg-limitations}

* Private Path service supports providers and consumers in the same region. Currently, Private Path service does not support providers and consumers in different regions.
* Providers may not update service endpoints once the Private Path service is created. If they want to change endpoints, they must create a new Private Path service.
* Private Path service supports TCP in datapath. Currently, Private Path service does not support UDP in datapath.
* The VPE can only be connected to from within the VPC it resides in.

## Related links
{: #ppsg-limitations-related-links}

* [Known issues for network load balancers](/docs/vpc?topic=vpc-nlb-limitations)
