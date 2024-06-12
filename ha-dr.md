---

copyright:
  years: 2018, 2023
lastupdated: "2023-06-21"

keywords: high availability, disaster recovery, SLA, placement group

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery
{: #ha-dr-vpc}

All {{site.data.keyword.cloud}} general availability (GA) services have a Service Level Agreement of 99.99% availability.
{{site.data.keyword.vpc_short}} is offered in Dallas, Toronto, Frankfurt, London, Madrid, Tokyo, Osaka, Sydney, Washington DC, and São Paulo. Each location has three different data centers for redundancy.
{: shortdesc}

See [ensure zero downtime](/docs/overview?topic=overview-zero-downtime#zero-downtime) to learn more about the high
availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. For more information about high
availability and disaster recovery for {{site.data.keyword.vpc_short}}, see
[Your responsibilities by using Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc). You can
also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

For examples of deploying a highly available web application, see [Building a highly available 3-tier web application in VPC](/docs/ha-infrastructure?topic=ha-infrastructure-ha-3-tier).

For more information about how you can use Veeam software to back up your storage data on a virtual server instance, see
[About Veeam](/docs/vpc?topic=vpc-about-veeam).

Placement Groups supports anti-affinity placement strategies for workload high availability. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

## IBM Cloud Load Balancer for VPC and VPN for VPC Backups
{: #lb-vpn-backups}

{{site.data.keyword.cloud_notm}} Load Balancer for VPC and VPN for VPC have off-site storage and replication of configuration data in an out-of-region disaster recovery node with daily backups. The disaster recovery location and backups are located within the regulatory boundary.
