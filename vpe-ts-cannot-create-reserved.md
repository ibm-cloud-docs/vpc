---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-27"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create a reserved IP address?
{: #troubleshoot-create-reserved-ip}
{: troubleshoot}
{: support}

When you create endpoint gateways, you can specify the same number of reserved IP addresses as the number of availability zones (AZs) that are in the region. It's typically three, one reserved IP address per AZ. Also, you can have only one reserved IP address per subnet because subnets don't span AZs.
{: shortdesc}

Unable to create a reserved IP address.
{: tsSymptoms}

Possible causes:
{: tsCauses}

1. If the endpoint gateway exists, and a reserved IP address is being added to it, you might find that a reserved IP address is already associated with the endpoint gateway in the same AZ as the one that you are trying to add.
1. If a new endpoint gateway is being created with multiple reserved IP addresses, and two reserved IP addresses are being specified in the same AZ.

Check to see whether either of these causes apply, then modify your configuration per the limitations. For example, you can't have more than one reserved IP address per subnet.
{: tsResolve}
