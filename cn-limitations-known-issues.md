---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for cluster networks
{: #known-issues-cluster-networks}

Before you create a cluster network, review the following known issues.
{: shortdesc}

- The new instance profiles are expected to take about 20 minutes to start.
- When you [create an instance](/apidocs/vpc/latest#create-instance) by using the API with the `cluster_network_attachments` property specified, the `instance.create` Activity Tracker event isn't generated. To avoid this issue, [create cluster network attachments](/apidocs/vpc/latest#create-cluster-network-attachment) separately.
