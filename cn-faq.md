---

copyright:
  years: 2024, 2025
lastupdated: "2025-06-18"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for cluster networks
{: #faqs-cluster-network}

This section contains answers to some frequently asked questions about {{site.data.keyword.cloud}} cluster networks.
{: shortdesc}

## Can my cluster networks communicate across subnets?
{: #can-my-cluster-networks-communicate-acrosssubnets}
{: faq}

Yes, as long as your cluster network virtual network interfaces configure the route correctly. Both virtual network interfaces must configure the route tables to send traffic through the same interface that it would receive traffic from the other virtual server (subnet). These route tables are needed only if the virtual server instances have at least one cluster network that isn't present on the other virtual server instance.
