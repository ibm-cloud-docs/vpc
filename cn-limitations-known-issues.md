---

copyright:
  years: 2024
lastupdated: "2024-11-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues and limitations for cluster networks
{: #limitations-cluster-network}

Before you create a cluster network, review the following known issues and limitations.
{: shortdesc}

## Known issues
{: #known-issues-cluster-networks}

- The new instance profiles are expected to take about 20 minutes to start.
- When [creating an instance](/apidocs/vpc/latest#create-instance) using the API, the `instance.create` Activity Tracker event will be missing if you specify the `cluster_network_attachments` property. If you want to avoid this, you can [create instance cluster network attachments](/apidocs/vpc/latest#create-instance-cluster-network-attachment) separately.
- If a cluster network reserved IP's `auto_delete` property is set to `false`, and it is attached to a cluster network interface, any attempt to delete the cluster network will result in it being stuck in a `deleting` state. To avoid this, before you delete the cluster network itself, first delete the cluster network interfaces, then delete the cluster network reserved IPs. You can also avoid this scenario by [updating cluster network reserved IPs](/apidocs/vpc/latest#update-cluster-network-reserved-ip) and setting the `auto_delete` property to `true`.

## Limitations
{: #limitations-cluster-networks}

- Cluster network attachments can only be set when the instance is stopped or created.
- Cluster network traffic is isolated and cannot be routed outside. Any access to a cluster network must be through an attached instance. As a result, without connectivity between the cluster network and the VPC network, the following resources cannot connect with cluster networks:
   - [Floating IPs](/docs/vpc?topic=vpc-fip-about&interface=ui)
   - [Load balancers](/docs/vpc?topic=vpc-nlb-vs-elb&interface=ui)
   - [Private Path services](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui)
   - [Public gateways](/docs/vpc?topic=vpc-about-public-gateways&interface=ui)
   - [Virtual Private Endpoint gateways](/docs/vpc?topic=vpc-about-vpe&interface=ui)
   - [VPNs](/docs/vpc?topic=vpc-vpn-overview&interface=ui)


- Services that are not supported:
   * [Flow logs](/docs/vpc?topic=vpc-flow-logs&interface=ui)
   * [Network ACLs](/docs/vpc?topic=vpc-using-acls)
   * [Routing tables](/docs/vpc?topic=vpc-about-custom-routes)
   * [Secondary IPs](/docs/vpc?topic=vpc-vni-about-secondary-ip)
   * [Security groups](/docs/vpc?topic=vpc-using-security-groups)
