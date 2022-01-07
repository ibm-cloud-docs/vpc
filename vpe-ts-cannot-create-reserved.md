---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create a reserved IP?
{: #troubleshoot-create-reserved-ip}
{: troubleshoot}
{: support}

When creating endpoint gateways, you can specify as many reserved IPs as there are availability zones (AZs) in the region, typically three (one reserved IP per AZ). Also, you can have only one reserved IP per subnet because subnets don't span AZs.  
{: shortdesc}

Unable to create a reserved IP address.
{: tsSymptoms}

Possible causes:
{: tsCauses}

1. If the endpoint gateway exists, and a reserved IP is being added to it, there might be a reserved IP already associated with the endpoint gateway in the same AZ as the reserved IP that you are trying to add.
1. If a new endpoint gateway is being created with multiple reserved IPs, there might be two reserved IPs being specified in the same AZ.

Check to see if either of these causes apply, then modify your configuration per the limitations. For example, you can't have more than one reserved IP per subnet.
{: tsResolve}
