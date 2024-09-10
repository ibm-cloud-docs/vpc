---

copyright:
  years: 2022, 2023
lastupdated: "2024-09-10"

keywords: disaster recovery for VPC, failover for VPC, BC for VPC, DR for VPC, business continuity for VPC, disaster recovery for VPC

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding business continuity and disaster recovery for IBM Cloud VPC
{: #bc-dr}

 [Disaster recovery](#x2113280){: term} involves a set of policies, tools, and procedures for returning a system, an application, or an entire zone to full operation after a catastrophic interruption. It includes procedures for copying and storing an installed system's essential data in a secure location, and for recovering that data to restore normalcy of operation.
{: shortdesc}

## Responsibilities
{: #bc-dr-responsibilities}

For more information about your responsibilities, see [Understanding your responsibilities when you use Virtual Private Cloud](/docs/vpc?topic=vpc-responsibilities-vpc).

## Disaster recovery strategy
{: #bc-dr-strategy}

{{site.data.keyword.cloud_notm}} has [business continuity](#x3026801){: term} plans in place to provide for the recovery of services within hours if a disaster occurs. You are responsible for your data backup and associated recovery of your content.

IBM Cloud VPC provides mechanisms to protect your data and restore service functions. Business continuity plans are in place to achieve targeted [recovery point objective](#x3429911){: term} (RPO) and [recovery time objective](#x3167918){: term} (RTO) for the service. The following table outlines the targets for VPC services.

| Disaster recovery objective | Target Value   |
|---|---|
|  RPO | VPN for VPC (site-to-site): 24 hours |
|  RTO | VPN for VPC (site-to-site): 5 hours  |
{: caption="Table 1. RPO and RTO for IBM Cloud VPC services" caption-side="bottom"}

## Locations
{: #ha-locations}

For more information about service availability within regions and zones, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).
