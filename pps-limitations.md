---

copyright:
  years: 2023
lastupdated: "2023-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Private Path service limitations
{: #ppsg-limitations}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

* Private Path service supports providers and consumers in the same region. Currently, Private Path service does not support providers and consumers in different regions.
* Private Path service supports services that are in [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#table-mzr){: external}. Currently, Private Path service does not support services that are in zones and regions other than MZRs.
* Currently, Private Path service does not support {{site.data.keyword.cloud_notm}} Bare Metal Servers for VPC as members of a pool.
* Private Path service supports TCP in datapath. Currently, Private Path service does not support UDP in datapath.
* Providers may only register a single domain per Private Path service, and creating a domain will create a DNS zone which is entirely managed by IBM DNS. This is the same limitation that applies to existing IBM VPE services today. For example, a Private Path service with "service_endpoints": ["api1.service.com","api2.service.com"] will create a DNS Zone of service.com; all other endpoints ( such as www.service.com ) will not resolve. If the Provider uses two separate Private Path services instead, they could register each as api1.service.com and api2.service.com respectively, without affecting the root domain.
* Providers may not update service endpoints once the Private Path service is created. If they want to change endpoints, they must create a new Private Path service.

## Related Links
{: #ppsg-limitations-related-links}

* [Network load balancer limitations](/docs/vpc?topic=vpc-nlb-limitations)
