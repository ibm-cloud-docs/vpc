---

copyright:
  years: 2024
lastupdated: "2024-10-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues and limitations for cluster networks
{: #limitations-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

Before you create a cluster network, review the following known issues and limitations.
{: shortdesc}

## Known issue
{: #known-issues-cluster-networks}

- The new instance profiles are expected to take about 20 minutes to start.

## Limitations
{: #limitations-cluster-networks}

- Cluster network attachments can only be set when the instance is stopped or created.
- Cluster network traffic is isolated and cannot be routed outside. Any access to a cluster network must be through an attached instance. As a result, without connectivity between the cluster network and the VPC network, the following resources cannot connect with cluster networks:
   - Public gateways
   - VPNs
   - Load balancers
   - Floating IPs
   - Endpoint gateways
   - Private Path services

- Services that are not supported:
   * Flow logs
   * Security groups
   * Routing tables
   * Secondary IPs
   * Network ACLs
   * Service gateway
