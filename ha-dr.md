---

copyright:
  years: 2018, 2025
lastupdated: "2025-05-05"

keywords: high availability, disaster recovery, SLA, placement group

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery
{: #ha-dr-vpc}

All {{site.data.keyword.cloud}} general availability (GA) services have a Service Level Agreement of 99.99% availability.
{{site.data.keyword.vpc_short}} is offered in Dallas, Toronto, Montreal, Frankfurt, London, Madrid, Tokyo, Osaka, Sydney, Washington DC, and São Paulo. Each location has three different zones for redundancy.
{: shortdesc}

For more information about the high
availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}, see [High availability through redundancy](/docs/resiliency?topic=resiliency-ha-redundancy#zero-downtime). For more information about high
availability and disaster recovery for {{site.data.keyword.vpc_short}}, see
[Your responsibilities by using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc). You can
also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

For an example of deploying a highly available web application, see [web app multi-zone resiliency](/docs/pattern-vpc-vsi-multizone-resiliency?topic=pattern-vpc-vsi-multizone-resiliency-overview).

For more information about how you can use Veeam software to back up your storage data on a virtual server instance, see
[About Veeam](/docs/vpc?topic=vpc-about-veeam).

Placement Groups supports anti-affinity placement strategies for high availability workloads. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

## IBM Cloud Load Balancer for VPC and VPN for VPC backups
{: #lb-vpn-backups}

{{site.data.keyword.cloud_notm}} Load Balancer for VPC and VPN for VPC have off-site storage and replication of configuration data in an out-of-region disaster recovery node with daily backups. The disaster recovery location and backups are located within the regulatory boundary.
