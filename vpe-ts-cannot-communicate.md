---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-27"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I communicate with the {{site.data.keyword.cloud_notm}} service from my virtual server instance?
{: #troubleshoot-cannot-communicate}
{: troubleshoot}
{: support}

To communicate with the {{site.data.keyword.cloud}} server, your endpoint gateway must be configured correctly.
{: shortdesc}

Cannot communicate with the {{site.data.keyword.cloud_notm}} service.
{: tsSymptoms}

This issue can be due to multiple causes.
{: tsCauses}

If you are unable to communicate with the {{site.data.keyword.cloud_notm}} service from your virtual server instance, follow these steps:
{: tsResolve}

1. Verify that the service instance is valid (not deleted).
1. Check all reserved IP addresses that are associated with the endpoint gateway and make sure they are valid IP addresses. For more information, see [Associated reserved IP shows address 0.0.0.0](/docs/vpc?topic=vpc-troubleshoot-reserved-ip).
1. Verify that the service can be reached by using the reserved IP address instead of the URL, assuming one was provided by the service. For more information, see [Virtual server instance can access a service by using the reserved IP, but cannot access the service's URL](/docs/vpc?topic=vpc-troubleshoot-cannot-access-url).
1. Verify that connectivity to cloud service endpoints is functional - `host google.com 161.26.0.10`. If the connection fails, open an IBM Support case.
1. Verify that network ACLs are defined on the virtual server instance (or reserved IP) subnet that prevents communication between the two.
