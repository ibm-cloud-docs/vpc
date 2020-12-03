---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: vpe, virtual private endpoint, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't I communicate with the {{site.data.keyword.cloud_notm}} service from my virtual server instance?
{: #troubleshoot-cannot-communicate}
{: troubleshoot}
{: support}

To communicate with the {{site.data.keyword.cloud}} server, your endpoint gateway must be configured correctly.
{: shortdesc}

Cannot communicate with the {{site.data.keyword.cloud_notm}} service.
{: tsSymptoms}

This issue can be due to multiple causes. See "How to fix it" for details.
{: tsCauses}

If you are unable to communicate with the {{site.data.keyword.cloud_notm}} service from your virtual server instance, follow these steps:
{: tsResolve}

1. Check whether the service instance is valid (not deleted).
1. Check all reserved IPs associated with the endpoint gateway and make sure they have valid IP addresses. See [Associated reserved IP shows address 0.0.0.0]() for details.
1. Check whether the service can be reached by using the reserved IP address instead of the URL, assuming one was provided by the service. See [VSI can access a service using
    the reserved IP, but cannot access the service's URL]() for details.
1. Check whether connectivity to cloud service endpoints is functional - `host google.com 161.26.0.10`. If this fails, open an IBM Support case.
1. Check whether network ACLs are defined on the virtual server instance (or reserved IP) subnet that prevents communication between the two.
