---

copyright:
  years: 2024, 2026
lastupdated: "2026-06-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for cluster networks
{: #known-issues-cluster-networks}

Before you create a cluster network, review the following known issues.
{: shortdesc}

- The new instance profiles are expected to take about 20 minutes to start.
- When you [create an instance](/docs/apis/vpc/latest#create-instance) by using the API with the `cluster_network_attachments` property specified, the `instance.create` Activity Tracker event isn't generated. To avoid this issue, [create cluster network attachments](/docs/apis/vpc/latest#create-cluster-network-attachment) separately.
