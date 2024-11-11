---

copyright:
  years: 2024
lastupdated: "2024-11-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues and limitations for cluster networks
{: #limitations-cluster-network}

Before you create a cluster network, review the following known issues and limitations.
{: shortdesc}

## Known issue
{: #known-issues-cluster-networks}

- The new instance profiles are expected to take about 20 minutes to start.

## Limitations
{: #limitations-cluster-networks}

- Cluster network attachments can only be set when the instance is stopped or created.
- Cluster network traffic is isolated and cannot be routed outside. Any access to a cluster network must be through an attached instance. As a result, without connectivity between the cluster network and the VPC network, the following resources cannot connect with cluster networks:
   - Endpoint gateways
   - Floating IPs
   - Load balancers
   - Private Path services
   - Public gateways
   - VPNs

- Services that are not supported:
   * Flow logs
   * Network ACLs
   * Routing tables
   * Secondary IPs
   * Security groups
   * Service gateway
