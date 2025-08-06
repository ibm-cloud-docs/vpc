---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for cluster networks
{: #limitations-cluster-network}

Before you create a cluster network, review the following known issues.
{: shortdesc}

- The new instance profiles are expected to take about 20 minutes to start.
- When [creating an instance](/apidocs/vpc/latest#create-instance) that uses the API, the `instance.create` Activity Tracker event is missing if you specify the `cluster_network_attachments` property. If you want to avoid this issue, you can [create cluster network attachments](/apidocs/vpc/latest#create-cluster-network-attachment) separately.
