---

copyright:
  years: 2023, 2024
lastupdated: "2024-05-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Private Path service limitations
{: #ppsg-limitations}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

* Private Path service supports providers and consumers in the same region. Currently, Private Path service does not support providers and consumers in different regions.
* Providers may not update service endpoints once the Private Path service is created. If they want to change endpoints, they must create a new Private Path service.
* Providers may only register a single domain per Private Path service. Creating a domain will create a DNS zone which is entirely managed by IBM DNS. 
* Private Path service supports TCP in datapath. Currently, Private Path service does not support UDP in datapath.

## Related link
{: #ppsg-limitations-related-links}

[Network load balancer limitations](/docs/vpc?topic=vpc-nlb-limitations)
